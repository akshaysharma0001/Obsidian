---
daily_target_hrs: 2
playback_speed: 1.5
target_deadline: 2027-06-30
tags:
  - webdev
  - tracker
---


# 🌐 Full-Stack Web Development Mastery Tracker
> "Ship real things, not just watched hours." Track your lectures, projects, and pacing here.

> [!tip] Controls & Legend
> - Tweak `daily_target_hrs`, `playback_speed`, and `target_deadline` in **Properties** above to recalculate your ETA and required daily pace live.
> - Tags: `#project` marks end-to-end build sessions · `#offline` marks recorded/warmup lectures.


```dataviewjs
const page = dv.current();
const tasks = page.file.tasks;
const dailyTarget = Number(page.daily_target_hrs) > 0 ? Number(page.daily_target_hrs) : 2;
const speed = Number(page.playback_speed) > 0 ? Number(page.playback_speed) : 1;

const tFile = app.vault.getAbstractFileByPath(page.file.path);
const cache = tFile ? app.metadataCache.getFileCache(tFile) : null;
const h2Headings = (cache && cache.headings) ? cache.headings.filter(h => h.level === 2) : [];

function getPhase(task) {
    let matched = null;
    for (const h of h2Headings) {
        if (h.position.start.line < task.line) {
            matched = h.heading;
        } else {
            break;
        }
    }
    return matched || (task.section && task.section.subpath) || "General";
}

let totalHrs = 0;
let completedHrs = 0;
let totalCount = 0;
let completedCount = 0;
let projectTotal = 0;
let projectDone = 0;
let nextTask = null;
const phases = new Map();
const completedList = [];

for (const t of tasks) {
    if (!t.duration) continue;
    const hrs = (t.duration && typeof t.duration.as === "function") ? t.duration.as("hours") : 0;
    const phaseName = getPhase(t);
    const isProject = Boolean(t.tags && t.tags.includes("#project"));
    const cleanTitle = t.text.split("⏳")[0].replace(/#\S+/g, "").trim();

    if (!phases.has(phaseName)) {
        phases.set(phaseName, { totalHrs: 0, completedHrs: 0, total: 0, done: 0 });
    }
    const p = phases.get(phaseName);

    totalHrs += hrs;
    totalCount += 1;
    p.totalHrs += hrs;
    p.total += 1;
    if (isProject) projectTotal += 1;

    if (t.completed) {
        completedHrs += hrs;
        completedCount += 1;
        p.completedHrs += hrs;
        p.done += 1;
        if (isProject) projectDone += 1;
        completedList.push(cleanTitle);
    } else if (!nextTask) {
        nextTask = {
            title: cleanTitle,
            week: (t.section && t.section.subpath) || phaseName,
            hrs: hrs
        };
    }
}

const remainHrs = Math.max(0, totalHrs - completedHrs);
const remainAtSpeed = remainHrs / speed;
const pct = totalHrs === 0 ? 0 : (completedHrs / totalHrs) * 100;
const daysLeft = Math.ceil(remainAtSpeed / dailyTarget);
const eta = remainHrs === 0
    ? "🎉 Completed!"
    : dv.date("today").plus({ days: daysLeft }).toFormat("dd MMM yyyy");

let deadlineNote = "";
if (page.target_deadline && remainHrs > 0) {
    const dl = page.target_deadline.ts ? page.target_deadline : dv.date(String(page.target_deadline));
    if (dl) {
        const daysToDl = Math.max(1, Math.ceil(dl.diff(dv.date("today"), "days").days));
        const reqPace = remainAtSpeed / daysToDl;
        deadlineNote = " · **🗓️ Deadline (" + dl.toFormat("dd MMM yyyy") + "):** Need <code>" + reqPace.toFixed(2) + " hrs/day</code> at " + speed + "x (" + daysToDl + " days left)";
    }
}

dv.header(2, "📊 Progress Dashboard");
dv.paragraph(
    '<div style="width:100%; background:var(--background-modifier-border); border-radius:8px; height:22px; overflow:hidden; margin-bottom:10px;">' +
    '<div style="width:' + pct.toFixed(2) + '%; background:var(--interactive-accent); height:100%; transition:width 0.3s ease; display:flex; align-items:center; justify-content:flex-end; padding-right:8px; color:var(--text-on-accent); font-size:12px; font-weight:600; min-width:44px;">' +
    pct.toFixed(1) + '%' +
    '</div></div>'
);

if (nextTask) {
    dv.paragraph(
        "**▶️ Up Next:** " + nextTask.title +
        " *(" + nextTask.hrs.toFixed(2) + "h raw / " + (nextTask.hrs / speed).toFixed(2) + "h at " + speed + "x)* — <code>" + nextTask.week + "</code>"
    );
}
dv.paragraph("**🛠️ Project Sessions:** " + projectDone + " / " + projectTotal + " completed" + deadlineNote);

dv.table(
    ["Metric", "Videos", "Raw Time (1x)", "At " + speed + "x Speed", "Share / ETA"],
    [
        ["🟢 Completed", completedCount + " / " + totalCount, completedHrs.toFixed(2) + " hrs", (completedHrs / speed).toFixed(2) + " hrs", pct.toFixed(2) + "%"],
        ["🟡 Remaining", (totalCount - completedCount) + " / " + totalCount, remainHrs.toFixed(2) + " hrs", remainAtSpeed.toFixed(2) + " hrs", (100 - pct).toFixed(2) + "%"],
        ["🔵 Total / ETA", totalCount + " total", totalHrs.toFixed(2) + " hrs", "~" + daysLeft + " days (" + dailyTarget + "h/day)", "🎯 " + eta]
    ]
);

dv.header(3, "🗂️ Phase Breakdown");
const phaseRows = [];
for (const [name, p] of phases.entries()) {
    const pPct = p.totalHrs === 0 ? 0 : (p.completedHrs / p.totalHrs) * 100;
    const status = p.done === p.total ? "✅" : (p.done > 0 ? "🔄" : "⬜");
    const filled = Math.round(pPct / 10);
    const bar = "█".repeat(filled) + "░".repeat(10 - filled);
    phaseRows.push([
        status + " " + name,
        p.done + "/" + p.total,
        p.completedHrs.toFixed(1) + " / " + p.totalHrs.toFixed(1) + " hrs",
        "<code>" + bar + "</code> " + pPct.toFixed(0) + "%"
    ]);
}
dv.table(["Phase", "Done", "Hours", "Progress"], phaseRows);

if (completedList.length > 0) {
    const itemsHtml = completedList.map(function(title) {
        return "<li>" + title + "</li>";
    }).join("");
    dv.paragraph(
        "<details><summary>✅ <strong>Completed Videos (" + completedList.length + ")</strong></summary><ul>" +
        itemsHtml +
        "</ul></details>"
    );
}
```

---

## General
- [x] How to Join Discord ⏳ [duration:: 4m 52s]

## Phase 1: Foundations, Async JS & DOM
### Week 1 - Foundations
- [x] 1.1 - Web Dev + DevOps Orientation ⏳ [duration:: 2h 2m 46s]
- [x] 1.2 | JavaScript Basics ⏳ [duration:: 1h 55m 29s]

### Cohort 2 Warmup Videos
- [x] Introduction, Setting up IDE ⏳ [duration:: 15m 5s] #offline
- [x] HTML Basics (Tags and Attributes) ⏳ [duration:: 33m 47s] #offline
- [x] CSS Basics ⏳ [duration:: 59m 41s] #offline

### Week 2 - Async JS & Offline Material
- [x] 2.1 (Web Dev) | Async JS ⏳ [duration:: 2h 27m 35s]
- [x] 2.2 (Web Dev) | Promises ⏳ [duration:: 2h 50m 27s]
- [x] 2.3 | Bash and Terminals (Basics) ⏳ [duration:: 34m 34s] #offline
- [x] 2.4 | Bash Advance (Laisha) ⏳ [duration:: 56m 5s] #offline
- [x] 2.5 | Installing Node.js, How to Solve an Assignment ⏳ [duration:: 45m 3s] #offline
- [x] 2.6 | Solving VSCode Assignment ⏳ [duration:: 34m 26s] #offline
- [x] 2.7 | Callback Hell, Rejects and async/await ⏳ [duration:: 37m 5s] #offline

### Week 3 - DOM
- [x] 3.1 | DOM (Simple) ⏳ [duration:: 2h 22m 47s]
- [x] 3.2 | DOM (Advance) ⏳ [duration:: 2h 20m 9s]

## Phase 2: Backend, Auth & MongoDB
### Week 4 - Node.js, Express & Offline Material
- [x] 4.1 | Node.js, Bun and JS Runtimes ⏳ [duration:: 2h 22m 48s]
- [x] 4.2 | HTTP Servers (Express) ⏳ [duration:: 2h 30m 22s]
- [x] Express and HTTP Server | Postman ⏳ [duration:: 1h 18m 44s] #offline
- [x] Middleware ⏳ [duration:: 47m 6s] #offline

### Week 5 - HTTP Fundamentals & Offline Material
- [x] 5.1 | Headers, Query Parameters ⏳ [duration:: 2h 18m 29s]
- [x] 5.2 | Middlewares and CORS ⏳ [duration:: 2h 4m 33s]
- [x] Git and GitHub ⏳ [duration:: 1h 26m 44s] #offline
- [x] Map, Filter and Arrow Fns ⏳ [duration:: 19m 13s] #offline
- [x] Axios vs Fetch ⏳ [duration:: 21m 42s] #offline

### Week 6 - Authentication & Offline Material
- [x] 6.1 | HTTP Deep Dive and Auth in Node.js ⏳ [duration:: 2h 44m 54s]
- [x] 6.2 | Auth and Connecting FE to BE ⏳ [duration:: 2h 24m 30s]
- [x] JWT and Auth Recap ⏳ [duration:: 40m 16s] #offline
- [x] Mongo Installation ⏳ [duration:: 6m 43s] #offline

### Week 7 - MongoDB and Validation
- [x] 7.1 | MongoDB ⏳ [duration:: 2h 7m 43s]
- [x] 7.2 | Passwords, Zod ⏳ [duration:: 2h 7m 51s]

### Week 8 - Backend Project & Offline Material
- [x] 8.1 | Backend of Course Selling App ⏳ [duration:: 2h 14m 29s] #project
- [x] 8.2 | Backend of Course Selling App - Part 2 ⏳ [duration:: 2h 16m 17s] #project
- [x] Mongo Deep Dive ⏳ [duration:: 2h 9m 56s] #offline

## Phase 3: React, UI/UX & Tailwind
### Week 9 - React Basics
- [x] 9.1 | React Basics ⏳ [duration:: 2h 2m 45s]
- [x] 9.2 | React useState ⏳ [duration:: 2h 14m 30s]
- [x] Week 9.3 - React from Basics Part-1 (Offline Video) ⏳ [duration:: 2h 24m 35s] #offline
- [x] Week 9.4 - React from Basics Part-2 (Offline Video) ⏳ [duration:: 55m 16s] #offline

### Week 10 - React Advanced
- [x] 10.1 | React Part 2 (SPAs, Routing) ⏳ [duration:: 1h 56m 5s]
- [x] 10.2 | React Part 3 (Context API, Rolling up the State) ⏳ [duration:: 1h 34m 15s]

### Week 11 - Hooks and State Management
- [x] 11.1 | Custom Hooks, useDebounce, useFetch ⏳ [duration:: 2h 15m 3s]
- [x] 11.2 | State Management using Recoil ⏳ [duration:: 2h 21m 48s]
- [x] 11.3 | Offline | Recoil Deep Dive ⏳ [duration:: 1h 33m 36s] #offline
- [x] useEffect, useMemo, useCallback | Cohort 2 ⏳ [duration:: 2h 33m 3s] #offline

### Week 12 - UI/UX
- [x] Week 12.1 | UI/UX Primitives by Keshav - Part 1 ⏳ [duration:: 2h 29m 1s]
- [x] Week 12.2 | UI/UX Primitives by Keshav - Part 2 ⏳ [duration:: 2h 17m 27s]

### Week 13 - Tailwind
- [x] Week 13.1 | Tailwind, Ref Arrays and Building Components ⏳ [duration:: 2h 12m 13s]
- [x] Week 13.2 | Tailwind Part 2, Creating Sidebars ⏳ [duration:: 2h 3m 56s]

## Phase 4: TypeScript & WebSockets
### Week 14 - TypeScript Fundamentals
- [ ] Week 14.1 | TypeScript Part 1 ⏳ [duration:: 2h 24m 14s]
- [ ] Week 14.2 | Types, Interfaces and Implementing Interfaces ⏳ [duration:: 2h 31m 13s]
- [ ] Week 14.3 | TypeScript Advance APIs ⏳ [duration:: 41m 54s]

### Week 15 - TypeScript Projects
- [ ] Week 15.1 | End to End App in TypeScript - Building a Second Brain App ⏳ [duration:: 2h 55s] #project
- [ ] Week 15.2 | Creating a UI Library, Button Component ⏳ [duration:: 2h 2m 57s] #project
- [ ] Week 15.3 | Brainly End to End ⏳ [duration:: 3h 21m 33s] #project

### Week 16 - WebSockets
- [ ] Week 16.1 | WebSockets ⏳ [duration:: 2h 4m 18s]
- [ ] Week 16.2 | WebSockets Project - Chat App ⏳ [duration:: 2h 10m 41s] #project

## Phase 5: Databases, Prisma & Next.js
### Week 17 - PostgreSQL
- [ ] Week 17.1 | Postgres and SQL Databases ⏳ [duration:: 2h 29m 55s]
- [ ] Week 17.2 | Postgres and SQL Databases - Part 2 ⏳ [duration:: 1h 59m 40s]

### Week 18 - Prisma and Next.js
- [ ] 18.1 | Prisma and ORMs ⏳ [duration:: 2h 1m 58s]
- [ ] 18.2 | Starting Next.js, Introducing SSR ⏳ [duration:: 2h 1m 38s]

### Week 19 - Next.js Continuation
- [ ] Week 19.1 | Next.js Continuation ⏳ [duration:: 2h 24m 27s]

### Week 20 - Next.js & NextAuth
- [ ] Week 20.1 | Next.js Continued ⏳ [duration:: 2h 16m 9s]
- [ ] Week 20.2 | NextAuth ⏳ [duration:: 2h 15m 3s]

## Phase 6: Monorepos, PayTM & Excalidraw
### Week 21 - Monorepos & PayTM
- [ ] Week 21.1 | Monorepos and Turborepo ⏳ [duration:: 2h 8m 55s]
- [ ] Week 21.2 | Monorepos Continued (Better Quality) ⏳ [duration:: 2h 3m 25s]
- [ ] 21.3 | CSR vs SSR vs Static Site Generation ⏳ [duration:: 38m 46s]
- [ ] 21.4 | Building PayTM Project ⏳ [duration:: 2h 5m 53s] #project
- [ ] 21.5 | PayTM Frontend ⏳ [duration:: 1h 4m 22s] #project

### Week 22 - Excalidraw Project (Part 1–2)
- [ ] Week 22.1 | End to End - Project #1 - Excalidraw ⏳ [duration:: 2h 20m 13s] #project
- [ ] Week 22.2 | End to End - Project #1 - Excalidraw Part-2 ⏳ [duration:: 2h 59m 40s] #project

### Week 23 - Excalidraw Project (Part 3–4)
- [ ] Week 23.1 | Excalidraw Part 3 ⏳ [duration:: 2h 38m 28s] #project
- [ ] Week 23.2 | Excalidraw Part 4 ⏳ [duration:: 2h 17m 57s] #project

## Phase 7: DevOps Bridge (Weeks 24–38)
> [!note] Separate Curriculum
> Weeks 24–38 cover DevOps and live in the DevOps folder. Link your DevOps note here (e.g., `[[DevOps Tracker]]`) once created.
- [ ] Complete Weeks 24–38 DevOps Curriculum

## Phase 8: BetterStack Project (Rust & Redis)
### Week 39
- [ ] Week 39 | Project Class - Creating BetterStack (Part 1) | 31/05 ⏳ [duration:: 2h 8m 22s] #project

### Week 40
- [ ] Week 40 | Creating BetterStack in Rust (Part 2) ⏳ [duration:: 3h 5m 4s] #project

### Week 41
- [ ] Week 41.1 | Project Review Class ⏳ [duration:: 2h 12m 38s] #project
- [ ] Week 41.2 | BetterStack (Part 3) (Offline Video) ⏳ [duration:: 1h 58m 9s] #project #offline

### Week 42
- [ ] Week 42.1 | Async Backend Communication and Redis Streams | BetterStack (Part 4) (Offline Video) ⏳ [duration:: 1h 32m 24s] #project #offline

### Week 43
- [ ] Week 43.1 | Adding Worker and Pusher using Redis Streams in JavaScript (Offline Video) ⏳ [duration:: 55m 51s] #project #offline
- [ ] Week 43.2 | Finishing the Frontend (Offline Video) ⏳ [duration:: 53m] #project #offline

## Phase 9: Scaling, Security & System Design
### Week 44
- [ ] Week 44.1 | Scaling HTTP Servers and WS Servers ⏳ [duration:: 1h 51m 10s]

### Week 45
- [ ] Week 45.1 | OpenAPI Spec, Autogenerated Clients, Rate Limiting, Captcha, DDoS Protection ⏳ [duration:: 1h 33m 15s]

### Week 46
- [ ] Week 46.1 | Performance Benchmarks - Rust vs Go vs JavaScript (Offline Video) ⏳ [duration:: 39m 26s] #offline

### Week 47
- [ ] Week 47.1 | Rate Limiting, DDoS and Captcha ⏳ [duration:: 1h 40m 1s]

### Week 48
- [ ] Week 48.1 | Deploy Anything with Coolify (Offline Video) ⏳ [duration:: 22m 53s] #offline

### Week 49
- [ ] Week 49.1 | Building CodeForces ⏳ [duration:: 2h 19m 45s] #project

### Week 50
- [ ] Week 50.1 | Continuing the CodeForces Project ⏳ [duration:: 1h 26m 12s] #project

### Week 51
- [ ] Week 51.1 | Sharding, Replication and System Design (Offline Video - Super 30) ⏳ [duration:: 1h 45m 37s] #offline

### Bonus / Offline
- [ ] Building Replit (Offline) ⏳ [duration:: 3h 22m 41s] #project #offline