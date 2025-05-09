
# 🏥 Acuity Staffing Tool

A Power Apps application used to capture and track patient acuity by bed, calculate Full-Time Equivalents (FTEs), and inform staffing adjustments. The tool integrates form-based acuity scoring with backend SharePoint data for real-time monitoring and reporting.

---

## 📸 Screenshots

<p align="center">
  <img src="./Images/Screenshot%202025-05-09%20115102.png" width="250" alt="Home Screen" />
  <img src="./Images/Screenshot%202025-05-09%20115112.png" width="250" alt="Form View" />
  <img src="./Images/Screenshot%202025-05-09%20115202.png" width="250" alt="Patient Input" />
  <img src="./Images/Screenshot%202025-05-09%20115218.png" width="250" alt="Task Capture View" />
  <img src="./Images/Screenshot%202025-05-09%20115240.png" width="250" alt="Empty Start State" />
  <img src="./Images/Screenshot%202025-05-09%20115046.png" width="250" alt="Loading Screen" />
</p>

---

## 📂 SharePoint Lists

### AcuityStaffing

| Column Name       | Type              | Purpose |
|------------------|-------------------|---------|
| Title            | Text              | Entry identifier |
| Date             | Date & Time       | Submission date |
| Unit             | Choice            | Nursing unit |
| Bed              | Choice            | Bed number |
| Shift            | Choice            | Day/Evening/Night |
| Acuity           | Number            | Calculated score |
| Hrs              | Number            | Hours assigned based on acuity |
| FTE              | Number            | Full-Time Equivalent derived from hours |
| Discharged       | Choice            | Patient status |
| Patient Initials | Text              | For tracking without PHI |
| [Various Tasks]  | Multiple Choice/Text | Covers care tasks (Trach, CPAP, Diet, Wounds, ADLs, etc.) |

### TaskTimeTable

| Column Name | Type             | Purpose |
|-------------|------------------|---------|
| TaskType    | Text             | Category (e.g., Vascular Access, ADL) |
| Task        | Text             | Task name |
| TaskMinutes | Number           | Time value for acuity scoring |
| CodeStatus  | Text             | Related patient code (optional mapping) |

---

## 🧮 Gallery Filter Formula

```powerapps
Sort(
    Filter(
        AcuityStaffing, 
        (IsBlank(Shiftdd.Selected.Value) || Shiftdd.Selected.Value = ThisRecord.Shift.Value) &&
        (IsBlank(Dropdown1.Selected.Value) || Dropdown1.Selected.Value = ThisRecord.Unit.Value) &&
        (IsBlank(DataCardValue9_1.SelectedDate) || DataCardValue9_1.SelectedDate = ThisRecord.Date)
    ),
    Date,
    SortOrder.Descending
)
```

### 🧠 Explanation:
- Filters by selected **Shift**, **Unit**, and **Date**—but only if values are chosen.
- Each `IsBlank(...) || Match` pattern ensures optional filtering logic.
- Sorted by **most recent records first**.

---

## ➕ New Button `OnSelect`

```powerapps
Set(varCheckedboxes, false);
Set(varFormMode, FormMode.New);

// Reset all task-tracking variables
Set(varSelectedTaskTrach, "");
Set(varSelectedTaskCpAP, "");
Set(varSelectedTaskACCU, "");
Set(varSelectedTaskOstomyTube, "");
Set(varSelectedTaskCOde, "");
Set(varSelectedTaskBladder, "");
Set(varSelectedTaskBowel, "");
Set(varSelectedTaskMeta, "");
Set(varSelectedTaskShower, "");
Set(varSelectedTaskVascular, "");
Set(varSelectedTaskADL,"");
Set(varSelectedTaskFIM, "");
Set(varSelectedTaskISO, "");
Set(varSelectedTaskWound, "");
Set(varSelectedTaskLOF, "");
Set(varSelectedTaskWeight, "");
Set(varSelectedTaskDiet, "");
Set(varSelectedTaskOral, "");

Navigate('Form Screen');
```

**Purpose:**  
- Prepares a clean slate when adding a new record by resetting all inputs and task-tracking variables.
- Sets mode to **new entry** and takes user to the form.

---

## ✅ Submit Button Logic

```powerapps
SubmitForm(Form1);
ResetForm(Form1);

// Reset task variables after submission
Set(varSelectedTaskTrach, "");
Set(varSelectedTaskCpAP, "");
Set(varSelectedTaskACCU, "");
Set(varSelectedTaskOstomyTube, "");

// Return to gallery screen
Navigate(Gallerysc);
```

**Purpose:**  
- Submits the completed form, resets it for the next use, clears select variables, and navigates back to the list view.

---

## 👨‍💻 Developed by

**Gary Crull**  
Power Platform Developer, VHA  
_Specializing in healthcare workflow tools, SharePoint integration, and staffing optimization apps._

