---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: compute as a service, CaaS, Borg, containers, cattle vs pets, serverless, managed compute
---

# Compute as a Service (Importance: ★★)

#compute #tools

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| CaaS | Computing power to run programs; must survive and scale with org growth |
| Borg | Google's internal precursor to Kubernetes/Mesos |
| Automation of Toil | SFTP/SSH → shell scripts → automated scheduling |
| Containerization | Lightweight isolation via cgroups + chroot; replaces VMs for batch jobs |
| Pets vs Cattle | Cattle = replicas; replaceable automatically; pets = manual nursing |
| Managing State | In-process state is transient; "real" storage = external, durable |
| Rightsizing | Automate resource parameter setting; >50% of Google fleet |
| Serverless | Multi-tenant framework servers; scale to zero; tradeoff with control |

## Taming the Compute Environment

### Automation of Toil
Evolution from manual processes to automated infrastructure:
- **2002 problem**: Jeff Dean described manually managing 50+ machines, no automatic failure migration, human-implemented "sign up" files for throttling
- **Simple automations**: Shell scripts for deployment, automated monitoring (Graphana/Prometheus), autohealing agents ("while true; do run && break; done")
- **Automated scheduling**: Central service knows all machines, assigns work on demand, scans for bad health, replaces failed machines automatically

### Containerization and Multitenancy
One-to-one mapping of machines to programs is **highly inefficient**. Solution: specify resource requirements (CPU, RAM, disk), let scheduler **bin-pack** replicas onto available machines.
- **Isolation problem**: Noisy neighbors (resource exhaustion, dependency version conflicts, security)
- **VMs** provide isolation but have significant **overhead** (full OS resources, slow startup)
- **Containers**: Lightweight mechanism based on **cgroups** (contributed by Google to Linux kernel in 2007) and chroot jails/bind mounts
- Open source: Docker, LMCTFY
- PID space exhaustion example (2011): 32,000 PID limit became an implicit API; fixing it became a multi-phase, years-long project

### Rightsizing and Autoscaling
Asking humans to determine resource requirements doesn't scale. Parameters become stale as software evolves, leading to outages.
- Google has only recently reached >50% of fleet resource usage determined by **rightsizing automation**
- "Easy things should be easy, complex things should be possible" — most engineers don't need to manually size containers

## Writing Software for Managed Compute

### Architecting for Failure (Pets vs Cattle)
- **Pets**: Broken server → human panics, nurses it back; difficult to replace; **maintenance burden grows linearly** with fleet
- **Cattle**: Replicas replica001 to replica100; if one fails, automation replaces it; stamped out **without manual setup**
- For batch jobs: divide work into small **chunks** assigned dynamically (not statically); lose at most one chunk on failure
- For serving jobs: container signals intent to reschedule → refuses new requests → finishes in-flight requests → load balancer redirects

### Batch vs Serving Jobs
| Characteristic | Batch | Serving |
|---------------|-------|---------|
| Interest | **Throughput** | **Latency** |
| Lifespan | Short (minutes/hours) | Long (restarted only with new releases) |
| Startup time | Fast | Longer |
| Failure model | Lose processing, redo chunks | Lose user requests |
| Framework | MapReduce → Flume | Load-balanced server clusters |

### Managing State
In-process state should be treated as **transient**. Real storage = external, durable, persistent.
- Persistent state managed via **state replication** (3-5 replicas with consensus)
- **Caching**: Transient local storage; provision cache for latency goals but provision application for total load
- **Batching writes**: Acceptable when 100% data coverage isn't needed (monitoring) or data is re-creatable
- **Periodic checkpointing**: Split long calculations into smaller time windows

### Connecting to a Service
Service discovery provides an **extra layer of indirection** — durable identifier resolved by the scheduler.
- Clients look up addresses at startup, monitor in background
- Design APIs for **idempotency** — repeated requests produce same result as single request
- Use **client-assigned identifiers** for mutating requests (e.g., order ID for pizza delivery)
- Network split scenario: scheduler loses contact → reschedules → machine comes back → two replicas think they're "replica072"

### One-Off Code
Engineers need compute for ad-hoc analysis. A few hundred cores for minutes beats waiting a day on a workstation.
- Engineer time >> compute cost (a thousand core hours costs far less than a day of engineering)
- Risk: accidentally consuming too many resources → use **quotas** or low-priority batch (effectively free at Google)

## CaaS Over Time and Scale

### Containers as an Abstraction
Containers abstract away the compute environment, separating deployed software from the machine.
- **Filesystem abstraction**: Incorporate external software without custom machine configs; predeclare dependencies
- **Network ports**: Google initially didn't abstract ports → PickUnusedPortOrDie has 20,000+ usages. Docker/Kubernetes solved this with namespaces.
- **Hyrum's Law applies**: PID space exhaustion, Bash-to-ash migration breaking teams that had custom /usr/bin/bash replacements

### One Service to Rule Them All
Borg (2003) combined batch and serving jobs into **one large pool per datacenter**.
- Serving machines became **cattle** — no per-team machine pool management
- **Complementary efficiency**: Serving jobs are overprovisioned (slack for spikes); batch jobs run in the spare capacity at lower priority
- At Google: most batch workload runs **effectively for free** on serving job slack
- Requirements for serving in shared pool: **throttled rescheduling**, **graceful shutdown warning** (several seconds), service discovery

### Submitted Configuration
Use a **standardized configuration language** over CLI commands or scripts.
- Configuration drifts over time (multiple datacenters, staging/dev/prod, attached services)
- Standard config enables organization-wide operations (swap memcache implementation, push security updates)
- Configuration language is a requirement for **deployment automation**

## Choosing a Compute Service

### Lock-in Factors
Compute infrastructure has **high lock-in**: code takes advantage of all system properties (Hyrum's Law), ecosystem of helper services (logging, monitoring, debugging) would need rewriting.
- Borg's Bash-to-ash migration: teams who replaced /usr/bin/bash with custom code had memory usage spike when ash was used instead

### Centralization vs Customization
Best for management overhead: **single CaaS** for entire fleet. But growing organizations have diverse needs:
- GCE (Cloud) VMs needed **live migration** (not cattle treatment)
- Search needed containers to handle **disk failure** independently
- Multiple bifurcations made Borg's API surface large and unwieldy; intersection of behaviors hard to predict/test
- Post-2012 cleanup: whitelisting for power features, unused functionalities removed

### Serverless
Higher abstraction: multi-tenant framework servers dynamically load/unload action code.
- **Pros**: Scale to zero, lower per-app overhead, reduced management
- **Cons**: Must be truly stateless; loss of control; may need hand-tweaking (e.g., Code Jam warm-up hack)
- Google's choice: did not invest heavily; Borg already provides most benefits; batch + serving diversity requires a unified architecture
- Serverless is most attractive for **smaller organizations** where cluster sharing doesn't amortize

### Public vs Private Cloud
Public cloud **outsources management overhead** to a provider. Attractive for orgs without infrastructure expertise.
- Scaling ease increases with abstraction level: colocation → VM time → managed containers → serverless
- Multi-cloud/hybrid considerations for avoiding vendor lock-in

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Pets vs cattle" | Cattle = replaceable, auto-healing; pets = manual, **grows linearly** |
| "Containerization" | cgroups + chroot; lightweight isolation vs VM **overhead** |
| "Borg" | Google's single-pool compute: batch + serving; precursor to **Kubernetes** |
| "Managed state" | In-process = **transient**; real storage = external, durable |
| "Idempotency" | Repeated requests = same result; use **client-assigned IDs** |
| "Batch vs serving" | Batch = throughput, short; serving = latency, long-lived |
| "Rightsizing" | Automate resource params; >50% of Google fleet automated |
| "Serverless" | Multi-tenant framework; **scale to zero**; loss of control |
| "One service" | Single pool; serving slack used by **batch for free** |

## Related Notes
- [[Continuous Delivery]]
- [[Continuous Integration]]
- [[Build Systems]]
- [[Software Engineering Fundamentals]]
- [[Dependency Management]]
