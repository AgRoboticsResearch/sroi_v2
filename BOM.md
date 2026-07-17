# BOM — SROI Handheld Data-Acquisition Device / Strawberry-Harvesting End Effector

> 🌐 [中文版](BOM_ch.md)

> This file is the **single source of truth** for the bill of materials; `README.md` references it by link.
> To adjust quantities / specs, edit this file directly — no need to change the README.
>
> Quantities have been checked step-by-step against the [Assembly Guide](README.md#assembly-guide); `※` = inferred from the symmetric structure, defer to the SolidWorks assembly file (`.SLDASM`).
> Hardware reference models are in `3DModel/五金件/`, for assembly reference only — **do not print**.

> Quantities default to the **handheld data-acquisition configuration**; differences for the joint-motor configuration are in [Section 5](#5-joint-motor-configuration-optional).

---

## 1. 3D-Printed Parts

> Print only the parts in this section; rails / carriages / bearings are purchased parts (see Section 3), and their `.SLDPRT` files are for assembly reference only.
> STL exports are in `3DModel/STL/`.

| # | Name | Source file | Qty | Notes |
|---|---|---|---|---|
| 1 | Gripper | `夹爪.SLDPRT` | 2 | Blade slot to be added |
| 2 | Adapter block (carriage↔gripper) | `转接块-转接滑块和夹爪.SLDPRT` | 2 | |
| 3 | Linkage (rocker↔adapter block) | `连杆-连接摇杆和转接块.SLDPRT` | 2 | |
| 4 | Rocker — common part | `摇杆-摇杆通用部分.SLDPRT` | 1 | Shared by both handheld / motor configurations |
| 5 | Rocker — handheld adapter (linear drive) | `摇杆-转接手持装置.SLDPRT` | 1 | Current version for handheld configuration |
| 6 | Rocker — motor adapter | `摇杆-转接电机.SLDPRT` | 1 | Joint-motor configuration |
| 7 | Handheld part — core block | `手持部分-核心块.SLDPRT` | 1 | Includes U-cam slot + spring cavity |
| 8 | Handheld part — top cover (full-stroke) | `手持部分-顶盖-全行程.SLDPRT` | 1 | Choose one of #8 / #9 (STL named `手持部分-顶盖.STL`) |
| 9 | Handheld part — top cover (half-stroke) | `手持部分-顶盖-半行程.SLDPRT` | optional 1 | For travel limiting |
| 10 | Button (gripper open/close control) | `按钮.SLDPRT` | 1 | Mechanical thumb button: pressing drives the rocker via the cylindrical pin → gripper opens/closes; a different part from the recording-trigger button (an electronic part, see Section 4 #6) |
| 11 | Grip — main body | `握把-主体.SLDPRT` | 1 | |
| 12 | Grip — bottom cover | `握把-底盖.SLDPRT` | 1 | Includes Type-C receptacle cavity + cable passthrough |
| 13 | Grip — charging-port fitting | `握把-充电口配件.SLDPRT` | 1 | |
| 14 | Camera mount — D405 | `相机支架-D405.SLDPRT` | 1 | |
| 15 | Tracker mounting plate | `Tracker安装板.SLDPRT` | 1 | Removable as a unit |
| 16 | Raspberry Pi case — bottom | `树莓派盒子-底.SLDPRT` | 1 | |
| 17 | Raspberry Pi case — top | `树莓派盒子-盖.SLDPRT` | 1 | |
| 18 | Rail base | `导轨座-与关节电机外法兰固定.SLDPRT` | 1 | Print `…(微调导轨孔位).STL`; shared by both handheld / motor configurations |

---

## 2. Fasteners and Hardware

> **Head types**: 杯头内六角 (socket head) = cylindrical head (DIN 912 / ISO 4762); 沉头内六角 = flat/countersunk head (DIN 7991 / ISO 10642); 平头内六角 = large flat head.
> **打塞螺丝 (shoulder screw)** = step screw (shoulder screw), spec written as `M4 × shoulder Φ5 × shoulder length`; no heat-set inserts are used anywhere in this design — machine screws self-tap directly into the 3D-printed parts.
> Reference-model directory: `3DModel/五金件/`.

### 2.1 Socket-head hex (cylindrical head)

| Spec | Qty | Use |
|---|---|---|
| M2 × 6 | 2 | Button → MGN5 carriage (Step 6) |
| M3 × 6 | 11 | Raspberry Pi → case-bottom ×2 (Step 1); adapter plate → MGN9 carriage ×8 (Step 3); charging-port fitting → grip bottom cover ×1 (Step 5) |
| M3 × 8 | 19 | Rocker two-part combine ×4 (Step 2); gripper → adapter plate ×8 (Step 3); gripper rail → rail base ×4 (Step 7); grip → core block ×3 (Step 6) |
| M3 × 10 | 4 | Rail base → core block ×4 (Step 6) |
| M3 × 12 | 3 | Camera mount → rail base ×3 (Step 7, driven into pre-inserted nuts) |

### 2.2 Countersunk-head hex (flat head)

| Spec | Qty | Use |
|---|---|---|
| M3 × 8 | 6 | D405 → camera mount ×2 (Step 4); recording button → grip ×2, grip bottom cover → grip ×2 (Step 5) |
| M3 × 10 | 4 | Case top cover → base ×2 (Step 1); Raspberry Pi case → grip ×2 (Step 7) |
| M3 × 12 | 2 ※ (optional) | Tracker adapter plate → camera-mount side ×2 (Step 4, only when using the Tracker) |

### 2.3 Flat-head hex (large flat head)

| Spec | Qty | Use |
|---|---|---|
| M4 × 10 | 2 | Core-block top cover ×2 (Step 6) |
| 1/4″-20 UNC × 5/16″ (≈ M6.35 × 7.94) | 1 (optional) | Tracker → adapter-plate center ×1 (Step 4, 1/4″ standard thread) |

### 2.4 Hex shoulder screw (step screw, M4 / shoulder Φ5)

| Spec | Qty | Use |
|---|---|---|
| M4 × Φ5 × 6 (shoulder length 6) | 4 | Rocker-arm self-tapping ×2 (Step 2); linkage → adapter plate self-tapping ×2 (Step 3) |
| M4 × Φ5 × 12 (shoulder length 12) | 1 | Handheld rocker center pivot ×1, passes through two sets of flanged bearings and drives into the core block (Step 2) |

### 2.5 Pins / nuts / standoffs / washers / springs

| Name | Spec | Qty | Use |
|---|---|---|---|
| Externally threaded cylindrical pin | M3 × 5 (thread length ~4, pin Φ4×5) | 1 | Button pivot pin, slides into the rocker U-slot (Step 6) |
| Plain nut | M3 | 7 | Recording-button standoffs ×2 (Step 5); rail base pre-inserted ×3 (Step 6); Tracker adapter plate pre-inserted ×2 ※ (optional, Step 4) |
| Single-pass standoff | M3 × 6+3 | 2 | Recording button ×2 (Step 5) |
| Flat washer | 2 × 4 × 0.5 (inner × outer × thick) | 2 | Button → carriage clamping face (Step 6) |
| Bearing-face flat washer | 5 × 8 × 0.5 | 1 | Spacer between handheld-rocker tail bearing and core block (Step 2) |
| Compression spring | 0.6 × 8.5 × 45 (wire dia × outer dia × length) | 1 | Button return, indirectly provides the gripper-open return force (Step 6) |
| MGN5-rail-dedicated screw | M2 × 7 (pan-head Phillips) | 3 | MGN5 rail → core block (Step 6, ordinary M2 machine screws may not fit — dedicated screw required) |

---

## 3. Mechanical Standard Parts (purchased)

| # | Name | Spec | Qty | Notes |
|---|---|---|---|---|
| 1 | Linear guide rail | MGN9, 156 mm | 1 | Main gripper rail; planned to be shortened to reduce opening |
| 2 | Linear guide carriage | MGN9C | 2 | One left, one right |
| 3 | Linear guide rail | MGN5, 40 mm | 1 | Button linear drive |
| 4 | Linear guide carriage | MGN5C | 1 | Button carriage (referred to as the N5 standard block in the docs) |
| 5 | Flanged bearing | MR115, 5 × 11 × 5 (inner × outer × height, flange Φ12.5×1) | 6 | Handheld rocker pivot ×2 (Step 2); linkages ×4 (Step 3) |
| 6 | Gripper blade | — | TBD | Cuts the strawberry stem; the gripper does not yet have a blade slot |

---

## 4. Electronic Components

| # | Name | Spec | Qty | Notes |
|---|---|---|---|---|
| 1 | Depth camera | Intel RealSense D405 | 1 | Main camera |
| 2 | Spatial-tracking Tracker | — | 1 (optional) | Mounted directly behind the camera, removable as a unit; used when not using ORB-SLAM |
| 3 | Main control board | Raspberry Pi (40-pin header) | 1 | The 4 mounting holes need to be enlarged with a Φ3 drill bit |
| 4 | Display | E-ink display | 1 | |
| 5 | Battery pack | 2× 18650 in parallel, with built-in buck-boost chip, 5V/5A Type-C output + Type-C charging port | 1 | Embedded in the grip; the charging port (Type-C receptacle) is constrained by the grip bottom cover + charging-port fitting, and the output end (Type-C plug) is routed through the bottom-cover cable passthrough together with the recording-button wires. Both Type-C ports are part of the battery pack, not purchased separately |
| 6 | Recording-trigger button | 3-pin (electronic part, must be purchased separately) | 1 | Mounted inside the grip; the 3-pin wire is pre-soldered to the Raspberry Pi pins and triggers data recording; a different part from the mechanical button (gripper open/close, see Section 1 #10) |
| 7 | Wire | — | some | Routes through the charging-port fitting U-slot / grip cable passthrough |

---

## 5. Joint-Motor Configuration (optional)

When changing from the "handheld data-acquisition configuration" to the "joint-motor configuration" mounted on a robotic arm, replace / add as follows; everything else (gripper, linkages, rails, rail base) is shared:

| Item | Handheld config | Joint-motor config |
|---|---|---|
| Drive source | Core block + button + grip + Raspberry Pi case | **Joint motor ×1** (outer flange fixed to the rail base; torque must be sufficient to cut the strawberry stem) |
| Rocker adapter | `摇杆-转接手持装置.SLDPRT` | `摇杆-转接电机.SLDPRT` |
| Rocker → drive flange | — | Add **4 × socket-head hex M3 × 8** (4 of the 6 holes in the motor-rotor flange, Step 2.4) |
| Handheld-rocker center pivot | Shoulder screw M4×Φ5×12 + flanged bearings ×2 + flat washer | Not needed |

---

## Notes

- **No heat-set inserts** are used anywhere in this design: machine screws self-tap directly into the reserved through-holes of the 3D-printed parts (rocker ends Φ3.6 through-hole tapped to M4; adapter plate Φ2.6 through-hole tapped to M3, etc., all chamfered for easier driving). It is recommended to pre-tap the thread once before final assembly to control perpendicularity.
- Items marked `※` are inferred from the symmetric structure; defer to the assembly file `.SLDASM`.
- The top-cover STL file is named `手持部分-顶盖.STL`, corresponding to the part `手持部分-顶盖-全行程.SLDPRT` (full-stroke version); the half-stroke STL has the same name with a `-半行程` suffix.
