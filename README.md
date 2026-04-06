## Supplier Wait Time Simulator — How It Works & How to Use It

### What this is

Imagine a supplier is like a single checkout lane at a grocery store. Parts arrive, join the line, and wait until the worker is free to process them. This simulator helps The Company understand how long parts spend waiting in that line — before any work even begins on them.

That waiting time is what makes delivery dates unpredictable. The simulator helps answer: *what is causing the wait, and what would actually reduce it?*

---

### The three things that drive waiting

Every wait time at a supplier comes down to three things, and only three things:

**1. How busy the supplier is**
This is called utilization. Think of it like a highway — at 50% capacity, traffic flows freely. At 90% capacity, one extra car can cause a traffic jam. Suppliers work the same way. A supplier running near full capacity doesn't have a little wait time problem — they have an enormous one, and it gets dramatically worse the closer they get to 100%. The relationship is not gradual. It is a cliff.

**2. How unpredictable the arrivals are**
If purchase orders arrive on a perfectly regular schedule — one every Monday morning — the supplier can plan around it. But if orders arrive in unpredictable bursts, the supplier sometimes has nothing to do and sometimes has a pile-up. That unpredictability creates waiting even when the supplier has plenty of overall capacity.

**3. How unpredictable the work itself is**
Some jobs take 2 hours, some take 8 hours. When the supplier doesn't know how long the next job will take, they can't manage the line. That uncertainty creates the same bunching and waiting as unpredictable arrivals.

The simulator lets you adjust all three and see the effect on wait times immediately.

---

### Where the numbers come from — the Data Input tab

The biggest question for any real analysis is: *how do we know what numbers to use?* The Data Input tab answers this by letting you paste in raw records from a supplier — the kind of data that already exists in any order management or production system.

You need three things:

**When did each job arrive?**
A list of times — for example, "job 1 arrived at hour 0, job 2 at hour 1.2, job 3 at hour 2.5" and so on. These are just the timestamps from the order log.

**How long did each job take to process?**
The actual time each job spent being worked on — not including waiting, just the active processing time. This comes from production records.

**Which jobs belonged to The Company?**
A simple yes/no column next to each job. Enter 1 for The Company's jobs and 0 for everyone else's. If you don't have this breakdown, leave it blank and use the mix slider instead.

Once you paste in that data and click **Compute parameters from data**, the simulator shows you — in plain steps — how your raw records translate into the three drivers described above. You can see exactly what your supplier's busyness level is, how irregular their arrivals are, and how variable their processing times are. Nothing is assumed or made up.

When you're satisfied, click **Apply computed parameters to model** and the simulator runs using your real data.

---

### What the simulator then shows you

Once parameters are set — either from your data or using the sliders manually — the simulator runs and produces four charts:

**Wait time vs. how busy the supplier is**
This is the most important chart. It shows that as a supplier gets busier, wait times do not grow gradually — they spike. A supplier at 90% busy has wait times roughly nine times longer than one at 50% busy. The chart makes this visible and impossible to ignore.

**Wait time vs. how unpredictable things are**
This chart sweeps across different levels of unpredictability in arrivals and processing times, showing how much each one contributes to waiting. Reducing unpredictability at a busy supplier has an outsized effect.

**Formula estimate vs. simulated result**
The simulator runs two calculations side by side — a mathematical formula and a full computer simulation — and compares them. When they agree closely, you can trust the formula as a quick-check tool. When they diverge, the simulation is the more reliable picture.

**Jobs piling up over time**
This chart shows how the backlog grows and shrinks across a simulated workday. At low busyness levels, it stays near zero. At high busyness levels, it accumulates and never fully clears — which is exactly what suppliers in trouble look like.

---

### What The Company should take away from this

**Being almost full is not the same as having a little room.**
A supplier at 92% utilization is not 8% away from a problem — they are already deep in one. The math is unforgiving near the top. Even a small reduction in how busy a key supplier is can cut wait times dramatically.

**Unpredictability is as damaging as being overloaded.**
If The Company sends purchase orders in irregular bursts, it is making the supplier's wait time problem worse — even if the supplier has plenty of overall capacity. Smoothing out when orders are sent can reduce waiting without the supplier adding any resources.

**The average wait time hides the real risk.**
Most orders might wait a reasonable amount of time — but the worst 10 to 20% of orders wait far, far longer. It is those outliers that cause a line to go down, not the average. The distribution chart makes this visible.

**Real data makes the case.**
Using the Data Input tab with actual supplier records means the analysis is traceable. Every number on the screen came from a real timestamp or production log. That is what makes findings credible to a supplier, to leadership, and to anyone asking how you arrived at your conclusions.
