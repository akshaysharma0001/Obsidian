---
daily_target_hrs: 2
playback_speed: 1.5
tags:
  - dsa
  - tracker
---

# 🚀 The Ultimate DSA Mastery Tracker
> "Consistency is more important than intensity." Track your journey here.

> [!tip] Quick Resources & Controls
> - **Master Hub:** [DSA IMP Links (Notion)](https://app.notion.com/p/DSA-Resources-85ebc94496e282c797dc01a596bd6c62?assetsVersion=23.13.20260815.0402&clientBuildTarget=client)
> - Adjust `daily_target_hrs` and `playback_speed` in **Properties** above to update your ETA and adjusted watch time live.

```dataviewjs
const page = dv.current();
const tasks = page.file.tasks;
const dailyTarget = Number(page.daily_target_hrs) > 0 ? Number(page.daily_target_hrs) : 2;
const speed = Number(page.playback_speed) > 0 ? Number(page.playback_speed) : 1;

let totalHrs = 0, completedHrs = 0;
let totalCount = 0, completedCount = 0;
let nextTask = null;
const phases = new Map();

for (const t of tasks) {
    if (!t.duration) continue;
    const hrs = t.duration.as ? t.duration.as("hours") : 0;
    const phaseName = t.section?.subpath || "Uncategorized";

    if (!phases.has(phaseName)) {
        phases.set(phaseName, { totalHrs: 0, completedHrs: 0, total: 0, done: 0 });
    }
    const p = phases.get(phaseName);

    totalHrs += hrs;
    totalCount++;
    p.totalHrs += hrs;
    p.total++;

    if (t.completed) {
        completedHrs += hrs;
        completedCount++;
        p.completedHrs += hrs;
        p.done++;
    } else if (!nextTask) {
        nextTask = {
            title: t.text.split("⏳")[0].trim(),
            phase: phaseName,
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

// Visual progress bar
dv.header(2, "📊 Progress Dashboard");
dv.paragraph(`
<div style="width:100%; background:var(--background-modifier-border); border-radius:8px; height:22px; overflow:hidden; margin-bottom:10px;">
  <div style="width:${pct.toFixed(2)}%; background:var(--interactive-accent); height:100%; transition:width 0.3s ease; display:flex; align-items:center; justify-content:flex-end; padding-right:8px; color:var(--text-on-accent); font-size:12px; font-weight:600; min-width:44px;">
    ${pct.toFixed(1)}%
  </div>
</div>
`);

// Up Next callout
if (nextTask) {
    dv.paragraph(`**▶️ Up Next:** ${nextTask.title} *(${nextTask.hrs.toFixed(2)}h raw / ${(nextTask.hrs / speed).toFixed(2)}h at ${speed}x)* — \`${nextTask.phase}\``);
}

// Overall metrics table
dv.table(["Metric", "Lectures", "Raw Time (1x)", `At ${speed}x Speed`, "Share"], [
    ["🟢 Completed", `${completedCount} / ${totalCount}`, `${completedHrs.toFixed(2)} hrs`, `${(completedHrs / speed).toFixed(2)} hrs`, `${pct.toFixed(2)}%`],
    ["🟡 Remaining", `${totalCount - completedCount} / ${totalCount}`, `${remainHrs.toFixed(2)} hrs`, `${remainAtSpeed.toFixed(2)} hrs`, `${(100 - pct).toFixed(2)}%`],
    ["🔵 Total / ETA", `${totalCount} total`, `${totalHrs.toFixed(2)} hrs`, `~${daysLeft} days (${dailyTarget}h/day)`, `🎯 ${eta}`]
]);

// Per-phase breakdown table
dv.header(3, "🗂️ Phase Breakdown");
const phaseRows = [];
for (const [name, p] of phases.entries()) {
    const pPct = p.totalHrs === 0 ? 0 : (p.completedHrs / p.totalHrs) * 100;
    const status = p.done === p.total ? "✅" : (p.done > 0 ? "🔄" : "⬜");
    const filled = Math.round(pPct / 10);
    const bar = "█".repeat(filled) + "░".repeat(10 - filled);
    phaseRows.push([
        `${status} ${name}`,
        `${p.done}/${p.total}`,
        `${p.completedHrs.toFixed(1)} / ${p.totalHrs.toFixed(1)} hrs`,
        `\`${bar}\` ${pPct.toFixed(0)}%`
    ]);
}
dv.table(["Phase", "Done", "Hours", "Progress"], phaseRows);
```

---

## Phase 1: Basics & Orientation
- [x] Week 0 - DSA Orientation ⏳ [duration:: 10m 52s] | 🎞️ [Slides](https://docs.google.com/presentation/d/1-FizQJexCumQZ97n3WhbOJuefIR_8sZg8gVKXZq_i3s/edit?usp=sharing)
- [x] Week 1 - DSA | Introduction to C++ ⏳ [duration:: 2h 27m 2s] | 🎞️ [Slides](https://www.canva.com/design/DAG-YfsCSVY/eI46CeFBrFctMzdmkza-_g/edit?utm_content=DAG-YfsCSVY&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton) · 📄 [Notes](https://drive.google.com/file/d/17bBGv7PW7XQ1gln1U7Fz72qxg5p7BvGU/view?usp=sharing)
- [x] Week 2 - DSA | Loops and Pattern Printing ⏳ [duration:: 2h 37m 56s] | 📄 [Loops](https://drive.google.com/file/d/1B6P_7udbFWjca8nYylWczk3kkCbdDeTb/view?usp=sharing) · 📄 [Pattern Printing](https://drive.google.com/file/d/1dBGJL5UnQDuoBQXH3TTMcoeUqHyjzZLg/view?usp=sharing) · 📁 [Slides Folder](https://drive.google.com/drive/folders/1y7LWzyY049cUxXcuQ6pY84R9TbpUdMQ9)
- [x] Week 2.1 - DSA | Pattern Printing Solutions (Offline) ⏳ [duration:: 1h 11m 49s]
- [ ] Week 3 - DSA | Functions ⏳ [duration:: 2h 47m 25s] | 📘 [C++ Important](https://www.notion.so/2f7bc94496e2806fa962c3812474962e) · 📘 [Variable Scoping](https://www.notion.so/2f7bc94496e28025af47daff41a4757c) · 📄 [Notes](https://drive.google.com/file/d/1U4HXi6_UHaWQCRdjB7XnIT7gqUKoIp_J/view?usp=drive_link)

## Phase 2: Arrays, Strings & Math
- [ ] Week 4 - DSA | Arrays ⏳ [duration:: 2h 31m 5s] | 📄 [Notes](https://drive.google.com/file/d/1SYfc1Sntvj5CzpOVq6WwharOfDR25jWj/view?usp=drive_link) · 🎨 [Excalidraw](https://excalidraw.com/#json=_Aw_txs7h1M674xpbQ1eJ,boVa521htnqOCcQidRgCTQ) · 📑 [PDF](https://drive.google.com/file/d/1QFNrSuXSL4NwrGH7MK6BMLPfkCaBd3zc/view?usp=drive_link)
- [ ] Week 5 - DSA | Strings / 2D Arrays ⏳ [duration:: 2h 54m 24s] | 📄 [Strings](https://drive.google.com/file/d/1wp9k_DFY7I8kbOxOYZrGwe7vb1aFS5FS/view?usp=drive_link) · 📄 [2D Arrays](https://drive.google.com/file/d/1uIkiznrSBXslE4_EaVkCqtH7-4n42PJP/view?usp=drive_link)
- [ ] Week 6 - DSA | Sorting & Searching Basics ⏳ [duration:: 2h 15m 33s] | 📄 [Notes](https://drive.google.com/file/d/1hFKjsOsd1XZE8V8JcPdcjsjsNxd29gEW/view) · 🎨 [Excalidraw](https://excalidraw.com/#json=gkSsTlWzo6THioEw0PDFL,-tlp-_Jp8icD1Xmsa09zgA)
- [ ] Week 7 - DSA | Mixed Problem Solving Session ⏳ [duration:: 1h 57m 3s] | 📄 [Notes](https://drive.google.com/file/d/1tbIU2x2EwUo9ONuYB7CpffxZVpLx_Ij9/view?usp=drive_link)
- [ ] Week 8 - DSA | Time and Space Complexity ⏳ [duration:: 2h 14m 5s] | 📄 [Notes](https://drive.google.com/file/d/1IPjPKU4OgPqbtmF26AJUzp5bX2kR156G/view)
- [ ] Week 9 - DSA | Array Techniques (Prefix Sums, Sliding Window, Contribution) ⏳ [duration:: 2h 31m 20s] | 📄 [Notes](https://drive.google.com/file/d/1mUmvVJlfocfKl3EQEDCV8tqQIj0E4kxb/view?usp=drive_link)
- [ ] Week 10 - DSA | Sets and Maps ⏳ [duration:: 1h 38m 43s] | 📄 [Notes 1](https://drive.google.com/file/d/14KlVgCexT_IESv9On2iT67QkmWT7osB_/view?usp=drive_link) · 📄 [Notes 2](https://drive.google.com/file/d/1umUzs-bzDoIZXFXy0nxHDxsv5cpjzF2z/view) · 📘 [Notion Doc](https://app.notion.com/p/Introduction-to-Sets-and-Maps-328bc94496e280c4962bfe7a4c07ed0b)
- [ ] Week 10.1 - DSA | Sets and Maps (Offline) ⏳ [duration:: 1h 3m 35s]
- [ ] Week 11 - DSA | Two Pointers / Sliding Window ⏳ [duration:: 2h 34m 59s] | 📄 [Notes](https://drive.google.com/file/d/19hAN8IyJQF-wwBLheadQxBXByQrQCbbh/view?usp=drive_link)
- [ ] Week 12 - DSA | Interview Problems ⏳ [duration:: 2h 23m 58s] | 📄 [Notes](https://drive.google.com/file/d/1-aWaWF79VEwEs2WhCcrm6eMLXQWUj19z/view?usp=drive_link)
- [ ] Week 13 - DSA | Binary Search ⏳ [duration:: 1h 18m 57s] | 📄 [Notes 1](https://drive.google.com/file/d/16u_oi9cFCpceDsjx3wusUnDVwlMJCAgn/view) · 📄 [Notes 2](https://drive.google.com/file/d/1XcCijiv6GtwGTFtVDLFg18ntztNrqXg4/view)
- [ ] Week 13.1 - DSA | Binary Search (Offline) ⏳ [duration:: 1h 26m 38s]
- [ ] Week 14 - DSA | Number Theory Basics ⏳ [duration:: 2h 11m 28s] | 📄 [Notes](https://drive.google.com/file/d/18AH7K2QcfT4qr79ET_RUOVcl7423jjCg/view?usp=drive_link) · 💻 [Assignment Solutions](https://gist.github.com/DhruvPasricha/952bd22d9f6b9d00d4bb7988a032b16a)
- [ ] Week 15 - DSA | Bit Manipulation ⏳ [duration:: 2h 24m 2s] | 📄 [Notes 1](https://drive.google.com/file/d/1bewzJyEWcRR_0fIcmdHRk8hEqI5j1iOJ/view?usp=drive_link) · 🎨 [Notes 2](https://excalidraw.com/#json=IMMm2U2djJQJ9M8wMqgKH,cIHJAuPVj99-Od3O5F6mzg)
- [ ] Week 15.1 - DSA | Bit Manipulation - Part 2 ⏳ [duration:: 2h 27m 41s] | 🎨 [Excalidraw](https://excalidraw.com/#json=mKrf2UxfoKgbOhifw7BdG,WrBHxfUaYaOO80Q9gY8Ocg)

## Phase 3: Recursion, Backtracking & Sorting
- [ ] Week 16 - DSA | Recursion ⏳ [duration:: 2h 8m 58s] | 🎞️ [Slides](https://drive.google.com/file/d/1jp8PJ_0iGNiECRwoOFK4pOj8uVigyujX/view?usp=drive_link) · 🎨 [Excalidraw](https://excalidraw.com/#json=5V79XJYz9RUN8MHb7oDrp,fAhVbmlUJa9mkjVUMo1SDg)
- [ ] Week 17 - DSA | Recursion - II ⏳ [duration:: 2h 17m 51s] | 🎞️ [Slides](https://www.canva.com/design/DAHI-ra29Os/rH-AnjYUik_vvqItJlxXEQ/edit) · 🎨 [Excalidraw](https://excalidraw.com/#json=wr_VvWnYZWiSZfMaGA9sE,dKHz_MrQgMcr3TEEvkjmAw)
- [ ] Week 18 - DSA | Backtracking - I ⏳ [duration:: 1h 56m 50s] | 🎨 [Excalidraw](https://excalidraw.com/#json=XxtEmvr908PiB0_yKxXAZ,IrQ5WNv4Ut0M0GHOA6pcnw) · 📘 [Theory Doc](https://app.notion.com/35ecf0b1c575800da66aea44e5c61109?pvs=25)
- [ ] Week 19.1 - DSA | Backtracking - II ⏳ [duration:: 2h 3m 36s] | 🎨 [Excalidraw](https://excalidraw.com/#json=4Goaksbcqv_-zh7pqrt4F,w6aJ2VN38JJmxm_vf7CFWg)
- [ ] Week 19.2 - DSA | Backtracking - III ⏳ [duration:: 2h 8m 21s] | 🎨 [Excalidraw](https://excalidraw.com/#json=V2nr0FMsg-sK-_hqOc503,aL4uiFtlKWv5PX7XIjpi-A)
- [ ] Week 20.1 - DSA | Sorting - I ⏳ [duration:: 2h 15m 33s] | 🎨 [Excalidraw](https://excalidraw.com/#json=KZd2k88buyIbMK8RvxqHo,vnweOkRmIvo0zSf84uX6eQ)
- [ ] Week 20.2 - DSA | Sorting - II ⏳ [duration:: 2h 28m 31s] | 🎨 [Excalidraw](https://excalidraw.com/#json=d3_3BXTZN-OXWXguSHMsG,LK3D5HEClxxdDfhkIv1p_w)
- [ ] Week 21 - DSA | Sorting - III ⏳ [duration:: 2h 28m 45s] | 🎨 [Excalidraw](https://excalidraw.com/#json=awANPSsHOQQe9KDc-vY-q,IulMZUty48vFHnD1t0XlUA)

## Phase 4: Linked Lists & Memory
- [ ] Week 22.1 - DSA | Pointers and Memory Management ⏳ [duration:: 2h 31m 39s] | 🎨 [Excalidraw](https://excalidraw.com/#json=Qrq1mXYO-y7K2ECZfnttF,aXru_Hko_uhUWhC2MWCdOQ)
- [ ] Week 22.2 - DSA | Pointers and Memory Management - II (Pt 1) ⏳ [duration:: 2h 13m 11s] | 🎨 [Excalidraw](https://excalidraw.com/#json=RyF4ThEg4-nCEM-jhxr9l,kxBoTJrEQjy_CXwpRH81AA)
- [ ] Week 22.2 - DSA | Pointers and Memory Management - II (Pt 2) ⏳ [duration:: 8m 9s] | 🎨 [Excalidraw](https://excalidraw.com/#json=RyF4ThEg4-nCEM-jhxr9l,kxBoTJrEQjy_CXwpRH81AA)
- [ ] Week 23.1 - DSA | Linked List - I ⏳ [duration:: 2h 28m 28s] | 🎨 [Excalidraw](https://excalidraw.com/#json=JCFjQL-lTDQqwFcL5FGXY,lXaWDSrBhLt9Qf8Z5xLZhw)
- [ ] Week 23.2 - DSA | Linked List - II ⏳ [duration:: 2h 20m 56s] | 🎨 [Excalidraw](https://excalidraw.com/#json=Zrbt2DzAkHXQoRE-hBLOl,Dya6btplMV51krvkK3PXWg)
- [ ] Week 24.1 - DSA | Linked List - III ⏳ [duration:: 2h 51m 7s] | 🎨 [Excalidraw](https://excalidraw.com/#json=RtR8dhTw6kBU9xjDp6LCT,i8RIo-SJo0DtgUkTSfwyKg)
- [ ] Week 24.2 - DSA | Linked List - IV ⏳ [duration:: 2h 30m 33s] | 🎨 [Excalidraw](https://excalidraw.com/#json=ACL4h6Fw5JWtMznclTYK9,9kOyqn6QNEjz9uvKC2XGyg)

## Phase 5: Stacks, Queues & Trees
- [ ] Week 25 - DSA | Stack & Queue - I ⏳ [duration:: 2h 9m 28s] | 🎨 [Excalidraw 1](https://excalidraw.com/#json=-D4EEagfDTtoPqMrn4jNy,R5AZFtUJMcFt_kmNl0jXMw) · 🎨 [Excalidraw 2](https://excalidraw.com/#json=p5IrBJerBZav_5iedLFgS,TjvRza27sHOSVxrQwIDehw)
- [ ] Week 26 - DSA | Stack & Queue - II ⏳ [duration:: 2h 43m 15s] | 🎨 [Excalidraw](https://excalidraw.com/#json=Op4JydeUrnMIVVVulIsUE,PIAW0S26FC_Gybs6iWKbeg)
- [ ] Week 26.2 - DSA | Stack & Queue - III ⏳ [duration:: 2h 9m 48s] | 🎨 [Excalidraw](https://excalidraw.com/#json=irUfl8QWnWsZPAcau7D4_,xHGeF7PndmJIsP31JT6vJA)
- [ ] Week 27.1 - DSA | Interview Problems ⏳ [duration:: 1h 49m 26s] | 🎨 [Excalidraw](https://excalidraw.com/#json=WexbvIis-MEDoCjLHGTls,pmRlBKLVmNIrHdp7Xw9AAA)
- [ ] Week 27.2 - DSA | Binary Trees - I ⏳ [duration:: 2h 16m 57s] | 🎨 [Excalidraw](https://excalidraw.com/#json=RY6lsLN4PA4wMgHCkoVG7,rRsMHSmbL7Kplp0CZiV7vQ)
- [ ] Week 28.1 - DSA | Binary Trees - II ⏳ [duration:: 2h 32m 8s] | 🎨 [Excalidraw](https://excalidraw.com/#json=-D4EEagfDTtoPqMrn4jNy,R5AZFtUJMcFt_kmNl0jXMw)
- [ ] Week 29 - DSA | Binary Search Trees ⏳ [duration:: 2h 46m 0s] | 🎨 [Excalidraw](https://excalidraw.com/#json=t7aWhFa5xnEAn-WGsLeCY,LiGY4BodqF-NpBENzRM6Dg)
- [ ] Week 31.1 - DSA | Binary Trees Misc Solving ⏳ [duration:: 2h 41m 41s] | 🎨 [Excalidraw](https://excalidraw.com/#json=xI6XE9YYOlL9ff8EyDJjL,Xdev0alyC-I6yBv29l5FRw)
- [ ] Week 31.2 - DSA | Binary Trees Misc Solving - II ⏳ [duration:: 2h 33m 19s] | 🎨 [Excalidraw](https://excalidraw.com/#json=Vcx-gC0MUXj1_JG9z2ZtW,cgSFLKOg7nwW4HrsP5u9vA)

## Phase 6: Heaps & Greedy
- [ ] Heap - I (Offline) ⏳ [duration:: 1h 34m 18s]
- [ ] Heap - II (Offline) ⏳ [duration:: 53m 6s]
- [ ] Heap - III (Offline) ⏳ [duration:: 2h 36m 25s]
- [ ] Week 33.1 - DSA | Heaps - Revision ⏳ [duration:: 2h 1m 46s] | 🎨 [Excalidraw](https://excalidraw.com/#json=2g-6xgfKjIFJdGdGxAQB8,qXfmF07OVeJR67--r2AdnA) · 📝 [Assignment](https://codeforces.com/group/Dggke54VzD/blog/entry/18021)
- [ ] Week 33.2 - DSA | Greedy ⏳ [duration:: 2h 28m 40s] | 🎨 [Excalidraw](https://excalidraw.com/#json=zG9v3_-k0_oAvgh1ED72s,0ljGjp7uWS5a94YwlNlcZA) · 🎞️ [Slides](https://www.canva.com/design/DAHTfouenxg/A_oWwi3i1zUNTtX_EfYqFg/edit)
- [ ] Week 34 - DSA | Greedy - II ⏳ [duration:: 2h 33m 40s] | 🎨 [Excalidraw](https://excalidraw.com/#json=StsRf3IuWhKMrYiUePqmF,AT9-1FXXwrlk7_ScW-laaA)

## Phase 7: Dynamic Programming
- [ ] DP Basics (Offline Video) ⏳ [duration:: 1h 42m 54s] | 🎨 [Excalidraw](https://excalidraw.com/#json=Kd6LwBkub0APDZVXFNhED,M6jFCcBbPpUG0FdFzcWTHQ) · 📝 [Assignment](https://codeforces.com/group/4vcXCPx8NY/contest/715177)
- [ ] DP: 0/1 Knapsack (Offline) ⏳ [duration:: 2h 16m 2s]
- [ ] DP: Unbounded Knapsack (Offline) ⏳ [duration:: 2h 7m 41s] | 🎨 [Excalidraw](https://excalidraw.com/#json=Kd6LwBkub0APDZVXFNhED,M6jFCcBbPpUG0FdFzcWTHQ) · 📝 [Assignment](https://codeforces.com/group/4vcXCPx8NY/contest/715177)
- [ ] Week 35 - DSA | Dynamic Programming - I ⏳ [duration:: 2h 6m 43s] | 🎨 [Excalidraw](https://excalidraw.com/#json=scYltRn-ifYJRZNmPegYT,LDVfs6ltH3zv0mizdcjglg)
- [ ] Week 36.1 - DSA | DP on Stocks ⏳ [duration:: 2h 13m 37s] | 🎨 [Excalidraw](https://excalidraw.com/#json=B2Rg7ybPOL2XfRdnNtOi5,rQliVZf08RFHR0gaBWdB3Q)
- [ ] Week 36.2 - DSA | Longest Common Subsequence ⏳ [duration:: 2h 20m 54s] | 🎨 [Excalidraw](https://excalidraw.com/#json=9M-dykxkXtJaE-53sxlAA,Zw2V2_wq_GDOkVpIaaKtUA)
- [ ] Week 37.1 - DSA | Dynamic Programming (Path Recovery) ⏳ [duration:: 2h 37m 41s] | 🎨 [Excalidraw](https://excalidraw.com/#json=A_NlGtYv9frfTKKXBgb-G,m3yCSjaePwUhBk9_vcyK0w)
- [ ] Week 38.1 - DSA | Doubt Session ⏳ [duration:: 1h 55m 26s] | 🎨 [Excalidraw](https://excalidraw.com/#json=T3xMIunF7bBSrWhOuw4O6,IwrDkD-F3GYF4hk9YPeTQg)
- [ ] Week 38.2 - DSA | DP (Tabulation and Problem Solving) ⏳ [duration:: 2h 15m 35s] | 🎞️ [Slides](https://www.canva.com/design/DAHTTuqAJ4U/LSU7PLsqNDRu74chsLrr6Q/edit)
- [ ] LIS - I (Offline Video) ⏳ [duration:: 1h 2m 19s] | 🎨 [Class Notes](https://excalidraw.com/#json=DUXgH0fu5HkqvMr3ch1kB,4g9EptwCh5mGrwo8LuGNdA)
- [ ] LIS - II (Offline Video) ⏳ [duration:: 45m 31s] | 🎨 [Class Notes](https://excalidraw.com/#json=CAtwAsRP9kIGFVYuhU8oA,2wZt8pYTKNPfXB6XzR_BWQ)
- [ ] Week 39.1 - DSA | DP: Longest Increasing Subsequence ⏳ [duration:: 1h 53m 47s] | 🎨 [Excalidraw](https://excalidraw.com/#json=12_Guvq6Rsso44Xw6kp6r,OFiGhXEWUVRoDdpHV5iOiw)
- [ ] Week 40.1 - DSA | DP: Contest Discussion and Space Optimisation ⏳ [duration:: 2h 18m 31s] | 🎨 [Excalidraw](https://excalidraw.com/#json=XYcdiWUAbhlSqtVPG4Nh_,5_-mx889FOw1tKa3Lxzrkw)

## Phase 8: Graphs & Generic Trees
- [ ] Week 41 - DSA | Graph: Basics and DFS ⏳ [duration:: 2h 19m 3s] | 🎨 [Excalidraw](https://excalidraw.com/#json=f6txer36ddBNaSBHL33WJ,JiMNeQqjTFkB3yHYhGTsaw)
- [ ] Week 42.1 - DSA | Graph: BFS ⏳ [duration:: 2h 6m 53s] | 🎨 [Excalidraw](https://excalidraw.com/#json=m-mX37fdtVyTokB-RkwjM,o1TtpF37mV3kUeTzNFKfFA)
- [ ] Week 42.2 - DSA | Graph: Problem Solving Session ⏳ [duration:: 2h 5m 25s] | 🎨 [Excalidraw](https://excalidraw.com/#json=d1s6EvIdo230cwPTel1fC,Zss6AcXN7CuXqiPJgB7jqA)
- [ ] Week 43.1 - DSA | Graph: Cycle Detection and Topological Sort ⏳ [duration:: 1h 23m 17s] | 🎨 [Excalidraw](https://excalidraw.com/#json=W8woJ--K2ZXEroyNrHijb,6PvnouVRVHSO-P45fZyWCw)
- [ ] Week 44 - DSA | Graph: Shortest Path Algorithms ⏳ [duration:: 2h 26m 42s] | 🎨 [Excalidraw](https://excalidraw.com/#json=X37Dhs2OkyiX6CrkkHpdd,egER9a52BeTw1m178BRcXg)
- [ ] Week 45 - DSA | Graph: Generic Trees ⏳ [duration:: 1h 49m 7s] | 🎨 [Excalidraw](https://excalidraw.com/#json=5hjXn0F_Aw5aG-gfXK2ap,TNHyDnOquAl5eiHKsf-Ylw)