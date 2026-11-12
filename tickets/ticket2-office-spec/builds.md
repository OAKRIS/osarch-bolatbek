### Build 1 — Accountant

**Load:** 1C, Excel with large sheets, browser with 10–15 tabs, e-mail, electronic document flow. One or two heavy spreadsheet recalculations per day. No video editing, no local servers.

**Bottleneck:** Single-core CPU speed and RAM. 1C and Excel recalculate on one thread most of the time; a slow core means the interface freezes during month-end close. 16 GB is the floor; 8 GB would swap constantly with 1C plus Excel plus browser.

**Parts:** Ryzen 5 5600G, MSI A520M-A Pro, DeepCool AG400, Team Group Elite DDR4-3200 16 GB (2x8), Kingston NV3 1 TB NVMe, MSI MAG A550BN 550 W, Corsair 3000D case, Wintek WCH2481B 23.8" IPS 1080p, Windows 11 Pro OEM.

**Trade-off:** No dedicated GPU. The integrated graphics in the Ryzen 5 5600G handle the desktop, Excel and 1C without issue. The only 16 GB build: the saved \~63,000 KZT (versus a 32 GB kit) is not needed for this load. Money goes into a reliable 1 TB NVMe SSD and an IPS monitor — the accountant stares at numbers all day, so screen quality matters more than graphics horsepower.

**Total:** 458,120 KZT (PC 325,130 + monitor 37,990 + Windows 95,000; budget.csv rows 2–10)

\---

### Build 2 — Sales Manager

**Load:** CRM in a browser (30–40 tabs open simultaneously), Excel, four hours of video calls a day, often all at once. Chrome alone with forty tabs plus a Teams/Zoom call can reach 11–12 GB of RAM.

**Bottleneck:** Memory, not CPU. On 16 GB the machine starts swapping during a customer call. The CPU idles in this role; the memory does not.

**Parts:** Ryzen 5 5600G, MSI A520M-A Pro, DeepCool AG400, GeIL Pristine DDR4-3200 32 GB (2x16), Kingston NV3 1 TB NVMe, MSI MAG A550BN 550 W, Corsair 3000D case, Wintek WCH2481B 23.8" IPS 1080p, Windows 11 Pro OEM.

**Trade-off:** Took a mainstream Ryzen 5 5600G instead of a Ryzen 7 and spent the difference on 32 GB of RAM instead of 16 GB. The extra 16 GB costs 63,000 KZT (139,990 vs 76,990 for the 16 GB kit) and directly prevents the one symptom this person would feel every day: a frozen call while the disk swaps.

**Total:** 521,430 KZT (PC 388,440 + monitor 37,990 + Windows 95,000; budget.csv rows 11–19)

\---

### Build 3 — Web Developer

**Load:** VS Code with extensions, two or three browsers with DevTools open, Node.js, a local dev server, Docker (occasionally), Git. The editor and browsers are the heavy part; builds are short and mostly single-threaded.

**Bottleneck:** RAM and storage speed. Node modules and Docker images fill disk quickly; 1 TB is the floor. 32 GB RAM prevents the editor from stuttering when Docker and three browsers are open together. CPU cores matter less than single-core speed for the editor and browser.

**Parts:** Ryzen 5 5600G, MSI A520M-A Pro, DeepCool AG400, GeIL Pristine DDR4-3200 32 GB (2x16), Kingston NV3 1 TB NVMe, MSI MAG A550BN 550 W, Corsair 3000D case, Wintek WCH2481B 23.8" IPS 1080p, Windows 11 Pro OEM.

**Trade-off:** No dedicated GPU. Web development does not use the GPU for anything meaningful. The integrated graphics are sufficient. The money goes into 32 GB RAM and a fast NVMe SSD — the two things that actually make the editor and local server feel responsive.

**Total:** 521,120 KZT (PC 388,130 + monitor 37,990 + Windows 95,000; budget.csv rows 20–28)

\---

### Build 4 — Designer

**Load:** Photoshop, Illustrator, Figma, sometimes InDesign. Large PSD files (500 MB–2 GB), many layers, colour-accurate work. No 3D rendering, no video editing.

**Bottleneck:** RAM and monitor. Photoshop with large files and multiple layers will consume 16–24 GB easily. 32 GB is the sensible minimum for this role. The monitor matters more here than in any other role: a cheap TN panel with poor color reproduction makes design work unreliable.

**Parts:** Ryzen 5 5600G, MSI A520M-A Pro, DeepCool AG400, GeIL Pristine DDR4-3200 32 GB (2x16), Kingston NV3 1 TB NVMe, Palit GeForce RTX 3050 6 GB, MSI MAG A550BN 550 W, Corsair 3000D case, 27" Gigabyte M27QA EK (1440p), Windows 11 Pro OEM.

**Trade-off:** Took an entry-level GPU (RTX 3050 6 GB) instead of a high-end card. Photoshop uses the GPU for some filters and canvas acceleration, but a flagship card is wasted money here. The saved budget went into 32 GB RAM and a 27" 1440p monitor (158,690 KZT, about four times the price of the standard monitor). The CPU is the same Ryzen 5 5600G as the other builds — fast enough for Photoshop's single-core-heavy operations, cheaper than a Ryzen 7. Note: the M27QA is a general-purpose IPS panel; its color gamut and factory calibration have not been verified here (see README, question 3).

**Total:** 808,810 KZT (PC 555,120 + monitor 158,690 + Windows 95,000; budget.csv rows 29–38)

\---

### Shared items

|Item|Cost (KZT)|budget.csv row|
|-|-:|-:|
|Keyboard and mouse sets x4 (Genius KM-170)|26,560|39|
|UPS x2 (SVC U-650-L, accountant and designer)|63,980|40|
|Assembly and basic POST test x4|40,000|41|
|**Shared subtotal**|**130,540**||

\---

### Total

458,120 + 521,430 + 521,120 + 808,810 + 130,540 = **2,440,020 KZT** (matches the total in budget.csv row 42)

Remaining from 3,000,000: **559,980 KZT**

This remaining budget covers:

* Contingency for price movement between the date of this document (prices from 11.11.2026) and purchase
* Optional upgrades, if the director wants to spend closer to the full budget: a second 1 TB SSD for local backups on the accountant's machine (about +45,000 KZT, indicative, not in budget.csv), or a color-calibrated monitor or hardware calibrator for the designer if the M27QA proves insufficient. Neither is necessary for the load described.

Windows 11 Pro licenses (95,000 KZT each) are already included in each build above.



### Why not four identical machines

The sales manager needs 32 GB RAM; the accountant does not. The designer needs a larger, higher-resolution monitor and a GPU; the web developer needs neither. The accountant needs a reliable IPS panel for spreadsheets; the web developer would benefit more from a second monitor than a better one.

Four identical machines at 750,000 KZT each (the full budget split four ways) would mean: accountant overpaying for RAM and GPU they do not use, designer getting a monitor that is not good enough for design work, and sales manager getting 16 GB and swapping during calls.

Differentiating the builds is cheaper and produces better outcomes for each role.

