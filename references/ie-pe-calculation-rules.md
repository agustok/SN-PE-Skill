# IE & PE Engineering Calculation Rules (FATP Standard)

## 1. Calendar & Shift Standards
- **Working days / month**: `22 days` (FATP standard)
- **Effective hours / shift**: `7.16 hours` (`430 minutes` effective out of 480-minute 8-hour shift, accounting for breaks, 5S, and shift handover)
- **Shifts / day**: Up to `3 shifts` max capacity per line; round up required shifts via `ROUNDUP(Demand / (Planned_UPH * 7.16 * 22 * Lines), 0)`
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
- **Takt Time**:
  $$\text{Takt (s)} = \frac{3600}{\text{Planned UPH}}$$
- **Operation Loading**:
  $$\text{Op Loading} = \frac{\text{Proposed NT (s)}}{\text{Takt (s)}}$$
- **First Pass Yield (FPY)**:
  $$\text{Total FPY} = \prod_{i} \text{FPY}_i = \exp\left(\sum_i \ln(\text{FPY}_i)\right)$$

## 3. Packing Estimation Rule
Customer SOPs frequently cover assembly and testing (`P1..Pn`) but omit packing (`PK1..PKn`).
When packing is missing from the SOP:
1. Add standard FATP packing stations (`PK1..PKn`): device label/IMEI scan, accessory pre-kitting, unit bagging/case, giftbox loading, giftbox weighing & scan, giftbox label & seal, carton packing & label, carton sealing & palletizing/MES pack-out.
2. Mark `工序名称 (SOP)` as `(SOP 无)` / `(Not in SOP)` and clearly note in `Summary` that Packing DL is an **FATP estimate**.
3. Add the missing Packing SOP / packaging spec to the **Customer Clarification List**.

## 4. Indirect Labor (IDL) Standard & Ratio
- Standard 9-role FATP IDL baseline per line per shift (12 IDL total for full line; scale down proportionally for low-volume shared lines):
  - `Line leader` (1), `Supporter` (1), `IQC` (1), `IPQC` (1), `OQC` (1), `Material handler` (2), `Packing boy` (2), `Repair technician` (1), `TE / ME` (2).
- **Target IDL / DL Ratio**: `≤ 30%`. If `IDL / DL > 30%`, add a remark recommending multi-line sharing or combined roles (e.g., combined IQC/IPQC/OQC or shared TE/ME).

## 5. Equipment, Jigs & Consignment Rules
- **Custom (`Y/N`)**:
  - `Y`: Product-specific jigs, fixtures, press molds, RF shield-box inserts, test carriers, custom assembly cradles.
  - `N`: General-purpose equipment (electric screwdrivers, ionizing air guns, label printers, weighing scales, benches, ESD wrist straps, MES scanners/PCs).
- **Propose Customer Consign (`Y/N`)**:
  - `Y`: Dedicated test equipment (e.g., CMW500, audio analyzers, RF shield boxes, test PCs with proprietary SW), golden samples, and custom product-specific jigs/fixtures (`Custom = Y`).
  - `N`: Standard factory/PTSN-supplied production equipment, hand tools, IT/MES hardware, line infrastructure, and consumables.
