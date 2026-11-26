# Workbook Schema & Excel Formulas

Output filename convention:
`<ProjectCode>_FATP_Production-Requirement-Summary_v1_<YYYYMMDD>.xlsx`

Required sheets (in order):
1. `Summary`
2. `Process & Manpower`
3. `Equipment & Aux`
4. `SKU List`
5. `BOM` *(include whenever customer provides a BOM; omit or leave blank with `TBD` note if BOM is not provided)*

---

## 1. `Summary` Sheet

### Title Block (Rows 1–2)
- **Row 1**: `<ProjectCode> (<Product/Customer>) – FATP Production Requirement Summary`
- **Row 2**: Subtitle summarizing scope, SOP version, and demand basis (e.g., `SKD smartphone · SOP <ID> v1.0 · <notes>`).

### Demand & Forecast Table (Rows 4–6)
Columns: `Market` | `Monthly` | `Annual` | `Lifecycle` | `Years` | `Annual ÷ 12` | `Remark`
- `Annual ÷ 12` (Col F): `=IF(ISNUMBER(C5),C5/12,"-")`
- `Total` (Row 6): `=SUM(B5:B5)`, `=SUM(C5:C5)`, `=SUM(D5:D5)`, `=C6/12`

### SKU Summary (Rows 8–9)
- `Total SKUs`: `=COUNTA('SKU List'!A5:A<last>)` (or `=COUNTIF('SKU List'!A3:A<last>,"<Prefix>*")`)
- `Colours`: `=COUNTA('SKU List'!F5:F<last>)` with colour list concatenated or noted in Col C.

### Capacity & Utilization (Mode A — Multi-Shift / Full-Line)
| Item | Formula / Value | Remark |
|---|---|---|
| Planning demand (units / month) | `=MAX(B6,F6)` | `MAX(monthly, annual ÷ 12)` |
| Working days / month | `22` | FATP standard |
| Effective hours / shift | `7.16` | FATP standard (430 min) |
| Line UPH (SOP, max) | `='Process & Manpower'!K<total_row>` | SOP pitch + 10% allowance |
| Line UPH (proposed balance, max) | `='Process & Manpower'!K<summary_total_row>` | Incl. packing (FATP est.) |
| Planned UPH (1 shift) | `=B16` (or target UPH) | Required UPH note via `TEXT()` |
| Max capacity / line / month (3 shifts) | `=B18*B15*B14*3` | `Planned UPH × h × days × 3` |
| Lines required | `=ROUNDUP(B13/B19,0)` | |
| Line utilization (3-shift basis) | `=B13/(B20*B19)` | Format as `0.0%` |
| Shifts / day required | `=ROUNDUP(B13/(B18*B15*B14*B20),0)` | |

### Capacity & Run Plan (Mode B — Low-Volume Run-Day Batching)
| Item | Formula / Value | Remark |
|---|---|---|
| Monthly demand (pcs) | `=MAX(B5,F5)` | |
| Effective hours / shift | `7.16` | |
| Line DL | `='Process & Manpower'!J<total_row>` | |
| Line UPH max | `='Process & Manpower'!K<summary_total_row>` | Bottleneck note |
| Target UPH | `<TargetUPH>` | Takt & bottleneck loading note |
| Output / shift (pcs) | `=B18*B15` | |
| Run days / month (1 shift) | `=ROUNDUP(B14/B19,0)` | |
| Output / run day (planned) | `=B14/B20` | |
| Line loading (run days ÷ 22) | `=B20/22` | Format as `0.0%` |
| Lines required | `1` | |

### Manpower (per line per shift)
Columns: `Item` | `SOP DL basis` | `Proposed DL` | `Remark`
- `DL / line / shift`: linked to `'Process & Manpower'` total DL
- Section breakdown rows: `Main assembly`, `Testing`, `Final assembly & QA`, `Packing` (via `SUMIF` summary block)
- `IDL / line / shift (FATP estimate)`: linked to `'Process & Manpower'` IDL total
- `IDL / DL ratio`: `=F18/F13` (and `=G18/G13`), target `≤ 30%`
- `Total headcount`: `=$B$20*$B$22*(F13+F18)` (Mode A) or `=B26+B27` (Mode B)
- `UPPH (DL)`: `=B16/F13` and `=B18/G13`

### Equipment & Fixtures Summary
- `Custom-made items`: `=COUNTIF('Equipment & Aux'!F5:F<last>,"Y")`
- `Propose customer consign (Y)`: `=COUNTIF('Equipment & Aux'!J5:J<last>,"Y")`
- `PTSN supply (N)`: `=COUNTIF('Equipment & Aux'!J5:J<last>,"N")`

---

## 2. `Process & Manpower` Sheet

### Table 1: Station-by-Station Breakdown (Row 4 Header)
Columns A–R:
- `A`: `Section` (`Main assembly`, `Testing`, `Final assembly & QA`, `Packing`, or custom sub-assy sections)
- `B`: `Station` (`P1..Pn`, `PK1..PKn`)
- `C`: `工序名称 (SOP)` *(original SOP name if non-English; otherwise omit or keep as `SOP Station Name`)*
- `D`: `Process (EN)`
- `E`: `Key station ★` (`★` if CTQ / key station)
- `F`: `Avg time (s)` (raw SOP cycle time)
- `G`: `Allow.` (default `0.1` = 10%)
- `H`: `Std time (s)` → `=F<r>*(1+G<r>)`
- `I`: `DL (SOP)`
- `J`: `NT (SOP)` → `=H<r>/I<r>`
- `K`: `UPH (SOP)` → `=3600/J<r>`
- `L`: `Op (proposed)` (grouped/merged operation ID, e.g. `P1`, `Op1`)
- `M`: `Proposed DL`
- `N`: `Merged C/T (s)` → `=SUMIF($L$5:$L$<last>,L<r>,$H$5:$H$<last>)`
- `O`: `Proposed NT (s)` → `=N<r>/M<r>`
- `P`: `FPY` (station first-pass yield, e.g., `0.998`, `0.995`, `0.990`)
- `Q`: `Equipment / fixtures (EN)`
- `R`: `设备 / 治具 (SOP)`

**Total Line Row**:
- `H`: `=SUM(H5:H<last>)`
- `I`: `=SUM(I5:I<last>)`
- `J`: `=MAX(J5:J<last>)`
- `K`: `=MIN(K5:K<last>)`
- `M`: `=SUM(M5:M<last>)`
- `N`: `=SUM(N5:N<last>)` *(or sum of unique Ops)*
- `O`: `=MAX(O5:O<last>)`
- `P`: `=EXP(SUMPRODUCT(LN(P5:P<last>)))`

### Table 2: Line Balance Summary (by Section)
Columns: `Section` | `DL (SOP)` | `C/T sum (s)` | `NT max (SOP)` | `UPH (SOP)` | `LOB (SOP)` | `UPPH (SOP)` | `FPY` | `Proposed DL` | `Prop. NT max` | `Prop. UPH` | `Prop. LOB` | `Prop. UPPH`
- `DL`: `=SUMIF($A$5:$A$<last>,A<r>,$I$5:$I$<last>)`
- `C/T sum`: `=SUMIF($A$5:$A$<last>,A<r>,$H$5:$H$<last>)`
- `NT max`: `=IF(B<r>=0,"-",MAXIFS($J$5:$J$<last>,$A$5:$A$<last>,A<r>))`
- `UPH`: `=IF(B<r>=0,"-",3600/D<r>)`
- `LOB`: `=IF(B<r>=0,"-",C<r>/(D<r>*B<r>))`
- `UPPH`: `=IF(B<r>=0,"-",E<r>/B<r>)`
- `FPY`: `=EXP(SUMPRODUCT(($A$5:$A$<last>=A<r>)*LN($P$5:$P$<last>)))`

### Table 3: Proposed Station Balance vs Takt
Columns: `Op` | `Stations` | `Proposed DL` | `Merged C/T (s)` | `NT (s)` | `Takt (s)` | `Op loading`
- `Takt (s)`: `=3600/Summary!$B$18`
- `Op loading`: `=E<r>/F<r>`

### Table 4: IDL per Line per Shift
Columns: `Role` | `FATP standard (SOP basis)` | `Proposed` | `Remark`
Standard 9 roles:
1. `Line leader` (1)
2. `Supporter` (1)
3. `IQC` (1)
4. `IPQC` (1)
5. `OQC` (1)
6. `Material handler` (2)
7. `Packing boy` (2)
8. `Repair technician` (1)
9. `TE / ME` (2)
- `Total IDL`: `=SUM(B<first>:B<last>)`

---

## 3. `Equipment & Aux` Sheet

Header (Row 4):
`No.` | `Category` | `Station(s)` | `Item name` | `Description (EN)` | `Custom (Y/N)` | `Qty / line` | `Lines` | `Total qty` | `Propose customer consign (Y/N)`

- `Lines` (Col H): `=Summary!$B$20` (Mode A) or `=Summary!$B$23` (Mode B)
- `Total qty` (Col I): `=IF(ISNUMBER(G<r>),G<r>*H<r>,G<r>)`

Standard Categories:
- `Production equipment`
- `Test equipment`
- `Jig & fixture (custom)`
- `Hand tools`
- `IT / MES`
- `Line infrastructure`
- `Consumables`
- `Process materials`
- `IQC area`
- `Repair area`
- `Store`

---

## 4. `SKU List` Sheet

Header (Row 4):
`Material Code` | `Description` | `BT / Product` | `Channel` | `Region` | `Colour` | `Mix % (input)` | `Monthly qty`
- `Monthly qty` (Col H): `=G<r>*Summary!$B$13`
- `Total` row: `=SUM(G5:G<last>)` and `=SUM(H5:H<last>)`

---

## 5. `BOM` Sheet (When Provided by Customer)

Header (Row 1):
`Item` | `Material Code` | `Description` | `Material Type` | `Supply Type` | `Usage Quantity` | `Unit` | `Inventory Category` | `Unit Price (Excl. Tax)` | `Total Price (Excl. Tax)` | `Minimum Packaging Quantity (MPQ)` | `Minimum Order Quantity (MOQ)` | `Procurement Lead Time`
