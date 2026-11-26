# SN-PE-Skill

**Industrial Engineering (IE) & Production Engineering (PE) Skill for FATP Draft RFQ / Quotation Generation**

`SN-PE-Skill` is used to **analyze and calculate UPH, DL/IDL, CT (Cycle Time), Investment (Equipment, Jigs & Fixtures, IT/MES, Facility Support), Capacity, and Line Balancing** from customer RFQ packages (SOP, BOM, Drawings, Testing Plans, Forecasts, SKU Lists). It automatically generates a standardized **FATP Production Requirement Summary (`.xlsx`)** in **Standard English**, complete with:
- **UPH & Capacity Sizing**: Line productivity (92%–95%), 1-line / 1-shift priority (22 working days, 7.16 effective hours/shift), or low-volume run-day batching
- **CT & Line Balancing**: Station-by-station cycle time (CT), standard time (+10% allowance), Takt Time, LOB, UPPH, FPY, plus mandatory **MKG & Postel label** processes for Indonesia launches
- **DL / IDL Headcount**: Direct Labor (DL) per section and Indirect Labor (IDL ≈ 30% of DL)
- **Investment & Equipment Sizing**: ST/CT-driven production/test equipment, custom jigs/fixtures, customer consign vs. factory supply, plus mandatory **IQC, Store, Repair, IT/MES, and General** baseline equipment
- **Missing-Data Audit**: Automatic **Customer Clarification List** for any missing or ambiguous customer inputs

---

## Repository Structure

```text
SN-PE-Skill/
├── SKILL.md                                                 # Core skill instructions (Agent Skills standard)
├── README.md                                                # Setup & usage guide
├── references/
│   ├── workbook-schema-and-formulas.md                      # Exact 5-sheet Excel schema & dynamic formulas
│   ├── ie-pe-calculation-rules.md                           # FATP IE/PE standards (22d, 7.16h, 10% allow., IDL/DL)
│   └── clarification-checklist.md                           # Missing-info audit & customer clarification template
└── assets/
    └── FATP_Production-Requirement-Summary_Template.xlsx    # Clean, formula-ready Excel template
```

---

## How to Use Across Platforms

### 1. Claude (Claude.ai & Claude Code)
- **Claude.ai (Web / Desktop)**:
  1. Zip the repository folder (`SKILL.md`, `references/`, `assets/`).
  2. Go to **Settings → Capabilities → Skills** (or **Customize → Skills**) and upload the `.zip` file.
  3. Start a chat, attach your customer's SOP/BOM/Drawings/Testing Plan, and ask:
     > *"Use SN-PE-Skill to analyze these customer documents and generate a Draft FATP Production Requirement Summary."*
- **Claude Code (CLI / IDE)**:
  ```bash
  git clone https://github.com/agustok/SN-PE-Skill.git ~/.claude/skills/sn-pe-skill
  ```

### 2. Antigravity
- Clone or add this repository into your workspace or global skills directory, or load `SKILL.md` directly via the Skills manager.
- Attach customer documents in chat and ask Antigravity to generate the FATP Production Requirement Summary.

### 3. ChatGPT (Custom GPT / ChatGPT Projects)
1. Create a new **Custom GPT** or **ChatGPT Project**.
2. Paste the contents of `SKILL.md` into **Instructions**.
3. Upload all files from `references/` and `assets/FATP_Production-Requirement-Summary_Template.xlsx` into **Knowledge / Project Files**.
4. Enable **Code Interpreter / Data Analysis** so ChatGPT can read customer Excels/PDFs and output the `.xlsx` workbook.
5. Upload your customer documents in a chat and prompt it to generate the draft RFQ workbook and clarification list.