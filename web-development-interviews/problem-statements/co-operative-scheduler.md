---
description: A custom co-operative scheduler
---

# Co-operative Scheduler

#### Difficulty: Medium

Implement a Co-operative scheduler for defined co-operative tasks.

Entities:

* Co-operative Task: A task that finishes or halts within the provided time. It can return its current state when halting and can start again from that point.
* Co-operative Scheduler: This receives tasks and schedules it based on priority and whenever the javascript engine is free. It should not affect refresh rate or interactions on the browser.
