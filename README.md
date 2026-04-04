## Kingman's VUT Simulator — Methodology & User Guide

### What this models

The simulator models a **single-server queue** — one supplier workstation processing a stream of incoming part orders, some from The Company and some from other customers. It answers a deceptively simple question: *given how busy this supplier is and how variable their arrivals and service times are, how long will a part wait before processing even begins?*

That wait time — the queue wait, not the processing time itself — is what drives lead time unpredictability in supply chains.

---

### The core equation: Kingman's VUT formula

The mathematical backbone is Kingman's approximation for the mean waiting time in a G/G/1 queue (a general single-server queue with general arrival and service distributions):

**Wait = ((CVₐ² + CVₛ²) / 2) × ProcessingTime × u / (1 − u)**

This breaks into three intuitive factors:

**V — Variability factor: (CVₐ² + CVₛ²) / 2**
The coefficient of variation (CV) measures how erratic a process is relative to its average. CVₐ captures variability in *when* parts arrive; CVₛ captures variability in *how long* each part takes to process. If both arrivals and service were perfectly regular (CV = 0), there would be no queue at all — parts would flow through like clockwork. Real suppliers have neither, so variability creates idle gaps and bunching that force parts to wait.

**U — Utilization factor: u / (1 − u)**
As utilization u approaches 1 (100%), the denominator approaches zero and wait time explodes toward infinity. At 50% utilization the multiplier is 1×; at 80% it's 4×; at 90% it's 9×; at 95% it's 19×. This nonlinearity is why suppliers who appear to have "only 10% spare capacity" are often chronically late — they are operating on the near-vertical part of the curve.

**T — Processing time**
Wait scales linearly with how long each job takes. A supplier processing large structural assemblies over many hours will accumulate far longer queues than one processing small fasteners in minutes, even at identical utilization and variability.

---

### From raw data to model parameters

A key question for any analyst is: *where do CVₐ, CVₛ, and u actually come from?* The Data Input tab answers this explicitly by walking through the derivation from raw operational records.

The inputs required are:

- **Arrival timestamps** — the time each job arrived at the supplier, in hours from the start of the observation window
- **Processing (service) times** — how long each job took to complete, in hours
- **Job labels** — a column of 1s and 0s indicating which jobs belong to The Company (optional; defaults to the mix slider if omitted)

The simulator then computes each parameter transparently, one step at a time:

**Step 1 — Inter-arrival times (IATs)**
The raw arrival timestamps are converted into the gaps between consecutive arrivals: IAT = [t₂ − t₁, t₃ − t₂, ...]. These gaps are what the queue actually "sees" — not the absolute timestamps.

**Step 2 — CVₐ from IATs**
The mean (μ) and standard deviation (σ) of the IATs are computed. CVₐ = σ / μ. A value near 0 means arrivals are highly regular; a value near 1 (exponential) or above means arrivals are bursty and unpredictable.

**Step 3 — CVₛ from service times**
The same calculation is applied to the processing times. CVₛ = σ(S) / μ(S). High CVₛ reflects a supplier whose jobs vary widely in complexity or whose process is inconsistent.

**Step 4 — Average processing time T**
The mean of the service times becomes T directly — the processing time input to the VUT formula.

**Step 5 — Utilization u**
u = μ(S) / μ(IAT). This is the ratio of how long jobs take on average to how often they arrive. If jobs arrive every 5 hours on average and take 4 hours to process, u = 0.80 (80%).

**Step 6 — The Company job mix**
If job labels are provided, the fraction of jobs labelled 1 becomes the mix percentage. If no labels are provided, the mix slider value is used.

**Step 7 — VUT prediction**
All derived parameters are plugged into Kingman's formula and the predicted wait time is displayed before any simulation is run — showing exactly what the approximation produces from real data.

Once the derivation is complete, clicking **Apply computed parameters to model** pushes all values into the sliders and runs the full simulation on the Dashboard, so the charts reflect conditions drawn from actual supplier records rather than assumed inputs.

---

### The simulation engine

The VUT formula is an approximation. The simulator also runs a **discrete-event simulation** (DES) alongside it so you can see how accurate that approximation is and observe dynamics the formula cannot capture.

The simulation works as follows:

1. **Generate arrivals.** 800 jobs are created with inter-arrival times drawn from a statistical distribution matching the CVₐ set by the sliders (or derived from data). For low CVₐ (regular arrivals), it uses an Erlang distribution; for high CVₐ (bursty arrivals), exponential draws.
2. **Label each job.** Each arriving job is independently tagged The Company or other based on the mix percentage.
3. **Process the queue.** Jobs enter a single-server FCFS (first-come, first-served) queue. When the server is free, the next job starts immediately; when it's busy, the job waits. Service times are drawn from the same distributional family, parameterized by CVₛ.
4. **Record outcomes.** For every job the simulation records arrival time, service start time, and completion time, from which wait time = start − arrival is derived. The Company and non-Company waits are tracked separately.
5. **Aggregate and visualize.** The simulation produces average waits, a wait time distribution, and queue length snapshots over time.

The simulation re-runs every time a slider changes.

---

### Why The Company and non-Company waits converge

The Company parts don't consistently wait more or less than other parts. This is because the model uses a **shared FCFS queue with no priority discipline** — job type is irrelevant to wait time in a shared queue. What matters is the state of the server when a job arrives, which is random. This comparison therefore illustrates the *baseline*: what The Company experiences when it has no preferential treatment at a supplier. Modelling priority queuing — where The Company's jobs jump the queue — would require a different queuing discipline.

---

### The four dashboard charts

**Wait time vs. utilization** — The hockey-stick curve. Shows that the relationship between utilization and wait is not linear. Most of the damage happens in the last 15% of capacity.

**Wait time vs. variability** — Sweeps CVₐ and CVₛ independently while holding everything else fixed. Shows that variability and utilization are multiplicative — high variability at low utilization is manageable, but high variability at high utilization is catastrophic.

**VUT vs. simulation comparison** — Plots both estimates across utilization levels. The VUT formula is generally accurate for moderate utilization and CV ≈ 1. It tends to diverge at very high utilization or extreme CV values.

**Queue length over time** — Shows the simulated backlog across the run. At low utilization the queue stays near zero; at high utilization it grows steadily or oscillates with periodic surges.

---

### What this tells The Company

**Utilization is the multiplier, not just a metric.** A supplier running at 90% isn't "10% away from full" — they are 9× more exposed to delays than one running at 50%. Reducing a key supplier from 92% to 85% utilization can cut expected wait times by more than half.

**Variability reduction compounds with capacity.** Investments in smoothing arrival schedules (more consistent release of purchase orders) or reducing service variability (standardized work, fewer rework loops) have outsized returns precisely when utilization is already high.

**Lead time averages are misleading.** The wait distribution histogram shows that even when the average wait is moderate, the tail of the distribution — the worst 10–20% of orders — can be several times longer. It is those tail events that cause line-down situations, not the average.

**Real data closes the credibility gap.** The Data Input tab allows an analyst to paste actual supplier records and trace exactly how those records produce the queue parameters and predicted wait times — making the model auditable and grounded in observable evidence rather than assumed inputs.
