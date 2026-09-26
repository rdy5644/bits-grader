# BITS Pilani Digital — Advanced Grading Console

A browser-based grading console for reviewing student marks, configuring grade ranges, inspecting grade distributions, and exporting finalized grades.

> This is a prototype application for the CodeForge challenge. It is not an official BITS Pilani academic grading system.

## Live Application and Repository

- **Live application:** [https://rdy5644.github.io/bits-grader/](https://rdy5644.github.io/bits-grader/)
- **GitHub repository:** [https://github.com/rdy5644/bits-grader](https://github.com/rdy5644/bits-grader)
- **README source:** [https://github.com/rdy5644/bits-grader/blob/main/README.md](https://github.com/rdy5644/bits-grader/blob/main/README.md)

The application is hosted as a static site using GitHub Pages. Open the live application link to use the grader without downloading the source code.

## Capabilities

- Upload Excel workbooks in `.xls` or `.xlsx` format.
- Validate the workbook structure before importing data.
- Support multiple courses in one workbook.
- Automatically select the first available course after upload.
- Display:
  - Minimum mark
  - Maximum mark
  - Average
  - Median
  - Marks distribution histogram
  - Grade distribution summary
  - Student count
  - Pass rate
  - Highest grade
  - Ungraded student count
- Configure grade bands from A through E.
- Automatically maintain continuous, non-overlapping grade ranges.
- Search student records by BITS ID or marks.
- Review calculated grades before finalization.
- Download invalid-row reports with error descriptions.
- Download finalized grades as a CSV file.
- Download a clean sample workbook template.
- Reset the current workbook and grading session.
- Use keyboard-accessible controls, labels, tooltips, focus states, and live validation messages.
- Work responsively on smaller screens.

## Getting Started

No build step is required.

1. Open `index.html` in a modern browser.
2. Enter the instructor name.
3. Upload a valid `.xls` or `.xlsx` workbook.
4. Select a course.
5. Review the analytics and student-level grading table.
6. Confirm or adjust the grade ranges.
7. Review the final distribution.
8. Select **Finalize & Download** and confirm the review summary.

The application uses the SheetJS library from the jsDelivr CDN. An internet connection is required when opening the page unless the dependency is bundled locally.

## Required Workbook Format

The first worksheet must contain exactly these three columns:

```text
Student’s BITS ID | Course | Total Marks (out of 100)
```

Example:

| Student’s BITS ID | Course | Total Marks (out of 100) |
|---|---|---:|
| 2024XXXX | Course A | 82 |
| 2024YYYY | Course A | 71 |
| 2024ZZZZ | Course A | 64 |

Use **Download sample format** in the application to generate a valid workbook template.

### Input rules

- Marks must be numeric and between `0` and `100`, inclusive.
- Blank marks are invalid.
- Negative marks are invalid.
- Marks above `100` are invalid.
- Blank student IDs are invalid.
- Blank course names are invalid.
- Students who did not appear for the examination and should receive an NC grade should not be included.
- Fractional marks are accepted as numeric values, although whole-number marks are recommended according to the input guidance.

Legacy headers such as `BITS ID` and `Total Marks` are not accepted. The required production headers must be used exactly.

## Validation and Error Handling

The application validates:

- Missing required columns.
- Unexpected or extra columns.
- Incorrect column names.
- Blank student IDs.
- Blank courses.
- Blank marks.
- Non-numeric marks.
- Marks outside the `0–100` range.

If the workbook structure is invalid, the file is rejected and the current valid grading data is preserved.

If individual rows are invalid, valid rows remain available for grading while invalid rows are excluded. The application shows the number of excluded records and provides **Download invalid-row report**.

The invalid-row workbook contains:

```text
Source row
Student’s BITS ID
Course
Total Marks (out of 100)
Error description
```

## Default Grade Ranges

The initial ranges are:

| Grade | Minimum | Maximum |
|---|---:|---:|
| A | 80 | 100 |
| A- | 70 | 79 |
| B | 60 | 69 |
| B- | 50 | 59 |
| C | 40 | 49 |
| C- | 30 | 39 |
| D | 20 | 29 |
| E | 0 | 19 |

Grade ranges are inclusive. The application automatically synchronizes adjacent bands to avoid gaps and overlaps. For example, changing A’s minimum to `85` updates A-’s maximum to `84`.

The following conditions are prevented or reported:

- Minimum greater than or equal to maximum.
- Overlapping grade bands.
- Gaps between adjacent grade bands.
- Invalid ranges outside the `0–100` scale.

Use **Reset Range** to restore the default bands.

## Analytics

For the selected course, the console provides:

- A marks histogram grouped into 10-point bands.
- Student counts above each bar.
- Marks and student-count axis labels.
- A score curve overlay.
- Minimum, maximum, average, and median values.
- Grade count pills.
- Total valid students.
- Pass rate.
- Highest grade awarded.
- Number of ungraded students.

If a valid mark does not fall inside any configured range, it is identified as ungraded and a warning is shown before export.

## Student Review

The Student Review section provides a searchable table containing:

- Student’s BITS ID.
- Total marks.
- Calculated grade.

Search supports BITS IDs and mark values. The table updates when:

- A different course is selected.
- Grade ranges are changed.
- A search term is entered.

## Finalization and Export

Before exporting, the application displays a final confirmation summary containing:

- Instructor name.
- Selected course.
- Number of valid students.
- Grade distribution by grade band.

After confirmation, the application downloads a CSV file containing:

```text
Instructor
Course
Student’s BITS ID
Total Marks (out of 100)
Grade
```

CSV values are escaped safely so commas, quotation marks, and special characters in names, courses, IDs, and grades do not corrupt the file.

Finalization requires:

- A non-empty instructor name.
- A selected course.
- At least one valid student mark.
- Valid, continuous grade ranges.

## User Interface Features

- Quick workflow instructions.
- Visible labels for all important controls.
- Contextual help tooltips.
- Marks-file format guidance.
- Upload filename and row-count status.
- Color-coded success, warning, and error messages.
- Clear workbook reset action.
- Empty states for missing data.
- Responsive layout for smaller screens.
- Keyboard focus indicators.
- ARIA labels, live regions, and alert semantics for accessibility.

## Project Structure

```text
index.html       Main application, styles, markup, and JavaScript
instructions.md  CodeForge challenge instructions and requirements
README.md        Application documentation
```

## Deployment

The application is a static HTML page and can be deployed to any static hosting provider, including:

- GitHub Pages
- Netlify
- Vercel
- Cloudflare Pages

For GitHub Pages:

1. Push the project to a GitHub repository.
2. Open repository **Settings**.
3. Open **Pages**.
4. Select the deployment branch and root folder.
5. Save and open the generated public URL.

## Privacy Considerations

Workbook processing happens in the browser. The application does not send uploaded student data to an application backend. The SheetJS library is loaded from a third-party CDN, so production deployments should review CDN availability and security requirements.

Do not upload real academic records to this prototype without following the institution’s data-protection and authorization requirements.

## Limitations

- The application currently exports CSV rather than a formatted Excel gradebook.
- Grade ranges are configured globally for the selected grading session.
- There is no authentication or role-based access control.
- There is no backend persistence or audit-log service.
- Timer information is session-local and is not stored remotely.
- The page depends on the SheetJS CDN unless the library is bundled locally.
