# SROI Handheld Data-Acquisition Device / Strawberry-Harvesting End Effector

> 🌐 [中文版](README_ch.md)

An **end effector** for strawberry harvesting, available in two configurations:

- **Handheld data-acquisition configuration** — driven by a grip + trigger-style "recording-trigger button", paired with a RealSense D405 camera and a spatial-tracking Tracker, used to collect teleoperation / demonstration data.
- **Joint-motor configuration** — the end interface mates with the outer flange of a joint motor and is driven directly by the motor, so it can be mounted on a robotic arm to perform actual harvesting.

Both configurations share the same "parallel gripper + linear guide" transmission end. You switch between them simply by swapping the "rocker adapter" and the drive source.

> Want to build one? Get the parts from the [BOM](BOM.md), then follow the [Assembly Guide](#assembly-guide).

---

## Directory Structure

```
3DModel/                     # SolidWorks source files
├── *.SLDPRT                 # Parts (custom-drawn)
├── *.SLDASM                 # Assemblies (two: handheld + joint-motor)
├── 五金件/                  # Fastener / bearing reference models (do NOT print — for assembly reference only)
└── STL/                     # Exports for 3D printing
BOM.md                       # Bill of materials (single source of truth)
README.md
```

> `STL/` contains only the parts that need to be **3D-printed**; standard parts (linear guides, carriages, bearings) are provided as `.SLDPRT` under `3DModel/` for assembly reference only and must be **purchased separately**.

---

## Mechanism

The end effector uses a **rocker + linkage + linear guide** transmission that converts a single rotary input into the parallel open/close motion of the gripper:

```
Handheld: press Button (linear) ──▶ U-cam slot on core-block pivot ──▶ pivot (rotation)
Motor:    motor shaft ──▶ rocker-adapter-motor ──▶ rocker (rotation)
                                     │
                                     ▼
                           rocker (rotates 5° → 50°)
                                     │  linkages ×2
                                     ▼
                 adapter block ── MGN9C carriage ── translates along MGN9 rail
                                     │
                                     ▼
                            parallel gripper  open / close
        After releasing, a compression spring inside the button cavity provides the return force (the gripper opens automatically).
```

**Key transmission parameters (small-opening configuration):**

| Parameter | Value |
|---|---|
| Rocker rotation travel | 45° (5° → 50°) |
| Corresponding carriage travel | 24 mm |
| Current rail length | MGN9 156 mm (full opening ~96 mm) |

> **Design trade-off**: Although an arc-shaped cam slot was intended to make the motion smoother, in testing it produced a "gripper doesn't move → impact" dead zone in the mid-stroke, making it harder to control. The final design uses a **straight U-slot** with the angle adjusted in the closing section to amplify the gripping force — a compromise between smoothness and gripping force (see [Version Differences](#version-differences)).

---

## Bill of Materials (BOM)

The full BOM is maintained in a separate file: **👉 [BOM.md](BOM.md)**

Overview: 18 types of 3D-printed parts · socket / countersunk / flat-head hex screws · hex shoulder screws (M4 step screws) · MGN9 + MGN5 linear guides · MR115 flanged bearings · D405 / Tracker / Raspberry Pi / battery.

> The BOM is the "shopping list" (quantities aggregated by spec); the assembly guide below is "where it goes" (each step lists the parts used in that step). To adjust quantities, edit `BOM.md` — this README does not duplicate them.

---

## Assembly Guide

> No heat-set inserts are used anywhere: machine screws self-tap directly into the 3D-printed parts. Tapping generates frictional heat that softens PLA and degrades thread quality — so when two screws are involved, **tighten them alternately in small steps**, and **pre-tap the thread once with a screw, back it out, then drive it in for the final assembly**; this also gives better control over perpendicularity.
>
> Step-by-step photos for each step are TBD.
>
> **Assembly order**: Steps 1–5 are independent modules that can each be assembled separately; Step 6 (core block) and Step 7 (final assembly) must be interleaved (the top cover goes on last) — see Step 6 for details.

### Step 1: Raspberry Pi case assembly

1. The four mounting through-holes on the Raspberry Pi must first be enlarged with a **Φ3 drill bit** (the original size is slightly smaller than Φ3).
2. The **3-pin wire** for the **recording-trigger button** must be soldered to the Raspberry Pi pins in advance.
3. Place the Raspberry Pi into **case-bottom**; the two holes nearest the 40-pin header are for the screws that fix the Raspberry Pi to the case-bottom.
   - **2 × socket-head hex M3×6**
4. Place the **e-ink display** on top (the two fixing screws from step 3 must be installed before the e-ink display, otherwise they interfere).
5. Place the case top cover on and drive screws in from the bottom to lock the top cover to the base.
   - **2 × countersunk-head hex M3×10**

> After assembly: cutouts are reserved for the SD card, power button/indicator, Type-C power port, and Micro-HDMI port; one of the Micro-HDMI cutouts leaves room to route the recording-button wire; a ~1 mm gap is left between the e-ink viewing window and the display, plus the gap at the USB port, giving overall good ventilation.

### Step 2: Rocker assembly

1. The rocker is printed as separate parts to improve quality: the common part is the rocker arm, with a cylinder underneath; two cylinder variants are designed for "joint-motor adapter" or "handheld-device adapter". First combine the two rocker parts with four screws. For the **joint-motor** connection the orientation does not matter; for the **handheld-device** connection there is a specific orientation requirement (this could later be optimized to be orientation-independent).
   - **4 × socket-head hex M3×8**
2. Both ends of the rocker have a pre-made **Φ3.6 through-hole** for the M4 screw (chamfered for easier driving), using a machine screw as a self-tapping screw. When two screws are involved, **tap them alternately in small steps** with cooling time in between; and **pre-tap once before final assembly** (see the general notes above) to control perpendicularity to the mounting plane.
   - **2 × hex shoulder screw M4×Φ5×6** (Φ5 = shoulder diameter, 6 = shoulder length)
3. **(Handheld device)** Install one flanged bearing on each side of the cylinder (coaxial), with a shoulder screw passing through the middle and driven into the reserved hole in the core block. The Φ11 hole for the rear flanged bearing is chamfered so it sinks a bit further when inserted — this is intentional; add a washer between the tail bearing and the core block as a spacer so the rotation is not obstructed. Try to **drive this in as perpendicular as possible** — although the self-tapping hole is somewhat self-aligning, driving it crooked makes the pivot non-perpendicular to the core-block end face and affects overall operation.
   - **2 × flanged bearing MR115 5×11×5**
   - **1 × hex shoulder screw M4×Φ5×12**
   - **1 × bearing-face flat washer 5×8×0.5**
4. **(Joint motor)** The motor rotor flange has 6 threaded holes; only 4 of them are used when mounting the rocker.
   - **4 × socket-head hex M3×8**

### Step 3: Parallel gripper assembly

1. Take one **MGN9 rail** (156 mm long) and two **MGN9C carriages**.
2. Take two **adapter plates** (which connect the carriage, the gripper, and the linkage) and fix them to the MGN9C carriages. Before fully tightening, manually adjust the two adapter plates to be as parallel as possible — their mounting accuracy directly affects subsequent gripper accuracy. The adapter plates have an **orientation requirement**; refer to the illustration.
   - **8 × socket-head hex M3×6**
3. Fix the gripper to the adapter plate (Φ2.6 through-hole on the adapter plate, self-tapping machine screw, chamfered). Before fully tightening, manually adjust the gripper position so it is as misalignment-free as possible when closed.
   - **8 × socket-head hex M3×8**
4. Attach **AprilTags** at the reserved positions on the gripper.
5. Take two linkages; on each, press in two flanged bearings (see illustration for orientation).
   - **4 × flanged bearing MR115 5×11×5**
6. Combine the linkage with the adapter plate: pass a shoulder screw through the flanged bearing and drive it into the reserved hole on the adapter plate (self-tapping); try to keep the screw axis perpendicular to the mounting plane for better transmission quality. There is an **orientation requirement**; see the illustration.
   - **2 × hex shoulder screw M4×Φ5×6**

### Step 4: Camera and camera-mount assembly

1. The camera mount fits the D405, with the RGB lens aligned to the device center. Two countersunk holes on the back of the mount connect to the D405, with room reserved for routing the data cable.
   - **2 × countersunk-head hex M3×8**
2. **(Optional)** If you are not using ORB-SLAM to compute trajectory, you can use an HTC Tracker: first mount it on the adapter plate (the adapter plate has a boss for Tracker positioning), then secure it with a single screw.
   - **1 × flat-head hex 1/4″-20 × 5/16″** (≈ M6.35×7.94)
3. **(Optional)** Connect the Tracker adapter plate to the side of the camera mount: the adapter plate has slots for pre-inserted nuts (the nuts can be inserted in advance and will not fall out), and the camera mount has countersunk holes.
   - **2 × countersunk-head hex M3×12**
   - **2 × plain nut M3**

### Step 5: Grip assembly

1. Assemble the recording button: first attach single-pass standoffs to the button.
   - **2 × single-pass standoff M3×6+3**
   - **2 × plain nut M3**
2. Fix the recording button inside the grip (long fingers help); two countersunk holes on the side of the grip.
   - **2 × countersunk-head hex M3×8**
3. Assemble the charging port of the grip bottom cover: insert the battery's **Type-C receptacle** into the reserved cavity of the bottom cover, pass the two wires through the U-slot of the charging-port fitting, then use the charging-port fitting to rigidly constrain the receptacle inside the cavity, and finally fix the fitting to the bottom cover.
   - **1 × socket-head hex M3×6**
4. Insert the battery into the grip; the Type-C plug (output end) and the recording-button wires are both routed out through the cable passthrough of the bottom cover (tidy the wires — it is not difficult).
5. Install the grip bottom cover onto the grip (two countersunk holes on the bottom).
   - **2 × countersunk-head hex M3×8**

### Step 6: Core-block assembly

> The core block is itself a module, but it must be **interleaved** with the final assembly in Step 7: once the top cover is closed on the core block, the screw holes for connecting the rocker and grip become blocked and hard to drive. So first assemble the modules of Steps 1–5 individually, then assemble the core block while attaching the rocker and grip to it as described below, leaving the **top cover for last**.

1. Take the rail base and **pre-insert 3 nuts** at the positions used to connect the camera mount (the hexagonal counterbores are sized so that, once inserted, the nuts do not fall out).
   - **3 × plain nut M3**
2. Assemble the rail base with the core block: the six through-holes on the rail base were originally for connecting the outer flange of the joint motor — the handheld device simply replaces the motor with the core block, and with a suitable transmission setup the gripper can be opened/closed manually. The rail base and core block are fixed with four screws.
   - **4 × socket-head hex M3×10**
3. Prepare one **40 mm MGN5 rail** and one MGN5C carriage, and fix the rail to the corresponding mounting position on the core block.
   - **3 × MGN5-rail-dedicated screw M2×7** (pan-head Phillips; ordinary commercial M2 machine screws may not fit)
4. Assemble the button: the button consists of a 3D-printed part and an **externally threaded cylindrical pin**. The pin requires good perpendicularity when driven in — as a rule of thumb, pre-tap the thread once by hand with a socket-head screw, then drive in the pin (the pin has a slotted head, which is tiring and easy to drive crooked).
   - **1 × externally threaded cylindrical pin M3×5** (M3 thread length ~4 mm, pin portion Φ4×5)

> **The following is interleaved with final assembly; leave the top cover for last.**

5. **Connect the rocker to the core block** (this step actually belongs to final assembly, but the top cover would block the screw, so it must be done before closing): drive the rocker pivot — already fitted with its flanged bearings — into the reserved hole in the core block, as perpendicular as possible. Details in Step 2.3.
6. **Insert the spring and button**: place the **spring** into the cylindrical cavity reserved on the core block, and place the button into the slide — at this point the cylindrical pin on the button should slide into the U-slot on the cylindrical portion of the rocker, and pressing the button should rotate the rocker. Fix the button to the MGN5 standard carriage (the rail + carriage constrain the button's linear motion). Before fully tightening, adjust the button position so it does not rub against the inner wall of the channel; when adjusted correctly, pressing should be very smooth.
   - **2 × socket-head hex M2×6**
   - **2 × flat washer 2×4×0.5**
   - **1 × compression spring 0.6×8.5×45** (wire dia × outer dia × length)
7. **Install the grip** (likewise must be done before closing the cover): the core block has 3 reserved through-holes for connecting the grip.
   - **3 × socket-head hex M3×8**
8. **Finally, put on the core-block top cover** (its main purpose is to prevent the button from popping out). At this point the rocker and grip are both attached to the core block. Depending on the use case, choose either a **full-stroke** or **half-stroke** top cover.
   - **2 × flat-head hex M4×10**

### Step 7: Final assembly

> The connections of the rocker to the core block and of the grip to the core block were already done in Step 6 (before closing the cover). This step attaches the remaining modules to the core block / rail base / grip.

1. Fix the rail of the parallel-gripper assembly to the rail base on the core block; before fully tightening, it is recommended to manually adjust the rail position.
   - **4 × socket-head hex M3×8**
2. Assemble the camera mount with the rail base (nuts are already pre-inserted on the rail base, see Step 6.1).
   - **3 × socket-head hex M3×12**
3. Fix the Raspberry Pi case to the grip (two through-holes on the lower part of the grip). After assembly, the bottom edge of the case is flush with the bottom edge of the grip, so it can stand upright and stably on a table.
   - **2 × countersunk-head hex M3×10**

---

## Version Differences

### V1 → V2 (travel limiting)
- **V1 problem**: the travel limit of the sliding gripper relied entirely on the printed parts themselves, causing noticeable jamming in use.
- **V2 attempt**: switched to "smooth shaft + brass bushing" constraint (only 10 mm of installation space was available, and no linear bearing that small could be found).
- **V2 result**: worse than pure 3D printing — sliding friction requires clearance, making it unstable; pure printing was smoother but had play, and the transmission still jammed.

### V2 → V3 (gripper closing / gripping force / transmission)
- **Insufficient gripper closing force** (the main deficiency of the current device): the gripper needs to carry a blade and cut the strawberry stem; during handheld acquisition it should also be able to cut a strawberry stem by hand, but the thumb-operated button cannot transmit enough force to the gripper.
  - Improvement: change the lengths of the rocker and linkages to amplify the force in the closing section — **shorten the rocker** (greater closing torque), **lengthen the linkages**.
- **Rail too long**: the current 156 mm rail opens ~96 mm fully, which is too large for fine strawberry harvesting (~40 mm travel is needed). Shortening the rail would make the whole device smaller and more elegant — left unadjusted for now, listed as an optional optimization.
- **Transmission smoothness**: after switching to **MGN9 linear guides**, it is silky smooth — all previous jamming essentially stemmed from insufficient transmission smoothness.
- **Cam-slot finalization**: an arc slot produces a dead zone and impact in the mid-stroke with poor results; the final design uses a **straight U-slot** with the angle adjusted in the closing section to amplify the gripping force (a smoothness-vs-force compromise). To constrain the linear motion of the button, an **MGN5 rail** was introduced, and the core block was redesigned accordingly.
- **Camera mount / Tracker**: the D405 mount was slimmed down to only the mounting holes for minimum volume; the Tracker mounting plate is removable as a unit, with the Tracker center placed directly behind the camera to simplify the TF transform.
- **Rail-base nut holes**: the 3 inset nut holes were changed to a "snap-in, won't fall out" size for easier installation.
- **Raspberry Pi case**: cutout dimensions optimized; the bottom cover thickened underneath so it stands more stably and avoids being damaged by force near the e-ink display.

---

## Known Limitations / Future Work

- The gripper does not yet have a **blade slot** or a blade-fixing scheme; whether it can cut a strawberry stem by hand still needs verification.
- The button currently cannot use the full travel; the cam slot still has fine-tuning room — the goal is "linear button ↔ linear rotation of the pivot".
- The rail can be shortened (156 mm → enough for ~40 mm travel) to reduce overall volume.

---

## License / Notes

The SolidWorks source files, STLs, and documentation in this project are used for internal R&D and data acquisition. If you want to replicate it, be sure to verify the quantities and specs against the assembly files (`.SLDASM`) — items marked `※` in [BOM.md](BOM.md) are values inferred from the symmetric structure.
