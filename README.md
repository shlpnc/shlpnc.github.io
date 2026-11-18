# P&C Project Deliverables Dashboard

The Planning & Coordination (P&C) department at Shanta Holdings Limited tracks project deliverables in this sign-in protected dashboard. It shows every task across the portfolio by status, project, resource and date. Each figure, bar, slice and table value filters the whole dashboard, the same way a Power BI report does.

The website is a single file, `index.html`. The task data inside it is encrypted and opens only after a valid sign-in.

## What is in this repository

| File | Purpose |
|---|---|
| `index.html` | The dashboard. GitHub Pages serves this file as the website. |
| `README.md` | This guide. |

The publisher (`Shanta_PC_Dashboard_Publisher_ADMIN.html`) and the user list (`PC_User_ID.xlsx`) must **never** be uploaded here. P&C Admin keeps both on their own computer.

## Task statuses

Each task falls into exactly one of four statuses, judged against the data date shown in the dashboard.

| Status | Rule |
|---|---|
| Completed on time | Has an actual finish on or before its baseline finish |
| Completed late | Has an actual finish after its baseline finish |
| Overdue | No actual finish, and its baseline finish is before the data date |
| Upcoming | No actual finish, and its baseline finish is on or after the data date |

**Total tasks** is the sum of the four statuses, and **Completed** is on time plus late.

- **KPI achieved** = completed on time ÷ total tasks. This matches the earlier Power BI dashboard.
- **On-time rate to date** = completed on time ÷ (completed + overdue). It leaves out upcoming tasks.

## Pages

- **Overview**: headline status, completion, KPI and the 30-day outlook. It also has status by project, resource, phase and deliverable type, the delivery pipeline, the oldest overdue tasks and generated findings.
- **Projects**: a list of projects. Select one to see its five status counts, schedule span, pipeline, resources, open work packages, phases, deliverable types, overdue tasks and next-due tasks. **Full details** opens a side panel with every task for that project, ignoring filters.
- **Resources**: the same layout for each resource (person). Other departments' members can be added the same way.
- **KPI performance**: the five status counts and two gauges, plus status by resource and by project. It also has monthly and yearly breakdowns, days late, and average delay by project.
- **Schedule**: plan versus actual (S-curve), project schedule, upcoming tasks by week, forward load by resource and month, who is delivering what, due-window bands and baseline versions.
- **Tracker**: every task with sorting, paging and search. Select a task to open its details. These include baseline and actual dates on a small timeline, durations, the work package and the tasks around it. Use ‹ and › or the arrow keys to step through the list.

Other features:

- **Copy link** saves the current page and filters in the web address, so the view can be shared. Anyone opening the link still has to sign in.
- **Keyboard shortcuts**: keys 1 to 6 switch pages, and / jumps to search.
- **Light and dark mode.**
- **Automatic sign-out** after 30 minutes without activity.

## Updating the data each month (P&C Admin)

1. Open `Shanta_PC_Dashboard_Publisher_ADMIN.html` in Chrome or Edge on your computer.
2. **Step 1:** drop the new task export (Excel or CSV with ProjectName, WBS, WorkCategory, TaskName, BaselineNumber, BaselineStart, ActualStart, BaselineFinish, ActualFinish, ResourceNames).
3. **Step 2:** drop the user list (Excel or CSV with User ID and Password columns).
4. **Step 3:** check the five counts and set the **data date** to the date of the export.
5. **Step 4:** keep **index.html** selected and select **Build dashboard file**.
6. In this repository, select **Add file**, then **Upload files**. Drag in the new `index.html` and select **Commit changes**. It replaces the old file.
7. The website and the SharePoint page show the new data within a few minutes. If someone still sees old data, ask them to refresh the page.

Rebuild in the same way whenever someone joins or leaves, after changing the user list.

## Setting up GitHub Pages (first time only)

1. On github.com, select **+**, then **New repository**. Name it, for example `pc-dashboard`, and choose **Public**. GitHub Pages on a free account needs a public repository. The task data in `index.html` is encrypted, so only signed-in users can read it.
2. Upload `index.html` and `README.md` as described above.
3. Open **Settings**, then **Pages**. Set Source to **Deploy from a branch**, Branch to **main** and the folder to **/ (root)**, then select **Save**.
4. After a few minutes the site address appears there, for example `https://your-name.github.io/pc-dashboard/`.

## Showing it on a SharePoint page

SharePoint does not run HTML files from a document library. Its **Embed** web part only accepts a web address inside iframe code, which is why the dashboard is hosted on GitHub Pages and framed in SharePoint.

1. **Allow the address (site owner, once per site):** select the **Settings** gear, then **Site information**, then **View all site settings**. Under Site Collection Administration, open **HTML Field Security**. Choose **Allow contributors to insert iframes only from the following domains**, add `github.io`, and select **OK**.
2. **Add the web part:** edit the SharePoint page, add an **Embed** web part, and paste the code below into **Website address or embed code**. Use your own address. Step 5 of the publisher can also write this code for you.

   ```html
   <iframe src="https://your-name.github.io/pc-dashboard/" width="100%" height="900" frameborder="0" style="border:0" title="Project deliverables dashboard"></iframe>
   ```

3. Republish the page. Viewers sign in inside the frame. The **Open in new tab** button, shown only inside SharePoint, opens the full-screen version.

Use a full-width section on the SharePoint page so the dashboard has room. A height of 900 to 1100 pixels works well.

## Security

- The task data is encrypted with AES-256-GCM. Each user's ID and password derive a key (PBKDF2-SHA-256, 250,000 rounds) that unlocks the data. Passwords are never stored in the file.
- Without a valid sign-in, the file shows only the sign-in page. Viewing the page source reveals no project names, people or dates.
- Because anyone can download a public file and try passwords offline, **password strength is the real protection**. Give each person their own password of 10 or more characters. Change the passwords and rebuild if someone leaves.
- After five wrong attempts the sign-in page makes the person wait before trying again.
- The page asks search engines not to index it.

## Browser support

Current versions of Chrome, Edge, Firefox and Safari. The page must be opened over `https` (GitHub Pages does this) or as a local file.

## Troubleshooting

| Problem | What to do |
|---|---|
| SharePoint says "Embedding content from this website isn't allowed" | A site owner needs to add `github.io` under HTML Field Security (see above). |
| The SharePoint frame is blank | Check that the address opens on its own in a browser, and that GitHub Pages is switched on. |
| "The user ID or password is incorrect" | IDs are not case-sensitive, but passwords are. In the Excel list, format the User ID column as Text so IDs like 0276 keep the leading zero. |
| Old figures still showing | Refresh the page. GitHub Pages can take a few minutes after an upload. |
| Signed out unexpectedly | The dashboard signs out after 30 minutes without activity, or when the tab is closed. |
