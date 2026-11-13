# Lab N ─ 05



Machine: DESKTOP-EA7P1RT, lab01

Date: 13.11.2026



## What i did:



1. Download staff-broken.csv from canvas
2. Add it to shared folder
3. mkdir -p \~/labs/lab05-encoding/scripts \~/labs/lab05-encoding/evidence
4. cd \~/labs/lab05-encoding/evidence
5. cp staff-broken.csv /home/arrrs/labs/lab05-encoding/evidence
6. sha256sum staff-broken.csv
7. file staff-broken.csv            | tee file-before.txt
8. file -e csv staff-broken.csv     | tee -a file-before.txt
9. xxd -l 16 staff-broken.csv       | tee hexdump-before.txt
10. for enc in CP1251 CP866 KOI8-R RK1048 PT154; do
echo "=== $enc"
iconv -f "$enc" -t UTF-8 staff-broken.csv | head -5
done
11. iconv -f UTF-8 -t UTF-8 staff-broken.csv > /dev/null \&\& echo valid
12. iconv -f RK1048 -t UTF-8 staff-broken.csv > /dev/null \&\& echo ok
13. iconv -f RK1048 -t UTF-8 staff-broken.csv | less
14. cat -A staff-broken.csv | head -3
15. tr -cd '\\r' < staff-broken.csv | wc -c
16. wc -l staff-broken.csv
17. cd ..
18. nano scripts/fix.sh
19. \#!/usr/bin/env bash
20. set -euo pipefail
SRC\_ENC="RK1048"
IN=evidence/staff-broken.csv
OUT=evidence/staff-fixed.csv
{
printf '\\xEF\\xBB\\xBF'
iconv -f "$SRC\_ENC" -t UTF-8 "$IN" # | sed 's/\\r$//'
} > "$OUT"
file -e csv "$OUT"
xxd -l 16 "$OUT"
21. bash scripts/fix.sh
22. cd \~/labs/lab05-encoding/evidence
23. file staff-fixed.csv         | tee file-after.txt
24. file -e csv staff-fixed.csv  | tee -a file-after.txt
25. xxd -l 16 staff-fixed.csv
26. cat staff-fixed.csv
27. sha256sum staff-broken.csv
28. scp -r arrrs@192.168.201.129:\~/labs/lab05-encoding/evidence arrrs@192.168.201.129:\~/labs/lab05-encoding/scripts "C:\\Users\\Admin\\OneDrive\\Desktop\\lol\\osarch-bolatbek\\labs\\lab05-encoding"



## Result:

## Diagnosis (before the fix)

file says:

First 16 bytes: d2e0 e12e b93b d4c8 ce3b c4ee ebe6 edee 

the table:

|Offset|Bytes|What they are|
|-|-|-|
|00 – 02|d2 e0 e1|No BOM present. These are raw encoded bytes for the first characters of the table header|
|03|2e|ASCII period . character|
|04 |b9|A single-byte Cyrillic character|
|05|3b|ASCII semicolon ; character|
|06 — 08|d4 c8 ce|Encoded bytes for the next column header title|
|09|3b|ASCII semicolon ; character|
|0A – 0D|d4 ee eb e6|Raw encoded bytes for part of the third column header|
|0E – 0F|ed ee|Continuation of the third column header text bytes|



Encoding:     A single-byte code page   — evidence: Cyrillic letters as single bytes between 80 and FF in the hex dump

Line ending:  CRL   — evidence: "...a.kassymova@tumar-office.kz;12.09.2019^M$" ^M$ at the end = CRLF

BOM:          absent — evidence: sequence does not start with EF BB BF



## What each application was doing wrong:

Excel:        assumed the system's local Windows-1251 code page because the file lacked a UTF-8 BOM, resulting in mojibake for extended characters.

LibreOffice:  assumed incorrect text import filter heuristics because the file metadata lacked an explicit character set flag.

cat:          assumed raw byte streaming directly to the standard output because the terminal font lacked glyph support for extended characters rather than experiencing a conversion error.



## Fix

Command / script:  scripts/fix.sh

1. Changed: encoding RK1048 -> UTF-8 ; line ending kept;

&#x20;        BOM added because Excel requires a UTF-8 BOM to auto-detect the encoding properly when double-clicked.



## Verification

file after:   (from evidence/file-after.txt)

Opened in:    Excel 2016 16.0.4266.1001, LibreOffice Calc 26.8.3, cat on Ubuntu 26.04

&#x20;             — evidence/opens-correctly/Excel

&#x20;             — evidence/opens-correctly/LibreOffice

&#x20;             — evidence/opens-correctly/Ubuntu



## What did  not work the first time:

The first time fix.sh script was ran i accidentally put "staff-broken.csv" in both IN and OUT. The broken file got overwritten, the data inside got jumbled. I fixed the fix.sh script and ran it again, adding the broken file from the shared folder before that.



## Evidence:

evidence/staff-broken.csv ─ untouched original file

evidence/staff-fixed.csv ─ converted output file

evidence/file-before.txt ─ output of the file command on the original data

evidence/file-after.txt ─ output of the file command on the fixed data

evidence/hexdump-before.txt ─ the first 16 bytes in hex

evidence/opens-correctly ─ screenshots of the fixed table in excel, LibreOffice and ubuntu

