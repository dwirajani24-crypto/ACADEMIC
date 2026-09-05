# Academic Ledger

Academic Ledger is a static, local-first A-Level study planner designed for long-term daily use. It combines a recurring Week A / Week B timetable with a preparation flow, timestamp-based study timer, reflection journal, weekend improvement agenda, Pressing Matters, calendar, history, subject dashboards, analytics, topic management, themes and JSON backup/restore.

Subjects included:
- Mathematics
- Further Mathematics
- Economics
- Computer Science

No A-Level topics are pre-populated. Topics are created by the user.

## Files

```text
/
├── index.html
├── manifest.json
├── sw.js
├── README.md
└── icons/
    ├── icon-180.png
    ├── icon-192.png
    └── icon-512.png
```

`index.html` intentionally contains the CSS and JavaScript so the project can be deployed without a build system.

## Run locally

For the basic interface, opening `index.html` directly works. IndexedDB and localStorage can still be used by the browser.

The service worker cannot be installed from `file://`. To test PWA/offline behaviour locally, serve the folder over HTTP. For example, with any static HTTP server available on your machine, serve this directory and visit its HTTP address.

## GitHub repository setup

1. Create a new GitHub repository.
2. Copy all files and the `icons` folder into the repository root.
3. Commit and push the files.
4. Make sure `index.html` is in the root of the published branch/folder.

## GitHub Pages setup

1. Open the repository's Settings.
2. Open Pages.
3. Under the deployment/source option, choose the branch containing these files and its root (`/`).
4. Save.
5. GitHub will provide the published Pages address.
6. Open that address in a normal HTTPS browser session.

The service worker is registered only when the app is not opened with `file://`. GitHub Pages provides HTTPS, so PWA installation and service-worker caching are supported there.

## iPhone

Open the published GitHub Pages address in Safari. Use Safari's Share menu and choose "Add to Home Screen" when available. Launching it from the Home Screen gives the standalone web-app experience supported by iOS.

iOS does not guarantee every form of background notification behaviour available on desktop browsers. The planner therefore does not depend on notifications: Pressing Matters, the timetable and all stored records remain usable inside the app.

## iPad

Open the published address in Safari. Add it to the Home Screen if you want a standalone app-style launch. The responsive interface supports portrait and landscape layouts.

## Updating

Replace the project files in GitHub and push the changes. The service worker uses a versioned cache name. If you make a substantial release, change the `CACHE` value in `sw.js` so the new static assets are cached under a new cache version.

Existing IndexedDB/localStorage data is stored by the site origin and is not automatically replaced when the application code is updated.

## Timetable

The supplied Week A and Week B timetable is included as editable default data near the beginning of the JavaScript under `EDITABLE APP DATA`.

The editor supports editing, adding, deleting, reordering by day through move/edit operations, adding/removing dinner or break entries, resetting an individual day, resetting a whole week, and resetting the entire timetable.

You can also edit it from the Timetable screen:
- edit entries
- change subject
- change duration
- change description
- add or delete entries
- move entries between days while editing
- reset a day by editing/deleting as needed
- reset a whole week
- reset the whole timetable

The timetable is stored locally and does not require source-code edits after deployment.

## Topics

Topics are intentionally empty on first launch.

Use Subjects > Manage topics, or Settings > Manage topics, to:
- create a topic
- assign it to one of the four subjects
- add an optional subtopic
- rename it
- archive/delete it

Deleting a topic does not modify old session records. Historical sessions retain the topic name they had when recorded.

## Study sessions

Starting a timetable session opens a preparation modal. The timer starts only after "Begin session".

The timer:
- uses timestamps rather than a simple decrementing counter
- supports pause/resume
- supports early finish
- can recover after a refresh
- stores the active timer locally
- opens reflection after completion

A completed reflection records the objective, topic, tasks, actual work, what went well, improvement, difficulty, objective status and timing information.

## Weekend improvements

A weekday reflection with an improvement can be converted into a Weekend Agenda task. The app avoids creating a duplicate active task for the same source session.

Weekend tasks can be assigned to Saturday, Sunday or Both/customised by editing. Incomplete tasks retain their history. A Sunday Home review provides the weekly review flow, including planned/completed hours, completion rate, subject breakdown, strongest subject, improvements and missed/rescheduled sessions. At the end of the weekend cycle, overdue incomplete tasks can be escalated to Pressing Matters rather than deleted.

## Calendar and history

Calendar dates with completed study receive subtle indicators. Selecting a date shows its study time and sessions. History provides search and filters for subject, date range, week and status and renders a bounded result set for sensible performance.

## Progress

Progress is calculated from actual stored sessions. It includes:
- total study time
- current-week study
- subject totals
- completed sessions
- completion percentage
- study streak
- improvements
- Week A vs Week B
- weekly chart
- subject breakdown

## Export/import

Use Settings > Export data to download a JSON backup.

Import:
1. Choose a JSON backup.
2. The schema is validated before replacement.
3. A confirmation is shown before existing data is replaced.
4. Malformed files are rejected and the existing data is retained.

For important long-term records, keep periodic exports in a separate location.

## Storage and privacy

IndexedDB is the primary data store. If IndexedDB cannot be opened, the app falls back to localStorage.

Stored data includes:
- settings
- timetable
- topics
- study sessions
- weekend tasks
- Pressing Matters
- weekly review storage
- active timer state

Study data is not sent to an external server by this application.

Clearing browser/site storage can remove local records. Use JSON exports as backups.

## Offline behaviour

The service worker caches the application's static assets when installed on an HTTP(S) origin. After those assets have been cached, the interface can load without a network connection.

A service worker does not work from `file://`, so direct opening is useful for the basic interface but not for installing the offline PWA layer.

## Notifications

Browser notification permission is optional. The application checks for notification support and handles denied/unsupported states without disabling the planner.

Notification behaviour varies by browser and operating system. In particular, iOS has platform-specific restrictions and does not guarantee every kind of background web notification. The app therefore keeps all important task management inside the interface.

## Themes

The built-in themes are:
1. Academia Autumn
2. Christmas
3. Plain / Minimal
4. Soft Pink
5. Monochrome
6. Forest
7. Midnight Academia
8. Library / Sepia

Themes are implemented with CSS variables and saved in local settings.

## Data model

The app is designed for at least two academic years of records. There is no automatic history deletion.

Completed sessions include, at minimum:
`id`, `date`, `week`, `day`, `subject`, `durationPlanned`, `durationActual`, `topic`, `objective`, `specificTasks`, `targetOutcome`, `actualWork`, `wentWell`, `improvement`, `difficulty`, `objectiveStatus`, `status`, `createdAt`, `completedAt`.

## Troubleshooting

If the page ever appears incomplete:
1. Refresh the page.
2. Check the browser console for an extension or browser-specific error.
3. Export your data if possible.
4. If site storage is corrupted, the application is designed to continue with safe defaults/fallback storage; an exported JSON backup can be imported afterwards.

If a service worker appears to be serving an older release, update the cache version in `sw.js`, deploy, then reload the site.

## Licence / assets

The interface and icon supplied with this project are original project assets. No third-party image assets are required.


## Reliability fixes

The current build repairs malformed saved timetable data, fixes the planned-minutes rendering error that could prevent Home from binding buttons, uses safe ID generation, keeps the shared modal stable during timer use, catches button-action failures, and provides a timetable retry/repair path. Add Session and session preparation also include direct topic management, and topic creation persists immediately.
