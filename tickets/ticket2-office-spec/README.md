# Ticket N2 — "We are kitting out the new office"



## 1\. What was asked

From: Daniyar K. (Director)

Subject: computers for the new office



We are opening the second office and need four computers — accountant,

sales, web developer, designer. Budget 3,000,000 tenge for everything.

Everyone says we should just buy the same four machines, is that right?

Make sure they last a few years. Monitors included.



## 2\. What was actually needed

The request names four roles and a total budget, but it does not describe the applications, number of simultaneous tasks, file sizes, expected monitor quality, peripherals, operating-system edition, warranty, or whether assembly and backup power must fit inside the budget. Those details affect the configuration more than job titles alone.



I would ask which accounting/CRM/design applications are used, how many browser tabs and video calls are typical, whether the designer works with large color-critical files, whether Windows Pro is required by company policy, and whether peripherals/UPS/assembly are included. I use the following reasonable assumptions because no answers were supplied:

* All four employees use Windows 11 Pro, have one 1 TB SSD, at least 16 GB RAM and a monitor, as required by the brief.
* Accountant uses 1C/Excel and ordinary office applications; sales uses a browser CRM and video calls; web developer uses VS Code, browsers, Node.js and occasional Docker; designer uses 2D Adobe/Figma applications, not 3D or video editing.
* 32 GB is assigned to the three heavier multitasking/creative roles. The accountant receives 16 GB, which meets the minimum and fits the stated load.
* The designer receives a modest dedicated GPU and a better IPS monitor. The other three use integrated graphics.
* Prices are a snapshot from Kazakhstan-facing vendor pages on 2026-11-11; prices and stock can change. The budget includes reasonable allowances for items where the public listing did not expose an exact price. These allowances are not represented as confirmed vendor quotations.

The assumption that four identical PCs are best is incorrect. The roles have different bottlenecks: memory for sales and development, display/GPU for design, and balanced CPU/SSD performance for accounting.3. What I did

Steps in order. Commands in code blocks. Decisions with reasons.



## 3\. What I did



1. Converted each role's daily work into a load description and identified its likely bottleneck.
2. Selected a mainstream AM4 Ryzen 5 platform to avoid paying for workstation-class performance that the brief does not require.
3. Kept the hard requirements in every build: at least 16 GB RAM, 1 TB SSD, licensed Windows and a monitor.
4. Allocated 32 GB RAM to sales, web development and design; kept 16 GB for accounting.
5. Added a modest RTX 3050 only to the designer's PC. It supports GPU-accelerated 2D workflows without the cost of a high-end graphics card.
6. Compared a DIY parts-based approach with pre-built systems from a local vendor, and reserved money for shared accessories, UPS units and assembly.



#### Budget summary

|Item|Cost|
|-|-:|
|Accountant PC + monitor + Windows|469,830 KZT|
|Sales PC + monitor + Windows|532,830 KZT|
|Web developer PC + monitor + Windows|532,830 KZT|
|Designer PC + monitor + Windows|733,520 KZT|
|Keyboards and mice (4 sets)|40,000 KZT|
|UPS units (2)|60,000 KZT|
|Assembly/testing allowance|60,000 KZT|
|**Planned total**|**2,429,010 KZT**|
|**Remaining from 3,000,000 KZT**|**570,990 KZT**|



## Three required questions

#### 1\. Which PC would I upgrade first after 18 months?

The web developer's PC. Development tools, browser profiles, Docker images and project dependencies tend to grow over time. I would first check actual RAM use; if it is regularly close to 32 GB, upgrade to 64 GB. This is a targeted upgrade based on measured use, not an automatic purchase.



#### 2\. What do pre-built machines cost, and what is gained or lost by not building them?

Eco-Service's published Almaty list provides three relevant tower configurations: Eco-Start at approximately 252,000 KZT (Ryzen 5 5600G/16 GB/1 TB), Alma Budget NVIDIA at 332,000 KZT (i3-12100F/RTX 3050/16 GB/1 TB), and Step Up RX 6600 at 462,000 KZT (Ryzen 5 5600/16 GB/1 TB). To price four roles using this one vendor's published options, I would use Eco-Start for the accountant (252,000), Eco-Start plus a provisional 63,000 KZT RAM upgrade for sales (315,000), Step Up plus the same RAM upgrade for web development (525,000), and Alma Budget NVIDIA plus the RAM upgrade for the designer (395,000). The resulting estimated tower subtotal is 1,487,000 KZT. This is not a formal quote: the RAM upgrade amount is the difference between the listed 16 GB and 32 GB kits used in this estimate.



These are tower prices, not complete office packages. Add the three standard monitors and one designer monitor (193,660 KZT), four Windows 11 Pro OEM licenses (411,960 KZT), and the shared 160,000 KZT allowance for peripherals, UPS and assembly. The estimated complete pre-built route is therefore about 2,252,620 KZT, before replacing any allowances with written quotes. A Sulpak alternative, ARG Nomad B310, is listed at 472,990 KZT for a tower with 16 GB, 1 TB SSD and RTX 3050, but without an operating system.



Buying pre-built gives one supplier responsible for assembly, a single warranty contact and less staff time spent checking compatibility. Building from parts gives better control over the exact RAM, SSD, monitor and upgrade path, and avoids paying for a graphics card where it is not needed. The disadvantages are multiple component warranties, time spent assembling/testing, and the need to confirm compatibility and final component stock. The published pre-built comparison is approximate because Eco-Service's list does not quote the exact RAM upgrades, monitors, Windows licenses or peripherals.



#### 3\. Who is most likely to complain within the first month, and about what?

The designer is the most likely to notice a limitation first: the 24-inch-class Full HD monitor may feel cramped when Photoshop, panels and reference material are open together, even though the GPU and RAM are suitable for the described 2D workload. If the designer confirms that they routinely work with several large canvases, I would use part of the remaining budget for a 27-inch 1440p IPS monitor with a documented color gamut.



## 4\. Evidence

tickets/ticket2-office-spec/builds.md — workload, bottleneck, trade-off and total for each role.

tickets/ticket2-office-spec/budget.csv — itemized parts, KZT prices, source links and the price date.

tickets/ticket2-office-spec/evidence/pcpartpicker-links.md — compatibility reference links and explanation of regional pricing limitations.

tickets/ticket2-office-spec/evidence/vendor-quotes/price-snapshot.md — captured vendor listing details and URLs used for this estimate.

tickets/ticket2-office-spec/evidence/vendor-quotes/prebuilt-comparison.md — published local pre-built examples and their limitations.

## 5\. What I would do differently

Before final purchase, I would ask each employee to confirm their actual applications and typical project sizes, then request written quotes for all components from one or two local suppliers. This would replace the provisional case, PSU, monitor and assembly allowances with exact prices and confirm warranty terms. I would also test the designer's monitor for color coverage before ordering.

