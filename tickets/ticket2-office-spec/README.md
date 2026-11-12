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
* The designer receives a modest dedicated GPU (RTX 3050 6 GB) and a 27" 1440p monitor. The other three use integrated graphics and a 23.8" IPS 1080p monitor.
* Prices are a snapshot from Kazakhstan-facing vendor pages (shop.kz, itmag.kz, dns-shop.kz, svc.kz) dated 11.11.2026; prices and stock can change. Every line in the budget comes from a listed vendor price in budget.csv; none are unquoted allowances. They are listing prices, not formal written quotations.

The assumption that four identical PCs are best is incorrect. The roles have different bottlenecks: memory for sales and development, display/GPU for design, and balanced CPU/SSD performance for accounting.

## 3\. What I did

1. Converted each role's daily work into a load description and identified its likely bottleneck.
2. Selected a mainstream AM4 Ryzen 5 5600G platform (MSI A520M-A Pro board, DeepCool AG400 cooler, MSI MAG A550BN 550 W PSU, Corsair 3000D case) for all four builds, to avoid paying for workstation-class performance that the brief does not require.
3. Kept the hard requirements in every build: at least 16 GB RAM, 1 TB SSD (Kingston NV3), licensed Windows 11 Pro OEM and a monitor.
4. Allocated 32 GB RAM (GeIL Pristine 2x16) to sales, web development and design; kept 16 GB (Team Group Elite 2x8) for accounting.
5. Added a modest Palit RTX 3050 6 GB only to the designer's PC. It supports GPU-accelerated 2D workflows without the cost of a high-end graphics card. The designer also gets the 27" Gigabyte M27QA EK monitor; the other three get the Wintek WCH2481B.
6. Compared a DIY parts-based approach with pre-built systems from a local vendor, and added shared accessories, UPS units and assembly.

#### Budget summary

|Item|Cost|
|-|-:|
|Accountant PC + monitor + Windows|458,120 KZT|
|Sales PC + monitor + Windows|521,430 KZT|
|Web developer PC + monitor + Windows|521,120 KZT|
|Designer PC + monitor + Windows|808,810 KZT|
|Keyboards and mice (4 sets)|26,560 KZT|
|UPS units (2)|63,980 KZT|
|Assembly and POST test (4)|40,000 KZT|
|**Planned total**|**2,440,020 KZT**|
|**Remaining from 3,000,000 KZT**|**559,980 KZT**|

## Three required questions

#### 1\. Which PC would I upgrade first after 18 months?

The web developer's PC. Development tools, browser profiles, Docker images and project dependencies tend to grow over time. I would first check actual RAM use; if it is regularly close to 32 GB, upgrade to 64 GB. This is a targeted upgrade based on measured use, not an automatic purchase.

#### 2\. What do pre-built machines cost, and what is gained or lost by not building them?

Eco-Service's published Almaty list provides three relevant tower configurations: Eco-Start at approximately 252,000 KZT (Ryzen 5 5600G/16 GB/1 TB), Alma Budget NVIDIA at 332,000 KZT (i3-12100F/RTX 3050/16 GB/1 TB), and Step Up RX 6600 at 462,000 KZT (Ryzen 5 5600/16 GB/1 TB). To price four roles using this one vendor's published options, I would use Eco-Start for the accountant (252,000), Eco-Start plus a 63,000 KZT RAM upgrade for sales (315,000), Step Up plus the same RAM upgrade for web development (525,000), and Alma Budget NVIDIA plus the RAM upgrade for the designer (395,000). The resulting estimated tower subtotal is 1,487,000 KZT. This is not a formal quote: the RAM upgrade amount is the price difference between the 16 GB and 32 GB kits in budget.csv (139,990 − 76,990).

These are tower prices, not complete office packages. Add the three standard monitors and one designer monitor (3 × 37,990 + 158,690 = 272,660 KZT), four Windows 11 Pro OEM licenses (4 × 95,000 = 380,000 KZT), and keyboards/mice plus UPS units (26,560 + 63,980 = 90,540 KZT; assembly is not needed because the towers arrive built). The estimated complete pre-built route is therefore about 2,230,200 KZT, roughly 209,820 KZT below the 2,440,020 KZT parts-based plan, before any written quotes. A Sulpak alternative, ARG Nomad B310, is listed at 472,990 KZT for a tower with 16 GB, 1 TB SSD and RTX 3050, but without an operating system.

Buying pre-built gives one supplier responsible for assembly, a single warranty contact and less staff time spent checking compatibility, and it is somewhat cheaper on paper. Building from parts gives better control over the exact RAM, SSD, monitor and upgrade path, a consistent AM4 platform across all four machines (one set of spare parts), and avoids paying for a graphics card where it is not needed. The disadvantages are multiple component warranties, time spent assembling/testing, and the need to confirm compatibility and final component stock. The pre-built comparison is approximate because Eco-Service's list does not quote the exact RAM upgrades, and the towers use different CPUs and GPUs than the DIY builds.

#### 3\. Who is most likely to complain within the first month, and about what?

The designer is the most likely to notice a limitation first, but now it is about color rather than screen size: the 27" 1440p Gigabyte M27QA gives plenty of room for Photoshop panels, but it is a general-purpose IPS monitor and I have not verified a documented color gamut or factory calibration. If the designer works on print or color-critical jobs, colors may not match what they expect. In that case I would use part of the remaining 559,980 KZT for a hardware calibrator or a monitor with a documented color gamut. The 6 GB of video memory on the RTX 3050 is a secondary risk with very large canvases.

## 4\. Evidence

tickets/ticket2-office-spec/builds.md — workload, bottleneck, parts, trade-off and total for each role.

tickets/ticket2-office-spec/budget.csv — itemized parts, KZT prices, source links and the price date (11.11.2026).

tickets/ticket2-office-spec/evidence/pcpartpicker-links.md — compatibility reference links and explanation of regional pricing limitations.

tickets/ticket2-office-spec/evidence/vendor-quotes/price-proof — captured vendor listing details used for this estimate.



## 5\. What I would do differently

Before final purchase, I would ask each employee to confirm their actual applications and typical project sizes, then request written quotes for all components from one or two local suppliers. This would replace the listing prices with confirmed ones and confirm warranty terms. I would also test the designer's monitor for color coverage before ordering, and double-check the motherboard price for the sales build (budget.csv row 13 lists 30,900 KZT while the same board is 30,590 KZT in the other three builds, which looks like a typo worth 310 KZT).

