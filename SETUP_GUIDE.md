# Google Form Auto-Population Setup Guide

This guide will help you connect your Google Form submissions to automatically populate your website.

## Step 1: Connect Google Form to Google Sheet

1. Open your Google Form: https://docs.google.com/forms/d/e/1FAIpQLSc5Uo75HvYyaBGhQdpnjtVgPwArm-zECGfoFipMOvzD20ZIMA/viewform
2. Click on the **Responses** tab at the top
3. Click the green **Link to Sheets** icon (looks like a spreadsheet)
4. Choose to create a new spreadsheet or link to an existing one
5. A Google Sheet will open with all form responses

## Step 2: Make the Sheet Publicly Viewable

1. In your Google Sheet, click the **Share** button (top right)
2. Click **Change to anyone with the link**
3. Set permission to **Viewer** (not Editor)
4. Click **Done**

**Important:** The sheet must be publicly viewable for the website to access it.

## Step 3: Get Your Spreadsheet ID

1. Look at the URL of your Google Sheet
2. The URL will look like: `https://docs.google.com/spreadsheets/d/SPREADSHEET_ID/edit`
3. Copy the `SPREADSHEET_ID` (the long string of letters and numbers between `/d/` and `/edit`)

## Step 4: Update Your Website Code

1. Open `index.html` and `archive.html`
2. Find this line: `var SPREADSHEET_ID = "";` (or `const SPREADSHEET_ID = "";`)
3. Paste your spreadsheet ID between the quotes
4. Check the sheet name (usually "Sheet1") and update `TAB_NAME` if needed

## Step 5: Update Column Names in Code

1. Open your Google Sheet and look at the column headers (first row)
2. Common headers might be: "Timestamp", "Name", "Message", "Image", etc.
3. In both `index.html` and `archive.html`, find these lines:
   ```javascript
   const text = entry["Your Text Field Name"] || entry["Message"] || entry["Text"] || "";
   const imageUrl = entry["Your Image Field Name"] || entry["Image"] || entry["Picture"] || "";
   ```
4. Replace `"Your Text Field Name"` and `"Your Image Field Name"` with your actual column names from the sheet

## Step 6: Handle Images (Important!)

Google Forms stores uploaded images in Google Drive. The spreadsheet contains Drive links, not direct image URLs.

### Option A: Automatic Conversion (Already Implemented)
The code automatically converts Google Drive links to direct image URLs. However, for this to work:
- The images must be publicly accessible
- You may need to manually make each image public, OR
- Use Option B below for better results

### Option B: Use Google Apps Script (Recommended for Better Image Handling)

1. In your Google Sheet, go to **Extensions** → **Apps Script**
2. Replace the code with this script:

```javascript
function onFormSubmit(e) {
  const sheet = e.source.getActiveSheet();
  const row = e.range.getRow();
  const imageColumn = 3; // Change this to your image column number (A=1, B=2, C=3, etc.)
  
  const imageUrl = sheet.getRange(row, imageColumn).getValue();
  
  if (imageUrl && imageUrl.includes('drive.google.com')) {
    // Extract file ID
    const fileIdMatch = imageUrl.match(/\/file\/d\/([a-zA-Z0-9_-]+)/);
    if (fileIdMatch) {
      const fileId = fileIdMatch[1];
      // Convert to direct image URL
      const directUrl = `https://drive.google.com/uc?export=view&id=${fileId}`;
      sheet.getRange(row, imageColumn).setValue(directUrl);
    }
  }
}
```

3. Save the script
4. Set up a trigger: Click the clock icon → Add Trigger → Choose `onFormSubmit` → Event type: "On form submit"

## Testing

1. Submit a test entry through your Google Form
2. Wait a few seconds
3. Refresh your website
4. Your new submission should appear automatically!

## Troubleshooting

- **No data showing:** Check browser console (F12) for errors. Verify spreadsheet ID is correct and sheet is public.
- **Images not showing:** Make sure images are publicly accessible in Google Drive, or use the Apps Script solution.
- **Wrong data showing:** Check that column names in code match your Google Sheet headers exactly (case-sensitive).

## Notes

- New form submissions will appear automatically when the page is refreshed
- The opensheet.elk.sh service fetches data in real-time
- Make sure your Google Sheet column headers match what's in the code
