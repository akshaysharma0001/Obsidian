# 18 LPA SDE-1 Roadmap Tracker (36 Weeks)

> **Daily Rhythm:** `5:00–7:30 AM` C++ DSA | `8:30–9:30 AM` Commute (Lectures & Notes) | `9:00–10:00 PM + Sunday` Dev, LLD & Hiring

## Overall Progress

```dataviewjs
const tasks = dv.current().file.tasks;
const total = tasks.length;
const completed = tasks.where(t => t.completed).length;
const remaining = total - completed;
const pct = total > 0 ? ((completed / total) * 100).toFixed(1) : "0.0";
const remainPct = total > 0 ? (100 - parseFloat(pct)).toFixed(1) : "0.0";

const wrap = dv.el("div", "", {
    attr: {
        style: "padding: 16px; border-radius: 10px; border: 1px solid var(--background-modifier-border); background: var(--background-secondary); margin-bottom: 14px;"
    }
});

const header = wrap.createEl("div", {
    attr: { style: "display: flex; justify-content: space-between; font-weight: 600; margin-bottom: 8px;" }
});
header.createEl("span", { text: "Roadmap Completion" });
header.createEl("span", { text: `${pct}% (${completed} / ${total})` });

const barBg = wrap.createEl("div", {
    attr: { style: "width: 100%; height: 14px; background: var(--background-modifier-border); border-radius: 999px; overflow: hidden;" }
});
barBg.createEl("div", {
    attr: { style: `width: ${pct}%; height: 100%; background: linear-gradient(90deg, #10b981, #3b82f6); border-radius: 999px; transition: width 0.3s ease;` }
});

dv.table(
    ["Metric", "Checkboxes", "Percentage"],
    [
        ["Completed", `${completed} / ${total}`, `${pct}%`],
        ["Remaining", `${remaining} / ${total}`, `${remainPct}%`],
        ["Total Tasks", `${total}`, "100.0%"]
    ]
);
```

## Countdown to June 30, 2027 (Interview-Ready Deadline)

```dataviewjs
const targetDate = new Date("2027-06-30T23:59:59+05:30").getTime();

const box = dv.el("div", "", {
    attr: {
        style: "padding: 18px; border-radius: 10px; border: 1px solid var(--background-modifier-border); background: var(--background-secondary); text-align: center; margin-bottom: 20px;"
    }
});

box.createEl("div", {
    text: "TIME LEFT UNTIL JUNE 30, 2027",
    attr: { style: "font-size: 0.85em; letter-spacing: 1px; opacity: 0.75; font-weight: 600; margin-bottom: 8px;" }
});

const display = box.createEl("div", {
    attr: { style: "font-size: 1.75em; font-weight: 700; font-family: var(--font-monospace); color: var(--text-accent);" }
});

let timer = null;

function renderCountdown() {
    if (timer !== null && !box.isConnected) {
        clearInterval(timer);
        return;
    }
    const diff = targetDate - Date.now();
    if (diff <= 0) {
        display.setText("0d : 00h : 00m : 00s — Interview Ready!");
        if (timer !== null) clearInterval(timer);
        return;
    }
    const days = Math.floor(diff / (1000 * 60 * 60 * 24));
    const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
    const minutes = Math.floor((diff / (1000 * 60)) % 60);
    const seconds = Math.floor((diff / 1000) % 60);
    const pad = (n) => String(n).padStart(2, "0");

    display.setText(`${days} Days  |  ${pad(hours)} Hours  |  ${pad(minutes)} Min  |  ${pad(seconds)} Sec`);
}

renderCountdown();
timer = setInterval(renderCountdown, 1000);
```

## Phase 1: Core Foundations (Weeks 1–12 | Dec 2026 – Feb 2027)

### Week 1
- [ ] **DSA:** Learn C++ syntax & STL (`vector`, `unordered_map`, `set`, `pair`, `sort`)
- [ ] **DSA:** Solve 10 Basic Hashing & Array Easy problems
- [ ] **Commute:** Watch Harkirat W14.1 (TypeScript Part 1)
- [ ] **Dev:** Initialize flagship repo (Node.js, Express, strict TypeScript, ESLint, clean folder structure)

### Week 2
- [ ] **DSA:** Master Sorting algorithms (Merge Sort, Quick Sort)
- [ ] **DSA:** Solve 10 Array Easy/Mediums (rotate array, move zeros, union, missing number)
- [ ] **Commute:** Watch Harkirat W14.2 (TypeScript Interfaces & Types)
- [ ] **Dev:** Add Zod request validation middleware + centralized error handler

### Week 3
- [ ] **DSA:** Solve 10 Array Mediums (2Sum, Kadane's, Majority Element, Dutch National Flag, Next Permutation)
- [ ] **Commute:** Watch Harkirat W17.1 (PostgreSQL & SQL Part 1)
- [ ] **Dev:** Spin up local PostgreSQL, design `Users` table, and connect Prisma (W18.1)

### Week 4
- [ ] **DSA:** Solve 10 Array Mediums (Subarray Sum K, Merge Intervals, 3Sum, Longest Consecutive Sequence)
- [ ] **Commute:** Watch Harkirat W17.2 (PostgreSQL Part 2)
- [ ] **Dev:** Build JWT access/refresh authentication + bcrypt hashing
- [ ] **Milestone:** Push clean Auth Service to GitHub (40 DSA problems total)

### Week 5
- [ ] **DSA:** Solve 12 Binary Search 1D problems (Lower/Upper Bound, Rotated Sorted Array, Peak Element)
- [ ] **Commute:** Watch Harkirat W21.4 (PayTM DB Transactions & Row Locks)
- [ ] **Dev:** Design core relational schema for flagship project (Bookings/Tasks, Inventory/Jobs, Logs)

### Week 6
- [ ] **DSA:** Solve 10 Binary Search on Answer problems (Koko Eating Bananas, Ship Packages, Aggressive Cows)
- [ ] **Commute:** Watch Harkirat W16.1 (WebSockets)
- [ ] **Dev:** Seed 100k dummy rows, add compound indexes, and benchmark queries with `EXPLAIN ANALYZE`

### Week 7
- [ ] **DSA:** Solve 12 Medium String problems (Anagram, Longest Palindromic Substring, String to Integer, Hashing)
- [ ] **Commute:** Review SQL Indexing & ACID Isolation Levels notes
- [ ] **Dev:** Implement cursor-based pagination for feed/history endpoints (replace `OFFSET`)

### Week 8
- [ ] **DSA:** Solve 12 Two Pointers & Sliding Window problems (Longest Substring No Repeat, Max Consecutive Ones, Subarrays with K)
- [ ] **Commute:** Revise DSA Mistake Log (Weeks 1–7)
- [ ] **Dev:** Implement PostgreSQL ACID transactions + row locking (`SELECT FOR UPDATE`)
- [ ] **Milestone:** Run 100-request simultaneous concurrency test with zero double-bookings

### Week 9
- [ ] **DSA:** Solve 12 Linked List problems (Reverse, Middle, Cycle Detection, Intersection, Merge Sorted)
- [ ] **Commute:** Watch Harkirat W42.1 (Async Backend & Redis Streams)
- [ ] **Dev:** Spin up local Redis and build read-through caching layer for high-traffic GET endpoints

### Week 10
- [ ] **DSA:** Solve 10 Linked List Hard/Mediums (Reverse in K-Group, Copy List with Random Pointer, Flatten LL)
- [ ] **Commute:** Watch Harkirat W45.1 (OpenAPI, Rate Limiting, DDoS)
- [ ] **Dev:** Implement cache invalidation on writes and verify latency drop (200ms → <20ms)

### Week 11
- [ ] **DSA:** Solve 12 Stack & Queue problems (Valid Parentheses, Min Stack, Next Greater Element, Asteroid Collision)
- [ ] **Commute:** Watch Harkirat W47.1 (Rate Limiting & Captcha)
- [ ] **Dev:** Build Redis sliding-window rate limiter middleware (IP + user-level caps)

### Week 12
- [ ] **DSA:** Solve 10 Monotonic Stack/Queue problems (Largest Rectangle in Histogram, Trapping Rain Water, LRU Cache)
- [ ] **Commute:** Revise DSA Mistake Log (Weeks 8–11)
- [ ] **Dev:** Add WebSocket live status updates (using W16.1)
- [ ] **Milestone:** Write integration tests for auth, locks, and rate limiter (130 DSA problems total)

## Phase 2: Scale, Architecture & Advanced DSA (Weeks 13–24 | Mar 2027 – May 2027)

### Week 13
- [ ] **DSA:** Solve 12 Recursion & Backtracking problems (Subsets, Combination Sum I/II, Permutations)
- [ ] **Commute:** Watch Harkirat W43.1 (Workers & Pusher via Redis Streams)
- [ ] **Dev:** Integrate BullMQ + Redis worker service to offload heavy tasks (receipts/notifications)

### Week 14
- [ ] **DSA:** Solve 10 Backtracking Mediums (N-Queens, Word Search, Palindrome Partitioning, Rat in a Maze)
- [ ] **Commute:** Watch Harkirat W44.1 (Scaling HTTP & WebSocket Servers)
- [ ] **Dev:** Add idempotency keys, dead-letter queue (DLQ), and exponential backoff retries to workers

### Week 15
- [ ] **DSA:** Solve 12 Binary Tree problems (Traversals, Max Depth, Diameter, Balanced Tree, Max Path Sum)
- [ ] **Commute:** Study Applied AI: Prompt Caching & Structured Outputs
- [ ] **Dev:** Build Applied AI worker (async LLM processing/agent task with strict Zod output parsing)

### Week 16
- [ ] **DSA:** Solve 12 Binary Tree & BST problems (LCA, Right/Top View, Validate BST, Kth Smallest, Serialize/Deserialize)
- [ ] **Commute:** Study Applied AI: Context Engineering & Eval Harnesses
- [ ] **Dev:** Write automated eval script (20–30 test cases) measuring AI worker accuracy, latency, and token cost

### Week 17
- [ ] **DSA:** Solve 12 Heap / Priority Queue problems (Kth Largest, Top K Frequent, Merge K Sorted Lists, Median Stream)
- [ ] **Commute:** Watch Harkirat DevOps W24.1–24.2 (Linux VMs, Nginx, PM2)
- [ ] **Dev:** Rent AWS EC2 / DigitalOcean VM; configure SSH, UFW firewall, and Nginx reverse proxy

### Week 18
- [ ] **DSA:** Solve 10 Greedy problems (Jump Game I/II, Meeting Rooms/N-Meetings, Merge/Insert Intervals, Gas Station)
- [ ] **Commute:** Watch Harkirat DevOps W25 (Process Management & CI/CD)
- [ ] **Dev:** Write multi-stage `Dockerfile` for API and Worker services; verify local container builds

### Week 19
- [ ] **DSA:** Solve 12 Graph Traversal problems (BFS, DFS, Number of Islands, Rotting Oranges, Cycle Detection)
- [ ] **Commute:** Watch Harkirat DevOps W26.1–26.2 (Docker Part 1 & 2)
- [ ] **Dev:** Build `docker-compose.yml` uniting API, Worker, PostgreSQL, and Redis on isolated networks

### Week 20
- [ ] **DSA:** Solve 12 Graph Algorithm problems (Topological Sort/Kahn's, Course Schedule, Dijkstra, Union-Find)
- [ ] **Commute:** Watch Harkirat DevOps W27.1 (Docker Compose + CI/CD)
- [ ] **Dev:** Set up GitHub Actions CI/CD to auto-test, build, and deploy Docker stack behind HTTPS Nginx
- [ ] **Milestone:** Live backend running on a public HTTPS domain (220 DSA problems total)

### Week 21
- [ ] **DSA:** Solve 10 1D DP problems (Climbing Stairs, House Robber I/II, Frog Jump, Decode Ways)
- [ ] **Commute:** Watch Harkirat W49.1 (Codeforces Queue Architecture)
- [ ] **Dev:** Write **k6** load-testing scripts simulating 1,000+ concurrent virtual users against live API

### Week 22
- [ ] **DSA:** Solve 10 Grid & Knapsack DP problems (Unique Paths, Min Path Sum, 0/1 Knapsack, Partition Equal Subset)
- [ ] **Commute:** Watch Harkirat W50.1 (Codeforces Part 2)
- [ ] **Dev:** Profile bottlenecks under load (DB connection pool, Redis pipeline, event loop lag) and patch them

### Week 23
- [ ] **DSA:** Solve 10 Subsequence & String DP problems (Coin Change, LIS, LCS, Edit Distance)
- [ ] **Commute:** Watch Harkirat W51.1 (Sharding, Replication, System Design)
- [ ] **Dev:** Re-run k6 benchmarks; generate throughput/p95 latency charts and Excalidraw architecture diagram

### Week 24
- [ ] **DSA:** Solve 8 Stock DP & Trie problems (Buy/Sell Stock variants, Implement Trie, Word Search II)
- [ ] **Commute:** Revise Full DSA Mistake Log (Weeks 1–23)
- [ ] **Dev:** Finalize flagship README (benchmarks, trade-offs, AI eval results) and record 2-min demo video
- [ ] **Milestone:** Portfolio-ready flagship project complete (255+ DSA problems total)

## Phase 3: Interview Polish & 18 LPA Conversion (Weeks 25–36 | Jun 2027 – Aug 2027)

### Week 25
- [ ] **DSA:** Solve 10 unseen random LeetCode Mediums (strict 25-min timer, speak approach aloud)
- [ ] **DSA:** Give Sunday LeetCode Weekly Contest
- [ ] **Commute:** Revise OS Fundamentals (Processes, Threads, Deadlocks, Virtual Memory)
- [ ] **LLD:** Code **Rate Limiter** and **In-Memory Cache** cleanly using SOLID principles

### Week 26
- [ ] **DSA:** Solve 10 unseen random LeetCode Mediums (timed)
- [ ] **DSA:** Give Sunday LeetCode Contest + upsolve 2 missed problems
- [ ] **Commute:** Revise DBMS Fundamentals (B+ Trees, Indexing, ACID, Normalization)
- [ ] **LLD:** Code **Parking Lot** and **Splitwise**
- [ ] **Mock:** Complete Mock Interview #1

### Week 27
- [ ] **DSA:** Solve 10 unseen random Mediums (focus on Trees & Graphs)
- [ ] **DSA:** Give Sunday LeetCode Weekly Contest
- [ ] **Commute:** Revise Computer Networks (TCP/UDP, HTTP/2, DNS, TLS, Load Balancers)
- [ ] **Career:** Audit Fare Labs work for hard metrics (latency, bundle size, bugs fixed)
- [ ] **Mock:** Complete Mock Interview #2

### Week 28 (June 30 Deadline Week)
- [ ] **DSA:** Solve 10 unseen random Mediums (focus on DP & Sliding Window)
- [ ] **DSA:** Give Sunday LeetCode Weekly Contest
- [ ] **Commute:** Prepare Behavioral & Project Deep-Dive stories (STAR format)
- [ ] **Career:** Lock final 1-page ATS resume + LinkedIn profile overhaul
- [ ] **Mock:** Complete Mock Interviews #3 & #4

### Week 29
- [ ] **DSA:** Solve daily 2 timed Mediums from Mistake Log + Sunday Contest
- [ ] **Commute:** Research 30 target companies (10–14 LPA warm-ups + 15–22 LPA targets)
- [ ] **Outreach:** Send 30 personalized referral DMs/cold emails (attach k6 benchmark + demo link)

### Week 30
- [ ] **DSA:** Solve daily 2 timed Mediums + Sunday Contest
- [ ] **Commute:** Rehearse 2-min explanations of DB locks, Redis caching, and AI evals
- [ ] **Outreach:** Send 30 more referral DMs
- [ ] **Interviews:** Start taking Online Assessments & warm-up interviews (10–14 LPA)

### Week 31
- [ ] **DSA:** Solve daily 2 timed Mediums + patch any failed OA patterns + Sunday Contest
- [ ] **Commute:** Review JS/Node.js internals (Event Loop, Streams, Cluster, Memory Leaks)
- [ ] **Interviews:** Complete active warm-up interviews; log every missed technical question and patch gaps

### Week 32
- [ ] **DSA:** Solve daily 2 timed Mediums + Sunday Contest
- [ ] **Commute:** Study target company engineering blogs (Swiggy, Razorpay, Zepto, AI startups)
- [ ] **Outreach:** Send 30 targeted DMs to 18–22 LPA teams
- [ ] **Milestone:** Convert 1 warm-up baseline offer for negotiation leverage

### Week 33
- [ ] **DSA:** Drill company-tagged Medium/Hard problems for active loops + Sunday Contest
- [ ] **Commute:** Review HLD basics (URL Shortener, Notification System, Rate Limiter)
- [ ] **Interviews:** Execute active Technical & Machine Coding rounds at 15–22 LPA companies

### Week 34
- [ ] **DSA:** Drill company-tagged problems + Mistake Log revision + Sunday Contest
- [ ] **Commute:** Prep Hiring Manager questions (scaling trade-offs, conflict, ownership)
- [ ] **Interviews:** Execute Technical & Hiring Manager rounds + send 15–20 backup outreach DMs

### Week 35
- [ ] **DSA:** Light timed maintenance (1–2 Mediums/day to stay sharp and avoid burnout)
- [ ] **Commute:** Practice salary negotiation scripts & review SDE-1 market bands
- [ ] **Interviews:** Complete final Hiring Manager / Bar Raiser rounds and align timelines for competing offers

### Week 36
- [ ] **DSA:** Maintenance only
- [ ] **Commute:** Review offer breakdowns (Base vs. ESOPs vs. Joining Bonus)
- [ ] **Closing:** Negotiate using market SDE-1 bands and competing offers
- [ ] **FINAL GOAL:** Sign **18 LPA** SDE-1 offer by August 31, 2027