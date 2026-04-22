---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, compute as a service, Borg, containers, cattle, serverless, state management
---

# Compute as a Service Practice (10 questions)

#practice #compute

## Related Concepts
- [[Compute as a Service]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Pets vs cattle | Cattle = auto-replaced; pets = manual; **linear cost** |
> | Containers | cgroups + chroot; lightweight vs **VM overhead** |
> | Managed state | In-process = **transient**; durable = external |
> | Idempotency | Repeated request = same result; **client-assigned IDs** |
> | Batch vs serving | Throughput vs **latency**; complementary resource usage |
> | Serverless | Scale to zero; **truly stateless** required |

---

## Question 1 - Pets vs Cattle [recall]
> Explain the "pets versus cattle" metaphor for servers and why the cattle model is critical for scaling.

> [!answer]- Show Answer
> - **Pets**: When a server breaks, a human comes to look at it (usually in a panic), understands what went wrong, and nurses it back to health. It's difficult to replace and requires manual setup.
> - **Cattle**: Named replica001 to replica100; if one fails, automation removes it and provisions a new one. The distinguishing characteristic is that it's easy to **stamp out a new instance** fully automatically.
>
> Cattle is critical for scaling because with pets, maintenance burden grows **linearly or superlinearly** with fleet size. With cattle, the system **self-heals** — returns to a stable state after failure without human intervention. Additional effort is needed to make the system function smoothly during failures (dynamic work assignment, graceful shutdown).

---

## Question 2 - Container Evolution [recall]
> Why did Google choose containers over VMs for Borg (2003), and what isolation mechanism did they use?

> [!answer]- Show Answer
> Google chose containers because VMs have **significant overhead**: they need resources to run a full OS inside and take time to boot up, making them a poor fit for batch jobs with small resource footprints and short runtimes.
>
> Containers use:
> - **cgroups** (contributed by Google to the Linux kernel in 2007): resource isolation for CPU, RAM
> - **chroot jails**: filesystem isolation
> - **bind mounts and/or union/overlay filesystems**: filesystem layering
>
> Open source implementations include **Docker** and **LMCTFY**. Containers provide a lightweight isolation mechanism that minimizes interference between co-located workloads while avoiding VM overhead.

---

## Question 3 - Batch vs Serving Jobs [recall]
> Compare batch and serving jobs across four characteristics, and explain why Borg combining them in a single pool creates efficiency.

> [!answer]- Show Answer
> | Characteristic | Batch | Serving |
> |------|-------|---------|
> | Interest | **Throughput** | **Latency** |
> | Lifespan | Short (min/hours) | Long-lived |
> | Failure model | Lose + redo processing | Lose user requests |
> | Startup | Fast | Often slower |
>
> Efficiency from combining: Serving jobs must be **overprovisioned** for spikes/outages, resulting in ~30% utilization. Batch jobs can run in the **spare 70%** at lower priority, reclaimed if serving needs resources. Because batch cares about aggregate throughput (not individual tasks) and individual replicas are cattle, they happily soak up spare capacity. Result: at Google, batch effectively runs **for free** on serving slack.

---

## Question 4 - Managing State [recall]
> What strategies does Google recommend for managing state in a "cattle" compute environment?

> [!answer]- Show Answer
> Core principle: in-process state is **transient**; real storage must be external and durable.
>
> Five strategies:
> 1. **External persistent storage**: Anything surviving beyond one request/chunk goes off-machine (replicated 3-5x with consensus for writes)
> 2. **Caching**: Transient local storage for latency improvement; provision cache for latency goals, but provision app for **total load** (survive cache loss)
> 3. **Warm-up**: Pull data from external storage to local at startup for serving latency
> 4. **Write batching**: Accumulate writes locally, periodically flush; acceptable when 100% coverage isn't needed or data is re-creatable
> 5. **Periodic checkpointing**: Split long calculations into smaller windows by saving state to persistent storage periodically

---

## Question 5 - Idempotency and Service Discovery [recall]
> Why is idempotency important in a managed compute environment, and how do client-assigned identifiers help?

> [!answer]- Show Answer
> Idempotency matters because in a managed compute environment, the server you're talking to might be **killed and rescheduled** before responding. Retrying requests is necessary (standard for network communication) but for mutating requests, repeats can cause duplication.
>
> **Client-assigned identifiers** solve this: the client assigns an ID to each request (e.g., an order ID). If the server receives a request with an ID that's already recorded, it assumes it's a **repeated request** and reports success (optionally validating parameters match). This guarantees the result of issuing a request twice is the same as issuing it once.
>
> Additional scenario: network partitions can cause the scheduler to reschedule work, then the original machine comes back — creating **two replicas** thinking they're the same instance, another source of duplicate requests.

---

## Question 6 - PID Namespace Problem [application]
> The Borg PID space exhaustion (2011) is described as a long-running Hyrum's Law problem. Explain the three-phase solution and why it was so difficult.

> [!answer]- Show Answer
> The problem: Linux PID_MAX defaulted to 32,000. This limit became an **implicit API guarantee** that users depended on — for example, log storage processes assumed PIDs fit in 5 digits and broke with 6-digit PIDs.
>
> Three-phase solution:
> 1. **Temporary upper bound**: Limit PIDs per container so one thread-leaking job can't render the whole machine unusable
> 2. **Split PID space**: Increase limit for threads (few users depended on the 32K guarantee for thread PIDs) but keep 32,000 for processes
> 3. **PID namespaces**: Give each container its own complete PID space (Hyrum's Law again: many systems assumed {hostname, timestamp, pid} uniquely identifies a process, which would break with namespaces)
>
> Phase three was **still ongoing 8+ years later** because of the effort to identify all places making the uniqueness assumption. Key lesson: not that you should use PID namespaces, but that container abstractions inevitably accumulate **implicit dependencies** that become extremely expensive to change.

---

## Question 7 - Serverless Trade-offs [application]
> Your small team is choosing between managed Kubernetes and a serverless offering (e.g., AWS Lambda). Under what conditions is each a better choice?

> [!answer]- Show Answer
> **Serverless is better when**:
> - Small team; cluster sharing doesn't amortize management costs
> - Workloads are **truly stateless** (request-scoped processing)
> - Traffic is variable/low — serverless scales to **zero** (no minimum cost)
> - Willing to accept **loss of control** over environment for reduced management overhead
> - Using a common server framework (HTTP handlers/actions)
>
> **Managed Kubernetes is better when**:
> - Organization has diverse workload types (stateful services, databases, long-running processes)
> - Need for **custom environment** (special hardware, OS, dependencies)
> - High steady traffic where minimum-presence cost is acceptable
> - Want a **unified architecture** for all workload types
> - Anticipate **outgrowing** serverless constraints as the org grows
>
> Key consideration: serverless built on top of containers (AppEngine on Borg, Knative on Kubernetes, Lambda on EC2) provides a **break-out path** to containers if needed.

---

## Question 8 - Rightsizing Automation [recall]
> Why is asking humans to determine resource requirements flawed, and what has Google's experience been with automating this?

> [!answer]- Show Answer
> Human-specified resource requirements are flawed because:
> - Engineers don't regularly interact with these numbers — they are **not intuitive**
> - Initial estimates require time at launch, and this cost scales with number of services
> - As programs evolve (grow), **configuration doesn't keep up**, eating into slack for spikes
> - Eventually results in **outages** when slack is insufficient for an actual spike
>
> Google's experience: Only recently reached **>50% of fleet resource usage** determined by rightsizing automation. Although only half of usage, it covers a **larger fraction of configurations**, meaning most engineers don't need to manually size containers. Philosophy: "easy things should be easy, complex things should be possible."

---

## Question 9 - Bash to Ash Migration [analysis]
> The Borg team's attempt to switch from Bash to ash as a lighter shell illustrates a key lesson about compute services. What is that lesson, and what happened?

> [!answer]- Show Answer
> The lesson: Compute infrastructure has **extremely high lock-in** due to Hyrum's Law. Even the smallest implementation details become depended upon.
>
> What happened: Borg executed `/usr/bin/bash -c $USER_COMMAND` for all commands. To save memory at Google's scale, the team switched to `/usr/bin/ash -c`. But some teams had already noticed the Bash memory overhead and **replaced /usr/bin/bash with custom lightweight code** in their filesystem overlays. When ash was used instead (not overwritten by custom code), their memory usage **increased** (now including real ash instead of their custom code), causing alerts, rollbacks, and unhappiness.
>
> Broader implication: Any compute service choice becomes surrounded by an ecosystem of helper tools (logging, monitoring, debugging, alerting). Even **enumerating these tools** is a challenge for a medium or large organization, making compute architecture changes extremely costly.

---

## Question 10 - One Service to Rule Them All [analysis]
> Analyze why Google chose to run all workloads (batch + serving) in a single Borg pool rather than separate clusters. What are the efficiency gains and the challenges?

> [!answer]- Show Answer
> **Two efficiency gains**:
> 1. **Serving machines as cattle**: Without a unified pool, each team manages its own machine pool, creating n different management practices that diverge over time. A single Borg pool avoids this **linear scaling** of overhead. Company-wide changes (new server architecture, datacenter switches) become manageable.
>
> 2. **Complementary resource utilization**: Serving jobs are overprovisioned (need slack for spikes). In a serving-only pool, machines sit ~30% utilized. With batch jobs in the same pool, batch runs in the **spare capacity** at lower priority. If serving needs resources, they're reclaimed (freeze CPU, kill for RAM). Batch cares about aggregate throughput, so losing individual tasks is fine. Result: batch runs **effectively for free**.
>
> **Challenges**:
> - **Throttled rescheduling**: Can't kill/restart 50% of serving replicas (unlike batch)
> - **Graceful shutdown**: Serving jobs need seconds of warning to finish in-flight requests and stop accepting new ones
> - **Service discovery**: Must be built-in or modular for dynamically moving replicas
> - **Diverse needs**: GCE VMs needed live migration; Search needed custom disk failure handling — leading to API surface bifurcation that was difficult to test and maintain

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Pets vs cattle | Cattle = auto-healing; pets = **linear maintenance cost** |
> | Containers | cgroups + chroot; lighter than **VMs** |
> | Batch vs serving | Complementary: batch runs in serving **slack for free** |
> | Managed state | In-process = transient; external = **durable, replicated** |
> | Idempotency | Client-assigned IDs; handle **retry duplication** |
> | Rightsizing | Automate resource params; **>50%** of Google fleet |
> | Serverless | Truly stateless; scale to zero; **loss of control** |
> | Lock-in | Hyrum's Law on infra details; **ecosystem** of helper tools |
> | PID exhaustion | Implicit API; **years-long** fix due to hidden dependencies |
> | Configuration | Standardized language; enables **org-wide operations** |
