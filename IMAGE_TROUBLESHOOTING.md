# Image troubleshooting (archive postcards)

If images from the form don’t appear on the archive, use these steps.

## 1. Check the browser console

1. Open the **archive** page on your site.
2. Open **Developer Tools**:  
   - Windows/Linux: `F12` or `Ctrl+Shift+J`  
   - Mac: `Cmd+Option+J`
3. Open the **Console** tab.
4. Look for:
   - **"Archive columns:"** – List of column names. The 7th (index 6) should be your image column (e.g. “Upload your image”).
   - **"Image raw (G), row 0:"** – The exact value from column G for the first row.  
     - If this is **empty**, the sheet isn’t storing the link or column G isn’t the image column.
   - **"Image URL, row 0:"** – The URL the page is using for the image.  
     - It should look like: `https://drive.google.com/thumbnail?id=...&sz=w1000`
5. If you see **red errors** (e.g. 403, CORS, or “Failed to load”), note the message; it usually points to a sharing or URL issue.

## 2. Confirm column G is the image column

- Open your **Google Sheet** (the one linked to the form).
- Row 1 = headers. Find which column has the **image / file upload** question.
- In the code, **column G** = 7th column (A=1, B=2, … G=7).
- If the image is in a **different column** (e.g. C or H), say which letter and we can change the code to use that column instead.

## 3. Check what’s in the sheet (column G)

- In the sheet, click a cell in the **image column** for a submission that should show a photo.
- You should see either:
  - A **clickable link** (e.g. `https://drive.google.com/file/d/xxxxx/view`), or
  - A **file name** that’s a link.
- If the cell is **empty** or only has text (no link), the form isn’t saving the file link there. Check the form’s “File upload” question and that it’s linked to the same sheet.

## 4. Google Drive sharing (most common fix)

For images to load on your site, Drive must allow “Anyone with the link” to view the files.

1. Open **Google Drive** (same account as the form).
2. Find the **folder** where form uploads go (often “Postcard responses” or similar, or check the form’s file upload settings).
3. **Right‑click the folder** → **Share**.
4. Under “General access”, set to **“Anyone with the link”** and **“Viewer”**. Save.
5. Optionally, open **one uploaded image** → **Share** → set to **“Anyone with the link”** → **Viewer** (sometimes the folder share isn’t enough for older files).

## 5. Test the image URL directly

1. From the console, copy the **"Image URL, row 0:"** value (or build it: `https://drive.google.com/thumbnail?id=FILE_ID&sz=w1000` using the file ID from the sheet link).
2. Paste it into a **new browser tab** and press Enter.
3. If you see a **Google sign‑in or “Access denied”** page, sharing is still restricted — repeat step 4 for the folder/file.
4. If you see the **image**, the URL is correct and the problem may be CORS or how the page loads; we can adjust the code (e.g. proxy or different URL format).

## 6. Quick checklist

- [ ] Console shows column names and “Image raw (G), row 0:” is not empty.
- [ ] In the sheet, column G (7th column) is the form’s file upload column.
- [ ] Cells in column G contain Google Drive links (or file IDs).
- [ ] Drive folder (and optionally each image) is shared as “Anyone with the link” → Viewer.
- [ ] Pasting the thumbnail URL in a new tab shows the image, not a sign‑in page.

If you’ve done all of the above and images still don’t appear, send:
- What “Image raw (G), row 0:” shows in the console.
- What “Image URL, row 0:” shows.
- What happens when you open that URL in a new tab (image vs sign‑in vs error).
