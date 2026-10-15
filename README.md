# SUSS Timetable Planner

A single-page web app for planning a semester at the Singapore University of Social Sciences (SUSS). Upload the official PDFs, pick a class group for each course, and the planner checks for clashes on every real session date. That includes one-off sessions, exams and overseas trip dates, not just the usual weekly slot.

**Live demo:** `https://<your-username>.github.io/suss-timetable-planner/`. Click **Try with sample data** if you don't have the PDFs.

## Why I built it

During course registration, the timetable comes as a 300+ page PDF. Checking by hand whether a set of class groups clashes is slow and easy to get wrong. Classes alternate between 2-hour and 3-hour weeks, there are one-off sessions and exams, and some courses have overseas trips during the break. I wanted to compare a few backup timetables side by side before registration opened.

## What it does

- **Reads the SUSS PDFs in the browser.** Drop in any of these and each one is recognised automatically:
  - **Course schedule** (required): the full interim course schedule for the semester.
  - **Curriculum plan** (optional): course names, credit units, prerequisites, which semesters each course runs in, and SUSS Core requirements.
  - **Study plan** (optional): the courses recommended for each intake and semester, added to your plan for you.
  - **Overseas course info sheet** (optional, e.g. NIE351): trip destinations, costs and dates.
- **Works every semester.** Upload the new course schedule and plans are kept separately for each semester.
- **Checks clashes on every date**, including exams and trip dates, and highlights them on a weekly calendar and a week-by-week strip.
- **Lets you keep several plans** (backups), and compares them in one table: clashes, campus days, credit units and trips.
- **Suggests timetables.** It searches all combinations of class groups for clash-free timetables, with options to keep the classes you've already picked, use daytime classes only, start in the morning only, or keep certain weekdays free. Results are ranked by fewest campus days.
- **Tracks your progress.** Mark the courses you've done to see which SUSS Core courses you still need, get warnings about prerequisites, and see notes on courses running for the last time before they're replaced.
- **Exports** a plan to your calendar (.ics) and backs up all plans (.json).

Nothing is uploaded to a server. PDFs are parsed in the browser with [PDF.js](https://mozilla.github.io/pdf.js/), and the extracted data and your plans are saved in the browser's local storage.

## How it works

- `parseSchedule` groups the PDF's text into rows by position. It then matches the course code, class group, date, start and end times and campus, and attaches wrapped text (for example "WRITTEN EXAM" or "Restricted") to the nearest row.
- `parseCurriculum` finds each table's column positions from the header rows, so multi-line cells (course names, prerequisites, remarks) end up in the right column.
- `parseStudyPlan` places each course code in its year and semester by position, then works out the calendar semester from the intake.
- The suggestion search is a depth-first search with pruning. Each class group is checked against what's already placed, using a per-date map of occupied times plus blocked trip days.

## Built with

Plain HTML, CSS and JavaScript in one file, with no build step. PDF.js is loaded from cdnjs.

Built with AI assistance (Claude). I defined the problem, the requirements and the edge cases (alternating session lengths, exams, trips clashing with special-semester classes, courses spanning two semesters), and tested the results against the real SUSS timetable.

## Disclaimer

This is an unofficial student project, not affiliated with SUSS. SUSS can change schedules at any time, so always confirm in eCR. The repository doesn't include any SUSS documents. You upload your own copies.
