# LifeOS — Release Notes
### v4.0 Final · March 2026

> A personal productivity command center built as a single offline HTML file.  
> Tracks tasks, goals, habits, notes, planner events, and focus sessions — all stored locally in your browser.

---

## 🚀 New Features

**Personalised Welcome Screen**  
A full-screen onboarding popup greets new users on first launch, asks for their name, and stores it in localStorage. The app uses this name in dashboard greetings. Returning users skip straight to their dashboard.

**Goal Fireworks Celebration**  
Dragging a goal's progress slider to 100% triggers a full-screen multi-burst confetti animation with a trophy banner at the center of the screen. Particles use gravity physics and varied shapes (circles and rectangles). The banner displays the goal's title and can be dismissed manually.

**Dark / Light Mode Toggle**  
A sun/moon button in the top bar switches between themes. On first load the app detects the system's preferred color scheme. Every surface, border, badge, and semantic color adapts. Preference is saved across sessions.

**Planner Event Alarms**  
Events can now have alarms enabled when created. A background checker runs every 30 seconds and fires at the exact scheduled minute with:
- A triple-beep audio tone via the Web Audio API (no external files needed)
- An in-app popup notification that auto-dismisses after 15 seconds
- A native browser push notification when permission is granted

**Clear All Button on Every Page**  
Each section — Tasks, Goals, Planner, Habits, and Notes — now has a "Clear All" button in its action bar. Each one asks for confirmation before deleting. Clearing Habits also wipes the full check-log history.

**Expanded Quote Library (60+ quotes)**  
The daily motivational quote pool grew from 12 to over 60 entries sourced from athletes, entrepreneurs, authors, and philosophers. Quotes cycle on a deterministic daily formula so each day shows a reliably different quote.

**In-App Toast Notifications**  
A non-blocking toast bar appears at the bottom-right to confirm actions — timer completions, alarm permission grants, session resets — without interrupting the user's flow with a blocking dialog.

**Focus Timer Audio Feedback**  
A short two-tone chime plays via the Web Audio API when a Pomodoro session finishes, alerting the user even when they are not looking at the screen.

**Priority Color-Coded Task Borders**  
Task list rows now have a left-edge color indicator — red for high priority, amber for medium, green for low — making urgency immediately visible without reading the badge text.

**Goal Category Icons**  
Each goal card displays an emoji icon based on its category: 💼 Career, 💪 Health, 💰 Finance, 📚 Learning, 🌱 Personal, ❤️ Relationships, 🎨 Creative. Goals with no category default to 🎯.

**Planner Notification Permission Banner**  
If browser notifications are not yet enabled, the Planner shows a contextual inline banner with a one-click "Enable" button. If notifications are blocked at the browser level, a separate message explains the in-app popup fallback.

**Time-Aware Dashboard Greeting**  
The dashboard greeting changes based on the current hour — morning, afternoon, evening, and late-night variants — and addresses the user by their chosen name.

---

## 🔧 Bug Fixes

**Habits disappearing after being checked**  
Checking a habit triggered a full panel re-render which visually removed all habits from the list, even though the data was still stored correctly. The toggle now surgically updates only that specific row's checkbox in the DOM — habits are always visible regardless of their checked state.

**Habit counter not resetting when a habit is deleted**  
Deleting a habit left its ID scattered across every date entry in the check-log (`hlog`). The done counter kept including it even though the habit no longer existed. Deletion now scrubs the habit's ID from the entire `hlog` before saving.

**Footer showing shortened name instead of full name**  
The sidebar footer "Done by" credit was being overwritten at runtime by a JavaScript line that set it to whatever the user typed at the welcome screen. The dynamic JS update has been removed — the footer is now a static HTML element with the full name hardcoded.

**Streak counter showing wrong value after unchecking**  
The streak calculation was reading from the log's previous state rather than the freshly updated copy. The function now receives and reads from the mutated array directly.

**File not saving to outputs directory**  
A previous build was silently failing to write the final HTML file to the correct output path, causing users to download a stale version. The write path was corrected and the JavaScript was validated with a Node.js syntax check before delivery.

**AlarmFired flag not resetting on new day**  
Once an alarm fired, its `alarmFired` flag was never cleared. This meant a recurring event scheduled on a future date would never alarm again after firing once. On app init, all past events with `alarmFired = true` are now reset so future occurrences fire correctly.

---

## ✨ Improvements

**Fully responsive on all screen sizes**  
The sidebar collapses off-screen on mobile with a hamburger menu and a darkened backdrop overlay. Metric grids, quick-nav cards, and the note grid all collapse gracefully to 2-column or single-column layouts. The planner week grid has a minimum width with horizontal scroll on narrow screens.

**Dashboard focused on overview only**  
The old dashboard mixed in action buttons from Tasks, Goals, and Planner. It is now a pure overview: live stats, year progress bar, daily quote, and jump-to navigation cards. Each page owns its own actions exclusively.

**Senior-level task list design**  
Tasks are sorted by priority then by due date. Completed tasks drop to the bottom of the list. Each row has a left-border priority color, a smooth animated checkbox, a hover-reveal delete button, and ellipsis truncation for long notes.

**Motivational microcopy throughout**  
Timer completion messages are personality-rich. Goal completion triggers a celebration. Empty states include encouraging taglines. The year progress bar shows days remaining with a motivational line.

**Goal progress bar updates live**  
Dragging the progress slider no longer requires a full panel re-render. The bar width and percentage label update in real time as the slider moves. The fireworks animation is triggered asynchronously only after 100% is reached.

**Habits 21-day history dots update instantly**  
The history dot for today toggles on and off in sync with the checkbox using DOM selectors — no re-render needed. Today's dot has an orange ring outline to distinguish it from past days at a glance.

**Planner events sorted by time**  
Events in each day column are now sorted chronologically by their time field so the daily schedule reads top-to-bottom in correct order.

**Sidebar badges show real-time counts**  
The Tasks badge shows the live count of active (non-done) tasks. The Habits badge shows today's completion ratio (e.g. `2/5`). Both update immediately after any action without requiring navigation.

---

## 🗑 Removed

**Cross-page action buttons on Dashboard**  
The old dashboard had "New Task", "My Goals", and "Planner" buttons that duplicated functionality from other pages. Replaced by clean navigation cards that jump to each section.

**Dynamic sidebar author name**  
The JavaScript that overwrote the footer credit with the user's typed name has been removed. The credit is now static HTML and cannot be changed at runtime.

**Blocking `alert()` dialogs for timer completion**  
`window.alert()` calls were replaced with non-blocking toast notifications and audio feedback so the timer panel re-renders cleanly without the user needing to dismiss a modal dialog first.

---

## 📦 Technical Notes

- Single-file HTML app — no build tools, no dependencies, no server required
- All data stored in `localStorage` with prefixed keys (`lo4_*`) to avoid collisions
- JavaScript written in ES5-compatible syntax for maximum browser compatibility
- Alarm system uses `setInterval` polling (30s) rather than `setTimeout` scheduling for reliability across tab suspensions
- Web Audio API tones generated programmatically — no audio files bundled
- Fireworks canvas cleared automatically after 6 seconds to free memory

---

## 👤 Author

**Done by:** Ahmed Ezz Eldin Elaraby  
4th year · CSC
