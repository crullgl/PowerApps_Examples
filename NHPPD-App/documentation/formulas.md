🔹 Theme Colors for Status Bars
powerapps
Copy
Edit
Set(varColorRed, ColorValue("Red"));
Set(varColorYellow, ColorValue("Yellow"));
Set(varColorGreen, ColorValue("Green"));
Set(varColorNone, RGBA(0,0,0,0));
Controls red/yellow/green formatting for HPPD comparisons.

🔹 Load All Staffing Data
powerapps
Copy
Edit
Clear(col_StaffingHPPD); 
ClearCollect(
    col_StaffingHPPD,
    Sort('STAFFING METHODOLOGY', ID, SortOrder.Descending)
);
Loads full SharePoint list into a local Power Apps collection.

🔹 Filter Records Based on UI Inputs
powerapps
Copy
Edit
ClearCollect(
    col_StaffingHPPDFilter,
    Sort(
        Filter(
            col_StaffingHPPD,
            Date >= DatePicker1.SelectedDate &&
            Date <= DatePicker1_1.SelectedDate &&
            (IsBlank(ShiftSelectiondd.SelectedText) || ShiftSelectiondd.Selected.Value in colUnitNamesWithBlank || Shifts.Value = ShiftSelectiondd.Selected.Value) &&
            (UnitSelectiondd.Selected.Value in colUnitNamesWithBlank || Unit.Value = UnitSelectiondd.Selected.Value)
        ),
        Date,
        SortOrder.Descending
    )
);
Applies shift/unit/date filtering for user-selected views.

🔐 Role-Based Access Control
powerapps
Copy
Edit
Set(
    currentUserInGroup,
    !IsBlank(
        LookUp(
            HPPDAppSecuityGroup,
            UserEmail.Email = User().Email && RoleName.Value = "Admin"
        )
    )
);
ButtonCanvas3.Visible = currentUserInGroup;
Only shows admin buttons if user is listed in the SharePoint role list.

📊 Power Apps Collections
Collection Name	Description
col_StaffingHPPD	Raw SharePoint data
col_StaffingHPPDFilter	Filtered data for gallery
colHPPDGoal	Pulls shift-specific goals
colNOD24hr	24-hour data for review view
colUnitNamesWithBlank	Dropdown helper for "All"
colHPPD24Items/2	T-24 hour entry screens
colNewItems, colSelItems, colDelRecs	Used during patch/submit logic
