# RLIMS - Attendance & Lesson Plan Management System

An intuitive, lightweight, single-file web application designed for **R L Institute of Management Studies (RLIMS)** for the **2026–27 Academic Year (ODD Semester)**. 

This platform enables faculty members and administrative staff to easily record daily student attendance, map lesson plan progress, export detailed reports into formatted Excel (`.xlsx`) files, and synchronize records directly with Google Drive.

---

## 🌟 Key Features

* **Daily Attendance Entry**:
  * Filter by Academic Year (**MBA I Year** or **MBA II Year**).
  * Auto-populated subjects based on selected year, core courses, and electives (*Digital Business, Business Analytics, General Management*).
  * Common events/activity tracking (*Guest Lectures, Outbound Learning, Seminars, Mentoring, etc.*).
  * Quick attendance toggles (**Present**, **Absent**, **OD** - On Duty) with bulk status marking buttons.

* **Embedded Lesson Plan Tracker**:
  * Track syllabus coverage directly during attendance entry.
  * Fields include: **Unit**, **Chapter No**, **Hour's Alloted**, and **Hour's Taken** (defaults to selected period).

* **Structured Excel (`.xlsx`) Export**:
  * Generates clean, well-formatted Excel files using **SheetJS**.
  * Contains institutional headers, session metadata, unit lesson plan details, and color-coded attendance rosters.
  * Standardized file naming convention: `YYYY-MM-DD_Subject_Year_Section_Hour.xlsx`.

* **Google Drive Cloud Sync**:
  * Direct upload of `.xlsx` files to a specified Google Drive folder via a Google Apps Script Web App URL endpoint.

* **Student Roster & Staff Database**:
  * Pre-loaded with MBA 1st and 2nd Year student rosters.
  * Bulk import capability for updating student and staff databases via `.csv` or `.xlsx` files.
  * Local storage persistence (`localStorage`) to maintain database state across browser restarts.

---

## 🛠️ Tech Stack & Dependencies

* **Frontend Framework**: HTML5, Modern JavaScript (ES6+)
* **Styling**: [Tailwind CSS](https://tailwindcss.com/) (via CDN)
* **Excel Operations**: [SheetJS / XLSX](https://sheetjs.com/) (via CDN)
* **Backend Integration**: Google Apps Script (REST Web App endpoint)

---

## 🚀 Getting Started

### 1. Running the Application
No build tools, Node.js, or server setup required! 

1. Download or clone `index.html`.
2. Double-click or open `index.html` in any web browser (*Chrome, Edge, Firefox, Safari*).

### 2. Marking Attendance & Lesson Plan
1. Navigate to the **Mark Attendance & Lesson Plan** tab.
2. Select the **Academic Year**, **Subject / Activity**, **Section / Specialization**, and **Hour / Period**.
3. Fill in the **Lesson Plan** details (**Unit**, **Chapter No**, **Hour's Alloted**, **Hour's Taken**).
4. Click **Load Student Roster**.
5. Adjust attendance radios for each student or use quick action buttons (*Mark All Present / Absent / OD*).
6. Click **Save Attendance & Lesson Plan as Excel (.xlsx)** to download locally or **Direct Upload to Google Drive**.

---

## ☁️ Google Apps Script Integration Setup

To enable direct Google Drive cloud exports:

1. Open Google Drive and create a target folder (e.g., `RLIMS_Attendance_2026_27`).
2. Create a new **Google Apps Script** project.
3. Paste the following Apps Script code:

```javascript
function doPost(e) {
  try {
    var data = JSON.parse(e.postData.contents);
    var folderId = "YOUR_GOOGLE_DRIVE_FOLDER_ID"; // Replace with your Drive Folder ID
    var folder = DriveApp.getFolderById(folderId);
    
    var decoded = Utilities.base64Decode(data.fileData);
    var blob = Utilities.newBlob(decoded, data.mimeType, data.fileName);
    var file = folder.createFile(blob);
    
    return ContentService.createTextOutput(JSON.stringify({ status: "success", fileId: file.getId() }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ status: "error", message: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

4. Deploy as a **Web App**:
   * Execute as: **Me**
   * Who has access: **Anyone**
5. Copy the generated Web App URL and paste it into the **Google Drive Sync** tab inside the RLIMS web interface.

---

## 📄 Excel Export Format Structure

Each exported Excel file follows this structured layout:

| Row Index | Section | Content |
| :--- | :--- | :--- |
| **Row 1** | Header Title | R L Institute of Management studies |
| **Row 2** | File Name | `YYYY-MM-DD_Subject_Year_Section_Hour` |
| **Row 3** | Metadata | Date, Hour, and Faculty / Coordinator Name |
| **Row 5-7** | **Lesson Plan** | Table containing **Unit**, **Chapter No**, **Hour's Alloted**, **Hour's Taken** |
| **Row 9+** | **Roster** | **s.no**, **roll no**, **name**, **attendance status** |

---

## 📌 File Structure

```text
.
├── index.html       # Combined Single-File Web Application (HTML/CSS/JS)
└── README.md        # Documentation and Setup Guide
```