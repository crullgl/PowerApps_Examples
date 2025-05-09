# 🧪 Lab Accession Tracker App
<p align="center">
  <img src="./Images/Screenshot%202025-05-09%20100136.png" alt="Accession Tracker Form" width="300" />
  <img src="./Images/Screenshot%202025-05-09%20100156.png" alt="Filtered Results" width="300" />
</p>

# Lab Accession Tracker App

A Power Apps application that allows lab personnel to efficiently search, filter, and review high-volume accession data from a SharePoint list. Designed with delegation compliance in mind, it handles over 170,000 records with optimized filters.

---

## 📖 What This App Does

- Supports **real-time filtering** by Accession Number, Location, and Date range.
- Handles **tens of thousands of records** without delegation warnings.
- Uses delegation-compliant Power Apps formulas for optimal SharePoint integration.
- Provides a clear gallery interface for lab users to review, sort, and filter results quickly.

---

## 📂 SharePoint List: `Accession Log`

| Column Name     | Type                | Purpose |
|------------------|---------------------|---------|
| **Title**         | Single line of text | Optional label or generic title for each entry |
| **AccessionNumber** | Single line of text | Unique identifier for the lab sample (used in filtering) |
| **CollectorDUZ**  | Single line of text | Staff ID of the sample collector |
| **Location**      | Choice              | Facility or department submitting the sample |
| **Comments**      | Single line of text | Additional context or notes related to the accession |

> **Note:** Be sure to index `AccessionNumber` and `Created` fields in SharePoint to maintain performance with large datasets.

---

## 🧮 Power Apps Gallery Filtering Formula (Fx)

This is the main code used to populate the gallery with delegation-compliant filtering:

```powerapps
Sort(
    Filter(
        'Accession Log', 
        StartsWith(AccessionNumber, AccessenNumbertxt.Text) &&
        (
            IsBlank(ComboBox1.Selected.Value) || 
            Location.Value = ComboBox1.Selected.Value 
        ) &&
        (
            IsBlank(FromDatedd.SelectedDate) || 
            IsBlank(Todatepickerdd.SelectedDate) || 
            (Created >= FromDatedd.SelectedDate && Created <= DateValue(Todatepickerdd.SelectedDate + 1))
        )
    ),
    Created, 
    SortOrder.Descending
)

