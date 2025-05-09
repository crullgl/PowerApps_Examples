# 🪑 Desk Reservation Power App Documentation

![Reservation UI](images/Screenshot%202025-05-09Screenshot%20075641.png).
![Reservation UI](images/Screenshot%202025-05-08Screenshot%200154346.png)
![Recurring Settings](images/Screenshot%202025-05-08%20153342.png)
![Booking Overview](images/Screenshot%202025-05-08%20114656.png)
![Time Slot Selection](images/Screenshot%202025-05-08%20095423.png)
![Calendar View](images/Screenshot%202025-05-02%20125620.png)
![Desk Info Screen](images/Screenshot%202025-05-02%20125321.png)

This Power Platform app allows users to reserve desks and rooms, including support for **recurring reservations**. Below is an overview of the app’s data structure, recurrence logic, and key user interactions.

---

## 📋 SharePoint Lists

### **Desks List**
| Column Title     | Column Type          | Description |
|------------------|----------------------|-------------|
| Title            | Single line of text  | Desk identifier or label. |
| Description      | Multiple lines of text | Desk details or features. |
| Map Link         | Hyperlink or Picture | Link/image of desk map. |
| Active           | Number               | 1 = active, 0 = inactive. |
| RoomLocation     | Single line of text  | Room where desk is located. |
| Asset            | Choice               | e.g., Sit/Stand Desk. |
| DeskLocation     | Hyperlink or Picture | Desk position visual/link. |
| Capacity         | Single line of text  | Desk capacity. |
| RoomPicture      | Hyperlink or Picture | Room photo. |
| Division         | Choice               | Department or unit. |
| POC              | Person or Group      | Primary desk contact. |
| VTel             | Hyperlink or Picture | V-Tel setup image or link. |
| Projector/TV     | Hyperlink or Picture | Room A/V setup link. |
| SmartBoard       | Hyperlink or Picture | SmartBoard availability. |
| SecondPOC        | Person or Group      | Backup contact. |

---

### **DeskAccessControl List**
| Column Title | Column Type     | Description |
|--------------|------------------|-------------|
| Admin        | Person or Group | People allowed to manage desk settings and reservations. |

---

### **DeskReservations List**
| Column Title             | Column Type         | Description |
|--------------------------|---------------------|-------------|
| Title                    | Single line of text | Reservation title. |
| DeskText                 | Single line of text | Desk label. |
| Reserved By              | Person or Group     | Who made the reservation. |
| Check Out From           | Date and Time       | Start of reservation. |
| Check Out To             | Date and Time       | End of reservation. |
| Check Out From Text      | Single line of text | Formatted start date. |
| Check Out To Text        | Single line of text | Formatted end date. |
| Check Out From Number    | Number              | Start timestamp. |
| Check Out To Number      | Number              | End timestamp. |
| Manager                  | Person or Group     | Reservation approver. |
| Service                  | Single line of text | Requesting department. |
| Description              | Single line of text | Purpose of reservation. |
| Reason for desk reservation | Single line of text | Justification. |
| RoomReservation          | Yes/No              | Is a room also reserved? |
| Asset                    | Choice              | Type of item reserved. |
| DeskFloor                | Single line of text | Floor of reserved desk. |
| IsRecurring              | Yes/No              | Repeats over time? |
| RecurrenceFrequency      | Choice              | How often it recurs. |
| RecurrenceEndDate        | Date and Time       | When recurrence stops. |
| RecurrencePattern        | Choice              | Pattern type (Daily, Weekly, etc.). |
| RecurrenceID             | Number              | Unique series ID. |
| ParentReservationID      | Lookup              | Link to main reservation. |
| Meeting Category         | Choice              | Purpose (Training, Work, etc.). |

---

## 🔁 Recurring Reservation Logic

1. **Enable Recurring Toggle**  
   User selects frequency and number of repeats.

2. **Loop Using `Sequence()`**  
   Generates `[0, 1, 2, ...]` based on how many times the reservation should repeat.

3. **Calculate Dates**  
   - `DateAdd()` and `Switch()` handle different patterns (Daily, Weekly, Bi-Weekly, Monthly).
   - Monthly recurrence uses logic to hit the Nth weekday of the month.

4. **Create Collection**  
   `CollectionRecurrence` stores all StartDateTime/EndDateTime combinations.

5. **Conflict Check**  
   Filters `'Desk Reservations'` to catch any overlap with new entries using `LookUp()`.

6. **Group Records**  
   All entries share a `RecurrenceID`. The first one can be used as the `ParentReservationID` to group or delete later.

---

## 🧮 “Continue” Button Logic

When the user clicks **Continue** after selecting date and time:

- Sets variables: `selectedDate`, `selectedEndDate`, `startTime`, and `endTime`.
- Adjusts `endTime` if the end date is earlier than the start.
- Filters existing reservations that overlap the new selection into `colBookedDesks`.
- If any are rooms, it expands to desks linked to that room.
- Navigates to `DeskSelect` screen.
- Builds a filterable collection `colRoomLocations` of all unique rooms with an "All Conference Rms" option.



