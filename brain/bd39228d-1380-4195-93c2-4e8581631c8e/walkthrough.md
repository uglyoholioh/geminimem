# NUSmods Timetable Import - Walkthrough

## Summary

Added the NUSmods timetable import functionality directly to the planner page, allowing users to import their class schedules without navigating away from the planner.

## Changes Made

### [page.tsx](file:///Users/oli/Desktop/LMSManager/frontend/app/planner/page.tsx)

Added the following features to the planner page:

1. **Import Timetable Button** - A blue-themed button in the controls section that opens the import panel
2. **Import Panel** - Collapsible panel with two import methods:
   - **ICS File** - Upload an ICS file exported from NUSmods
   - **NUSmods URL** - Paste a NUSmods share URL to parse and select classes
3. **Class Selection Modal** - Integrated the existing `ClassSelectionModal` component for selecting which classes to import from NUSmods URLs

render_diffs(file:///Users/oli/Desktop/LMSManager/frontend/app/planner/page.tsx)

## Demo

![Planner Import Demo](/Users/oli/.gemini/antigravity/brain/bd39228d-1380-4195-93c2-4e8581631c8e/verify_planner_import_1770098385658.webp)

## How to Use

### Import via ICS File (Recommended)

1. Go to [NUSmods](https://nusmods.com)
2. Open your timetable
3. Click **Export** → **ICS**
4. On the planner page, click **Import Timetable**
5. Select the **ICS File** tab (default)
6. Click to upload your `.ics` file
7. Click **Import File**

### Import via NUSmods URL

1. On NUSmods, click **Share** to get your timetable URL
2. On the planner page, click **Import Timetable**
3. Select the **NUSmods URL** tab
4. Paste your share URL
5. Click **Parse & Select Classes**
6. Select which classes to import in the modal
7. Click **Import**

## Verification

✅ Page loads without errors  
✅ Import Timetable button appears in controls section  
✅ Import panel opens with ICS and NUSmods tabs  
✅ ICS file upload area functional  
✅ NUSmods URL input and parse button functional  
✅ Panel closes correctly  
✅ Imported classes appear in the weekly timetable view
