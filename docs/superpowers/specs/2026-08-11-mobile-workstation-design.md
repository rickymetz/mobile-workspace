# Mobile Workstation — Design Spec

**Date:** 2026-08-11
**Status:** Approved, pending pre-order verification (see Open Risks)

A rolling, height-adjustable cart that carries a permanent Windows desktop and
accepts a docked laptop. Both share one monitor, one keyboard and mouse, one
network connection, and one power cord.

---

## 1. Requirements

| Requirement | Decision |
|---|---|
| Location | One room. Repositioned and height-adjusted daily. |
| Network | Flat subnet with the house LAN. AirPlay must work from house devices. |
| Laptop docking | Core feature, used weekly or more. |
| Laptops supported | Apple Silicon MacBook Air/Pro 13–14", assorted Windows laptops. |
| Uplink | Wi-Fi only. No Ethernet drop is possible in the room. |
| Power protection | None. No UPS. |
| Budget | ~$1,000 for everything not already owned. |

### Already owned

- HP OMEN desktop with RTX 4070
- Laptop (docks to the cart)
- LG 32GX850A-B monitor — 32" 4K OLED, 165 Hz, 2× HDMI 2.1 + 1× DisplayPort 1.4
- Keyboard and mouse using a 2.4 GHz USB dongle
- Apple TV 4K
- VESA monitor arm and laptop arm, both clamp-on

### Out of scope

- Battery backup of any kind
- Power over Ethernet
- Off-site or multi-room use

---

## 2. Key decisions

### 2.1 No HDMI switch — connect all three sources directly

The monitor has three video inputs and the build has exactly three sources.

| Source | Input | Result |
|---|---|---|
| HP OMEN | DisplayPort 1.4 | 4K at 165 Hz, G-Sync active |
| Guest laptop | HDMI 2.1 #1 | 4K at 60 Hz (the dock's ceiling) |
| Apple TV 4K | HDMI 2.1 #2 | 4K at 60 Hz HDR |

**Why:** the original plan routed every source through a 4K-at-60-Hz HDMI
switch. That would cap a 165 Hz OLED at 60 Hz and would likely break G-Sync.
Direct connection preserves the full panel, removes a box and a power brick,
and saves $29.

**Cost:** the planned "Extra HDMI" front input has no free port. To connect an
occasional extra device, unplug the dock from HDMI 2.1 #1.

**Switching method:** the monitor's own input button.

### 2.2 USB switching stays

The monitor has a built-in USB hub, but it is a single USB-B upstream port. It
follows whichever host is plugged into that port, not the selected video input.
It therefore cannot replace the USB switch.

The UGREEN USB 3.0 sharing switch moves the keyboard and mouse dongle and any
shared USB devices between the OMEN and the dock.

**Feed the switch from the dock's 10 Gbps USB-C port, not its USB-A ports.**
The A83C3's two USB-A ports run at 480 Mbps. Using one would drop every shared
device to USB 2.0 speed.

### 2.3 Travel router runs in Client/Bridge mode

Router mode would put the cart behind NAT on its own subnet. AirPlay and
HomeKit discovery use mDNS, which does not cross NAT, so nothing on the house
network would find the Apple TV.

In Client/Bridge mode, cart devices get IP addresses from the house router and
sit on one flat subnet.

**Accepted limitation:** all cart traffic shares a single Wi-Fi uplink.

### 2.4 Plain gigabit switch, not PoE

TP-Link TL-SG105 replaces the TL-SG1005P. Wired clients are the OMEN, the Apple
TV, and the dock — three, plus the uplink, leaving one spare port.

**Why:** the PoE model cost $28 more to serve a hypothetical future camera or
access point. A PoE injector costs about $15 on the day one is actually needed.

### 2.5 No UPS

**Why:** it consumed $200 of a $1,000 budget and added roughly 20 lb to a cart
that is moved daily. Power in the room is reliable enough that clean-shutdown
protection is not worth that trade.

**Consequence:** an outage drops the desktop uncleanly. Accepted.

### 2.6 Single power inlet

One cord runs from the wall to the cart. One 8-outlet mountable strip on the
stationary lower column feeds everything on board.

Seven loads, not six — the cart's own lift motor was missing from the original
plan:

1. HP OMEN
2. Monitor
3. Anker dock (140 W brick)
4. TP-Link TL-WR3002X
5. TP-Link TL-SG105
6. Apple TV 4K
7. Cart lift motor

Peak draw is roughly 770 W, comfortable on one 15 A circuit. Use a strip with a
14 AWG cord.

**Rule:** the strip's cord is the only thing unplugged to move the cart.

### 2.7 Physical layout

**Mount to the stationary lower section:** power strip, router, network switch,
dock. Only the cables feeding the two clamped arms cross the moving joint.

**Cable slack:** the column travels from 25.7" to 51.2", about 25.5". Cables
crossing the joint need a service loop of about 30" in spiral wrap.

**Load and stability.** Capacity is not the constraint:

| Item | Weight |
|---|---|
| HP OMEN | ~22 lb |
| Monitor (panel only) | 10.8 lb |
| Monitor arm | ~8 lb |
| Laptop arm | ~5 lb |
| Laptop | ~3.5 lb |
| Electronics and cables | ~8 lb |
| **Total** | **~57 lb** against an 88 lb desktop rating |

Tipping is the constraint. Two clamp-on arms cantilever about 27 lb off one
edge at heights up to 51".

**Mitigations:**

1. Put the OMEN on the lowest platform, on the same side as the arms, to
   counterbalance.
2. Lock the casters whenever the cart is parked.
3. Lower the cart fully before moving it.

---

## 3. Bill of materials

### To buy

| Item | Price |
|---|---|
| VIVO DESK-V111VTW cart | $219.99 |
| Anker Nano Docking Station A83C3 | $149.99 |
| TP-Link TL-WR3002X travel router | $68.99 |
| UGREEN USB 3.0 sharing switch | $45.99 |
| TP-Link TL-SG105 gigabit switch | ~$16.99 |
| 8-outlet mountable power strip, 14 AWG | ~$25 |
| Cables — see the cable count below | ~$85 |
| Mounting hardware | ~$25 |
| **Total** | **~$637** |

Roughly $360 under the ceiling.

### Cable count

Derived from the wiring table in `index.html`.

| Cable | Qty |
|---|---|
| DisplayPort 1.4 | 1 |
| HDMI 2.1 | 2 |
| Cat6, short | 4 |
| USB-A to USB-B | 1 |
| USB-C to USB-B | 1 |
| USB-B | 1 |
| Spiral wrap and anchors | — |

The dock's USB-C upstream cable ships in the box.

### Optional

| Item | Price | Note |
|---|---|---|
| TP-Link UE306 USB 3.0 to Gigabit Ethernet | $11.99 | Fallback for Windows laptops with unreliable USB-C. |

### Cut from the original plan

| Item | Saved | Reason |
|---|---|---|
| Anker 4-in/1-out HDMI switch | $29.14 | Would cap the 165 Hz monitor at 60 Hz. |
| GOLDENMATE LiFePO₄ UPS | $199.99 | Budget and weight. |
| PoE premium (SG1005P → SG105) | $28.00 | Speculative requirement. |

---

## 4. How the system works

### Using the desktop

1. Select DisplayPort on the monitor.
2. Select the HP OMEN on the USB switch.

### Using a laptop

1. Mount the laptop on the arm.
2. Connect one USB-C cable to the dock. This carries video, Ethernet, USB, and
   charging up to 100 W.
3. Select HDMI 1 on the monitor.
4. Select Laptop on the USB switch.

### Using the Apple TV

Select HDMI 2 on the monitor. The Apple TV uses its own remote and stays on the
house network.

### Moving the cart

1. Lower the cart fully.
2. Unplug the single power cord.
3. Unlock the casters.

---

## 5. Open risks

Each item must be checked. The first two should be checked before ordering.

| # | Risk | Check | Fallback |
|---|---|---|---|
| 1 | The cart's work-surface edge may not accept a C-clamp. Both arms depend on it, and VIVO does not publish the edge profile. | Measure edge thickness and confirm no frame lip within 2" of the edge. | Different cart, or move the monitor to its own stand. |
| 2 | The TL-WR3002X may not bridge its Ethernet port onto the host subnet in every mode. | Plug a laptop into its LAN port and confirm it receives a house-router IP. | Try Extender mode. Worst case, accept NAT and lose AirPlay from house devices. |
| 3 | Windows laptops vary in USB-C DisplayPort Alt Mode and PD support. | Test each laptop before relying on the one-cable path. | Use the laptop's own charger and HDMI port, plus the UE306 adapter. |
| 4 | Chaining the dock hub into the USB switch adds a second hub tier. | Test the keyboard, mouse, and any shared device before permanent mounting. | Connect shared USB directly to the dock and switch manually. |
| 5 | Heat buildup from the OMEN and dock on the lower platform. | Leave clearance around the tower and check temperatures under load. | Reposition components or add spacing. |

---

## 6. Deliverables

1. `index.html` — the one-page build sheet, revised to match this spec. It
   carries the diagrams and the wiring table, which is the connection-by-
   connection reference to build from.
2. This document.
