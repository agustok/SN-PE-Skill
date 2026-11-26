# SN-PE-Skill

**Industrial Engineering (IE) & Production Engineering (PE) Skill for FATP Draft RFQ / Quotation Generation**

`SN-PE-Skill` enables AI assistants (**Claude**, **ChatGPT**, and **Antigravity**) to ingest customer RFQ packages (SOP, BOM, Drawings, Testing Plans, Forecasts, SKU Lists) and automatically generate a standardized **FATP Production Requirement Summary (`.xlsx`)** in **Standard English**, complete with:
- Capacity & line sizing (or low-volume run-day batching)
- Station-by-station line balancing, Takt Time, UPH, UPPH, LOB, and FPY
- Direct Labor (DL) & 9-role Indirect Labor (IDL) headcount sizing
- Equipment, custom jigs/fixtures, and customer consignment vs. factory supply breakdown
- Missing-data audit & ready-to-send **Customer Clarification List**

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