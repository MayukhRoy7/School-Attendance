# School Attendance Tracker 2026–27

A single-page attendance tracker for one school year (April 2026 to March 2027). Pick your school days, mark the days you were absent, and see your attendance percentage by year, by month, or for any custom date range. A separate "Future model" tab lets you plan what-if scenarios without touching your real data.

It is one HTML file with no dependencies, no build step, and no server. Your data stays in your browser.

## Features

**Setup and daily use**

- **Step 1, School days:** Tap the days you have school. Shortcuts fill in every Mon–Fri or Mon–Sat, then you tap holidays to remove them.
- **Step 2, Mark absences:** School days up to yesterday appear unmarked (dashed). Tap the days you were absent. When you press Finish, the unmarked days count as present.
- **Step 3, Summary:** A "today" card lets you mark today as present or absent. Future days stay locked until they arrive.
- **Editing later:** Tap any past school day to flip it between present and absent.
- **Confirmations:** Changing an existing mark, removing a marked school day, importing, and resetting all ask you to confirm first.

**Numbers and reports**

- **Overall attendance:** Present days divided by marked days (present + absent), with a bar and a line at 80%.
- **Days you can still miss:** `floor(total school days / 5) − absences so far`. This is the most absences you can still have and finish the year at 80% or more, assuming you attend every other day. It uses the school days you have selected, so select the rest of the year in Step 1 for a realistic figure.
- **Month-wise percentage:** Shown above every month. It is present days divided by all school days in that month.
- **Custom range:** Pick a start date and an end date (both included) to see school days, present, absent, days not marked yet, and the percentage. It uses the same rule as the months.

**Future model (Step 4)**

- A practice area that starts as a copy of your real data. Nothing in it changes your real attendance, and it is not saved automatically.
- Paint days with a brush: Present, Absent, Not decided, or No school. Painting on a non-school day turns it into a school day.
- Apply a brush to a date range, or set every undecided day from today onward to present.
- See the projected year-end percentage, month-wise percentages, and how many undecided days must be present to reach 80%. Undecided days count as not present.
- **Export model** saves a `.txt` file. **Import model** loads one back. **Reset model** returns to a fresh copy of your real data.

**Other**

- **Export / Import data:** Back up your real data as a `.json` file and restore it later or on another device.
- **Light, Dark, Auto theme:** Auto follows your device setting. Your choice is remembered.
- Responsive layout that works on phones and desktops.

## How percentages are calculated

| Where | Formula |
| --- | --- |
| Overall (Summary) | present ÷ (present + absent) |
| Each month | present ÷ school days in that month |
| Custom range | present ÷ school days in that range |
| Future model | present ÷ all school days in the model |

Month and range percentages include school days still to come, so they rise as you mark those days present.

## Getting started

1. Save the page as `index.html` in the root of a repository.
2. Open it in a browser. You can double-click the file, or host it (see below).

### Host it for free with GitHub Pages

1. Push `index.html` to a GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select your main branch and the `/ (root)` folder, then save.
4. After a minute, the page is live at `https://<your-username>.github.io/<repository-name>/`.

## Where your data is stored

Everything is saved in your browser's local storage on the device you use.

| Key | What it holds |
| --- | --- |
| `attendance-2026-27` | School days, marks, and the current step |
| `attendance-theme` | Your theme choice (Auto, Light, or Dark) |

- Nothing is sent to a server.
- A different browser, a different device, or a private window starts empty.
- Clearing site data erases it. Use **Export data** regularly as a backup.
- The Future model is kept in memory only. Use **Export model** to keep a plan.

## Backup file formats

Both are plain JSON, so you can keep them as text files.

- **Data export** (`attendance-2026-27-backup-<date>.json`): `{ "app": "attendance-2026-27", "sd": {...}, "st": {...}, "stage": 3 }`
- **Model export** (`attendance-model-<date>.txt`): `{ "app": "attendance-2026-27-model", "sd": {...}, "st": {...} }`

`sd` lists the school days (`"YYYY-MM-DD": 1`) and `st` holds the marks (`"P"` for present, `"A"` for absent). Import rejects files whose `app` value does not match, so a model file can't be loaded as real data by mistake.

### Note on saving files

The Export buttons try to use the host page's file-save feature. On a plain site such as GitHub Pages that feature may not exist, and the page then shows the export text in a box for you to copy into a file yourself. Import accepts either a chosen file or pasted text.

## Customising

Everything is in `index.html`. Search for these to change them:

- **School year:** The `days` array is built from `new Date(2026,3,1)` to `new Date(2027,2,31)`. The month loop in `cal()` and the labels in the title and heading also use April 2026 to March 2027. Update the storage key `SK` too if you start a new year, so old data isn't mixed in.
- **Attendance target (80%):** It appears in `ok=pc>=80`, `Math.floor(s.sc/5)` (the days-you-can-miss formula), `Math.ceil(a.N*4/5)` and `p>=80` (model and months), and the bar marker `left:80%` in the CSS, plus the text that says "80%".
- **Weekday shortcuts:** The `wk5` and `wk6` actions list which weekdays are filled in.
- **Colours:** All colours are CSS variables at the top of the stylesheet, with separate values for light and dark.

## Tech

- Plain HTML, CSS, and JavaScript in one file
- No framework, no package manager, no external requests
- Works in current versions of Chrome, Edge, Firefox, and Safari

## Limitations

- Built for one school year and one student.
- "Today" comes from your device's clock.
- Data does not sync between devices. Use Export and Import to move it.
