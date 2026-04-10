# Master Data Dictionary & Reference Guide

[cite_start]This serves as the authoritative technical reference for analytics engineering across Telephone, AI, and Order Management systems[cite: 2].

## 1. Global Standards
### Organizational Hierarchy Mapping
[cite_start]Reporting follows a dual-logic system where leadership and agent levels have distinct rules[cite: 4, 5].

| Leadership Level | Role | Description |
| :--- | :--- | :--- |
| Level 1 | Supervisor | [cite_start]Most specific management level[cite: 6]. |
| Level 2 | Manager | [cite_start]Mid-level operational management[cite: 6]. |
| Level 3 | Director | [cite_start]Broadest organizational oversight[cite: 6]. |

> [cite_start]**Note:** Agent level specificity **decreases** as the level number increases (Level 1 is an individual contributor)[cite: 7, 8].

---

## 2. Telephone Source of Truth: Avaya ECHI
[cite_start]Standardized to **Eastern Standard Time (EST/EDT)**[cite: 11].

### Performance Metric Formulas
* [cite_start]**AHT (Average Handle Time):** `ACD(tlk_tm) + Hold + ACW` [cite: 33]
* **Technical Note:** Prefix "H" denotes interval-level data; [cite_start]"D" denotes daily-level data[cite: 28].

| Field Name | Description |
| :--- | :--- |
| **ACW_TM** | [cite_start]After Call Work time (sum multiple fields)[cite: 27]. |
| **Ans_HLD_TM** | [cite_start]Total time call was held in segment[cite: 27]. |
| **ACD time** | [cite_start]Direct call time excluding queue, hold, and ACW[cite: 27]. |

---

## 3. Order Management System (OMS)
[cite_start]Primary identifier: `ORDER_HEADER_KEY`[cite: 19, 73].

### Field Exclusions
* **DRAFT_ORDER_FLAG:** If 'Y', these are test orders and **must be excluded** from production reporting[cite: 73].

### Key EXTN_ Columns (Line Level)
| Column Name | Functional Description |
| :--- | :--- |
| `EXTN_CANCEL_REASON_CODE` | Code identifying reason for line cancellation[cite: 76]. |
| `EXTN_ORDER_TYPE` | Fulfillment method (BOPIS, BODFS, STH)[cite: 76]. |
| `EXTN_SKU_CODE` | Specific SKU identifier[cite: 76]. |

---

## 4. Master SQL Library
```sql
-- Link header metadata with item-level details [cite: 142]
SELECT * FROM `pr-edw-views-thd.ORD_COM.COM_HDR` h
JOIN `pr-edw-views-thd.ORD_COM.COM_LINE` l 
  ON h.Order_Header_Key = l.Order_Header_Key
LIMIT 100;
