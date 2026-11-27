# Missing Data Audit & Customer Clarification Checklist

Whenever customer documents are incomplete, ambiguous, or inconsistent, **do not silently assume** final commercial commitments. Mark affected cells as `TBD` / `TBC`, document any temporary FATP engineering assumption in the `Remark` column, and output a structured **Customer Clarification List** in Standard English.

## Audit Checklist by Domain

### 1. Demand, Forecast & SKU Mix
- [ ] Monthly, annual, and total lifecycle volume (pcs)
- [ ] Ramp-up schedule / peak monthly demand vs. average monthly demand
- [ ] Product lifecycle duration (years)
- [ ] Complete FG SKU list (part numbers, descriptions, memory/config variants, colours, regions/channels, and % mix)

### 2. SOP & Assembly Process
- [ ] Complete assembly SOP with station sequence and step-by-step work instructions
- [ ] Station cycle times (`Avg time (s)`) and whether allowance is already included
- [ ] Packing SOP (giftbox, accessories, labels, IMEI/SN scan, carton spec, palletization)
- [ ] Indonesia regulatory items (if Indonesia launch): **MKG** (Manual & Warranty Card) and **Postel / SDPPI label** (mandatory for smartphones/wireless devices) artwork, spec, and placement
- [ ] Soldering scope boundary: exclude any **SMT, wave soldering, or dipping** components/processes (FATP has no SMT/wave/dipping machines); confirm PCBA arrives pre-assembled & wave-soldered upstream and list only **manual soldering** items (wire leads, manual THT/connectors) for FATP
- [ ] Key / CTQ stations (`★`) and special environmental requirements (ESD, cleanroom class, temperature/humidity, torque specs, glue curing time)

### 3. Testing Plan & Quality (QA)
- [ ] Complete test flow (MMI/function, RF coupling, audio/IPX7 waterproof, aging/burn-in, FQC)
- [ ] Test cycle time per unit and parallel test capability (slots per tester)
- [ ] Target FPY per station and overall line FPY target
- [ ] Golden sample quantity and calibration/maintenance responsibility

### 4. Equipment, Jigs, Fixtures & Consignment
- [ ] Customer-consigned equipment list vs. factory-supplied equipment
- [ ] 2D/3D drawings for custom jigs & fixtures (or confirmation if customer supplies fixtures)
- [ ] Proprietary test software, MES integration requirements, and label format templates

### 5. BOM & Materials
- [ ] Complete multi-level BOM with material codes, usage qty, unit, MPQ, MOQ, and lead time
- [ ] Consumables & process materials specification (thermal gel, glue type, tape, solder, etc.)
- *(Note: Commercial business model — Consign / Buy & Sell / Turnkey — is handled by the Business Side, not calculated by IE/PE.)*

---

## Standard Output Format for Customer Clarification List

Present the clarification list in chat at the end of your response using this structure:

```markdown
## Customer Clarification List — <ProjectCode>

| # | Category | Item / Gap Identified | Impact on RFQ / Sizing | Question for Customer |
|---|---|---|---|---|
| 1 | Demand | Annual & lifecycle volume not provided | Affects line amortization & NRE | Please confirm annual forecast, total lifecycle volume, and ramp-up plan. |
| 2 | Packing SOP | Packing stations omitted from SOP | Estimated 8 stations / 15 DL (FATP basis) | Please provide Packing SOP, giftbox/carton BOM, and label/MES scan requirements. |
```
