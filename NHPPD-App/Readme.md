
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

**Hours Per Patient Day (HPPD)** is a key healthcare metric that tracks the number of direct care hours provided to each patient in a 24-hour period.

**Formula:**
```
(RN + NA + LPN) * 8 / Census
```

---

## 📂 SharePoint Lists and Column Details

### STAFFING METHODOLOGY
Tracks daily staffing input.

| Column Name         | Type             | Purpose |
|---------------------|------------------|---------|
| RN's, NA, LPN       | Number           | Staff counts |
| Census              | Number           | Patient count |
| Date, Unit, Shift   | Date/Choice      | Staffing breakdown |
| CalculatedHPPD02    | Calculated       | Main HPPD value |
| Goal2, 10above, 10below | Text         | Goal thresholds |
| Sitter Staff, Charge RN | Number/Choice | Additional roles |
| Comment, Alert      | Choice/Metadata  | Context info |

### STAFFING CONTROL #
Stores HPPD goals per unit/shift.

| Ward, Shift         | Choice           | Used to filter views |
| HPPD_Goal           | Text             | Target value |
| Hrs, Comment        | Number/Text      | Supporting data |

### NODACTUAL24HR
Used in the T-24 review.

| Date24Hr            | Date             | Targeted review day |
| HPPD Budget/High/Low| Text             | Target range |
| Actual_HPPD         | Text             | Reported value |
| Actions_Taken       | Text             | Follow-up notes |

### HPPD 24hr Budget
Comparison goals for reporting.

| HPPD_Budget         | Text             | Goal values |
| UnitName, Division  | Choice           | For filtering logic |
| Active_Disabled     | Choice           | Flag for enabling view |

### HPPDAppSecuityGroup
Admin control access.

| UserEmail           | Person or Group  | Matches current user |
| RoleName            | Choice           | Defines access level |

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
