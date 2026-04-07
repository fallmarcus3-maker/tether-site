# Feature Brief: The Queue
**Tether — UI/UX Design Brief**
**Date:** March 26, 2026

---

## 1. Problem Statement

Trade businesses have two types of work:

1. **Scheduled tasks** — jobs with a time and date on the calendar (already handled by Tether)
2. **Evergreen tasks** — things that need to happen but have no specific time: clean the bay, sharpen the router bits, restock shop supplies, call the supplier, touch up the paint on the shop wall

Right now these tasks live in someone's head, on a paper list, or get forgotten entirely. When a crew member has 45 minutes between jobs, there is no structured answer to "what should I be doing right now?"

Trade schedules are fluid — a job gets pushed, a crew member shows up early, a slot opens up. The question comes up constantly and the answer is either ask the boss (who is on-site) or do nothing.

**The Queue** is that answer.

---

## 2. Feature Summary

A lightweight, always-accessible pool of unscheduled tasks that any team member can pick up and complete whenever there is a gap in their day. No dates, no times — just a list of "whenever you can" work with estimated durations.

Tasks can be **one-time** or **recurring**. Recurring tasks auto-reset after a set interval once marked done. Owner sets it once — it runs forever. This turns operational knowledge (the stuff experienced tradespeople carry in their heads) into a self-running system. New employees immediately have answers. Experienced ones don't waste mental energy remembering what needs doing.

**The product pitch: set up your shop's operating rhythm once, and Tether keeps it running.**

---

## 3. Design Constraints

- **Sawdust Rule:** If it takes more than 3 seconds with dirty hands, it fails. Employee interactions must be zero-friction.
- **Owner = planner, Employee = executor.** Owners and managers add and manage queue items. Employees claim and complete them.
- **No notification noise.** The queue is a place the owner checks, not a thing that demands attention. The bell stays reserved for decisions: join requests, time-off, payroll, AI suggestions.
- **Don't clutter the calendar.** Radar stays clean. Queue tasks are never placed on the calendar unless explicitly promoted by the owner.
- **Feel native to the app.** Glassomorphic cards, Inter font, existing primary / accent / signal green palette.

---

## 4. Naming

| Context | Label |
|---|---|
| Owner / Manager view | **The Queue** |
| Employee view | **Open Work** |

*The Queue* feels like a backlog of shop jobs — familiar to trade workers. *Open Work* is what an employee sees: work that is available and waiting for them.

---

## 5. Where It Lives

The Queue surfaces in three places — the same content, three different lenses:

| Surface | Who Sees It | Entry Point |
|---|---|---|
| Home Page (Owner) | Owner / Manager | Collapsible section below today's feed |
| Employee Feed | Employees | "Open Work" section at bottom of feed |
| Global Access Chip | Everyone | Persistent pill above the nav bar |

---

## 6. Task Types

### One-Time Task
A task that gets done once and is then archived. No auto-reset.

### Recurring Task
A task that resets automatically after a set interval once marked done. Displays how long ago it was last completed rather than when it is next due. This is more honest to how trades think — nobody thinks "the floor is due Thursday," they think "when did we last sweep it?"

---

## 7. UI: Add Queue Task Sheet

Opens from the `[+ Add to Queue]` button or as a new task type in the existing FAB flow. Intentionally shorter than the full Add Task sheet — no date, no time, no revenue fields.

```
┌─────────────────────────────────────────────────────┐
│  NEW QUEUE TASK                              [✕]    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  What needs to get done?                           │
│  ┌─────────────────────────────────────────────┐   │
│  │  e.g. "Sweep the shop floor"                │   │
│  └─────────────────────────────────────────────┘   │
│                                                     │
│  How long will it take?                            │
│  [10 min]  [20 min]  [45 min]  [1 hr]  [Custom]   │
│                                                     │
│  Category                                          │
│  [Shop]  [Admin]  [Site]  [Supply]                 │
│                                                     │
│  Assign to (optional)                              │
│  [Anyone]  [Jake]  [Mike]  [Sarah]                 │
│                                                     │
│  ─────────────────────────────────────────────     │
│  Repeats                              [  ●  ] ON   │
│                                                     │
│  ↓ expands when toggled ON                         │
│                                                     │
│  [Daily]  [Weekly]  [Every 2 Weeks]  [Monthly]     │
│  ─────────────────────────────────────────────     │
│                                                     │
│  ┌─────────────────────────────────────────────┐   │
│  │              Add to Queue                   │   │
│  └─────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

**Notes:**
- Repeats toggle is OFF by default — one-time is the common case
- Frequency chips animate in below the toggle on expand
- Four options cover 95% of trade use cases — no custom interval in v1
- Assignment defaults to "Anyone" — unassigned tasks are visible to all employees

---

## 8. UI: Task Cards

### One-Time Task Card
```
┌───────────────────────────────────────────────────┐
│    Call supplier re: pricing        ⏱ ~10 min   │
│    Admin                            [Marcus]       │
└───────────────────────────────────────────────────┘
```

### Recurring Task Card — Recently Done
```
┌───────────────────────────────────────────────────┐
│ ↺  Sweep the shop floor             ⏱ ~15 min   │
│    Shop                    Last done 2 days ago    │
└───────────────────────────────────────────────────┘
```

### Recurring Task Card — Overdue (~1.5× interval elapsed)
```
┌───────────────────────────────────────────────────┐
│ ↺  Check oil levels                 ⏱ ~20 min   │
│    Shop                    Last done 9 days ago ⚠  │
└───────────────────────────────────────────────────┘
```

### Recurring Task Card — Never Completed
```
┌───────────────────────────────────────────────────┐
│ ↺  Order monthly supplies           ⏱ ~30 min   │
│    Admin                       Never completed     │
└───────────────────────────────────────────────────┘
```

**Visual language:**
- **Left-edge color bar:** Muted amber/gold for all queue tasks — distinct from the 4 time-bucket colors, signals "unscheduled / available"
- **Overdue escalation:** When ~1.5× interval has elapsed, the left bar shifts to a warmer more urgent tone. No badge, no number — just color.
- **↺ icon:** `repeat_rounded`. Immediately distinguishes recurring tasks from one-time tasks at a glance.
- **"Never completed":** Shown in muted gray — useful during onboarding to see which systems haven't cycled through yet.
- Card style otherwise matches existing task cards: 12px rounded corners, Inter font, subtle shadow.

---

## 9. UI: Owner Home Page — Queue Section

Collapsible section below the Today feed. Defaults open when queue has items, closed when empty.

```
┌─────────────────────────────────────────────────────┐
│  THE QUEUE                             [3 open]  ▾  │
├─────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────┐    │
│  │ ↺  Sweep the shop floor      ⏱ ~15 min    │    │
│  │    Shop               Last done 9 days ago ⚠│    │
│  └─────────────────────────────────────────────┘    │
│  ┌─────────────────────────────────────────────┐    │
│  │    Restock supplies          ⏱ ~20 min    │    │
│  │    Supply                   [Unassigned]   │    │
│  └─────────────────────────────────────────────┘    │
│  ┌─────────────────────────────────────────────┐    │
│  │ ↺  Order monthly supplies    ⏱ ~30 min    │    │
│  │    Admin               Never completed     │    │
│  └─────────────────────────────────────────────┘    │
│                          [+ Add to Queue]           │
└─────────────────────────────────────────────────────┘
```

**Notes:**
- Section header matches existing stats card / notification bell header style
- `[+ Add to Queue]` uses secondary button style, full width, anchored at bottom of section
- No "Coming Up" sub-section — the queue is a flat list of what is available right now. Recurring tasks that haven't elapsed yet simply don't appear.

---

## 10. UI: Employee Feed — Open Work Section

At the bottom of the employee's Feed tab, below their scheduled tasks for the day.

```
┌─────────────────────────────────────────────────────┐
│  OPEN WORK                                          │
│  Grab one if you have time                          │
├─────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────┐    │
│  │  Sweep the shop floor                       │    │
│  │  ⏱ ~15 min                    [Claim It]   │    │
│  └─────────────────────────────────────────────┘    │
│  ┌─────────────────────────────────────────────┐    │
│  │  Restock supplies                           │    │
│  │  ⏱ ~20 min                    [Claim It]   │    │
│  └─────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────┘
```

**Notes:**
- Subtitle "Grab one if you have time" — conversational, on-brand
- `[Claim It]` button: signal green (#10B981), full pill shape — matches clock-in button language
- On claim: card updates to show "Claimed by you" with a `[Done]` button in green
- Tasks assigned to a specific employee only show for that employee
- Unassigned tasks show for everyone
- Section hides entirely if no open work — no empty state clutter
- Employees do not see the "last done" timestamp — they just see the task name and duration

---

## 11. UI: Global Access Chip

A persistent floating chip anchored just above the nav bar, visible from any tab when the queue has active items.

```
              ┌─────────────────────┐
              │  ↺ 3 Open Tasks     │
              └─────────────────────┘
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    [Office] [Crew] [Radar] [TODAY] [Jobs] [Insights] [Settings]
```

**Notes:**
- Only visible when queue has items
- Uses the muted amber accent — visually distinct from the notification bell (urgent) — the chip signals opportunity, not alert
- Tapping opens The Queue as a bottom sheet
- Animates in/out with a slide-up when queue count changes to/from 0
- When the shortest available task is ≤15 min, subtitle can read "Quick win: 10 min" instead of the count

---

## 12. Task States & Lifecycle

### One-Time Task
```
[Open] → [Claimed] → [Done] → [Archived]
           ↓
        [Released back to Open]
```

### Recurring Task
```
[Open] → [Claimed] → [Done] → [Waiting: interval elapsed?]
           ↓                          │
        [Released]              NO → stays hidden
                                YES → resets to [Open] silently
```

**Reset behavior:**
- Resets are silent — no notification, no bell badge
- New interval is calculated from the last completion date, not from today
- If the owner edits the interval after the task has been running, the next reset uses the new interval from the last completion date

| State | Left Bar Color | Visible To Employees |
|---|---|---|
| Open | Amber | Yes |
| Claimed | Blue | Yes (shows claimant) |
| Done / Waiting | — | No (hidden from feed) |
| Overdue (~1.5× interval) | Warm amber-red | Yes |

---

## 13. Owner Controls

| Action | Interaction |
|---|---|
| Add task | `[+ Add to Queue]` button or FAB → "Filler Task" type |
| Edit task | Tap card → opens Add Queue Task sheet in edit mode |
| Reorder | Drag handle — same pattern as Radar overflow bin |
| Promote to calendar | Long-press → "Schedule this" → opens AddTaskSheet pre-filled, removes from queue |
| Archive / Delete | Swipe left on card |

---

## 14. Scope — v1 Boundaries

**In scope:**
- One-time and recurring queue tasks
- Four frequency options: Daily, Weekly, Every 2 Weeks, Monthly
- Four categories: Shop, Admin, Site, Supply
- Assignment to a specific employee or "Anyone"
- Claim and complete flow for employees
- Backward-looking "last done" display for recurring tasks
- Silent auto-reset on interval elapsed

**Out of scope for v1:**
- Custom frequency intervals (e.g. every 3 days)
- Due dates on queue tasks — if it has a due date it should be a scheduled task
- Revenue / job costing on queue tasks (they are overhead, not billable)
- Employee ability to add to the queue — owner and manager only
- AI gap-filling from queue (Phase 2 — but data model should be designed with it in mind)
- Reset notifications

---

## 15. Future: AI Integration Hook

The Queue is designed as the input pool for future gap-filling intelligence:

> *"Jake has 40 minutes free between 2pm and 2:40pm. The queue has 'Clean the detail bay' (~45 min) and 'Restock supplies' (~20 min). Recommendation: Restock supplies."*

The Radar view could surface a "Fill This Gap" action on light-heat days, pulling matching tasks from the queue by duration. This is Phase 2, but the data model should account for it from the start.

---

## 16. Open Questions for Designer

1. **Amber accent definition** — needs a name and hex in the design system. Should read as "opportunity / available" not "warning." Consider a warm gold or soft teal as alternatives.
2. **Global chip vs. notification bell** — ensure these two don't visually compete. The bell = urgent decisions. The chip = available work. Color and placement should make this immediately clear.
3. **Claim interaction for gloved hands** — is `[Claim It]` a button on the card, or is the whole card tappable? Small tap targets are a real problem for trade workers. Consider a large tap zone or swipe-to-claim gesture.
4. **Category chips vs. time buckets** — should queue tasks reuse the existing 4 time bucket colors, or have their own category system? Reusing time buckets keeps it familiar. New categories (Shop / Admin / Site / Supply) give more flexibility but add a new concept.
5. **Empty state for Queue section** — show a prompt ("Nothing in the queue — add some shop work"), or hide the section entirely when empty?
