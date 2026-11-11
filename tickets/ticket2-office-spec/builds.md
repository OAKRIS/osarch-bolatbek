### Build 1 — Accountant



**Load:** 1C (or similar accounting package), Excel with large sheets, browser with 10–15 tabs, e-mail, electronic document flow. One or two heavy spreadsheet recalculations per day. No video editing, no local servers.

**Bottleneck:** Single-core CPU speed and RAM. 1C and Excel recalculate on one thread most of the time; a slow core means the interface freezes during month-end close. 16 GB is the floor; 8 GB would swap constantly with 1C plus Excel plus browser.

**Trade-off:** No dedicated GPU. The integrated graphics in the Ryzen 5 5600G handle the desktop, Excel and 1C without issue. The money saved goes into a reliable 1 TB NVMe SSD and a decent IPS monitor — the accountant stares at numbers all day, so screen quality matters more than graphics horsepower.



**Total:** 335,000 KZT (budget.csv rows 2–8)





### Build 2 — Sales Manager



**Load:** CRM in a browser (30–40 tabs open simultaneously), Excel, four hours of video calls a day, often all at once. Chrome alone with forty tabs plus a Teams/Zoom call can reach 11–12 GB of RAM.

**Bottleneck:** Memory, not CPU. On 16 GB the machine starts swapping during a customer call. The CPU idles in this role; the memory does not.

**Trade-off:** Took a Ryzen 5 5600 instead of a Ryzen 7 and spent the saved money on 32 GB of RAM instead of 16 GB. The extra 16 GB costs roughly 60,000–70,000 KZT and directly prevents the one symptom this person would feel every day: a frozen call while the disk swaps.



**Total:** 390,000 KZT (budget.csv rows 10–16)





### Build 3 — Web Developer



**Load:** VS Code with extensions, two or three browsers with DevTools open, Node.js, a local dev server, Docker (occasionally), Git. The editor and browsers are the heavy part; builds are short and mostly single-threaded.

**Bottleneck:** RAM and storage speed. Node modules and Docker images fill disk quickly; 1 TB is the floor. 32 GB RAM prevents the editor from stuttering when Docker and three browsers are open together. CPU cores matter less than single-core speed for the editor and browser.

**Trade-off:** No dedicated GPU. Web development does not use the GPU for anything meaningful. The integrated graphics are sufficient. The money goes into 32 GB RAM and a fast NVMe SSD — the two things that actually make the editor and local server feel responsive.



**Total:** 395,000 KZT (budget.csv rows 18–24)





### Build 4 — Designer



**Load:** Photoshop, Illustrator, Figma, sometimes InDesign. Large PSD files (500 MB–2 GB), many layers, colour-accurate work. No 3D rendering, no video editing.

**Bottleneck:** RAM and color accuracy. Photoshop with large files and multiple layers will consume 16–24 GB easily. 32 GB is the sensible minimum for this role. The monitor matters more here than in any other role: a cheap TN panel with poor color reproduction makes design work unreliable.

**Trade-off:** Took a mid-range GPU (RTX 3050 or equivalent) instead of a high-end card. Photoshop uses the GPU for some filters and canvas acceleration, but a flagship card is wasted money here. The saved budget went into 32 GB RAM and a color-accurate IPS monitor. The CPU is a Ryzen 5 5600 — fast enough for Photoshop's single-core-heavy operations, cheaper than a Ryzen 7.



**Total:** 475,000 KZT (budget.csv rows 26–33)





### Total



335,000 + 390,000 + 395,000 + 475,000 = 1,595,000 KZT



Remaining from 3,000,000: 1,405,000 KZT



This remaining budget covers:

\- Keyboards and mice (4 sets): \~40,000 KZT

\- A UPS for the designer and accountant (power stability): \~60,000 KZT

\- Windows 11 Pro licenses: included in each build below

\- Contingency for price movement between the date of this document and purchase



If the director insists on spending closer to the full budget, the designer's monitor can be upgraded to a 27" 1440p color-accurate model (+80,000 KZT) and the accountant's machine can receive a second 1 TB SSD for local backups (+45,000 KZT). Neither is necessary for the load described.





### Why not four identical machines



The sales manager needs 32 GB RAM; the accountant does not. The designer needs a color-accurate monitor and a GPU; the web developer needs neither. The accountant needs a reliable IPS panel for spreadsheets; the web developer would benefit more from a second monitor than a better one.



Four identical machines at 750,000 KZT each would mean: accountant overpaying for RAM and GPU they do not use, designer getting a monitor that is not good enough for color work, and sales manager getting 16 GB and swapping during calls.

Differentiating the builds is cheaper and produces better outcomes for each role.

