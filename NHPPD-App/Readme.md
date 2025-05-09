<p align="center">
  <img src="screenshots/Screenshot_090750.png" width="200" />
  <img src="screenshots/Screenshot_090808.png" width="200" />
  <img src="screenshots/Screenshot_090827.png" width="200" />
  <img src="screenshots/Screenshot_090845.png" width="200" />
</p>

# HPPD Staffing Methodology App

A Power Apps solution to track, calculate, and manage HPPD (Hours Per Patient Day) data across units and shifts. This tool improves staffing visibility, accountability, and operational decision-making for nurse managers, analysts, and leadership.

---

## 📖 What is HPPD?

**Hours Per Patient Day (HPPD)** is a key healthcare metric that tracks the number of direct care hours provided to each patient in a 24-hour period. It's used for:
- Budgeting and staffing accuracy
- Evaluating care efficiency
- Supporting compliance and reporting

**Formula:**


This app visually flags actual vs. goal alignment using green/yellow/red indicators and provides user-friendly data entry and review screens.

---

## 📂 SharePoint Lists and Column Details

### 1. `STAFFING METHODOLOGY` — Daily Staffing Records

| Column Name         | Type             | Purpose |
|---------------------|------------------|---------|
| Title               | Text             | Generic label |
| Created             | Date & Time      | Auto timestamp |
| Unit                | Choice           | Hospital unit |
| Date                | Date & Time      | Staffing day |
| RN's, NA, LPN       | Number           | Staff counts |
| CENSUS              | Number           | Patient count |
| Shifts              | Choice           | Day/Evening/Night |
| Sitter Staff        | Number           | Additional support staff |
| Charge RN, Scheduled NA/RN | Choice/Number | Staffing plan |
| CalculatedHPPD02    | Calculated       | Main HPPD formula |
| 10above / 10below   | Text             | Goal variance |
| Goal2               | Text             | Unit target |
| Comment, Alert      | Choice/Metadata  | Notes/tags |

---

### 2. `STAFFING CONTROL #` — HPPD Targets

| Column Name   | Type     | Purpose |
|---------------|----------|---------|
| Ward          | Choice   | Unit reference |
| HPPD_Goal     | Text     | Goal value |
| Shift         | Choice   | Shift type |
| Hrs           | Number   | Shift length |
| Comment       | Text     | Notes or updates |

---

### 3. `NODACTUAL24HR` — 24-Hour Actuals

| Column Name     | Type     | Purpose |
|------------------|----------|---------|
| Ward             | Text     | Unit name |
| Date24Hr         | Date     | Snapshot date |
| HPPD Budget/High/Low | Text | Goal thresholds |
| Actual_HPPD      | Text     | Actual value logged |
| Actions_Taken    | Text     | Response actions |

---

### 4. `HPPD 24hr Budget` — Comparison Dataset

| Column Name     | Type     | Purpose |
|------------------|----------|---------|
| Ward             | Choice   | Unit reference |
| HPPD_Budget      | Text     | Goal |
| UnitName         | Choice   | Friendly name |
| Division         | Choice   | Used for UD/DD tabs |
| Active_Disabled  | Choice   | Display logic toggle |

---

### 5. `HPPDAppSecuityGroup` — Admin Access

| Column Name   | Type           | Purpose |
|----------------|----------------|---------|
| UserEmail      | Person or Group| Email match for current user |
| RoleName       | Choice         | Currently: "Admin" |

---

## 🔧 Power Apps Logic (`Fx`) and Purpose

### 🔹 Dropdown Filters Setup

```powerapps
ClearCollect(colUnitNamesWithBlank, {{Value: Blank()}});
Collect(colUnitNamesWithBlank, Choices([@'STAFFING METHODOLOGY'].Unit));

