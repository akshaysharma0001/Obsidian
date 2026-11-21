---
daily_target_hrs: 2
playback_speed: 1.5
target_deadline: 2027-06-30
exclude_shared: false
tags:
  - devops
  - tracker
---

# ⚙️ DevOps Cohort Mastery Tracker
> "Automate everything you'd hate to debug at 3 AM." Track your infra, containers, K8s, and projects here.

> [!tip] Controls & Legend
> - Tweak `daily_target_hrs`, `playback_speed`, and `target_deadline` in **Properties** above to update your ETA live.
> - Set `exclude_shared: true` in **Properties** to hide `#shared` lectures already tracked in your Web Dev note (shows pure Weeks 24–38 DevOps hours).
> - Tags: `#project` = build/deployment streams · `#offline` = recorded/Cohort 2 videos · `#shared` = overlaps with Web Dev tracker.

```dataviewjs
const page = dv.current();
const tasks = page.file.tasks;
const dailyTarget = Number(page.daily_target_hrs) > 0 ? Number(page.daily_target_hrs) : 2;
const speed = Number(page.playback_speed) > 0 ? Number(page.playback_speed) : 1;
const excludeShared = Boolean(page.exclude_shared);

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
let skippedShared = 0;
let nextTask = null;
const phases = new Map();
const completedList = [];

for (const t of tasks) {
    if (!t.duration) continue;
    const isShared = Boolean(t.tags && t.tags.includes("#shared"));
    if (excludeShared && isShared) {
        skippedShared += 1;
        continue;
    }

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

const modeLabel = excludeShared ? " — DevOps-Only Mode (" + skippedShared + " shared hidden)" : "";
dv.header(2, "📊 Progress Dashboard" + modeLabel);
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

## General & Shell Foundations
### General
- [x] How to Join Discord ⏳ [duration:: 4m 52s] #shared

### Getting Started & Bash (Extras)
- [x] Bash and Terminal ⏳ [duration:: 34m 34s] #offline #shared
- [x] Bash and Terminal (Advanced) ⏳ [duration:: 56m 5s] #offline #shared

## Phase 1: Linux VMs, Proxies & CI/CD
### Week 24 - Virtual Machines & Reverse Proxies
- [ ] Week 24.1 | Starting DevOps, VMs ⏳ [duration:: 2h 31m 26s]
- [ ] Week 24.2 | Reverse Proxies, Process Management and Deployment ⏳ [duration:: 1h 53m 49s]

### Week 25 - Process Management, CI/CD & Certificates
- [ ] Week 25 | Process Management and CI/CD (01/02) ⏳ [duration:: 2h 2m 9s]
- [ ] Week 25 | Continuing CI, Certificate Management and Testing (02/02) ⏳ [duration:: 3h 30m 45s]
- [ ] Certificate Management | Cohort 2 ⏳ [duration:: 54m] #offline

## Phase 2: Docker & Containerization
### Week 26 - Docker
- [ ] Week 26.1 | Starting Docker ⏳ [duration:: 2h 15m 13s]
- [ ] Week 26.2 | Docker Part-2 ⏳ [duration:: 2h 16m 34s]

### Week 27 - Docker Compose & VM Deployments
- [ ] Week 27.1 | Docker Compose, CI/CD with Docker ⏳ [duration:: 2h 40m 8s]
- [ ] Week 27.2 | Implementation Stream - Deploying a Monorepo using Docker to VMs ⏳ [duration:: 2h 35m 17s] #project

## Phase 3: AWS Scaling & Container Orchestration
### Week 28 - Scaling & Autoscaling Groups
- [ ] Week 28.1 | Vertical and Horizontal Scaling, ASGs ⏳ [duration:: 2h 11m 23s]
- [ ] Week 28.2 | Autoscaling Groups ⏳ [duration:: 2h 27m 35s]

### Week 29 - Project: Bolt Autoscaler
- [ ] Week 29.1 | Building an Autoscaling Orchestrator for Bolt - Project Class ⏳ [duration:: 2h 32m 7s] #project

### Week 30 - AWS ECS, ECR & Monitoring
- [ ] Week 30.1 | AWS ECR, ECS and Container Orchestration ⏳ [duration:: 2h 29m 50s]
- [ ] Week 30.2 | Monitoring and New Relic ⏳ [duration:: 2h 5m 18s]

## Phase 4: Observability (Prometheus & Grafana)
### Weeks 31–32 - Prometheus & Grafana
- [ ] Week 31.1 | Monitoring, Prometheus and Grafana ⏳ [duration:: 2h 7m 53s]
- [ ] Week 32.1 | Prometheus and Grafana Part-2 ⏳ [duration:: 2h 14m 11s]

## Phase 5: Serverless, Kubernetes & GitOps
### Weeks 33–34 - Serverless & Kubernetes Basics
- [ ] Week 33.1 | Serverless ⏳ [duration:: 2h 11m 4s]
- [ ] Week 33.2 | Kubernetes - Architecture, Control Plane, Worker Nodes, Pods, Kinds ⏳ [duration:: 1h 48m 48s]
- [ ] Week 34.1 | Kubernetes Part-2 ⏳ [duration:: 2h 15m 11s]
- [ ] Week 34.2 | Kubernetes Part-3 ⏳ [duration:: 1h 56m 29s]

### Weeks 35–36 - Kubernetes Ingress, ConfigMaps & Storage
- [ ] Week 35.1 | Namespaces, ClusterIP and Ingress ⏳ [duration:: 2h 16m 16s]
- [ ] Week 36.1 | Secrets, ConfigMaps and Ingress ⏳ [duration:: 1h 53m]
- [ ] Week 36.2 | Secrets and ConfigMaps Part-2 ⏳ [duration:: 2h 16s]
- [ ] Kubernetes Part 5 (Volumes, PV, PVCs) | Cohort 2 Video ⏳ [duration:: 2h 15m 54s] #offline

### Weeks 37–38 - Autoscaling, ArgoCD & Helm
- [ ] Week 37.1 | Horizontal Pod Autoscaler, Node Autoscaler (03/04) ⏳ [duration:: 2h 6m 23s]
- [ ] Week 37.2 | ArgoCD and GitOps (04/05) ⏳ [duration:: 2h 13m 22s]
- [ ] Week 38 | Helm (10/05) ⏳ [duration:: 1h 17m 33s]

## Phase 6: BetterStack Project (Shared with Web Dev)
### Weeks 39–43 - BetterStack Project
- [ ] Week 39 | Project Class - Creating BetterStack (31/05) ⏳ [duration:: 2h 8m 22s] #project #shared
- [ ] Week 40.1 | Creating BetterStack in Rust ⏳ [duration:: 3h 5m 4s] #project #shared
- [ ] Week 41.1 | Project Review Class ⏳ [duration:: 2h 12m 38s] #project #shared
- [ ] Week 41.2 | BetterStack Part 3 (Offline Video) ⏳ [duration:: 1h 58m 9s] #project #offline #shared
- [ ] Week 42.1 | Async Backend Communication and Redis Streams | BetterStack Part 4 (Offline Video) ⏳ [duration:: 1h 32m 24s] #project #offline #shared
- [ ] Week 43.1 | Adding Worker and Pusher using Redis Streams in JavaScript (Offline Video) ⏳ [duration:: 55m 51s] #project #offline #shared
- [ ] Week 43.2 | Finishing the Frontend (Offline Video) ⏳ [duration:: 53m] #project #offline #shared

## Phase 7: Scaling, Security & Deployment (Shared with Web Dev)
### Weeks 44–48 - Scaling, Security & Deployment
- [ ] Week 44 | Scaling HTTP Servers and WS Servers ⏳ [duration:: 1h 51m 10s] #shared
- [ ] Week 45.1 | OpenAPI Spec, Autogenerated Clients, Rate Limiting, Captcha, DDoS Protection ⏳ [duration:: 1h 33m 15s] #shared
- [ ] Week 46.1 | Performance Benchmarks - Rust vs Go vs JavaScript (Offline Video) ⏳ [duration:: 39m 26s] #offline #shared
- [ ] Week 47.1 | Rate Limiting, DDoS and Captcha ⏳ [duration:: 1h 40m 1s] #shared
- [ ] Week 48.1 | Deploy Anything with Coolify (Offline Video) ⏳ [duration:: 22m 53s] #offline #shared