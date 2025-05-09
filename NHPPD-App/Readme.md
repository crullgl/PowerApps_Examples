
<p align="center">
  <img src="images/Screenshot_090750.png" width="200" />
  <img src="images/Screenshot_090808.png" width="200" />
  <img src="images/Screenshot_090827.png" width="200" />
  <img src="images/Screenshot_090845.png" width="200" />
</p>

# HPPD Staffing Methodology App

A Power Apps solution to track, calculate, and manage HPPD (Hours Per Patient Day) data across units and shifts. This tool improves staffing visibility, accountability, and operational decision-making for nurse managers, analysts, and leadership.

---

## 📖 What is HPPD?

**Hours Per Patient Day (HPPD)** is a key healthcare metric that tracks the number of direct care hours provided to each patient in a 24-hour period.

**Formula:**
```
(RN + NA + LPN) * 8 / Census
```

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
---

## 🔧 Power Apps Logic (Fx) in YAML

```yaml
# HPPD Power Apps Fx Code Examples

dropdown_filters:
  description: >
    Adds a blank option to the Unit dropdown for 'All' filtering support.
  fx: |
    ClearCollect(colUnitNamesWithBlank, {Value: Blank()});
    Collect(colUnitNamesWithBlank, Choices([@'STAFFING METHODOLOGY'].Unit));

color_variables:
  description: >
    Defines color codes for visual indicators (green = on target, yellow = 10% over, red = 10% under).
  fx: |
    Set(varColorRed, ColorValue("Red"));
    Set(varColorYellow, ColorValue("Yellow"));
    Set(varColorGreen, ColorValue("Green"));
    Set(varColorNone, RGBA(0,0,0,0));

load_staffing_data:
  description: >
    Loads the full 'STAFFING METHODOLOGY' SharePoint list into a collection sorted by newest entries first.
  fx: |
    Clear(col_StaffingHPPD);
    ClearCollect(
      col_StaffingHPPD,
      Sort('STAFFING METHODOLOGY', ID, SortOrder.Descending)
    );

filter_staffing_records:
  description: >
    Filters staffing records by selected date, shift, and unit. Supports blank filtering for 'All'.
  fx: |
    ClearCollect(
      col_StaffingHPPDFilter,
      Sort(
        Filter(
          col_StaffingHPPD,
          Date >= DatePicker1.SelectedDate &&
          Date <= DatePicker1_1.SelectedDate &&
          (
            IsBlank(ShiftSelectiondd.SelectedText) ||
            ShiftSelectiondd.Selected.Value in colUnitNamesWithBlank ||
            Shifts.Value = ShiftSelectiondd.Selected.Value
          ) &&
          (
            UnitSelectiondd.Selected.Value in colUnitNamesWithBlank ||
            Unit.Value = UnitSelectiondd.Selected.Value
          )
        ),
        Date,
        SortOrder.Descending
      )
    );

role_based_visibility:
  description: >
    Controls visibility of admin-only buttons by checking the user’s email against the HPPDAppSecuityGroup list.
  fx: |
    Set(
      currentUserInGroup,
      !IsBlank(
        LookUp(
          HPPDAppSecuityGroup,
          UserEmail.Email = User().Email &&
          RoleName.Value = "Admin"
        )
      )
    );
    ButtonCanvas3.Visible = currentUserInGroup;
    ButtonCanvas3_1.Visible = currentUserInGroup;
```

---

## 📊 Power Apps Collections

| Collection              | Description |
|--------------------------|-------------|
| `col_StaffingHPPD`       | Raw SharePoint list |
| `col_StaffingHPPDFilter` | Filtered records |
| `colHPPDGoal`            | Goal pull for gallery |
| `colUnitNamesWithBlank`  | Enables 'All' dropdown |
| `colNOD24hr`             | 24-hour reporting |
| `colHPPD24Items`, `colHPPD24Items2` | Input result screens |
| `colNewItems`, `colSelItems`, `colDelRecs` | Support form save logic |

---


## 🙋‍♂️ Maintainer

**Gary Crull**  
Program Analyst | Citizen Developer | VHA  
*Specializing in Power Platform and healthcare workflow optimization.*
