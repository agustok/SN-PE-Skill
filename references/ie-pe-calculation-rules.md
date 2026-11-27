# IE & PE Engineering Calculation Rules (FATP Standard)

## 1. Calendar & Shift Standards
- **Working days / month**: `22 days` (FATP standard)
- **Effective hours / shift**: `7.16 hours` (`430 minutes` effective out of an **8-hour workday / 1 shift**, accounting for breaks, 5S, and shift handover)
- **1 Line + 1 Shift Priority Rule**: If monthly forecast/demand can be fulfilled within **1 line + 1 shift** (`Planned UPH × 7.16 h × 22 days`), **always prioritize 1 line + 1 shift**. Only add shifts (2–3 shifts) or extra lines when demand exceeds 1-line, 1-shift capacity.
- **Shifts / day**: Up to `3 shifts` max capacity per line; calculate required shifts via `ROUNDUP(Demand / (Planned_UPH * 7.16 * 22 * Lines), 0)`
- **Standard Time Allowance**: `10%` (`0.10`) for fatigue, personal, and minor delay:
  $$\text{Std Time (s)} = \text{Avg Time (s)} \times (1 + 0.10)$$

## 2. Line Balancing & Throughput Formulas
- **Net Time (NT)** per station:
  $$\text{NT (s)} = \frac{\text{Std Time (s)}}{\text{DL}}$$
- **Station UPH**:
  $$\text{UPH} = \frac{3600}{\text{NT (s)}}$$
- **Line Bottleneck NT**:
  $$\text{Line NT}_{\max} = \max(\text{NT}_i)$$
- **Line UPH (max)**:
  $$\text{Line UPH} = \frac{3600}{\text{Line NT}_{\max}}$$
- **Line of Balance (LOB)**:
  $$\text{LOB} = \frac{\sum \text{Std Time}_i}{\text{Line NT}_{\max} \times \sum \text{DL}_i}$$
  - Target LOB: **≥ 80%** (ideal ≥ 85%). Flag stations with loading < 60% for merging, or > 100% of Takt for splitting / adding parallel DL.
- **Units Per Person Hour (UPPH)**:
  $$\text{UPPH} = \frac{\text{Line UPH}}{\text{Total DL}}$$
- **Line Productivity Setting**: **92% – 95%** (default **93%**, or per customer/line target) applied to convert theoretical bottleneck UPH into realistic planned output:
  $$\text{Planned UPH} = \text{Line UPH}_{\max} \times \text{Line Productivity (92\%–95\%)}$$
- **Takt Time**:
  $$\text{Takt (s)} = \frac{3600}{\text{Planned UPH}}$$
- **Operation Loading**:
  $$\text{Op Loading} = \frac{\text{Proposed NT (s)}}{\text{Takt (s)}}$$
- **First Pass Yield (FPY)**:
  $$\text{Total FPY} = \prod_{i} \text{FPY}_i = \exp\left(\sum_i \ln(\text{FPY}_i)\right)$$

## 3. Packing Estimation & Indonesia Regulatory Process Rules
Customer SOPs frequently cover assembly and testing (`P1..Pn`) but omit packing (`PK1..PKn`) or local Indonesia regulatory steps.
1. **Packing Estimation**: When packing is missing from the SOP, add standard FATP packing stations (`PK1..PKn`): device label/IMEI scan, accessory pre-kitting, unit bagging/case, giftbox loading, giftbox weighing & scan, giftbox label & seal, carton packing & label, carton sealing & palletizing/MES pack-out. Mark `工序名称 (SOP)` as `(SOP 无)` / `(Not in SOP)` and note in `Summary` that Packing DL is an **FATP estimate**.
2. **Indonesia Launch Mandatory Process (`MKG` & `Postel` Label)**:
   - For **any product launching in Indonesia**, **MKG** (Manual & Warranty Card / *Manual Kartu Garansi*) insertion **must** be included in the packing process.
   - For **smartphones** (and wireless/RF devices) launching in Indonesia, **Postel (SDPPI) label** printing/application and verification **must** be included in the `Process & Manpower` stations (and `Label printer` accounted for in `Equipment & Aux`).
   - Add missing Packing SOP, MKG, or Postel label artwork/spec to the **Customer Clarification List**.

## 4. Indirect Labor (IDL) Standard & Ratio (`IDL / DL ≈ 30%`)
- **Sizing Rule**: Calculate **IDL at approximately 30% of total DL** (`IDL / DL ≈ 30%`, i.e., `IDL ≈ ROUND(DL × 0.30, 0)`).
- Distribute the IDL headcount across the 9-role FATP standard per line per shift (12 IDL baseline for a ~40 DL full line):
  - `Line leader` (1), `Supporter` (1), `IQC` (1), `IPQC` (1), `OQC` (1), `Material handler` (2), `Packing boy` (2), `Repair technician` (1), `TE / ME` (2).
- On smaller or low-volume lines where 12 IDL would exceed `30%` of DL, scale down by combining or sharing roles across lines (e.g., combined IQC/IPQC/OQC, shared TE/ME, shared Material handler/Packing boy) so that `IDL / DL` stays around **30%**.

## 5. Equipment, Jigs & Consignment Rules (ST/CT-Driven)
- **Source Priority**:
  1. Read the **customer-provided Equipment List** (if supplied) and cross-check every item against the SOP/Testing Plan.
  2. If no equipment list is provided, extract all equipment, testers, jigs/fixtures, and tools directly from the **SOP** and **Testing Plan**.
- **ST / CT-Based Quantity Calculation**:
  Always calculate the required quantity per line using each station's **Standard Time (ST)** or **Equipment Cycle Time (CT)** vs. **Takt Time**:
  $$\text{Qty / station} = \left\lceil \frac{\text{Station ST (s) or Equipment CT (s)}}{\text{Takt (s)} \times \text{Slots per equipment}} \right\rceil$$
  - Example: If coupling test `ST = 135 s` (or `148.5 s` with allowance) and `Takt = 16.5 s` (`218 UPH`), `ROUNDUP(148.5 / 16.5, 0) = 9` sets of CMW500 + shield box + test PC per line.
  - Sum across all stations sharing the same equipment type, and flag any mismatch with customer-provided quantities in the **Customer Clarification List**.
- **Custom (`Y/N`)**:
  - `Y`: Product-specific jigs, fixtures, press molds, RF shield-box inserts, test carriers, custom assembly cradles.
  - `N`: General-purpose equipment (electric screwdrivers, ionizing air guns, label printers, weighing scales, benches, ESD wrist straps, MES scanners/PCs).
- **Propose Customer Consign (`Y/N`)**:
  - `Y`: Dedicated test equipment (e.g., CMW500, audio analyzers, RF shield boxes, test PCs with proprietary SW), golden samples, and custom product-specific jigs/fixtures (`Custom = Y`).
  - `N`: Standard factory/PTSN-supplied production equipment, hand tools, IT/MES hardware, line infrastructure, and consumables.
- **Shareable (`Y/N`)**:
  - `Y`: Facility/support equipment shareable across lines or projects (IQC, Store, Repair, General, shared IT).
  - `N`: Dedicated line/station equipment or consumables.

## 6. Mandatory Baseline Support Equipment (IQC / Store / Repair / IT / General)
Every FATP draft **must** include the following baseline support equipment outside of the production-line equipment (set `Lines/Cells = 1` for shared facility areas):

| Category | Item Name (SOP/Ref) | Description (EN) | Custom | Baseline Qty | Consign | Shareable |
|---|---|---|---|---|---|---|
| `IT / MES` | 电脑 | PC + Monitor (MES binding / SN / weighing / test) | N | Per MES/test stations | N | Y |
| `IT / MES` | — | Server (For MES) | N | 1 | Y | N |
| `IT / MES` | — | Switch HP 16 Port + Toten Box | N | 2 | N | N |
| `IT / MES` | 扫码枪 | Barcode scanner (MES / packing stations) | N | Per scan stations | N | N |
| `IT / MES` | 打印机 | Label printer (SN / giftbox / carton) | N | Per label stations | N | Y |
| `IT / MES` | 电子秤 / 大电子秤 | Electronic scale (unit & carton weighing) | N | 1 each | N | Y |
| `Repair area` | 防静电维修工作台 | ESD workbench with mat & grounding | N | 2 | N | Y |
| `Repair area` | 离子风扇 | Ionizer fan | N | 2 | N | Y |
| `Repair area` | 扫描枪 | Barcode scanner (MES repair log) | N | 2 | N | Y |
| `Repair area` | MES 电脑 | MES repair station PC | N | 1 | N | N |
| `Repair area` | 电批 | Electric screwdriver (bit per product) | N | 2 | N | Y |
| `Repair area` | 放大镜台灯 | Magnifier lamp | N | 2 | N | Y |
| `Repair area` | 加热分离台 | Heating plate / separator for rework | N | 1 | N | Y |
| `Repair area` | 静电手环 | ESD wrist strap | N | 2 | N | Y |
| `IQC area` | 防静电工作台 | ESD workbench with mat & grounding | N | 2 | N | Y |
| `IQC area` | 离子风扇 | Ionizer fan | N | 1 | N | Y |
| `IQC area` | 扫描枪 | Barcode scanner | N | 1 | N | Y |
| `IQC area` | 电脑 | IQC PC (receiving / MES) | N | 1 | N | Y |
| `IQC area` | 放大镜台灯 | Magnifier lamp | N | 1 | N | Y |
| `IQC area` | 游标卡尺 | Vernier caliper | N | 1 | N | Y |
| `Store` | 条码打印机 | Barcode printer – material / reel ID labels | N | 1 | N | Y |
| `Store` | 手动液压搬运车 | Hand Jack Pallet | N | 2 | N | Y |
| `Store` | 200KG 体重秤 | Weighing Scale 200KG | N | 1 | N | Y |
| `Store` | 重型货架 | Heavy Duty Rack | N | 6 | N | Y |
| `Store` | 无螺丝货架 | Boltless Rack | N | 4 | N | Y |
| `Store` | 扫描枪 | Barcode scanner (receiving / issue) | N | 1 | N | Y |
| `General` | 螺丝刀扭力测试仪 | Screw Torque HP-10 | N | 1 | N | Y |
| `General` | ESD测试仪 | Anti-Static ESD Checker | N | 1 | N | Y |
| `General` | 电视 | TV (line/dashboard display) | N | 2 | N | Y |
| `General` | 笔记本电脑 | Laptop | N | 4 | N | Y |
| `General` | 测力计 | Force Gauge | N | 1 | N | Y |

## 7. Manual Soldering Scope & Component Pin-Count Reference (FATP — No SMT)
FATP does **not** use SMT/reflow machines. Only include **manual soldering** operations in `Process & Manpower` and `Equipment & Aux`.

### Scope Boundary
- **In-Scope (Manual Soldering)**: Wire-to-board leads (speaker, mic, motor, battery, antenna), Through-Hole / DIP components (THT resistors, electrolytic capacitors, inductors, diodes, LEDs, switches, jacks, pin headers), shield-can tacking, and minor touch-up on large SMD pads (`≥ 0805 / 1206`).
- **Out-of-Scope (SMT Required — Flag in Clarification List)**: Fine-pitch SMD ICs (`BGA`, `QFN`, `LQFP`), `0201 / 0402 / 0603` passive arrays, full 24-pin USB-C SMD pins, or mass PCBA population. Assume PCBA arrives pre-populated from SMT supplier unless clarified.

### Average Pin / Lead Count by Component Type
When estimating manual soldering cycle time (`CT`) from a BOM/SOP without station timing:
$$\text{Manual Solder CT (s)} \approx \text{Prep/Pick/Place (3–5 s)} + (\text{Pin Count} \times 2.5\text{–}3.5\text{ s/joint}) + \text{Visual/Clean (2–3 s)}$$

| Component Type | Typical Package / Mounting | Avg. Pin / Joint Count | Manual Soldering Feasibility |
|---|---|---|---|
| Resistor (THT) | Axial / DIP | `2 pins` | Yes (Manual / DIP) |
| Capacitor (Electrolytic / Film) | Radial / Axial THT | `2 pins` | Yes (Manual / DIP) |
| Diode / Zener / TVS (THT) | Axial / DO-41 | `2 pins` | Yes (Manual / DIP) |
| LED (THT) | 3mm / 5mm (or RGB) | `2 pins` (`4 pins` RGB) | Yes (Manual / DIP) |
| Inductor / Choke (THT) | Radial / Toroid | `2 pins` (`4 pins` choke) | Yes (Manual / DIP) |
| Transistor / MOSFET | TO-92 / TO-220 | `3 pins` | Yes (Manual / DIP) |
| Tactile / Slide Switch / Button | THT / Hybrid | `2–4 pins` (+ `2` tabs) | Yes (Manual) |
| Wire Leads (Speaker / Mic / Motor / Battery) | Flying wire to PCB pad | `2 wires` (`+ / -`, `2 joints`) | Yes (Manual) |
| DC Power / Barrel Jack | THT | `3 pins` | Yes (Manual) |
| 3.5 mm Audio / Phone Jack | THT / Hybrid | `5–6 pins` | Yes (Manual) |
| Pin Header / Wire Connector (JST/PH) | THT | `2–8 pins` (per pin count) | Yes (Manual) |
| Potentiometer / Rotary Encoder | THT | `3–5 pins` (+ `2` tabs) | Yes (Manual) |
| Large SMD Discrete (`0805` / `1206`) | SMD (touch-up/rework only) | `2 pads` | Touch-up only |
| Small SMD (`0201` / `0402` / `0603`) / BGA / QFN | SMD | Multi-pin / fine-pitch | **No (SMT only)** |

### Required Equipment for Manual Soldering Stations
Include in `Equipment & Aux` when manual soldering stations exist:
- Temperature-controlled ESD soldering station + tips (`1` per station/operator)
- Fume extractor (`1` per station or shared dual-arm)
- Soldering fixture / PCB holder (`Custom = Y` if product-specific)
- Consumables: Lead-free solder wire (`SAC305`), flux pen/dispenser, tip cleaner/brass sponge.
