---
name: daily-task-planner
description: Turns a simple list of daily tasks into a prioritized, realistic schedule. Use this skill whenever the user gives a list of tasks/to-dos for the day (with or without deadlines, effort estimates, or available time) and wants help planning, prioritizing, or scheduling their day. Trigger on phrases like "plan my day", "here's my task list", "help me schedule these", "what should I do first", or any daily/to-do list the user wants organized into priorities and a time-blocked schedule. Also trigger for recurring daily planning requests — this skill is meant to be reused every day with a fresh list.
---

# Daily Task Planner

Turn a raw list of tasks into a prioritized, realistic, time-blocked plan for the day — using only what the user actually provides.

## Core Rule: Never Invent Information

Only use deadlines, durations, importance, or available time that the user explicitly states or that can be reasonably inferred from their exact wording (e.g., "urgent", "due today", "quick call" implies short). If something is missing:
- Do NOT make up a deadline, duration, or importance level.
- Make a clearly-labeled reasonable assumption (e.g., "assuming ~30 min, not specified") OR ask one short clarifying question if too much is missing to proceed at all.
- Never present an assumption as a fact the user gave you.

## Inputs to Look For

From the user's message, extract for each task (whatever is available):
1. **Task description**
2. **Deadline** (today, specific time, "this week", none given)
3. **Estimated effort/duration** (if not given, estimate reasonably based on the task type and flag it as an estimate)
4. **Stated urgency/importance** (explicit words like "urgent", "important", "can wait", "low priority")
5. **Available time for the day** (total hours/window user has to work with — if not given, ask once, or default to a standard 8-hour workday and say so)

## Step-by-Step Process

### 1. Classify each task into High / Medium / Low priority

Use a simple urgency × importance lens (like Eisenhower's matrix, but don't name-drop the framework unless asked):

- **High**: Urgent AND important — hard deadline today, blocks other work, or explicitly flagged critical/urgent by the user.
- **Medium**: Important but not urgent, or urgent but lower-stakes — should happen today if time allows, but the day doesn't break if it slips slightly.
- **Low**: Neither urgent nor important right now — nice to do, no real deadline, easily deferred.

Give a **one-line reason** for every task's priority (e.g., "High — due 2pm today and blocks the client call").

### 2. Estimate time for each task

- Use the user's stated estimate if given.
- Otherwise give a realistic, short estimate based on the task type, and mark it as an estimate (e.g., "~20 min (estimated)").
- Keep estimates simple: round to 15/30/60-minute blocks.

### 3. Build a realistic schedule

- Total up estimated time and compare to available time.
- Order tasks: High priority first (earliest, when energy/focus is typically best), then Medium, then Low if room remains.
- Add short buffers between tasks (e.g., 10–15 min) instead of back-to-back blocks — do not pack the schedule edge to edge.
- **If total task time exceeds available time**: do not shrink estimates to force a fit. Instead, move the lowest-priority (or least time-sensitive) tasks to "Later / Another Day" and say so explicitly, with a short reason.

### 4. Suggest what to do first

Call out the single best starting task and a one-line reason (usually the highest-priority, most time-sensitive item, or a quick win if several High items tie and one is very short).

### 5. Provide a final checklist

A simple, flat checklist (in schedule order) the user can tick off through the day. Keep it short and practical — no extra commentary in the checklist itself.

## Output Format

Always respond in this structure (plain text/markdown, no unnecessary preamble):

```
## Today's Plan

**Priorities**
- 🔴 High: [task] — [time estimate] — [one-line reason]
- 🟡 Medium: [task] — [time estimate] — [one-line reason]
- 🟢 Low: [task] — [time estimate] — [one-line reason]

**Suggested Schedule** (based on [X hours] available)
[Time block] — [Task] [(estimated) if not user-given]
[Time block] — [Task]
...

**Do This First:** [task] — [why]

**Move to Another Day (if any):**
- [task] — [why it's deferred]

**Checklist**
☐ [task]
☐ [task]
...
```

Keep the whole response compact — this is a quick daily-use tool, not a report. Avoid long explanations; the reasons should be one short line each.

## Handling Edge Cases

- **No available time given**: Ask once, or default to a standard workday (e.g., 9am–5pm minus a lunch break) and clearly label it as a default assumption.
- **No deadlines given at all**: Prioritize based on stated importance and task nature only; say priorities are based on importance since no deadlines were given.
- **Everything fits easily**: Still show the full structure, just skip the "Move to Another Day" section (or note "Nothing needs to move — it all fits").
- **User gives a new list tomorrow**: Treat each request independently — don't assume yesterday's leftover tasks carry over unless the user mentions them again.
