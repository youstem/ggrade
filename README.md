# Presentation Grader

A browser-based HTML/JavaScript tool for grading Canvas group presentations on the spot. Each student gets individual scores, and each group gets group scores.

## Two ways to open it

1. **Online (phone or computer):** go to **https://youstem.github.io/ggrade/**
    - Works in Safari on an iPhone, or in Chrome/Edge on a computer.
    - Nothing to install or download.
2. **A copy of `index.html` (computer):** if you were sent the file directly, save it anywhere and double-click it to open it in Chrome or Edge.
    - **On an iPhone, use the online link instead.** A downloaded copy is just a local file, and iOS can silently fail to run it — the page looks fine but no button does anything.

Either way, your roster, rubric and scores stay in your own browser. Nothing is uploaded or shared with anyone else using the same link.

## Files

- `index.html` — the grading application. No server or installation is required.
- `rubrics.csv` — example rubric file. Edit it to match your own rubric, keeping the same column headers (see "CSV format requirements" below).

Those two files are all a grader needs (or just the online link plus a rubric). The roster CSV comes straight from Canvas each time.

## CSV format requirements

Any CSV with these columns will work — it doesn't have to come from Canvas. Column-name matching ignores case and treats spaces, underscores and dashes as the same (so `group_name`, `Group Name` and `group-name` all match).

**Roster CSV** — one row per student:

- `name` — **Required.** Student's display name.
- `group_name` — **Required.** The presentation group's name. Students who share this value are grouped together (unless `canvas_group_id` is also present; see below).
- `canvas_user_id` — Recommended. A unique student ID. Tells apart two students with the same name, and is included in the exported CSV to make matching back to Canvas safer. If omitted, the app matches by name.
- `canvas_group_id` — Optional. A unique group ID. If present, this — not `group_name` — is used to group students, so two groups that happen to share a name are never merged.
- `login_id`, `sections` — Optional. Carried through if present; not used for grading.

Minimal example:

- `name,group_name`
- `Ball Jessie,Team A`
- `Headlam Camille,Team A`
- `Crivella Tyler,Team B`

**Rubric CSV** — one row per rubric item:

- `index` — **Required.** Display order / label for the item (e.g. `1`, `2`, ...).
- `rubric text` — **Required.** The full rubric description. Its first word is the compact column heading in the grading table (e.g. "Speaking role" → `Speaking (4)`).
- `max points` — **Required.** The maximum score; scores must be whole numbers from 0 to this value.
- `type` — Optional. `individual` (scored per student) or `group` (scored once for the whole group, and that score is given to every member). A blank cell — or no `type` column at all — means `individual`, so older rubric files still load. Any other value stops **Load files** with a message naming the bad row.

Example (this is what `rubrics.csv` contains):

- `index,rubric text,max points,type`
- `1,Speaking role,4,individual`
- `2,Response to questions,4,individual`
- `5,Argument and evidence,6,group`
- `6,Visual communication and timing,3,group`
- `7,Team coordination,3,group`

If a required column is missing from either file, **Load files** refuses to load and says which column is missing, rather than loading wrong or blank data.

## Use

1. Open the app (see "Two ways to open it" above).
2. On **Setup**, enter a **Class name/ID/code** (e.g. `PSCI-3350-001`). This is required — it keeps different courses/sections apart, and it prefixes the exported CSV's filename.
3. Export the roster CSV from Canvas. It needs at least:
    - `name`
    - `group_name`
    - preferably `canvas_user_id`
4. Select the roster CSV and the rubric CSV.
5. Click **Load files**. If nothing was saved yet for this exact roster + rubric, it just loads. If a save already exists, you're asked to choose:
    - **Same score sheet** — continue, restoring those scores, or
    - **New score sheet** — erase them and start blank.
6. The tabs across the top:
    - **Rubrics** — confirm the rubric, including which items are individual and which are group.
    - **Students & Scores** — every student and their scores (group items shaded and marked [G]).
    - **Groups** — group membership and whether each group is complete.
    - **GRADING** — grade presentations.
7. In GRADING, groups are listed in a random order, shuffled once per session.
8. Select a group. Members are listed alphabetically.
9. Rubric headings show `FirstWord (MaxPoints)`. Hover over a heading to see the full rubric text.
10. If the rubric has both kinds of items, there are two tabs:
    - **Individual** — one row per member.
    - **Group** — one row for the whole group, with the member names listed above it.

    Each tab shows ✓ once its scores are saved. Switching tabs never loses what you typed.

11. Enter whole-number scores from 0 to the item's maximum. **Any box left blank is saved as 0.** Typing alone does **not** save anything.
12. Click **Save Group**. It saves both tabs at once, and the group is then marked complete (✓).
13. Click **Export Scores CSV** to download `<classID>-scores-YYYY-MM-DD.csv`.

## The three GRADING buttons

Only one of them writes anything:

- **Save Group** — checks the scores typed for the *selected group*, fills every blank box with 0, and records them (in the app and in the browser's local storage). Nothing you type is kept until you click it. If a box is invalid, it's highlighted red, listed below the table, and the save is blocked until you fix it. Switching groups, leaving the Grading tab, or exporting while you have unsaved typing asks you to confirm first, since unsaved typing would be lost.
- **Clear Current Group** — after a confirmation, erases all *saved* scores — individual and group — for the *selected group only*.
- **Export Scores CSV** — downloads a CSV of every group's *saved* scores (not just the selected group). Export as many times as you like.

## Where scores are saved

- **While grading:** in this browser's local storage, in a slot tied to the *exact content* of the roster and rubric CSVs (not their filenames). Loading the same two files later — even next week — brings the scores back, so closing the tab by accident never loses work.
    - Scores stay in **that browser on that device**. They won't appear in a different browser, on a different computer or phone, or — on the same computer — between the online link and a downloaded copy of `index.html`. Pick one way to open the app and stick with it for a grading round.
    - If you reuse the exact same roster + rubric files for a new grading round (e.g. finals), **Load files** asks you to choose **Same score sheet** or **New score sheet** (step 5) instead of guessing.
- **Exported CSV:** a normal browser download, saved wherever your browser puts downloads (usually `Downloads`). To choose the folder each time, turn on "Ask where to save each file" (Chrome/Edge: Settings → Downloads).

## Export format

- Columns: `canvas_user_id`, student name, group name, then one column per rubric item.
- Group items are labelled `(group)` in the header, and each member's row repeats their group's score, so every student row is complete on its own.

## Privacy

Student data and grades are processed only in the browser. Even when the app is opened from the online link, the CSVs you pick and the scores you enter are never sent to GitHub or any other server.

## Phone screens

The grading screen is designed for narrow phone screens:

- groups are listed one per row (on desktop too, to keep the list easy to scan);
- rubric headings show `FirstWord (max)`;
- score columns use compact widths so all rubric columns fit without sideways scrolling;
- other wide tables (Students, Groups, Rubrics) scroll sideways instead of squeezing their columns;
- error messages appear below the grading table and name the student and rubric item — e.g. "[Individual tab] Ball, Jessie: Speaking (4) - enter between 0–4".

## Keeping the online version up to date (maintainer only)

Graders don't need this section.

- The online link is served by GitHub Pages from the `index.html` in the GitHub repo. Editing `index.html` in this folder does **not** change the online version. Upload/push the new `index.html` to the repo, and the link updates within a minute or two.
- Anyone using a copy you sent them keeps the old version until you send them the new file.

If GitHub Pages ever needs to be turned on again (for this repo or a new one):

1. In the repo, click **Settings**.
2. Left sidebar → **Pages**.
3. Under "Build and deployment" → Source: **Deploy from a branch**.
4. Branch: **main**, folder: **/ (root)** → **Save**.
5. Wait about a minute and refresh the Pages screen; it shows the live URL (currently `https://youstem.github.io/ggrade/`).

Share that URL — not the repo page, which only shows the source code.
