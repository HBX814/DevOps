# DevOps Interview Notes — Explained Simply

Oct 15, 2026 · @Harsh Bhati

## Before you start

These are your DevOps notes, rewritten as if a teacher is explaining them to you, topic by topic. They are for freshers and experienced engineers preparing for DevOps, Cloud, SRE or Platform Engineer interviews.

Every topic follows the same pattern: what it means in simple words, how it works, the commands or code, and how to say it in an interview.

**How to read the importance labels:**

- **(Asked often)** — comes up in many interviews.
- **(Almost always asked)** — prepare this one well.
- **(Must know)** — the classic questions; you should answer these smoothly, without thinking.

### What's new in 2026 (mention these to sound up to date)

Interviewers like candidates who follow the news. Drop one or two of these points naturally into your answers.

| Area | What changed, in simple words | Where it helps in the interview |
| --- | --- | --- |
| Kubernetes | The latest version is **v1.37 "Garhwal"** (released 26 Aug 2026), named after the Himalayan region of Uttarakhand. v1.38 is expected around Dec 2026. | Shows you follow the ecosystem |
| Kubernetes | **v1.35** dropped cgroup v1 (nodes must now use cgroup v2) and made **in-place Pod resize** stable, so you can change a Pod's CPU or memory without recreating it. **v1.37** made the `metrics.k8s.io` API stable and moved **HPA scale-to-zero** to beta. | Questions on resources and autoscaling |
| Ingress | The community **Ingress NGINX** controller was **retired in March 2026**. No more releases or security fixes. The recommended path now is **Gateway API** or another maintained controller. | "How would you expose an app today?" |
| Helm | **Helm 4** came out in Nov 2025, the first major release in 6 years. New installs use server-side apply, plugins can be WebAssembly, `--wait` is smarter (kstatus-based), and `--atomic` is renamed to `--rollback-on-failure`. Helm 3 charts still work. | Helm questions, CI scripts |
| Terraform | Terraform uses the **Business Source License** since v1.6, so it is no longer fully open source. **IBM completed buying HashiCorp in Feb 2025.** **OpenTofu** is the open-source (MPL 2.0) fork, run under the Linux Foundation / CNCF. | "Terraform or OpenTofu?" |
| DORA | DORA now tracks **5 delivery metrics** (the new one is *deployment rework rate*). Its 2025 report says **AI is an amplifier**: it makes strong teams stronger and weak teams weaker. It also says **platform engineering** is the base for getting value from AI. | "How do you measure DevOps success?" |

### Study roadmap (5 weeks)

Don't try to learn everything at once. Follow this order: each week builds on the last, and each week ends with something you build with your own hands.

| Week | Topics | Hands-on goal |
| --- | --- | --- |
| 1 | Linux, Networking, Git, Bash | Troubleshoot a VM, write 5 useful scripts |
| 2 | CI/CD (Jenkins + GitHub Actions), Docker | A pipeline that builds, tests, scans and pushes an image |
| 3 | Kubernetes, Helm, GitOps | Deploy the app on kind/minikube with Helm + Argo CD |
| 4 | Terraform, Ansible, AWS | Create a VPC + EKS (or EC2) with Terraform |
| 5 | Monitoring, SRE, DevSecOps, scenarios | Prometheus/Grafana dashboards + alerts, mock interviews |

## 1. DevOps Fundamentals

This is the base of every DevOps interview: what DevOps is, why it exists, and how teams measure it. Get these answers smooth first.

### 1.1 What is DevOps? (Almost always asked)

Earlier, companies had two separate teams. **Developers (Dev)** wrote the code. **Operations (Ops)** ran that code on servers. They worked apart and often blamed each other.

DevOps means both teams work together as one team, and they **automate** the slow, risky steps. Result: software reaches users faster and safer.

Think of a restaurant. The cooks are developers and the waiters are operations. If cooks just push plates over the counter and walk away, orders get mixed up. DevOps is cooks and waiters working as one team with a fixed, automatic system for every order.

**Memorize this 20-second answer:**

> DevOps is a culture and a set of practices that brings development and operations together to deliver software **faster, more reliably and more frequently**. It relies on **automation** (CI/CD, Infrastructure as Code, automated testing, monitoring), **small and frequent changes**, **shared ownership** of production, and **fast feedback** from users and systems.

Three things to remember:

- **DevOps is not a tool** and not only a job title. Tools like Jenkins, Docker, Kubernetes and Terraform only *help* you do DevOps.
- **The problem it solves is the "wall of confusion".** Developers are rewarded for *change* (new features). Ops are rewarded for *stability* (nothing breaks). So developers "throw code over the wall" to Ops. This causes slow releases, "works on my machine" bugs, risky manual deployments and blame games.
- **Benefits:** faster time to market, fewer failed releases, faster recovery, better teamwork, repeatable environments, easy scaling and happier teams.

### 1.2 CALMS framework

CALMS is a 5-letter checklist to see if a company is really doing DevOps.

| Letter | Meaning in simple words | Example |
| --- | --- | --- |
| **C** | **Culture:** everyone shares responsibility, works together, and learns from mistakes without blaming people | Developers also do on-call ("you build it, you run it") |
| **A** | **Automation:** machines do the builds, tests, infrastructure and deployments | CI/CD pipelines, Terraform |
| **L** | **Lean:** work in small pieces, don't start too many things at once, remove waste | Trunk-based development |
| **M** | **Measurement:** measure how work flows and what results you get | DORA metrics, SLOs |
| **S** | **Sharing:** share knowledge, tools and lessons learned | Blameless postmortems, internal docs |

### 1.3 The Three Ways

These come from the books *The Phoenix Project* and *The DevOps Handbook*. Imagine work flowing like water through a pipe.

1. **Flow:** work should move fast from left to right: Dev → Ops → customer. Use small batches, fewer hand-offs and automation.
2. **Feedback:** information should come back fast from right to left. Automated tests, monitoring, alerts and peer review catch problems early.
3. **Continual learning and experimentation:** a blameless culture, safe experiments, chaos engineering, and improving daily work a little every day.

### 1.4 DevOps lifecycle: the infinity loop (Asked often)

DevOps work never "finishes". It goes round and round like the infinity symbol (∞). What you learn from monitoring goes back into planning.

`Plan → Code → Build → Test → Release → Deploy → Operate → Monitor → (feedback) → Plan`

| Phase | What happens | Common tools |
| --- | --- | --- |
| Plan | Backlog, sprints, requirements | Jira, Azure Boards, Linear |
| Code | Version control, code review | Git, GitHub, GitLab, Bitbucket |
| Build | Compile, package, build images | Maven, Gradle, npm, Docker |
| Test | Automated tests, code quality | JUnit, PyTest, Selenium, SonarQube |
| Release | Give a version number and store the build output (artifact) | Nexus, Artifactory, ECR, GHCR |
| Deploy | Push to environments | Jenkins, GitHub Actions, GitLab CI, Argo CD, Helm |
| Operate | Run and configure infrastructure | Kubernetes, Terraform, Ansible, AWS |
| Monitor | Metrics, logs, traces, alerts | Prometheus, Grafana, ELK/Loki, OpenTelemetry, Datadog |

**Interview tip:** when asked to explain the lifecycle, walk through the phases in order and name one tool for each.

### 1.5 Waterfall vs Agile vs DevOps (Asked often)

Think of building a house. **Waterfall** plans the whole house first, builds it all, and shows the owner only at the end. **Agile** builds room by room and shows the owner after each room. **DevOps** makes sure every finished room is handed over quickly and safely, again and again.

|  | Waterfall | Agile | DevOps |
| --- | --- | --- | --- |
| Approach | One phase after another | Short repeated cycles (sprints) | Continuous flow to production |
| How often you release | Months or years | Every sprint (possible) | Many times a day possible |
| Focus | Plan everything up front | Business ↔ Dev teamwork | Dev ↔ Ops teamwork + automation |
| Feedback | At the end | After each sprint | Continuous, from production |
| Change | Expensive | Welcome | Small, frequent, low-risk |

**Key line to say:** Agile made *development* iterative. DevOps extends that to *delivery and operations*, so what Agile teams build actually reaches users quickly and safely.

### 1.6 CI vs Continuous Delivery vs Continuous Deployment (Almost always asked)

These three sound alike, so interviewers love asking them. The only real difference is **who pushes the final button to production**.

| Practice | What it means in simple words | Human approval before production? |
| --- | --- | --- |
| **Continuous Integration (CI)** | Developers merge code into the main branch often (at least daily). Every merge automatically builds the code and runs tests. | Not applicable |
| **Continuous Delivery** | Every change that passes the pipeline is **ready to release**. Going to production is a **business decision**: one click or an approval. | **Yes** |
| **Continuous Deployment** | Every change that passes all stages goes to production **automatically**. No button. | **No** |

**Memory trick:** *Delivery = ready to deploy at any time. Deployment = actually deployed automatically.* Delivery is a parcel packed and waiting at the door. Deployment is the parcel shipped the moment it is packed.

### 1.7 DORA metrics: how DevOps performance is measured (Almost always asked)

How do you know a DevOps team is doing well? **DORA** (DevOps Research and Assessment, now part of Google Cloud) gives **five numbers** in two groups: **speed (throughput)** and **stability (instability)**.

| Group | Metric | What it measures |
| --- | --- | --- |
| Speed | **Deployment frequency** | How often you deploy to production |
| Speed | **Change lead time** | Time from code commit to running in production |
| Speed | **Failed deployment recovery time** (old name: MTTR / time to restore) | How fast you recover when a deployment breaks something |
| Stability | **Change fail rate** | % of deployments that cause a failure needing a fix (rollback, hotfix) |
| Stability | **Deployment rework rate** (added 2024) | % of deployments that were unplanned, done only to fix a production incident |

- **Elite teams** (historically): deploy on demand (many times a day), lead time under a day, recovery under an hour, low change fail rate.
- **Speed and stability are not a trade-off.** Many people think going fast means breaking things. The research shows top teams are good at both.
- **Reliability** (meeting availability and latency targets) is tracked separately, as *operational* performance.
- **2025 report ("State of AI-assisted Software Development"):** about 90% of people use AI at work. AI use now goes with higher speed, but stability is still a worry. AI **amplifies** what is already there, so strong basics (small batches, good version control, good internal platforms) matter more than ever.
- Use these metrics to improve **teams and systems**, never to rank individual people.

### 1.8 DevOps vs SRE vs Platform Engineering (Asked often)

All three want fast and reliable software, but they come at it differently. **DevOps** is the idea. **SRE** is Google's concrete way of running operations with numbers. **Platform engineering** is a team that builds a self-service "internal app store" for developers.

|  | DevOps | SRE | Platform Engineering |
| --- | --- | --- | --- |
| What it is | Culture and practices | Google's engineering approach to operations ("class SRE implements interface DevOps") | A team that builds an **Internal Developer Platform (IDP)** and treats it like a product |
| Focus | Teamwork, automation, flow | Reliability using SLOs, error budgets, less toil | Self-service "golden paths" so developers have less to think about |
| Typical work | CI/CD, IaC, monitoring | SLOs, incident response, capacity, automation | Templates, portals (Backstage), ready-made CI/CD, infra APIs |

**Note:** many jobs titled "DevOps Engineer" in 2026 are really platform engineering jobs.

### 1.9 Key principles and buzzwords

Interviewers drop these words casually. Know each in one line.

- **Shift left:** do testing, security and quality checks *earlier* in the lifecycle, because early fixes are cheaper. Like checking spelling while writing, not after printing 1,000 copies.
- **Infrastructure as Code (IaC):** servers and networks are written as code files in Git (Terraform, CloudFormation), not created by clicking in a console.
- **Immutable infrastructure:** never fix a running server by hand. Build a new image or container and replace the old one.
- **Pets vs cattle:** pets are special, hand-made servers you nurse back to health. Cattle are identical servers you simply replace when one gets sick.
- **You build it, you run it:** the team that writes a service also runs it in production.
- **Everything as code:** pipelines, config, policies, dashboards and docs all live as code.
- **Small batch size:** smaller changes are easier to review, less risky and faster to fix.
- **Feature flags:** an on/off switch inside the code. You can deploy code with the feature switched off and turn it on later. Deploying is separate from releasing.
- **Value stream mapping:** draw every step from idea to production to find bottlenecks and waiting time.

### 1.10 The Twelve-Factor App (Asked often)

These are 12 rules for building apps that run well in the cloud and in containers.

| # | Factor | Meaning in simple words |
| --- | --- | --- |
| 1 | Codebase | One codebase in version control, deployed many times (dev, test, prod) |
| 2 | Dependencies | List and isolate every library clearly; never assume it is already on the server |
| 3 | Config | Keep config (DB URLs, passwords) in the environment, not in the code |
| 4 | Backing services | Treat DBs, caches and queues as plug-in resources you can swap by changing a URL |
| 5 | Build, release, run | Keep build, release and run as strictly separate stages |
| 6 | Processes | Run as stateless processes that share nothing; keep data in a DB or cache |
| 7 | Port binding | The app offers its service by listening on a port |
| 8 | Concurrency | Scale out by running more copies (instances) |
| 9 | Disposability | Start fast and shut down gracefully |
| 10 | Dev/prod parity | Keep dev, staging and prod as similar as possible |
| 11 | Logs | Treat logs as a stream of events and write them to stdout |
| 12 | Admin processes | Run one-off admin tasks as separate processes |

### 1.11 Monolith vs Microservices (Asked often)

A **monolith** is one big app: login, payments and orders all live in one codebase and are deployed together. **Microservices** split the app into small services, each doing one job and deployed on its own.

Think of one big shop versus a market of small shops. If the big shop's roof leaks, the whole shop closes. In the market, one shop can close for repair while others keep selling, but now you need roads, signboards and coordination between shops.

|  | Monolith | Microservices |
| --- | --- | --- |
| Deployment | One unit | Each service deployed on its own |
| Scaling | Scale the whole app | Scale only the busy services |
| Tech stack | Usually one | Can be different per service |
| When something fails | One bug can take down everything | Better isolation (if designed well) |
| Complexity | Simple to start, harder as it grows | Hard to operate: networking, observability, data consistency |
| Best for | Small teams, early products | Large companies, many teams, separate release cycles |

**Microservices challenges to mention:** service discovery (how services find each other), network delays and failures (use retries, timeouts, circuit breakers), distributed tracing (following one request across many services), data consistency (sagas, eventual consistency), versioned APIs, and many more CI/CD pipelines.

### 1.12 Fundamentals interview Q&A

**Q: Is DevOps a role or a culture?**

Mainly a culture and a set of practices. In companies, a "DevOps engineer" usually builds and runs the automation and platform (CI/CD, IaC, Kubernetes, observability) that lets product teams practice DevOps. A strong answer mentions both.

**Q: How is DevOps different from Agile?**

Agile improves teamwork between business and developers through short iterations. DevOps extends that teamwork to operations and automates the path to production, so changes go out often and reliably.

**Q: How do you measure whether DevOps is working?**

DORA metrics (deployment frequency, lead time, change fail rate, failed deployment recovery time, rework rate), SLO attainment, business results (feature adoption, customer satisfaction) and developer experience surveys. Track trends over time, per team.

**Q: What are the biggest challenges in a DevOps transformation?**

Cultural resistance, legacy systems, skill gaps, too many tools, teams with different goals, and security or compliance worries. Start small with one value stream, automate the biggest pain point, show the improvement with metrics, then expand.

**Q: What does "shift left" mean in practice?**

Running unit tests, static analysis, dependency scanning, IaC scanning and secret scanning in the developer's pull request pipeline (or even before commit), instead of at the end just before release.

## 2. Linux for DevOps

Almost every DevOps interview starts with Linux, because servers, containers and Kubernetes nodes all run on Linux. If your Linux is strong, everything after this becomes easier.

### 2.1 Linux architecture

Think of Linux as a building with three floors. The top floor is your apps and the shell (**user space**). The middle floor is the **kernel**, the manager that controls everything. The ground floor is the **hardware**.

```
User applications (nginx, java, python ...)
Shell (bash, zsh) + system libraries (glibc)      <- user space
--------------------------------------------------------------
Kernel: process & memory management, filesystems, <- kernel space
networking, device drivers, system calls
--------------------------------------------------------------
Hardware (CPU, RAM, disk, network card)
```

- **Kernel** = the core of the OS that talks to the hardware. Programs cannot touch hardware directly. They ask the kernel through **system calls** (`open`, `read`, `fork`, `exec` ...), like customers asking a bank cashier instead of walking into the vault.
- **Distributions** (flavours of Linux): Ubuntu/Debian (`apt`), RHEL/Rocky/AlmaLinux/Fedora/Amazon Linux (`dnf`/`yum`), Alpine (`apk`; very small, popular for containers).

### 2.2 Boot process (Asked often)

This is what happens between pressing the power button and seeing the login prompt.

1. **BIOS/UEFI:** checks the hardware (POST, power-on self-test) and finds the boot disk.
2. **Bootloader (GRUB2):** loads the kernel and the **initramfs** into memory.
3. **Kernel:** starts the hardware and mounts the initramfs, a temporary mini filesystem with the drivers needed to find the real disk. Then it mounts the real root filesystem.
4. **systemd (PID 1):** the first user-space process. It starts services in parallel.
5. **Default target:** `multi-user.target` (servers) or `graphical.target` (desktops). In old SysV terms, runlevel 3 is roughly multi-user and runlevel 5 is graphical.
6. **Login:** getty on the console, sshd for remote logins.

Commands: `systemctl get-default`, `systemctl set-default multi-user.target`

**Remember the order:** BIOS → GRUB → Kernel → systemd → Target → Login.

### 2.3 Filesystem hierarchy (Asked often)

Everything in Linux starts from `/` (root), like the trunk of a tree. Linux also says **"everything is a file"**: even devices and process details show up as files.

| Path | What lives there |
| --- | --- |
| `/` | Root of everything |
| `/etc` | System configuration (`/etc/passwd`, `/etc/hosts`, `/etc/fstab`, `/etc/ssh/sshd_config`) |
| `/var` | Data that keeps changing: logs (`/var/log`), caches, spool, `/var/lib/docker` |
| `/home`, `/root` | Home folders of users; root user's home |
| `/bin`, `/sbin`, `/usr/bin`, `/usr/sbin` | User and system programs (merged into `/usr` on modern distros) |
| `/lib`, `/usr/lib` | Shared libraries, kernel modules |
| `/opt` | Optional / third-party software |
| `/tmp` | Temporary files (often cleared on reboot) |
| `/proc` | Virtual filesystem with process and kernel info (`/proc/cpuinfo`, `/proc/meminfo`, `/proc/<pid>/`) |
| `/sys` | Virtual filesystem for devices and kernel settings (cgroups live in `/sys/fs/cgroup`) |
| `/dev` | Device files (`/dev/sda`, `/dev/null`) |
| `/boot` | Kernel, initramfs, GRUB config |
| `/mnt`, `/media` | Mount points |
| `/run` | Runtime data since boot (PID files, sockets) |

### 2.4 File permissions (Almost always asked)

Every file has permissions for three kinds of people: the **owner (u)**, the **group (g)** and **others (o)**. Each of them can have **read (r)**, **write (w)** and **execute (x)**.

```
-rwxr-x---  1 alice devs  4096 Oct 11 10:00 deploy.sh
| owner=rwx | group=r-x | others=---
first char = type: - file, d directory, l symlink
```

| Permission | Number | On a file | On a directory |
| --- | --- | --- | --- |
| r | 4 | Read the contents | List the names inside (`ls`) |
| w | 2 | Change the contents | Create, delete, rename files inside |
| x | 1 | Run it | Enter it (`cd`) and access files |

**How to get the number:** just add. rwx = 4+2+1 = **7**. r-x = 4+0+1 = **5**. --- = **0**. So `rwxr-x---` = **750**.

- `chmod 750 deploy.sh` → owner 7 (rwx), group 5 (r-x), others 0 (---). Symbolic way: `chmod u+x,g-w,o= file`.
- `chown alice:devs file` changes owner and group. `chown -R appuser:appuser /opt/app` does it for everything inside (`-R` = recursive).
- **umask** removes bits from the default permissions (files 666, dirs 777). umask `022` → files `644`, dirs `755`.

**Special permissions (Asked often)**

| Bit | Number | What it does | Example |
| --- | --- | --- | --- |
| SUID | 4000 | The program runs with the **file owner's** power | `/usr/bin/passwd` (`-rwsr-xr-x`) |
| SGID | 2000 | On a file: runs with the group's power. On a directory: new files get the directory's group | Shared team folders |
| Sticky bit | 1000 | On a directory: only the file's owner (or root) can delete or rename it | `/tmp` (`drwxrwxrwt`) |

In simple words: when you run `passwd`, SUID lets it run as root, so it can update `/etc/shadow` even though you are a normal user. In `/tmp` everyone can write, but the sticky bit means you can delete only your own files.

**ACLs** give finer control, for example read-write for just one extra user: `setfacl -m u:bob:rw file` and `getfacl file`.

### 2.5 Users, groups and sudo

- `/etc/passwd` (accounts), `/etc/shadow` (password hashes; only root can read), `/etc/group` (groups).
- UID 0 = root. Service/system accounts usually have UID below 1000.

```bash
useradd -m -s /bin/bash deploy      # create user with a home folder and shell
passwd deploy                        # set password
usermod -aG docker deploy            # add to an extra group (-a = append!)
id deploy; groups deploy             # check groups
userdel -r deploy                    # delete user + home folder
visudo                               # safely edit /etc/sudoers
```

**Gotcha:** `usermod -G docker user` **without `-a`** removes the user from all their other extra groups. Always use `-aG`.

### 2.6 Processes (Almost always asked)

- A **process** is a running program with its own PID (process ID) and its own memory. A **thread** is a lighter unit inside a process that shares the process's memory. Think of a process as a house and threads as people living in it, sharing the kitchen.
- Every process has a parent (PPID). PID 1 is `systemd` on a normal server, and **your app itself** inside a container.

**Process states** (`STAT` column in `ps`): `R` running or ready to run, `S` sleeping while waiting for something, `D` uninterruptible sleep (usually waiting for disk/NFS; cannot be killed), `T` stopped, `Z` zombie.

| Term | Simple meaning | What to do |
| --- | --- | --- |
| **Zombie** | The child finished, but the parent never collected its exit status (`wait()`). It is dead but still listed. Uses no CPU or RAM, only a PID slot | Fix or restart the parent. If the parent dies, PID 1 adopts and cleans it up. You cannot `kill` a zombie; it is already dead |
| **Orphan** | The parent died while the child is still running | PID 1 adopts it (this is normal) |
| **Daemon** | A background service with no terminal | Managed by systemd |

**Signals (Asked often):** a signal is a message you send to a process.

| Signal | Number | Meaning |
| --- | --- | --- |
| SIGHUP | 1 | Hang up; many daemons reload their config |
| SIGINT | 2 | Interrupt (Ctrl+C) |
| SIGKILL | 9 | Force kill; cannot be caught, no cleanup |
| SIGTERM | 15 | Polite "please stop"; the app can clean up (default for `kill`) |
| SIGSTOP / SIGCONT | 19 / 18 | Pause / resume |

SIGTERM is asking a guest politely to leave. SIGKILL is security throwing them out. **Always try SIGTERM before SIGKILL.** Docker and Kubernetes also send SIGTERM first, then SIGKILL after a grace period.

```bash
ps aux --sort=-%cpu | head                 # top CPU users
ps -ef | grep java                          # find a process
pgrep -a nginx; pkill -f "python app.py"
top / htop                                  # live view (P = sort by CPU, M = by memory)
kill -15 1234; kill -9 1234
nice -n 10 ./batch.sh; renice -n 5 -p 1234  # priority: -20 (highest) .. 19 (lowest)
nohup ./long_job.sh > job.log 2>&1 &        # keep running after logout
jobs; fg %1; bg %1
lsof -p 1234                                # files opened by a process
ls -l /proc/1234/fd                         # its file descriptors
```

### 2.7 CPU, memory and load average (Almost always asked)

**Load average** (`uptime`, `top`) is the average number of processes that are **running or waiting** (R + D states) over the last **1, 5 and 15 minutes**.

- Compare it with the number of CPU cores (`nproc`). Think of a bank with 4 counters. Load 4.0 on 4 cores = all counters busy. Load 8.0 = 4 people being served and 4 waiting in line.
- **High load but low CPU usage** → processes stuck in `D` state → usually **I/O wait** (slow disk or NFS). Check `%wa` in `top`, `iostat -xz 1`, `iotop`.

**Memory** (`free -h`):

- Linux uses spare RAM as **buff/cache**. That is healthy, not a leak. Look at the **`available`** column.
- **Swap** = disk used as extra memory (slow). Constant swapping (`vmstat 1`, columns `si`/`so`) means the machine has too little RAM.
- **OOM killer:** when memory runs out, the kernel kills a process (the one with the highest `oom_score`). Check with `dmesg -T | grep -i "killed process"` or `journalctl -k`.

**Toolbox:** `top`, `htop`, `vmstat 1`, `iostat -xz 1`, `mpstat -P ALL 1`, `pidstat 1`, `sar` (history), `free -h`, `nproc`, `lscpu`, `strace -p <pid>` (system calls), `perf top`.

### 2.8 Disk and storage (Almost always asked)

```bash
df -h                     # space used per filesystem
df -i                     # inode usage (can be 100% even when space is free!)
du -sh /var/log/*         # size of each item
du -xh / --max-depth=1 2>/dev/null | sort -rh | head   # biggest top-level folders
find / -xdev -type f -size +500M 2>/dev/null           # files bigger than 500 MB
lsblk; blkid              # disks and their UUIDs
mount /dev/xvdf /data     # mount (make it permanent in /etc/fstab using the UUID)
lsof +L1                  # deleted files still held open by processes
```

- **Inode** = the "ID card" of a file: type, permissions, owner, size, timestamps and where its data blocks are. It stores **everything except the filename** (names live in directory entries). Millions of tiny files can use up all inodes and cause "No space left on device" while `df -h` still shows free space.
- **Deleted-but-open files:** if a process keeps a deleted file open (for example a rotated log), the space is not freed. Find it with `lsof +L1`, then restart the process or empty the file with `: > /proc/<pid>/fd/<fd>`.
- **LVM:** Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV). Think of pouring disks (PV) into one big pool (VG), then cutting flexible partitions (LV) from it. Grow storage without downtime: `lvextend -r -L +20G /dev/vg0/data` (`-r` also resizes the filesystem).
- **RAID:** 0 = striping (speed, no safety), 1 = mirroring (a copy), 5 = striping + parity (survives one disk failure), 10 = mirror + stripe.

### 2.9 Hard link vs soft link (Almost always asked)

A **hard link** is a second name for the *same* file (same inode). A **soft link** is a shortcut, like a desktop shortcut on Windows: it only stores the path to the file.

|  | Hard link | Soft (symbolic) link |
| --- | --- | --- |
| What it is | Another name for the **same inode** | A small file holding a **path** to the target |
| If the original is deleted | Still works (data stays until the last link is removed) | Breaks (dangling link) |
| Across filesystems | No | Yes |
| Can link directories | No | Yes |
| Command | `ln file hardlink` | `ln -s target linkname` |
| Inode number | Same as the original | Different |

### 2.10 systemd and services (Asked often)

systemd is the manager that starts, stops and watches services. You talk to it with `systemctl` and read its logs with `journalctl`.

```bash
systemctl status nginx
systemctl start|stop|restart|reload nginx
systemctl enable --now nginx              # start at boot AND start now
systemctl list-units --type=service --state=failed
systemctl daemon-reload                   # after editing unit files
journalctl -u nginx -f                    # follow a service's logs
journalctl -u nginx --since "1 hour ago"
journalctl -xe                            # recent errors with explanations
```

A minimal unit file, `/etc/systemd/system/myapp.service`:

```ini
[Unit]
Description=My Python API
After=network-online.target

[Service]
User=appuser
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/python3 app.py
Restart=on-failure
RestartSec=5
Environment=PORT=8080

[Install]
WantedBy=multi-user.target
```

Reading it simply: **\[Unit\]** says what it is and to start after the network is up. **\[Service\]** says which user, which folder, which command, and to restart 5 seconds after a failure. **\[Install\]** says to start it at boot.

**restart vs reload:** `restart` stops and starts the process (a short downtime). `reload` asks the process to re-read its config without stopping (only if the app supports it, e.g. nginx).

### 2.11 Text processing: grep, awk, sed, find (Almost always asked)

One line each: **grep** searches lines, **awk** picks columns and does maths, **sed** finds-and-replaces or deletes lines, **find** searches for files.

```bash
grep -rin "error" /var/log/app/         # recursive, ignore case, show line numbers
grep -v "DEBUG" app.log                  # exclude lines
grep -c "500" access.log                 # count matching lines
grep -E "timeout|refused" app.log        # extended regex (OR)
tail -f app.log | grep --line-buffered ERROR

awk '{print $1}' access.log              # print first column
awk -F: '{print $1}' /etc/passwd         # custom separator
awk '$9 >= 500 {print $7}' access.log    # URLs that returned 5xx (combined log format)
awk '{sum += $10} END {print sum}' access.log   # add up a column (bytes sent)

sed -i 's/old/new/g' config.txt          # replace inside the file
sed -n '10,20p' file                     # print lines 10-20
sed -i '/^#/d' file                      # delete comment lines

cut -d',' -f2 data.csv
sort file | uniq -c | sort -nr           # count duplicates
wc -l file; tr 'a-z' 'A-Z' < file
find /var/log/app -name "*.log" -mtime +7 -delete       # delete logs older than 7 days
find . -type f -name "*.tmp" -print0 | xargs -0 rm -f
```

**Classic one-liner (Asked often): top 10 client IPs in an nginx/apache access log**

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```

Read it like a story: `awk` takes column 1 (the IP) → `sort` puts the same IPs together → `uniq -c` counts each one → `sort -nr` puts the biggest count first → `head -10` shows the top 10.

**Count HTTP status codes (Asked often):** `awk '{print $9}' access.log | sort | uniq -c | sort -nr`

### 2.12 Networking commands

```bash
ip a; ip r                          # addresses and routes (ifconfig/route are old)
ss -tulnp                           # listening TCP/UDP ports + which process (netstat is old)
ss -tulnp | grep :8080              # who is using port 8080? (or: lsof -i :8080)
ping -c 4 host; traceroute host; mtr host
dig example.com +short; dig +trace example.com; nslookup example.com
curl -v https://api.example.com/health       # DNS, TCP, TLS and headers in one view
curl -s -o /dev/null -w "%{http_code} %{time_total}\n" https://example.com
nc -zv db.internal 5432             # can I reach this TCP port?
tcpdump -i eth0 port 443 -nn        # capture packets
cat /etc/resolv.conf /etc/hosts     # DNS servers, fixed host entries
```

### 2.13 Package management

| Family | Install | Update everything | List installed |
| --- | --- | --- | --- |
| Debian/Ubuntu | `apt install nginx` | `apt update && apt upgrade` | `dpkg -l` |
| RHEL/Rocky/Amazon Linux | `dnf install nginx` (or `yum`) | `dnf upgrade` | `rpm -qa` |
| Alpine | `apk add nginx` | `apk upgrade` | `apk info` |

### 2.14 SSH (Almost always asked)

```bash
ssh-keygen -t ed25519 -C "me@laptop"     # key pair: ~/.ssh/id_ed25519 (+ .pub)
ssh-copy-id user@server                   # add your public key to the server's authorized_keys
ssh -i key.pem ec2-user@203.0.113.10
ssh -J bastion user@private-host          # jump through a bastion host
ssh -L 5432:db.internal:5432 bastion      # local port forward (tunnel to a private DB)
scp file user@host:/tmp/
rsync -avz ./dist/ user@host:/var/www/
```

- **How key login works:** the server keeps your **public key** in `~/.ssh/authorized_keys`. It sends a challenge. Your computer signs it with your **private key**, which never leaves your machine. The server checks the signature with the public key. It is like a bank matching your signature against the sample it already has.
- **Required permissions:** `~/.ssh` = 700, private key and `authorized_keys` = 600. Otherwise SSH refuses to use them.
- **Hardening** (`/etc/ssh/sshd_config`): `PermitRootLogin no`, `PasswordAuthentication no`, `AllowUsers`/`AllowGroups`, fail2ban, allow port 22 only from known IPs, or avoid open SSH completely with **AWS SSM Session Manager**.

### 2.15 Cron jobs (Asked often)

Cron is Linux's alarm clock: it runs commands on a schedule. Each line has 5 time fields, then the command.

```
# minute(0-59) hour(0-23) day-of-month(1-31) month(1-12) day-of-week(0-7; 0 and 7 = Sunday)
0 2 * * *     /opt/scripts/backup.sh >> /var/log/backup.log 2>&1   # every day at 02:00
*/5 * * * *   /opt/scripts/healthcheck.sh                           # every 5 minutes
0 9 * * 1-5   /opt/scripts/report.sh                                # weekdays at 09:00
0 0 1 * *     /opt/scripts/monthly.sh                               # 1st of every month
```

- `crontab -e` (edit), `crontab -l` (list). System-wide: `/etc/crontab`, `/etc/cron.d/`.
- **Gotcha:** cron runs with a **very small environment and PATH**. Use full paths and send output to a log file.
- **Modern alternative:** **systemd timers** (logs in journald, dependencies, handles missed runs).

### 2.16 Logs

- `/var/log/syslog` (Debian/Ubuntu) or `/var/log/messages` (RHEL): the general system log.
- `/var/log/auth.log` or `/var/log/secure`: logins, sudo, SSH.
- `dmesg -T`: kernel messages (hardware errors, OOM kills). `journalctl`: the systemd journal.
- **logrotate** (`/etc/logrotate.d/`) rotates, compresses and deletes old logs so the disk doesn't fill up.

### 2.17 Shell basics you must know

- `>` overwrite, `>>` append, `2>` errors (stderr), `2>&1` send errors to wherever normal output goes, `&>` both, `/dev/null` throws output away.
- `|` pipe, `&&` run the next command only if this one succeeded, `||` only if it failed, `;` run regardless.
- `~/.bashrc` (interactive non-login shells) vs `~/.bash_profile` (login shells). `source ~/.bashrc` reloads it.
- `export VAR=value` makes a variable visible to child processes. `env` / `printenv` list them.

### 2.18 Linux interview Q&A (Asked often)

**Q: What happens when you type `ls -l` and press Enter?**

1. The shell reads and parses the line, and expands aliases and wildcards (globs).
2. It searches `$PATH` to find `ls`.
3. It **forks** (makes a child copy of itself), and the child **execs** (turns into) `/usr/bin/ls`.
4. `ls` uses system calls (`openat`, `getdents64`, `statx`) to read the directory entries and inode details.
5. It writes the output to stdout (the terminal) and exits.
6. The shell **waits** for the child, stores its exit code in `$?` and shows the prompt again.

**Q: Which process is using port 8080?**

`ss -tulnp | grep :8080` or `lsof -i :8080`.

**Q: "No space left on device" but `df -h` shows free space. Why?**

The inodes are used up (`df -i`), usually by millions of small files (sessions, cache). Also check for space held by deleted-but-open files (`lsof +L1`).

**Q: `kill -9` vs `kill -15`?**

SIGTERM (15) lets the app shut down cleanly. SIGKILL (9) is instant and cannot be caught. Use 9 only when 15 fails.

**Q: How do you check the OS and kernel version?**

`cat /etc/os-release`, `uname -r`, `hostnamectl`.

**Q: What is swap, and is it bad?**

Disk space used as extra memory. A little swap is a safety net. Constant swapping means the machine needs more RAM (or the app has a memory leak).

**Q: How do you run a script automatically at boot?**

A systemd service with `WantedBy=multi-user.target` (preferred), or `@reboot` in cron.

**Q: `chmod 755` vs `chmod 644`?**

755 = owner rwx, group and others r-x (scripts, directories). 644 = owner rw, everyone else read-only (normal files).

**Q: What is `/proc`?**

A virtual filesystem the kernel creates in memory. It shows process and kernel information (`/proc/meminfo`, `/proc/<pid>/status`).

**Q: Find files modified in the last 24 hours / 30 minutes.**

`find /path -type f -mtime -1` / `find /path -type f -mmin -30`.

**Q: What does `cmd > out.log 2>&1` do?**

It sends normal output to `out.log`, then points errors (fd 2) to the same place as normal output (fd 1). Order matters: `2>&1 > out.log` would leave errors on the screen.

## 3. Networking for DevOps

Networking questions check one skill: can you find out why "the app is not reachable"? Learn the layers, how DNS and HTTPS work, and the classic 502 vs 504.

### 3.1 OSI and TCP/IP models (Almost always asked)

Sending data over a network is like sending a parcel. The OSI model splits that journey into 7 layers. Each layer does one job, from the app at the top (layer 7) down to the actual wire (layer 1).

| # | OSI layer | What it does | Examples | Where you see it in DevOps |
| --- | --- | --- | --- | --- |
| 7 | Application | Network services for apps | HTTP, DNS, SSH, SMTP | L7 load balancers, Ingress/Gateway, API gateways |
| 6 | Presentation | Encoding, encryption, compression | TLS, JSON, JPEG | TLS termination |
| 5 | Session | Opens and keeps sessions | RPC, session handling | Sticky sessions |
| 4 | Transport | End-to-end delivery, ports | TCP, UDP | L4 load balancers (NLB), security group rules |
| 3 | Network | IP addresses and routing | IP, ICMP, routers | VPCs, subnets, route tables, CIDR |
| 2 | Data Link | Frames inside one network, MAC addresses | Ethernet, ARP, switches, VLANs | Bridges, CNI internals |
| 1 | Physical | Bits on the wire or air | Cables, fiber, radio | - |

**Mnemonic (7 → 1):** **"All People Seem To Need Data Processing"** = Application, Presentation, Session, Transport, Network, Data Link, Physical.

**TCP/IP model** is the practical, real-world version with 4 layers: Application (OSI 5-7) → Transport (4) → Internet (3) → Link / Network Access (1-2).

### 3.2 TCP vs UDP (Almost always asked)

**TCP** is like a phone call: you connect first, check the other side is listening, and confirm everything was heard. **UDP** is like a loudspeaker announcement: fast, but no guarantee everyone heard it.

|  | TCP | UDP |
| --- | --- | --- |
| Connection | Connection first (handshake) | No connection |
| Reliability | Guaranteed delivery, in order, resends lost data | Best effort, no ordering |
| Flow / congestion control | Yes | No |
| Header size | 20+ bytes | 8 bytes |
| Speed | More overhead | Faster, lower delay |
| Used by | HTTP/1.1 and HTTP/2, SSH, databases, SMTP | DNS queries, video/voice, gaming, DHCP, **QUIC / HTTP/3** |

**TCP 3-way handshake:** `SYN` → `SYN-ACK` → `ACK`. In plain words: "Can we talk?" → "Yes, can you hear me?" → "Yes."

**Closing:** `FIN` → `ACK` → `FIN` → `ACK`. The side that closes first waits in `TIME_WAIT`.

### 3.3 IP addressing, CIDR and subnetting (Almost always asked)

- IPv4 = 32 bits (e.g. `10.0.1.25`). IPv6 = 128 bits.
- **CIDR** `/n` means the first *n* bits are the network part (like the street name). The rest are for hosts (house numbers). **Number of addresses = 2^(32 − n).**
- **Quick trick:** a smaller `/number` means a bigger network. `/16` is huge; `/32` is just one IP.

| CIDR | Subnet mask | Addresses | Typical use |
| --- | --- | --- | --- |
| /32 | 255.255.255.255 | 1 | One host (e.g. your IP in a security group) |
| /28 | 255.255.255.240 | 16 | Tiny subnet |
| /24 | 255.255.255.0 | 256 | Common subnet |
| /20 | 255.255.240.0 | 4,096 | Large subnet (EKS nodes + pods) |
| /16 | 255.255.0.0 | 65,536 | Whole VPC |
| /0 | 0.0.0.0 | everything | "Anywhere" (`0.0.0.0/0`) |

- **Private ranges (RFC 1918) (Asked often):** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`. These cannot travel on the internet directly. They need NAT to reach it.
- **Special addresses:** `127.0.0.0/8` is loopback (your own machine). `169.254.0.0/16` is link-local; cloud **instance metadata** lives at `169.254.169.254`. A server listening on `0.0.0.0` accepts connections on all network interfaces.
- **AWS reserves 5 IPs in every subnet (Asked often).** Normal networks lose 2 per subnet (network + broadcast). AWS takes the first four + the last, so a `/24` gives **251 usable** addresses.
- **Example:** split `10.0.0.0/16` into `/24`s → 256 subnets: `10.0.0.0/24`, `10.0.1.0/24`, ..., `10.0.255.0/24`. In Terraform: `cidrsubnet("10.0.0.0/16", 8, 1)` = `10.0.1.0/24`.

**Worked example:** how many addresses in a `/26`? 32 − 26 = 6, and 2^6 = 64. On AWS, usable = 64 − 5 = **59**.

### 3.4 DNS (Almost always asked)

DNS is the **phone book of the internet**. You know the name (`www.example.com`); DNS finds the number (the IP address).

**How `www.example.com` gets resolved:**

1. Browser cache → OS cache → `/etc/hosts`.
2. The question goes to a **recursive resolver** (your ISP, `8.8.8.8`, `1.1.1.1`, or the VPC resolver).
3. The resolver asks a **root server** → "ask the `.com` TLD servers".
4. It asks the **TLD server** → "ask example.com's authoritative name servers".
5. It asks the **authoritative server** → gets the record (e.g. an A record `203.0.113.10`).
6. The resolver saves (caches) the answer for the record's **TTL** and returns it.

Think of asking for a house address. You check your own diary first (cache). Then a helpful friend (resolver) asks head office (root), which says "ask the .com office" (TLD), which says "ask example.com's own office" (authoritative), which gives the real address.

| Record | What it is for |
| --- | --- |
| A / AAAA | Name → IPv4 / IPv6 address |
| CNAME | Alias to another name (not allowed at the zone apex, such as `example.com`) |
| MX | Mail servers |
| TXT | Free text: domain verification, SPF, DKIM, DMARC |
| NS | Authoritative name servers for the zone |
| SOA | Zone details (primary NS, serial, timers) |
| PTR | Reverse lookup (IP → name) |
| SRV | Service location (host + port) |
| CAA | Which certificate authorities may issue certificates for the domain |
| Alias (Route 53) | Like CNAME, but works at the apex and points to AWS resources (ALB, CloudFront, S3) |

- DNS uses **UDP 53** (TCP 53 for large answers and zone transfers).
- **TTL matters for migrations:** lower the TTL a day before you switch IPs, so clients pick up the change quickly.
- Debug with `dig name`, `dig +trace name`, `dig @8.8.8.8 name`, `nslookup name`.

### 3.5 HTTP and HTTPS (Almost always asked)

**Methods:** GET (read), POST (create), PUT (replace; idempotent), PATCH (partial update), DELETE (idempotent), HEAD (headers only), OPTIONS (CORS preflight). **Idempotent** means doing it once or ten times gives the same result.

**Status codes (Almost always asked).** Remember the families first: **2xx** = success, **3xx** = go somewhere else, **4xx** = the client's mistake, **5xx** = the server's mistake.

| Code | Meaning | Typical DevOps cause |
| --- | --- | --- |
| 200 / 201 / 204 | OK / Created / No Content | - |
| 301 / 302 / 304 | Moved permanently / Found (temporary) / Not modified (cache) | HTTP → HTTPS redirects |
| 400 | Bad Request | Broken request body |
| 401 | Unauthorized = **you are not logged in** | Missing or expired token |
| 403 | Forbidden = logged in, but **not allowed** | IAM/RBAC, WAF block, S3 bucket policy |
| 404 | Not Found | Wrong path, wrong ingress rule |
| 429 | Too Many Requests | Rate limiting |
| 500 | Internal Server Error | Application bug / unhandled exception |
| **502** | **Bad Gateway:** the proxy/LB got a **bad answer or no answer** from the app behind it | App crashed, wrong upstream port, app not listening, connection reset |
| **503** | **Service Unavailable** | No healthy targets, overload, maintenance mode |
| **504** | **Gateway Timeout:** the app behind it **did not answer in time** | Slow DB query, mismatched timeouts, traffic silently dropped by a firewall or security group |

**HTTP versions:** HTTP/1.1 (keep-alive, one request at a time per connection). HTTP/2 (binary, many requests at once over one TCP connection, header compression). HTTP/3 (runs over **QUIC on UDP**, faster setup, no TCP head-of-line blocking).

**TLS handshake, TLS 1.3 simplified (Asked often).** HTTPS = HTTP + TLS encryption. Before sending data, the browser and server "shake hands" to check identity and agree on a secret key.

1. **ClientHello:** the browser sends supported versions and ciphers, its key share, and SNI (the hostname).
2. **ServerHello:** the server picks a cipher, sends its key share, its **certificate** (+ intermediate chain) and a signature proving it owns the private key.
3. The browser **checks the certificate**: signed by a trusted CA, hostname matches, not expired.
4. Both sides calculate the same **symmetric session keys** (ECDHE). From now on, all data is encrypted.

- Asymmetric crypto (public/private keys) is slow, so it is used only for identity and key exchange. Fast **symmetric** ciphers (AES-GCM, ChaCha20) encrypt the actual data. Like sending a key once in a locked box, then using that key for the rest of the conversation.
- TLS 1.3 needs 1 round trip (TLS 1.2 needed 2). Certificates come from Let's Encrypt, AWS ACM, or cert-manager in Kubernetes. **Monitor certificate expiry**: expired certificates cause many outages.

### 3.6 Ports to remember (Asked often)

| Port | Service | Port | Service |
| --- | --- | --- | --- |
| 22 | SSH | 3306 | MySQL |
| 25 | SMTP | 5432 | PostgreSQL |
| 53 | DNS | 6379 | Redis |
| 80 / 443 | HTTP / HTTPS | 27017 | MongoDB |
| 123 | NTP (UDP) | 9092 | Kafka |
| 389 / 636 | LDAP / LDAPS | 2049 | NFS |
| 3389 | RDP | 8080 | Jenkins / alternate HTTP |
| 6443 | Kubernetes API server | 2379-2380 | etcd |
| 10250 | kubelet API | 30000-32767 | Kubernetes NodePort range |
| 9090 | Prometheus | 9100 | node\_exporter |
| 3000 | Grafana | 9093 | Alertmanager |
| 9200 / 5601 | Elasticsearch / Kibana | 9000 | SonarQube |
| 8081 | Nexus | 8200 | HashiCorp Vault |

### 3.7 Load balancers and proxies (Almost always asked)

A load balancer is like a hospital receptionist who sends each patient to a free doctor. It keeps checking which doctors are present (**health checks**) and never sends anyone to an absent one. This gives **high availability** and lets you **scale horizontally**.

- **L4 load balancer** (transport layer): routes by IP and port. Very fast, does not read HTTP (AWS NLB).
- **L7 load balancer** (application layer): routes by host, path, headers or cookies. It can end TLS, do redirects and work with a WAF (AWS ALB, NGINX, HAProxy, Envoy, Traefik).
- **Algorithms:** round robin, weighted round robin, least connections, least response time, IP hash / consistent hashing.
- **Sticky sessions** keep one user on the same backend. Better design: stateless apps that keep sessions in Redis or a DB.
- **Forward proxy** sits in front of **clients**. Example: a company proxy that hides employees and filters what they access. **Reverse proxy** sits in front of **servers**. It hides the servers and does load balancing, TLS, caching and compression (e.g. NGINX, Envoy).
- **API gateway** = reverse proxy + authentication, rate limiting, request changes and API keys (Kong, AWS API Gateway).

### 3.8 NAT, firewalls, VPN, CDN

- **NAT** swaps private source IPs for a public IP, so private machines can reach the internet (outgoing only). In AWS: **NAT Gateway**.
- **Stateful firewall** remembers connections, so reply traffic is allowed automatically (AWS **Security Groups**). It is like a guard who remembers you went out and lets you back in. A **stateless** firewall checks every packet with no memory, so you must allow reply traffic yourself (AWS **NACLs**).
- Linux firewalls: `iptables` / `nftables`, `ufw` (Ubuntu), `firewalld` (RHEL).
- **VPN** = an encrypted tunnel between two networks (site-to-site) or between a user and a network.
- **CDN** keeps copies of content at edge locations near users (CloudFront, Cloudflare, Akamai). Result: lower delay, less load on your main server, and protection from DDoS.

### 3.9 "What happens when you type https://www.google.com in a browser?" (Must know)

This is one of the most famous interview questions. Tell it as a story, step by step.

1. The browser checks its caches and the HSTS list (which forces HTTPS).
2. **DNS** resolution (browser → OS → resolver → root → TLD → authoritative) returns an IP.
3. **TCP 3-way handshake** to port 443 (or a **QUIC** handshake over UDP for HTTP/3).
4. **TLS handshake:** certificate check, session keys agreed.
5. The browser sends an **HTTP GET** with headers and cookies.
6. The request reaches a **CDN/edge** → **load balancer** → **web/app servers**, which may call caches, databases and other services.
7. The server returns an **HTTP response** (status, headers, HTML).
8. The browser **reads the HTML**, fetches CSS/JS/images (often in parallel over HTTP/2 or HTTP/3), builds the DOM and CSSOM, runs JavaScript and **draws** the page.
9. The connection is kept open for reuse.

### 3.10 Network troubleshooting checklist (Asked often)

Go from the bottom up, one check at a time.

1. **DNS:** does the name resolve correctly? `dig name`
2. **Reachability:** `ping` (ICMP may be blocked), `traceroute` / `mtr`
3. **Is the port open?** `nc -zv host port`, `curl -v`
4. **Is the service listening?** On the server: `ss -tulnp`. Is it bound to `0.0.0.0` or only to `127.0.0.1`?
5. **Firewalls:** iptables/ufw, security groups, NACLs, Kubernetes NetworkPolicies
6. **Routing:** route tables, NAT/IGW, peering routes
7. **TLS:** `openssl s_client -connect host:443 -servername host` (expiry, chain, hostname)
8. **Logs:** application logs and LB/proxy logs (502 vs 504 tells you a lot)

**Rule of thumb:** "It's always DNS", so check DNS first.

### 3.11 Networking interview Q&A

**Q: Difference between 502 and 504?**

502 = the app behind the proxy sent a bad answer or no answer at all (crash, wrong port, reset). 504 = the app was too slow, so the proxy gave up waiting (timeout).

**Q: Why can't a CNAME exist at the zone apex?**

The apex must hold SOA and NS records, and a CNAME cannot sit together with other records on the same name. Use an ALIAS/ANAME record (Route 53 Alias) instead.

**Q: How many usable IPs are in a /26 subnet on AWS?**

64 − 5 = **59**.

**Q: What is a default gateway?**

The router IP a machine sends traffic to when the destination is outside its own subnet.

**Q: Why do we need NAT if we have an Internet Gateway?**

The Internet Gateway gives internet access to resources with public IPs. Resources in a private subnet have no public IP. A NAT Gateway (placed in a public subnet) lets them make outgoing connections without being reachable from the internet.

## 4. Git and Version Control

Git questions are almost certain. Interviewers mostly ask the differences (fetch vs pull, merge vs rebase, reset vs revert) and what you would do in an emergency, like a leaked key.

### 4.1 Concepts

Git is like a "save game" system for code. Every commit is a save point you can go back to.

- **Distributed VCS:** every clone has the **full history** (unlike centralized SVN, where history lives only on the server). So you can work offline, and every copy is also a backup.
- **The areas:** Working directory → (`git add`) → **Staging area / index** → (`git commit`) → **Local repository** → (`git push`) → **Remote** (GitHub/GitLab/Bitbucket). Think of it as: your desk → packing items into a box → sealing and labelling the box → sending the box to the warehouse.
- **Commit** = a snapshot of the project + details (author, message, parent commit), identified by a SHA hash.
- **Branch** = a lightweight, movable pointer (like a sticky note) on a commit. **HEAD** = the pointer to where you are right now (your current branch/commit). **Detached HEAD** = HEAD points straight at a commit, not at a branch.
- **Tag** = a fixed pointer to a commit, used for releases (`v1.4.2`). Annotated tags also store author, date and message. A branch moves forward with new commits; a tag never moves.

### 4.2 Everyday commands (Asked often)

```bash
git clone <url>; git status; git add -p; git commit -m "feat: add health endpoint"
git log --oneline --graph --decorate --all
git diff; git diff --staged
git switch -c feature/login           # create + switch branch (older: git checkout -b)
git push -u origin feature/login
git fetch origin; git pull --rebase
git merge main; git rebase main
git stash push -m "wip"; git stash list; git stash pop
git tag -a v1.2.0 -m "release 1.2.0"; git push origin v1.2.0
git restore file.txt                  # throw away changes in the working directory
git restore --staged file.txt         # unstage
git log -p -- path/file; git blame path/file
git reflog                            # history of where HEAD moved (rescue tool)
git bisect start; git bisect bad; git bisect good v1.0   # binary search for a bad commit
git cherry-pick <sha>
git clean -fd                         # delete untracked files and folders
```

### 4.3 The most-asked Git differences (Must know)

| Question | Answer in simple words |
| --- | --- |
| `git fetch` vs `git pull` | `fetch` downloads remote changes into remote-tracking branches (`origin/main`) **without touching your branch**. `pull` = `fetch` + `merge` (or `rebase` with `--rebase`). |
| `merge` vs `rebase` | Merge joins two histories with a **merge commit** (history stays as it happened; safe on shared branches). Rebase **replays your commits on top** of another branch: a clean straight history, but the **commit IDs change**. Golden rule: **never rebase a branch other people have pulled**. |
| `reset` vs `revert` | `reset` moves the branch pointer backwards (rewrites history; use it locally). `revert` adds a **new commit that cancels** an old one (safe on shared branches). |
| `reset --soft / --mixed / --hard` | soft: undo the commit, keep the changes **staged**. mixed (default): keep the changes **unstaged**. hard: **throw the changes away**. |
| Fast-forward vs 3-way merge | Fast-forward just moves the pointer when the histories have not split. A 3-way merge creates a merge commit when both sides have new commits. |
| `cherry-pick` | Copy one specific commit onto the current branch (e.g. a hotfix into a release branch). |
| `stash` | Put unfinished work aside for a while, so you can switch to something else. |
| Squash | Combine several commits into one (`git rebase -i`, or "Squash and merge" on a PR). |
| Fork vs clone | Fork = a copy on the server, in your account (a GitHub feature). Clone = a copy on your own machine. |
| `git rm --cached file` | Stop tracking a file but keep it on disk (then add it to `.gitignore`). |

**Picture merge vs rebase like roads.** Merge joins two roads with a junction (the merge commit). Rebase lifts your road and places it at the end of the other road, so it looks like one straight road.

**Picture reset vs revert like an account book.** Reset tears out pages. Revert writes a new correction entry. On a shared book, you never tear out pages.

### 4.4 Merge conflicts

A conflict happens when the **same lines changed on both branches**, and Git cannot decide which one to keep:

```
<<<<<<< HEAD
timeout = 30
=======
timeout = 60
>>>>>>> feature/x
```

How to fix: edit the file to the correct result → `git add file` → `git commit` (or `git rebase --continue`).

How to prevent: small PRs, sync with `main` often, and use trunk-based development.

### 4.5 Branching strategies (Almost always asked)

| Strategy | How it works | Best for | Downsides |
| --- | --- | --- | --- |
| **GitFlow** | `main` (prod), `develop`, `feature/*`, `release/*`, `hotfix/*` | Versioned, scheduled releases (mobile apps, on-prem products) | Heavy, long-lived branches, painful merges, slows CD |
| **GitHub Flow** | `main` is always deployable; short-lived feature branch → PR → review → merge → deploy | Web apps / SaaS | Needs good automated tests |
| **Trunk-Based Development** | Everyone merges into `main` at least daily through very short-lived branches; unfinished work is hidden behind **feature flags** | High-performing CI/CD teams (DORA research links it to better delivery) | Needs testing discipline and feature flags |
| Release branches | Cut `release/1.4` from main to stabilise it and apply hotfixes | Supporting several versions at the same time | Extra work to back-port fixes |

**Anti-pattern:** **environment branches** (`dev`, `qa`, `prod` as branches) cause drift and painful merges. Instead, build once and promote the **same artifact** through the environments.

### 4.6 Good practices

- **Branch protection:** required reviews, required status checks (CI must pass), no force-push to `main`, signed commits, `CODEOWNERS`.
- **Conventional commits** (`feat:`, `fix:`, `chore:`) make automatic changelogs and semantic versioning possible.
- **Git hooks:** `pre-commit` (lint, format, **secret scanning**), `commit-msg`, `pre-push`, and server-side `pre-receive`. The `pre-commit` framework makes hooks easy to share.
- **`.gitignore`:** build outputs, `node_modules`, `.env`, `*.tfstate`, keys.
- Large binary files → Git LFS.

### 4.7 Git interview Q&A

**Q: How do you undo the last commit?**

Not pushed yet: `git reset --soft HEAD~1` (keep changes) or `git reset --hard HEAD~1` (throw them away). Already pushed to a shared branch: `git revert <sha>`.

**Q: You committed and pushed an AWS key. What do you do? (Almost always asked)**

1. **Revoke or rotate the key immediately.** Assume it is already stolen; bots scan public repos within minutes.
2. Check audit logs (e.g. CloudTrail) for misuse.
3. Remove it from history (`git filter-repo` or BFG), force-push, and ask teammates to re-clone.
4. Stop it happening again: secret scanning with push protection, pre-commit hooks (gitleaks), secrets in a secret manager, and OIDC instead of fixed keys in CI.

**Q: I lost commits after `git reset --hard`.**

`git reflog` → find the SHA → `git reset --hard <sha>` (or `git branch rescue <sha>`).

**Q: How do you find which commit introduced a bug?**

`git bisect`: a binary search between a known good commit and a known bad commit.

**Q: `git checkout` vs `git switch` / `git restore`?**

`checkout` did two jobs (switching branches and restoring files). Git 2.23 split them into `switch` and `restore` to make things clearer.

## 5. Shell Scripting and Python

Interviewers often ask you to write a small script on the spot. Learn the bash basics, the special variables and the exit codes, then practise the scripts in 5.2 until you can write them without looking.

### 5.1 Bash essentials (Almost always asked)

A bash script is just a list of commands saved in a file and run from top to bottom. The first line (the **shebang**) tells Linux which program runs it. The second line, `set -euo pipefail`, is a **seatbelt**: it stops the script as soon as something goes wrong, instead of carrying on blindly.

```bash
#!/usr/bin/env bash
set -euo pipefail          # -e: exit on error, -u: error on unset variables,
                           # -o pipefail: a pipeline fails if ANY command in it fails
# set -x                   # debug mode: print each command (or run: bash -x script.sh)

NAME="world"               # no spaces around =
echo "Hello, ${NAME}"      # always quote variables
TODAY=$(date +%F)          # command substitution (store a command's output)
COUNT=$((3 + 4))           # arithmetic

if [[ -f /etc/os-release ]]; then echo "file exists"; fi
if [[ -z "${VAR:-}" ]]; then echo "VAR is empty or unset"; fi
if (( COUNT > 5 )); then echo "big"; fi

for host in web1 web2 web3; do echo "checking $host"; done
while IFS= read -r line; do echo "$line"; done < servers.txt   # read a file line by line

case "${1:-}" in
  start) echo "starting" ;;
  stop)  echo "stopping" ;;
  *)     echo "usage: $0 {start|stop}"; exit 1 ;;
esac

log() { local level=$1; shift; echo "$(date '+%F %T') [$level] $*"; }
log INFO "deployment started"

trap 'rm -f /tmp/deploy.lock' EXIT    # cleanup runs on normal exit and on errors
```

Three habits that save you:

- **No spaces around `=`** when setting a variable: `NAME="world"`, never `NAME = "world"`.
- **Always quote variables** (`"$NAME"`), so a value with spaces does not break the command.
- **`trap ... EXIT`** means "whatever happens, run this cleanup at the end".

**Special variables (Asked often)**

| Variable | Meaning |
| --- | --- |
| `$0` | Script name |
| `$1` ... `$9` | Positional arguments |
| `$#` | Number of arguments |
| `$@` | All arguments as separate words (use `"$@"`) |
| `$*` | All arguments as one string |
| `$?` | Exit status of the last command (0 = success) |
| `$$` | PID of the current shell |
| `$!` | PID of the last background job |

**Test operators:** `-f` file exists, `-d` directory, `-e` exists, `-r`/`-w`/`-x` permissions, `-z` empty string, `-n` not empty, `-eq -ne -lt -le -gt -ge` for numbers, `==` / `!=` for strings.

**Exit codes worth memorizing (Asked often).** You will see these in Docker and Kubernetes too. Trick: **128 + signal number** = killed by that signal.

| Code | Meaning |
| --- | --- |
| 0 | Success |
| 1 | General error |
| 2 | Wrong use of a builtin / bad arguments |
| 126 | Found but not executable (permissions) |
| 127 | Command not found (typo, missing program, wrong PATH) |
| 130 | Stopped by Ctrl+C (128 + 2) |
| **137** | Killed by **SIGKILL** (128 + 9); in containers usually **OOMKilled** |
| **143** | Stopped by **SIGTERM** (128 + 15); a normal graceful stop |

**Quotes:** `'$HOME'` is printed exactly as written; `"$HOME"` is replaced by the variable's value.

### 5.2 Scripts interviewers love (Almost always asked)

**1. Disk usage alert.** Checks every filesystem and prints an alert when usage crosses a limit (default 80%).

```bash
#!/usr/bin/env bash
set -euo pipefail
THRESHOLD=${1:-80}
df -hP -x tmpfs -x devtmpfs | awk 'NR>1 {gsub("%","",$5); print $5, $6}' |
while read -r usage mount; do
  if (( usage >= THRESHOLD )); then
    echo "ALERT: ${mount} is ${usage}% full on $(hostname)"
  fi
done
```

How it works: `df` lists the disks → `awk` removes the `%` sign and prints usage + mount point → the loop compares each one with the limit.

**2. Restart a service if it is down.** If the service is not active, write a log line and restart it. Run it from cron every few minutes.

```bash
#!/usr/bin/env bash
SERVICE=${1:-nginx}
if ! systemctl is-active --quiet "$SERVICE"; then
  echo "$(date '+%F %T') $SERVICE is down - restarting" >> /var/log/service-watch.log
  systemctl restart "$SERVICE"
fi
```

**3. Website health check.** Gets only the HTTP status code. 200 = UP. Anything else = DOWN, and the script exits with 1 so cron or CI knows it failed.

```bash
#!/usr/bin/env bash
URL=${1:?usage: $0 <url>}
CODE=$(curl -s -o /dev/null -w '%{http_code}' --max-time 10 "$URL" || true)
if [[ "$CODE" == "200" ]]; then
  echo "UP   $URL"
else
  echo "DOWN $URL (HTTP $CODE)"
  exit 1
fi
```

**4. Backup with 7-day retention.** Makes a dated `.tar.gz` of the app folder, then deletes backups older than 7 days.

```bash
#!/usr/bin/env bash
set -euo pipefail
SRC=/var/www/app
DEST=/backups
STAMP=$(date +%F_%H%M)
mkdir -p "$DEST"
tar -czf "$DEST/app_${STAMP}.tar.gz" -C "$SRC" .
find "$DEST" -name 'app_*.tar.gz' -mtime +7 -delete
echo "Backup created: $DEST/app_${STAMP}.tar.gz"
```

**5. Run a command on many servers.** Reads hostnames from `servers.txt` and runs `uptime` and `df` on each one over SSH.

```bash
#!/usr/bin/env bash
while IFS= read -r host; do
  echo "== $host =="
  ssh -n -o ConnectTimeout=5 "$host" 'uptime; df -h /' || echo "FAILED: $host"
done < servers.txt
```

**Why `ssh -n`?** Without it, ssh eats the loop's input (the rest of `servers.txt`). This is a classic bug where the loop runs only for the first host.

**6. Handy one-liners**

```bash
ps aux --sort=-%mem | head -6                        # top memory processes
grep -c "ERROR" app.log                              # count errors
find /var/log -name "*.log" -size +100M               # large logs
docker ps -q -f status=exited | xargs -r docker rm    # remove exited containers
kubectl get pods -A | grep -v Running                 # pods that aren't Running
```

### 5.3 Python for DevOps

**Simple rule:** use Bash to glue a few commands together. Switch to Python when the logic gets complex: APIs, JSON/YAML processing, cloud automation, proper error handling and tests.

| Library | Used for |
| --- | --- |
| `subprocess`, `os`, `pathlib`, `shutil` | Run commands, work with files |
| `requests` | REST APIs, webhooks |
| `json`, `yaml` (PyYAML) | Read config files |
| `boto3` | AWS automation |
| `kubernetes` | Kubernetes API client |
| `paramiko`, `fabric` | SSH automation |
| `argparse`, `logging` | Build command-line tools |

**Example: find unattached EBS volumes to save cost (Asked often).** Volumes with status "available" are not attached to any server, but you still pay for them. This script lists them and adds up their size. The paginator reads long lists page by page.

```python
import boto3

ec2 = boto3.client("ec2", region_name="ap-south-1")
paginator = ec2.get_paginator("describe_volumes")

total_gib = 0
for page in paginator.paginate(Filters=[{"Name": "status", "Values": ["available"]}]):
    for vol in page["Volumes"]:
        total_gib += vol["Size"]
        print(f"{vol['VolumeId']}  {vol['Size']} GiB  created {vol['CreateTime']:%Y-%m-%d}")

print(f"Unattached storage: {total_gib} GiB")
```

**Example: count HTTP status codes in an access log.** This is the Python version of the awk one-liner from Linux. `Counter` counts how many times each code appears.

```python
from collections import Counter

codes = Counter()
with open("access.log") as f:
    for line in f:
        parts = line.split()
        if len(parts) > 8:
            codes[parts[8]] += 1

for code, count in codes.most_common():
    print(code, count)
```

### 5.4 Scripting interview Q&A

**Q: `$@` vs `$*`?**

Quoted `"$@"` keeps each argument separate (safe with spaces). `"$*"` joins all arguments into one string.

**Q: How do you debug a shell script?**

`bash -x script.sh` or `set -x`. Use `shellcheck` to catch mistakes without running it.

**Q: How do you keep a script running after logout?**

`nohup ... &`, `tmux`/`screen`, or (best for services) a systemd unit.

**Q: Bash or Python?**

Bash for gluing commands and short tasks. Python for complex logic, APIs, data processing, easier maintenance and unit tests.

**Q: What does `set -o pipefail` change?**

Without it, `false | true` returns 0 (the status of the last command only). With it, the pipeline returns the failing command's status.

## 6. Build Tools and Artifact Management

A build turns source code into something you can run, called an **artifact**. Interviewers want to hear "build once, deploy many" and how you version and store artifacts.

### 6.1 Build tools

Every language has its own build tool. It compiles the code, downloads libraries and packages the app. The manifest file is its recipe.

| Ecosystem | Tool | Manifest (recipe) | Typical CI command |
| --- | --- | --- | --- |
| Java | Maven | `pom.xml` | `mvn -B clean verify` |
| Java/Kotlin | Gradle | `build.gradle(.kts)` | `./gradlew build` |
| Node.js | npm / yarn / pnpm | `package.json` + lockfile | `npm ci && npm run build` |
| Python | pip / poetry / uv | `requirements.txt` / `pyproject.toml` | `pip install -r requirements.txt` |
| Go | go | `go.mod` | `go build ./...` |
| .NET | dotnet | `*.csproj` | `dotnet publish -c Release` |

**Maven lifecycle (Asked often):** `validate → compile → test → package → verify → install → deploy`

- Running a phase also runs **all the phases before it**. So `mvn package` also compiles and tests. Like climbing stairs: to reach step 4 you pass steps 1, 2 and 3.
- `install` copies the artifact to your local repository (`~/.m2`). `deploy` uploads it to a remote repository (Nexus/Artifactory).
- `clean` is a separate lifecycle (deletes `target/`). `-DskipTests` skips running tests.
- `SNAPSHOT` versions (`1.0-SNAPSHOT`) are development builds that can change. Release versions never change.

**`npm ci` vs `npm install`:** `npm ci` installs exactly what the lockfile says, fails if the lockfile is out of sync, and is faster and repeatable. So use `npm ci` in CI.

### 6.2 Artifacts (Asked often)

- **Artifact** = the output of a build: JAR/WAR, Docker image, npm package, Python wheel, binary, Helm chart. Think of it as the finished, packed product coming out of a factory.
- **Artifact repositories:** Nexus, JFrog Artifactory, GitHub Packages, AWS CodeArtifact. **Container registries:** Docker Hub, Amazon ECR, Azure ACR, Google Artifact Registry, GHCR, Harbor (self-hosted).
- **Build once, deploy many (Asked often):** build the artifact **once**, then **promote the same artifact** (same image digest) from dev → staging → prod. Add environment-specific config only at deploy time. If you rebuild for each environment, production runs something you never tested. It is like medicine: the exact batch that passed the lab test is the batch that gets sold.
- **Semantic versioning** `MAJOR.MINOR.PATCH` (e.g. `2.4.1`): MAJOR = breaking change, MINOR = new feature that breaks nothing, PATCH = bug fix. Pre-release: `1.0.0-rc.1`.
- **Image tags:** don't deploy `latest`. It keeps changing, so you never know what is really running. Use fixed tags such as the **git SHA** and/or semver (`myapp:1.4.2`, `myapp:3f9c2ab`). For maximum safety, pin by **digest** (`myapp@sha256:...`). Turn on tag immutability in the registry.
- **Retention/cleanup policies** (e.g. ECR lifecycle policies) stop registries from growing forever.

### 6.3 Code quality and testing

- **SonarQube:** checks code without running it (static analysis) for bugs, vulnerabilities, code smells, duplication and test coverage. A **Quality Gate** (e.g. "coverage on new code ≥ 80%, no new critical issues") can fail the pipeline.
- **Linters:** ESLint, ruff/pylint, `shellcheck` (bash), `hadolint` (Dockerfiles), `tflint` (Terraform), `yamllint`, `kubeconform` / `kube-linter` (Kubernetes manifests).
- **Testing pyramid (Asked often):** many fast **unit** tests at the bottom → fewer **integration** tests in the middle → a few slow **end-to-end/UI** tests at the top. A wide base of cheap, fast tests; a narrow top of slow, costly ones.
- **Other test types:** **smoke** (a quick check right after deploy), **regression**, **performance/load** (JMeter, k6, Gatling, Locust), **security** (SAST/DAST, see section 18), and **contract tests** (Pact) between microservices.
- **Flaky tests** pass or fail randomly, and they destroy trust in CI. Track them, quarantine them, and fix the root cause (timing, shared state, external dependencies).

## 7. CI/CD Pipelines

A CI/CD pipeline is an automatic assembly line for code. Every change goes through the same checks, in the same order, before it reaches users. Expect to explain a pipeline you built, stage by stage.

### 7.1 What a pipeline looks like (Almost always asked)

Think of a car factory. Parts arrive (code push) → the car is assembled (build) → quality is checked (tests, scans) → it is packed (image) → stored in a warehouse (registry) → delivered to showrooms one by one (dev, staging, prod) → customer feedback goes back to the factory (monitoring).

```
Developer push / pull request
   |
   v
[CI]  checkout -> build -> unit tests -> lint / SonarQube quality gate
      -> security scans (SAST, SCA, secrets) -> build image -> scan image
      -> push image to registry (tag = git SHA)
   |
   v   (the SAME artifact is promoted)
[CD]  deploy to dev -> integration / E2E tests -> deploy to staging
      -> manual approval (Continuous Delivery) -> deploy to prod (canary / blue-green)
      -> smoke tests + metric checks -> automatic rollback on failure
   |
   v
Monitor -> dashboards / alerts -> feedback to developers
```

**Interview tip:** when asked "explain your pipeline", walk through this picture from top to bottom and name the tool you used at each step.

### 7.2 CI/CD best practices (Almost always asked)

1. **Pipeline as code** (Jenkinsfile, `.github/workflows/*.yml`, `.gitlab-ci.yml`), stored in Git and reviewed like application code.
2. **Fail fast:** run the cheap checks (lint, unit tests) first.
3. **Build once, promote the same artifact** through the environments.
4. **Keep CI fast** (aim for under 10 minutes): cache dependencies and Docker layers, run jobs in parallel, split tests, build only what changed (monorepos).
5. **Fresh, isolated runners/agents** (containers or pods) with a clean state for every build.
6. **Secrets** never in code or logs. Use the CI secret store or a vault, and prefer **OIDC short-lived cloud credentials** over fixed access keys.
7. **Security scanning built in** (SAST, SCA, secrets, images, IaC) with sensible failure limits.
8. **Automatic deployment checks and rollback** (health checks, metrics, canary analysis).
9. **Everything versioned and immutable:** images, charts, IaC modules.
10. **Fast notifications** (Slack/Teams) and a visible pipeline status.
11. **Trunk-based development + feature flags** keep `main` always releasable.

### 7.3 Jenkins (Almost always asked; very common in Indian service companies)

Jenkins works like a manager and workers. The **controller** (manager) plans and assigns the work. The **agents** (workers) actually run the builds.

**Architecture**

- **Controller** (old name "master"): web UI, configuration, scheduling, plugins. Everything is stored in `JENKINS_HOME`.
- **Agents** (old name "slaves"): machines or containers that run builds. They connect over **SSH** or as **inbound agents** (JNLP/WebSocket).
- **Executors** = how many builds one agent can run at the same time.
- **Best practice:** set the controller's executors to **0** (never build on the controller) and use **dynamic agents** (Kubernetes plugin pods, Docker, EC2 fleets).

**Job types:** Freestyle (set up in the UI; old style), **Pipeline** (Jenkinsfile), **Multibranch Pipeline** (automatically creates a pipeline for every branch/PR that has a Jenkinsfile), Organization Folders.

**Declarative vs Scripted pipelines (Almost always asked).** Declarative is like filling a fixed form: easy to read and hard to get wrong. Scripted is free-form Groovy code: powerful, but messy.

| Declarative | Scripted |
| --- | --- |
| `pipeline { ... }` with a fixed structure | `node { ... }` with plain Groovy |
| Easy to read, syntax is checked, built-in `post`, `when`, `options` | Very flexible (loops, complex logic) but harder to maintain |
| The recommended default | For special cases, or use `script { }` blocks inside declarative |

**Features to mention:** triggers (GitHub/GitLab **webhooks**, better than Poll SCM), parameters, `credentials()` / `withCredentials` (secrets hidden in logs), **Shared Libraries** (reusable Groovy steps across repos via `@Library('my-lib') _`), `parallel` stages, `input` approvals, `stash`/`unstash`, `archiveArtifacts`, `junit` reports, `post { always | success | failure | cleanup }`, **Configuration as Code (JCasC)**.

**Sample declarative Jenkinsfile (Asked often)**

```groovy
pipeline {
  agent any                       // real setups: a Kubernetes/Docker agent label

  environment {
    AWS_REGION = 'ap-south-1'
    REGISTRY   = '123456789012.dkr.ecr.ap-south-1.amazonaws.com'
    IMAGE      = "${REGISTRY}/myapp"
  }

  options {
    timeout(time: 30, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '20'))
    disableConcurrentBuilds()
  }

  stages {
    stage('Init') {
      steps {
        script {
          env.TAG = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
        }
      }
    }

    stage('Build & Unit Test') {
      steps { sh 'mvn -B clean verify' }
      post { always { junit 'target/surefire-reports/*.xml' } }
    }

    stage('Code Quality') {
      steps {
        withSonarQubeEnv('sonarqube') { sh 'mvn -B sonar:sonar' }
        timeout(time: 5, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }

    stage('Build & Scan Image') {
      steps {
        sh 'docker build -t $IMAGE:$TAG .'
        sh 'trivy image --exit-code 1 --severity CRITICAL,HIGH $IMAGE:$TAG'
      }
    }

    stage('Push Image') {
      steps {
        // the agent uses an IAM role; otherwise wrap this in withCredentials
        sh '''
          aws ecr get-login-password --region $AWS_REGION |
            docker login --username AWS --password-stdin $REGISTRY
          docker push $IMAGE:$TAG
        '''
      }
    }

    stage('Deploy to Staging') {
      steps {
        // Helm 4 flag; on Helm 3 use --atomic
        sh 'helm upgrade --install myapp ./chart -n staging --set image.tag=$TAG --wait --rollback-on-failure'
      }
    }

    stage('Deploy to Production') {
      when { branch 'main' }      // works in Multibranch pipelines
      steps {
        input message: "Deploy ${env.TAG} to production?"
        sh 'helm upgrade --install myapp ./chart -n prod --set image.tag=$TAG --wait --rollback-on-failure'
      }
    }
  }

  post {
    success { echo "Deployed ${env.TAG}" }
    failure { echo 'Build failed - notify the team (Slack/email plugin)' }
    always  { cleanWs() }
  }
}
```

**What each part does, in simple words:**

- **environment:** variables used everywhere (region, registry, image name).
- **options:** stop after 30 minutes, keep only the last 20 builds, never run two builds at once.
- **Init:** take the short git commit ID and use it as the image tag.
- **Build & Unit Test:** compile and test with Maven; always publish the JUnit test report.
- **Code Quality:** run SonarQube; stop the pipeline if the quality gate fails.
- **Build & Scan Image:** build the Docker image; Trivy fails the build if it finds CRITICAL or HIGH vulnerabilities.
- **Push Image:** log in to ECR and push the image.
- **Deploy to Staging:** deploy with Helm, wait until it is healthy, and roll back automatically if it fails.
- **Deploy to Production:** only from `main`, and only after a human approves (`input`).
- **post:** print a message on success or failure; always clean the workspace.

**Tip:** in real pipelines, keep long manual approvals outside an agent (or use the stage-level `input` directive), so a waiting build does not block an executor.

**Jenkins Q&A**

- **How do you trigger builds on every push?** A webhook from GitHub/GitLab to Jenkins (Multibranch / GitHub plugin), not polling.
- **How do you store secrets?** Jenkins Credentials (or the Vault plugin). Use `withCredentials` and never echo them.
- **How do you back up Jenkins?** Back up `JENKINS_HOME` (jobs, config, plugins, credentials). Better: make Jenkins rebuildable with JCasC + Jenkinsfiles in Git.
- **A build is stuck in the queue. Why?** No free executors, agents offline, label mismatch, the controller has 0 executors and no agents are connected, or throttling plugins.
- **How do you scale Jenkins?** Distributed or temporary agents (Kubernetes plugin), separate controllers per team/area, and keep the controller light.
- **What is a Shared Library?** A versioned Git repo of Groovy code with reusable pipeline steps (`vars/`) and classes (`src/`) shared by many pipelines.

### 7.4 GitHub Actions (Almost always asked)

| Term | Meaning |
| --- | --- |
| Workflow | A YAML file in `.github/workflows/` |
| Event / trigger | `push`, `pull_request`, `schedule` (cron), `workflow_dispatch` (manual), `release`, `workflow_call` (reusable workflow) |
| Job | A group of steps on one runner; jobs run **in parallel** unless linked with `needs:` |
| Step | A shell command (`run:`) or an action (`uses:`) |
| Action | A reusable building block (e.g. `actions/checkout`) |
| Runner | The machine that runs jobs: GitHub-hosted (`ubuntu-latest`) or self-hosted (autoscaled on Kubernetes with Actions Runner Controller) |
| Secrets / variables | Encrypted secrets and plain config at repo, org or environment level |
| Environment | e.g. `production`, with protection rules (required reviewers, wait timers) |
| Matrix | Run the same job on many versions or operating systems |

**OIDC to the cloud (Almost always asked).** Storing a permanent AWS key in GitHub is like hiding your house key under the doormat. With OIDC, the job asks GitHub for a **short-lived ID card (OIDC token)**. AWS/Azure/GCP trusts GitHub, checks the ID card, and gives **temporary credentials** for one specific role. They expire soon, so no long-lived key can leak. It needs `permissions: id-token: write`.

```yaml
# .github/workflows/ci-cd.yml
# Tip: in production, pin third-party actions to a full commit SHA.
name: ci-cd

on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [22, 24]
    steps:
      - uses: actions/checkout@v6
      - uses: actions/setup-node@v6
        with:
          node-version: ${{ matrix.node }}
          cache: npm
      - run: npm ci
      - run: npm test

  build-and-push:
    needs: test
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: production          # protection rules / required reviewers
    permissions:
      contents: read
      id-token: write                # required for OIDC
    steps:
      - uses: actions/checkout@v6
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/github-actions-deploy
          aws-region: ap-south-1
      - id: ecr
        uses: aws-actions/amazon-ecr-login@v2
      - name: Build and push image
        env:
          IMAGE: ${{ steps.ecr.outputs.registry }}/myapp:${{ github.sha }}
        run: |
          docker build -t "$IMAGE" .
          # add an image scan (e.g. Trivy) here before pushing
          docker push "$IMAGE"
```

**Reading it simply:** the **test** job runs on every push and PR, on Node 22 and 24 (the matrix). The **build-and-push** job runs only after tests pass (`needs: test`) and only for a push to `main`. It logs in to AWS with OIDC, then builds and pushes an image tagged with the commit SHA.

**Security tips:** pin third-party actions to a commit SHA (an attacker can move a tag), give minimal `permissions:`, be very careful with `pull_request_target` (it runs with the main repo's secrets), never print secrets, protect production with environment reviewers, and keep actions updated with Dependabot.

### 7.5 GitLab CI (quick view)

`.gitlab-ci.yml` defines `stages` and jobs with `image`, `script`, `rules`, `artifacts`, `cache`, `needs` (DAG), `environment` and `include` (templates). Runners (shared/group/project) use executors such as shell, docker or kubernetes. GitLab also has a built-in container registry and security scanning templates.

```yaml
stages: [build, test, deploy]

build:
  stage: build
  image: maven:3.9-eclipse-temurin-21
  script: mvn -B package -DskipTests
  artifacts:
    paths: [target/*.jar]

test:
  stage: test
  image: maven:3.9-eclipse-temurin-21
  script: mvn -B test

deploy_prod:
  stage: deploy
  script: ./scripts/deploy.sh "$CI_COMMIT_SHORT_SHA"
  environment: production
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual
```

**Reading it simply:** three stages. **build** makes the JAR and saves it as an artifact. **test** runs the tests. **deploy\_prod** runs only on `main` and waits for someone to click the button (`when: manual`).

### 7.6 Choosing a CI/CD tool

| Tool | Strengths | Watch out for |
| --- | --- | --- |
| Jenkins | Free, huge plugin ecosystem, full control, works well on-prem | Plugin upkeep, controller maintenance, Groovy complexity |
| GitHub Actions | Built into GitHub, marketplace, simple YAML, OIDC | Minutes cost at scale, supply-chain risk from third-party actions |
| GitLab CI | All-in-one platform (code, registry, security scans) | Best when your code lives on GitLab |
| Azure DevOps Pipelines | Strong Azure/Microsoft integration, Boards, Artifacts | - |
| CircleCI / Buildkite / Tekton / Argo Workflows | SaaS speed / hybrid agents / Kubernetes-native pipelines | - |
| Argo CD / Flux | **CD only (GitOps)**: pull-based sync into Kubernetes | Pair it with a CI tool |

### 7.7 CI/CD interview Q&A (Almost always asked)

**Q: Your pipeline takes 40 minutes. How do you speed it up?**

First measure each stage. Then: cache dependencies and Docker layers (BuildKit cache), run independent jobs in parallel, split tests across runners, build and test only the changed services (path filters in monorepos), use smaller base images, use bigger or self-hosted runners, move slow E2E suites later or to nightly runs, and order stages to fail fast.

**Q: How do you handle database migrations in CD?**

Version the migrations with Flyway/Liquibase/Alembic. Run them as a separate step before the app rollout. Keep them **backward compatible** with the **expand → migrate → contract** pattern: add the new column, deploy code that writes to both, backfill, switch reads, and remove the old column in a later release. Never ship a destructive change in the same release that stops using the old schema.

Think of it like replacing a road: first build the new road beside the old one (expand), move traffic slowly (migrate), and close the old road only when nobody uses it (contract).

**Q: What is a webhook?**

An HTTP callback. For example, GitHub sends a POST request to Jenkins whenever a push or PR happens.

**Q: Artifact vs cache?**

An artifact is an output you keep or pass between jobs or releases (JAR, test report, image). A cache stores reusable inputs (dependencies) only to speed things up, and it may be deleted at any time.

**Q: How would you design CI/CD for 30 microservices?**

Shared, versioned pipeline templates (Jenkins shared library or reusable workflows), a pipeline per service triggered by changes in its folder, a standard Helm chart, contract tests between services, a GitOps config repo per environment, and automated canary analysis for production.

## 8. Deployment Strategies

A deployment strategy decides **how the new version replaces the old one** without hurting users. Blue-green and canary are asked the most.

First, picture each one as a shop:

- **Recreate:** close the shop, change everything, reopen. Simple, but customers see a "closed" sign.
- **Rolling update:** replace the staff one by one during the shift. The shop never closes.
- **Blue-Green:** you have two identical shops. When the new one (green) is ready, you change the signboard so customers go there. If something is wrong, you point the board back instantly.
- **Canary:** miners once carried a canary bird into mines as an early warning. Here you send a *few* customers to the new version first. If they are fine, you send more.
- **A/B testing:** show two versions to different customers to see which one sells more. It is a product experiment.
- **Shadow / dark launch:** a trainee cook makes the same dishes in the back with real orders, but customers only get the head cook's dishes.
- **Feature flags:** the new item is already in the kitchen, but the switch is off. You turn it on later.

| Strategy | How it works | Downtime | Rollback | Extra cost | Risk |
| --- | --- | --- | --- | --- | --- |
| **Recreate** | Stop all old instances, then start the new ones | Yes | Redeploy the old version | None | High |
| **Rolling update** | Replace instances gradually (Kubernetes default, via `maxSurge`/`maxUnavailable`) | No | Gradual roll back | Low | Medium; both versions run side by side for a while |
| **Blue-Green (Asked often)** | Two identical environments; switch all traffic from Blue (old) to Green (new) at the LB/DNS | No | **Instant** (switch back) | 2x during the release | Low, but the DB schema must work for both versions |
| **Canary (Asked often)** | Send a small % of traffic (1% → 10% → 50% → 100%) to the new version, watch metrics, then promote | No | Fast (move traffic back) | Low-medium | Lowest; only a few users are affected if it fails |
| **A/B testing** | Route by user attributes to compare **business** metrics (conversion) | No | - | Medium | More a product experiment than a safety method |
| **Shadow / dark launch** | Copy real traffic to the new version; users only see the old version's responses | No | Not needed | High | Real-load testing; watch for side effects like double writes |
| **Feature flags** | Ship code switched "off", turn it on at runtime for some or all users | No | Flip the flag | Low | Separates **deploy** from **release** |

**Tools:** **Argo Rollouts** / **Flagger** (automatic canary and blue-green on Kubernetes, with metric analysis), service mesh or **Gateway API** weighted routes, AWS ALB weighted target groups, AWS CodeDeploy, Route 53 weighted records. Feature flags: LaunchDarkly, Unleash, Flagsmith, **OpenFeature** (a vendor-neutral standard).

**Zero-downtime checklist (Almost always asked):**

- At least 2 replicas.
- **Readiness probes**, so traffic goes only to Pods that are ready.
- Graceful shutdown: handle **SIGTERM**, finish requests already in progress, use a `preStop` hook.
- Connection draining on the load balancer.
- `maxUnavailable: 0`.
- **PodDisruptionBudgets**.
- Backward-compatible DB migrations (expand/contract) and APIs.

**Rollback options:** redeploy the previous immutable image tag, `kubectl rollout undo`, `helm rollback`, switch blue-green back, turn off the feature flag, or `git revert` in a GitOps repo. For databases, prefer **roll-forward fixes** and migrations designed so the old version keeps working.

**Q: Blue-green or canary?**

Blue-green when you want an instant, all-or-nothing switch and can afford double capacity. Canary when you want to limit the damage and test with real traffic and metrics first. Many teams combine canary + automated analysis + feature flags.

## 9. Docker and Containers

Docker questions are guaranteed. Be ready for VM vs container, how containers work inside Linux, Dockerfile differences (CMD vs ENTRYPOINT), and how to make images small and safe.

### 9.1 VM vs container (Must know)

Virtual machines are like **separate houses**: each one has its own foundation, plumbing and wiring (a full guest OS). Containers are like **flats in one apartment building**: they share the same foundation and plumbing (**one host kernel**), but each flat is locked and private.

```
VIRTUAL MACHINES                         CONTAINERS
[App A][App B][App C]                    [App A][App B][App C]
[Libs ][Libs ][Libs ]                    [Libs ][Libs ][Libs ]
[GuestOS][GuestOS][GuestOS]              [ Container runtime  ]
[      Hypervisor      ]                 [ Host OS + ONE shared kernel ]
[   Hardware / Host OS ]                 [      Hardware       ]
```

|  | Virtual machine | Container |
| --- | --- | --- |
| What it virtualizes | Hardware (via a hypervisor) | The operating system (process isolation) |
| OS | A full guest OS per VM | Shares the **host kernel** |
| Size | GBs | MBs |
| Startup | Minutes | Seconds or less |
| Isolation | Strong (separate kernels) | Weaker (shared kernel), so harden it |
| How many per host | Tens | Hundreds |
| Typical use | Different OS/kernel, strong isolation, legacy apps | Microservices, CI jobs, portable packaging |

**Hypervisors:** **Type 1** runs directly on the hardware (bare metal: VMware ESXi, KVM, Hyper-V, Xen; used by clouds). **Type 2** runs on top of a normal OS (VirtualBox, VMware Workstation).

**Why containers?** The image packs the app **plus all its dependencies**. This solves "works on my machine", gives the same behaviour from dev to prod, starts fast, uses resources well, and makes deployments immutable.

### 9.2 How containers actually work (Almost always asked; impresses interviewers)

A container is **not a mini-VM**. It is a **normal Linux process** with blinkers on (it can see only its own things) and on a diet (it can use only limited resources).

- **Namespaces = isolation (what the process can SEE):** `pid` (its own process tree; the app is PID 1), `net` (its own network interfaces, IP and ports), `mnt` (its own filesystem view), `uts` (its own hostname), `ipc` (shared memory), `user` (maps root inside the container to a normal user on the host), `cgroup`, `time`.
- **cgroups = limits (what the process can USE):** CPU, memory, disk I/O, number of PIDs. **cgroup v2** is now the standard (Kubernetes v1.35 dropped cgroup v1).
- **Union filesystem (OverlayFS):** read-only image layers stacked together + a thin **writable container layer** on top (copy-on-write).
- **Extra security layers:** Linux capabilities, seccomp (filters system calls), AppArmor/SELinux.

**One-line answer:** "A container is a Linux process isolated by namespaces, limited by cgroups, running on a layered filesystem."

### 9.3 Docker architecture (Almost always asked)

When you type a docker command, it passes down a chain: the **CLI** sends it to the **daemon (dockerd)**, the daemon asks **containerd**, and containerd uses **runc** to actually create the container.

```
docker CLI --(REST API)--> dockerd (Docker daemon)
                              |
                              v
                          containerd   (pulls/stores images, manages container lifecycle)
                              |
                              v
                containerd-shim -> runc  (OCI runtime: sets up namespaces + cgroups, starts process)
                              |
                              v
                         your container process

Registry (Docker Hub / ECR / GHCR / Harbor) <-- docker push / docker pull
```

- **OCI** (Open Container Initiative) sets standard formats for images, runtimes and distribution. So images built with Docker also run on containerd, CRI-O, Podman and Kubernetes.
- **Kubernetes no longer uses Docker Engine** (dockershim was removed in v1.24). The kubelet talks to **containerd** or **CRI-O** through the **CRI**. Docker-built images still work because they are OCI images.

### 9.4 Image vs container

- **Image** = a read-only template (a stack of layers + details such as the default command). Think of a **class**, or a cake recipe.
- **Container** = a running (or stopped) instance of an image, with its own writable layer. Think of an **object**, or the actual cake.
- **Image reference:** `registry/repository:tag` (a label that can be moved) vs `@sha256:<digest>` (a fixed content hash that never changes).

### 9.5 Dockerfile instructions and the classic differences (Must know)

| Instruction | What it does |
| --- | --- |
| `FROM` | Base image (each build stage starts with one) |
| `WORKDIR` | Set the working directory |
| `COPY` / `ADD` | Copy files into the image |
| `RUN` | Run a command **at build time** (creates a layer) |
| `ENV` | Environment variable available at build **and run** time |
| `ARG` | Build-time variable (`--build-arg`); not available at runtime |
| `EXPOSE` | **Documents** a port; does not publish it |
| `USER` | Which user to run as |
| `CMD` | Default command/arguments **when the container starts** |
| `ENTRYPOINT` | Fixed program that runs when the container starts |
| `HEALTHCHECK` | Command Docker runs to check the container's health |
| `VOLUME`, `LABEL`, `STOPSIGNAL`, `SHELL`, `ONBUILD` | Others |

| Question | Answer in simple words |
| --- | --- |
| **CMD vs ENTRYPOINT** | `ENTRYPOINT` is the fixed program that always runs. `CMD` gives default arguments (or a default command) that are **easily replaced** by `docker run image <args>`. Together: `ENTRYPOINT ["python","app.py"]` + `CMD ["--port","8000"]`. Change the entrypoint with `--entrypoint`. |
| **Exec form vs shell form** | `CMD ["node","server.js"]` (exec form) runs the app as **PID 1**, so it **receives SIGTERM** and can shut down gracefully. `CMD node server.js` (shell form) wraps it in `/bin/sh -c`, which may not pass signals on, so stops are slow or forced. Prefer exec form. |
| **COPY vs ADD** | `COPY` just copies local files. `ADD` can also auto-extract local tar files and download URLs, which is less predictable. Prefer `COPY`. |
| **ARG vs ENV** | `ARG` exists only during the build; `ENV` stays in running containers. Neither is safe for secrets (visible in image history). Use BuildKit secrets: `RUN --mount=type=secret,id=npm_token ...` |
| **RUN vs CMD** | `RUN` executes while **building** the image; `CMD` executes when the **container starts**. |
| **EXPOSE vs `-p`** | `EXPOSE` is only documentation. `-p 8080:80` actually connects host port 8080 to container port 80. |

**Easy way to remember CMD vs ENTRYPOINT:** ENTRYPOINT is the machine; CMD is the default setting on the machine, which the user can change.

### 9.6 Layer caching (Almost always asked)

Every instruction creates a layer. Docker reuses cached layers **until the first instruction whose inputs changed**; everything after that is rebuilt. It is like a stack of plates: change one in the middle and every plate above it must be re-stacked.

So order instructions from **least changing to most changing**:

```dockerfile
# Bad: any source change re-installs every dependency
COPY . .
RUN npm ci

# Good: dependencies stay cached until package*.json changes
COPY package*.json ./
RUN npm ci
COPY . .
```

### 9.7 Multi-stage builds (Almost always asked)

Build with a big image that has the full SDK, then copy only the final output into a small runtime image. It is like cooking in a big kitchen but serving only the dish on a plate; the pots and pans never go to the dining table. Result: **smaller, faster to pull, more secure** (no compilers or source code in production).

**Node.js**

```dockerfile
# ---------- build stage ----------
FROM node:24-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# ---------- runtime stage ----------
FROM node:24-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY package*.json ./
RUN npm ci --omit=dev && npm cache clean --force
COPY --from=build /app/dist ./dist
USER node
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:3000/health || exit 1
CMD ["node", "dist/server.js"]
```

**Java (Spring Boot)**

```dockerfile
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /src
COPY pom.xml .
RUN mvn -B dependency:go-offline
COPY src ./src
RUN mvn -B package -DskipTests

FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /src/target/*.jar app.jar
RUN addgroup -S app && adduser -S app -G app
USER app
EXPOSE 8080
ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75", "-jar", "app.jar"]
```

`-XX:MaxRAMPercentage` sizes the Java heap as a % of the container's memory limit, which helps avoid OOMKills.

### 9.8 Image optimization and security best practices (Almost always asked)

1. Small base images: `alpine`, `-slim`, **distroless**, `scratch` (for static binaries), Chainguard/Wolfi images.
2. **Multi-stage builds.**
3. **`.dockerignore`**: leave out `.git`, `node_modules`, `.env`, test data, secrets.
4. Install and clean up in the same layer: `apt-get update && apt-get install -y --no-install-recommends pkg && rm -rf /var/lib/apt/lists/*`.
5. Pin base image versions (ideally digests) and rebuild regularly to get security patches.
6. Run as **non-root** (`USER`), drop capabilities, and use a read-only root filesystem where possible.
7. Never bake secrets into images. They stay in the layer history even if a later layer deletes them.
8. One main process per container; log to **stdout/stderr**.
9. **Scan** images (Trivy, Grype, Docker Scout, ECR scanning) and **sign** them (cosign).
10. Add a `HEALTHCHECK` for Docker/Compose (Kubernetes uses its own probes instead).

### 9.9 Docker networking (Almost always asked)

| Driver | Description | Use case |
| --- | --- | --- |
| `bridge` (default) | A private network on the host (`docker0`); containers get internal IPs; publish ports with `-p` | Single-host apps |
| User-defined bridge | Same, **plus automatic DNS by container name** and better isolation | Multi-container apps on one host (Compose does this) |
| `host` | Shares the host's network (no isolation, no `-p` needed) | Maximum network speed (Linux) |
| `none` | No networking | Isolated batch jobs |
| `overlay` | Spans multiple hosts | Docker Swarm |
| `macvlan` | The container gets its own MAC/IP on the physical LAN | Old apps that must appear on the LAN |

```bash
docker network create appnet
docker run -d --name db  --network appnet -e POSTGRES_PASSWORD=secret postgres:16
docker run -d --name api --network appnet -p 8080:3000 myapi:1.0   # api reaches the DB at host "db"
```

In simple words: on your own (user-defined) network, containers can call each other **by name**, like `db`, instead of remembering IPs.

### 9.10 Docker storage (Almost always asked)

The container's writable layer is **temporary**: its data disappears when the container is removed. To keep data, store it outside the container.

| Type | What it is | Use |
| --- | --- | --- |
| **Volume** | Managed by Docker (`/var/lib/docker/volumes`), e.g. `-v pgdata:/var/lib/postgresql/data` | Data that must survive, such as databases (preferred) |
| **Bind mount** | A host folder mapped in, e.g. `-v /home/me/src:/app` | Local development, injecting config files |
| **tmpfs** | In memory only | Sensitive or scratch data |

### 9.11 Docker Compose

Compose lets you describe a multi-container app in one file, `compose.yaml`. Run it with `docker compose up -d`, `docker compose logs -f`, `docker compose down -v`. Compose v2 is the `docker compose` plugin; the old Python `docker-compose` v1 is deprecated.

```yaml
services:
  web:
    build: .
    ports:
      - "8080:3000"
    environment:
      DATABASE_URL: postgres://app:secret@db:5432/appdb
      REDIS_URL: redis://cache:6379
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: secret        # demo only; use secrets/env files for real setups
      POSTGRES_DB: appdb
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app -d appdb"]
      interval: 5s
      retries: 5

  cache:
    image: redis:7-alpine

volumes:
  pgdata:
```

**Reading it simply:** three services. **web** is built from your Dockerfile and starts only after the database is healthy. **db** is Postgres, with its data kept in the `pgdata` volume. **cache** is Redis. They find each other by name (`db`, `cache`).

### 9.12 Commands cheat sheet (Asked often)

```bash
docker build -t myapp:1.0 .                 # build (add --no-cache to ignore the cache)
docker images; docker image prune
docker run -d --name web -p 8080:80 -e ENV=prod --restart unless-stopped nginx:1.28
docker ps; docker ps -a
docker logs -f --tail 100 web
docker exec -it web sh                      # shell inside a running container
docker inspect web                          # JSON: IP, mounts, ExitCode, OOMKilled ...
docker stats; docker top web
docker stop web; docker start web; docker rm -f web
docker cp web:/etc/nginx/nginx.conf .
docker tag myapp:1.0 123456789012.dkr.ecr.ap-south-1.amazonaws.com/myapp:1.0
docker push <image>; docker pull <image>; docker login <registry>
docker history myapp:1.0                    # layers and their sizes
docker system df; docker system prune -a    # free disk space (careful on shared hosts)
docker network ls; docker volume ls
```

**More classic differences (Asked often)**

| Question | Answer |
| --- | --- |
| `docker stop` vs `docker kill` | `stop` sends SIGTERM, waits (10 s by default), then SIGKILL. `kill` sends SIGKILL immediately by default. |
| `docker save` vs `docker export` | `save` writes an **image** with all its layers and history (`docker load` to import). `export` writes a **container's filesystem**, flattened, without history (`docker import`). |
| `docker exec` vs `docker attach` | `exec` starts a new process inside the container (e.g. a shell). `attach` connects to the main process's input/output (Ctrl+C may stop it). |
| `docker run` vs `docker create` + `start` | `run` = create + start in one step. |
| Restart policies | `no`, `on-failure[:N]`, `always`, `unless-stopped`. |
| `docker commit` | Makes an image from a container's changes. Avoid it for production (not repeatable); use Dockerfiles. |

### 9.13 Troubleshooting containers (Almost always asked)

| Symptom | Likely cause | What to check |
| --- | --- | --- |
| Container exits immediately | The main process finished or crashed (a container lives only as long as PID 1) | `docker logs`, `docker inspect --format '{{.State.ExitCode}} {{.State.OOMKilled}}' <name>`, run it with `-it ... sh`, make sure the process runs in the foreground (e.g. `nginx -g 'daemon off;'`) |
| Exit code **137** | SIGKILL, often **out of memory** | `OOMKilled: true` in inspect; raise `--memory` or fix the leak |
| Exit 127 / 126 | Command not found / not executable | Typo in CMD, program missing from a slim image, missing `chmod +x`, Windows (CRLF) line endings in scripts |
| App not reachable from the host | Port not published, or the app listens on **127.0.0.1 inside the container** | `docker port`, bind the app to `0.0.0.0`, check the host firewall |
| Containers can't find each other by name | They are on the default bridge network | Use a user-defined network or Compose |
| Host disk full | Old images/containers, build cache, huge JSON log files | `docker system df`, prune, set log rotation (`max-size`, `max-file`) |
| Permission denied on a volume | UID/GID mismatch between the container user and host files | Match UID/GID, `chown`, or use named volumes |

### 9.14 Container security checklist

- Non-root user, minimal and patched base images, scanning in CI, signed images.
- No `--privileged`. Use `--cap-drop ALL` and add back only what is needed.
- Read-only filesystem and resource limits (`--memory`, `--cpus`).
- **Never mount `/var/run/docker.sock` into untrusted containers.** That is the same as giving root on the host.
- Secrets as mounted files, not environment variables.
- Rootless Docker or **Podman** (no daemon, rootless by default).
- To build images in Kubernetes-based CI without a Docker daemon, use BuildKit (rootless mode) or Buildah.

### 9.15 Docker interview Q&A

**Q: Docker vs Kubernetes?**

Docker builds and runs containers on one machine. Kubernetes manages containers across a whole cluster: scheduling, scaling, self-healing, service discovery and rolling updates.

**Q: How would you shrink a 1.2 GB image?**

Multi-stage build, slim/distroless base, `.dockerignore`, production-only dependencies, clean package caches in the same layer, and check layers with `docker history` or `dive`.

**Q: Can a container run a different kernel than the host?**

No. Containers share the host kernel. That is why Linux containers on macOS/Windows run inside a small Linux VM.

**Q: What is a dangling image?**

An untagged image (`<none>:<none>`) left behind after rebuilds. Remove it with `docker image prune`.

**Q: How do you pass secrets to a container?**

As mounted files from a secret manager (Docker/Compose secrets, Kubernetes Secrets as volumes, Vault agent). Never as `ENV` in the Dockerfile.

**Q: Why does my container take 10 seconds to stop?**

The app is not handling SIGTERM (often because of shell-form `CMD`), so Docker waits for the timeout and then sends SIGKILL. Use exec form and handle SIGTERM, or use an init like `tini` (`docker run --init`).

## 10. Kubernetes

Kubernetes is the most important topic in most DevOps interviews. Know the architecture, what happens on `kubectl apply`, Services, probes and troubleshooting really well.

**Current release:** **v1.37 "Garhwal"** (Aug 2026). Kubernetes ships about 3 minor releases a year. The latest 3 minor versions are supported, with roughly 14 months of patches each.

### 10.1 What and why

Kubernetes (K8s) is an open-source **container orchestration** platform. It came from Google's Borg and is now a CNCF project. It automates **deployment, scaling, self-healing, service discovery, load balancing, rolling updates, config/secret management and storage**.

In simple words: Docker runs containers on one machine. Kubernetes is the manager that runs hundreds of containers across many machines and keeps them healthy.

**The core idea: declarative desired state + reconciliation (Almost always asked).** You write *what* you want in YAML (e.g. "3 replicas of image v2"). **Controllers** run never-ending loops that compare the **desired state** (stored in etcd) with the **actual state**, and fix any gap. This loop is why Kubernetes heals itself.

Think of an AC thermostat. You set 24°C (desired state). The AC keeps checking the room temperature (actual state) and adjusts until they match. If someone opens a window, it simply works harder. You never tell it *how*; you only say *what*.

### 10.2 Architecture (Must know)

A cluster has two parts. The **control plane** is the brain or head office: it decides. The **worker nodes** are the factories: they run your containers.

```
CONTROL PLANE
  kube-apiserver  <-->  etcd (all cluster state)
        ^
        |-- kube-scheduler            (chooses a node for each new Pod)
        |-- kube-controller-manager   (runs the control loops)
        |-- cloud-controller-manager  (cloud LBs, node lifecycle, routes)
        |
   (HTTPS: watches and status updates)
        |
WORKER NODES (on each node)
  kubelet --> container runtime (containerd / CRI-O) --> Pods
  kube-proxy (programs Service routing rules)
  CNI plugin (Pod networking) ; cluster add-ons: CoreDNS, metrics-server, Ingress/Gateway controller
```

**Easy way to remember each part:** API server = the reception desk (everyone talks through it). etcd = the record book (the cluster's memory). Scheduler = the person who decides which node gets each Pod. Controller manager = supervisors who keep checking that reality matches the plan. kubelet = the site supervisor on each node. kube-proxy = the traffic police for Services.

**Control plane components**

| Component | Role |
| --- | --- |
| **kube-apiserver** | The front door of the cluster (REST API). Every request goes through **authentication → authorization (RBAC) → admission control → validation**, then is saved to etcd. The **only** component that talks to etcd. Scales horizontally. |
| **etcd** | A distributed, strongly consistent key-value store (Raft consensus) holding all cluster state. Run an odd number of members (3 or 5) for quorum. **Back it up.** |
| **kube-scheduler** | Watches for Pods with no node yet and picks a node: **filtering** (enough resources? taints/tolerations, affinity, node selectors), then **scoring** (spread, utilization), then **binds** the Pod to the node. |
| **kube-controller-manager** | Runs the built-in controllers: Node, ReplicaSet, Deployment, StatefulSet, Job, EndpointSlice, ServiceAccount, Namespace, ... Each one is a reconcile loop. |
| **cloud-controller-manager** | Talks to the cloud provider: creates load balancers for `LoadBalancer` Services, manages node lifecycle and routes. |

**Node components**

| Component | Role |
| --- | --- |
| **kubelet** | The agent on every node. Receives Pod specs, starts containers through the **CRI**, mounts volumes, runs probes, reports Pod and node status. |
| **kube-proxy** | Makes Service virtual IPs work, using iptables, **nftables** (GA since v1.33) or IPVS (deprecated in v1.35). Some CNIs (e.g. Cilium with eBPF) replace it completely. |
| **Container runtime** | containerd or CRI-O. |

### 10.3 What happens when you run `kubectl apply -f deployment.yaml`? (Must know)

Tell this as a relay race: each component does its part and passes the baton.

1. `kubectl` reads your kubeconfig and sends an HTTPS request to the **API server**.
2. The API server checks **authentication** (certificate/token/OIDC) → **authorization** (RBAC) → **mutating admission** → schema validation → **validating admission**, then stores the Deployment in **etcd**.
3. The **Deployment controller** notices the new Deployment (through a watch) and creates a **ReplicaSet**.
4. The **ReplicaSet controller** creates the required number of **Pod** objects (no node yet, status `Pending`).
5. The **scheduler** sees these Pods, filters and scores the nodes, and **binds** each Pod to a node.
6. The **kubelet** on that node sees the Pod and pulls the image via the runtime. The **CNI** plugin gives the Pod an IP, the **CSI** driver mounts volumes, init containers run, then the app containers start.
7. The kubelet runs the **probes** and reports status. The Pod becomes `Running` and `Ready`.
8. The **EndpointSlice controller** adds the ready Pod's IP to the Service's endpoints. **kube-proxy** updates its rules, and traffic starts flowing.

### 10.4 Workload objects (Almost always asked)

| Object | What it does | Typical use |
| --- | --- | --- |
| **Pod** | The smallest unit you can deploy: one or more containers sharing a **network namespace** (same IP, talk over `localhost`) and volumes. Temporary. | Never create bare Pods in production |
| **ReplicaSet** | Keeps N identical Pods running (using a label selector) | Managed by Deployments |
| **Deployment** | Manages ReplicaSets: rolling updates, rollbacks, scaling | **Stateless** apps (web, APIs) |
| **StatefulSet** | Stable, unique Pod names (`db-0`, `db-1`), **its own storage per Pod** (`volumeClaimTemplates`), ordered start/stop, stable DNS via a headless Service | Databases, Kafka, ZooKeeper, Elasticsearch |
| **DaemonSet** | One Pod on every (matching) node | Log collectors (Fluent Bit), node-exporter, CNI agents, kube-proxy |
| **Job** | Runs Pods until the work is done (`completions`, `parallelism`, `backoffLimit`) | Batch work, migrations |
| **CronJob** | Creates Jobs on a cron schedule | Backups, reports, cleanups |

A Pod is like a flat shared by roommates: the containers inside share one address (IP) and can talk to each other over `localhost`.

**Deployment vs StatefulSet (Almost always asked).** Deployment Pods are like taxis: any one will do, and they are interchangeable. StatefulSet Pods are like numbered seats with your own locker: `db-0` always gets its own disk back.

|  | Deployment | StatefulSet |
| --- | --- | --- |
| Pod names | Random suffix (`web-7d9f8-abc12`) | Numbered and stable (`db-0`, `db-1`) |
| Storage | Usually shared or none; replicas are interchangeable | Each Pod gets **its own PVC**, which follows it if it moves |
| Ordering | Pods start/stop in parallel | Ordered create, scale and delete (by default) |
| Network identity | Only through the Service | Stable DNS per Pod: `db-0.db.<namespace>.svc.cluster.local` |
| Use | Stateless | Stateful, clustered software |

**Namespaces** split one cluster into sections (names, RBAC, quotas, network policies). Defaults: `default`, `kube-system`, `kube-public`, `kube-node-lease`.

**Labels** are identifying key-values used by selectors (`app: web`). **Annotations** are extra notes used by tools (`prometheus.io/scrape: "true"`).

### 10.5 A production-style Deployment + Service (Asked often)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  labels: {app: web}
spec:
  replicas: 3
  selector:
    matchLabels: {app: web}
  strategy:
    type: RollingUpdate
    rollingUpdate: {maxSurge: 1, maxUnavailable: 0}   # zero-downtime rollout
  template:
    metadata:
      labels: {app: web}
    spec:
      terminationGracePeriodSeconds: 30
      containers:
        - name: web
          image: myrepo/web:1.4.2                     # immutable tag, never :latest
          ports:
            - containerPort: 8080
          env:
            - name: DB_HOST
              valueFrom:
                configMapKeyRef: {name: web-config, key: db_host}
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef: {name: web-secret, key: db_password}
          resources:
            requests: {cpu: "250m", memory: "256Mi"}
            limits: {memory: "512Mi"}
          readinessProbe:
            httpGet: {path: /ready, port: 8080}
            periodSeconds: 10
          livenessProbe:
            httpGet: {path: /healthz, port: 8080}
            initialDelaySeconds: 15
            periodSeconds: 20
          securityContext:
            runAsNonRoot: true
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            capabilities: {drop: ["ALL"]}
---
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  type: ClusterIP
  selector: {app: web}         # must match the Pod labels!
  ports:
    - port: 80                 # Service port
      targetPort: 8080         # container port
```

**Reading it simply:** run 3 copies of `web:1.4.2`. During updates, add 1 new Pod at a time and never remove an old one before the new one is ready. Take config from a ConfigMap and the password from a Secret. Ask for 0.25 CPU and 256 MiB memory, and never allow more than 512 MiB. Check `/ready` before sending traffic and `/healthz` to see if it is alive. Run as non-root. The Service gives these Pods one stable address on port 80.

**Note on CPU limits:** many teams set CPU **requests** but no CPU **limit** (to avoid throttling), and set memory limit = request for predictability. Be ready to discuss this trade-off.

### 10.6 Services (Must know)

**Why we need them:** Pods come and go, and their IPs keep changing. A **Service** gives a **stable virtual IP and DNS name** and spreads traffic across all ready Pods that match its **label selector**. These Pods are tracked in **EndpointSlices** (the old Endpoints API is deprecated since v1.33).

Think of a company's main phone number. Employees change desks and phones, but the main number always connects you to someone available.

| Type | Behaviour | Use |
| --- | --- | --- |
| **ClusterIP** (default) | Internal virtual IP, reachable only inside the cluster | Service-to-service traffic |
| **NodePort** | Opens a fixed port (30000-32767) on **every node**, forwarding to the Service | Dev/test, or behind your own LB |
| **LoadBalancer** | Creates a cloud load balancer (via cloud-controller-manager or an LB controller) → NodePort/Pods | Exposing one service to the outside |
| **ExternalName** | A DNS CNAME to an external hostname (no proxying) | Reaching an external DB by an internal name |
| **Headless** (`clusterIP: None`) | No virtual IP; DNS returns the individual Pod IPs | StatefulSets, client-side load balancing |

- **DNS name:** `<service>.<namespace>.svc.cluster.local` (just `<service>` inside the same namespace).
- **The three ports (Asked often):** `port` = the Service's port. `targetPort` = the container's port. `nodePort` = the port opened on each node.

### 10.7 Ingress and Gateway API (Almost always asked; 2026 update)

**Ingress** = HTTP(S) routing rules (by host and path) to Services, plus TLS termination, through **one** entry point. It is like a building's reception that sends each visitor to the right department. It needs an **Ingress controller** to work (Traefik, HAProxy, Kong, NGINX Inc's controller, AWS Load Balancer Controller, cloud-native controllers, etc.). One shared entry point is much cheaper than a `LoadBalancer` Service per app.

**Warning, 2026 change:** the community **Ingress NGINX** controller (`kubernetes/ingress-nginx`) was **retired in March 2026**: no more releases, bug fixes or security patches. Existing installs still work, but SIG Network recommends moving to **Gateway API** or another maintained controller. Check whether you run it: `kubectl get pods -A -l app.kubernetes.io/name=ingress-nginx`.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web
spec:
  ingressClassName: traefik
  tls:
    - hosts: [app.example.com]
      secretName: app-tls
  rules:
    - host: app.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port: {number: 80}
```

**Gateway API ("Ingress 2.0")** splits the work by role, so each team owns its own piece:

- **GatewayClass** (infrastructure provider: which implementation) → **Gateway** (cluster operator: listeners, ports, TLS) → **HTTPRoute / GRPCRoute / TLSRoute / TCPRoute** (app teams: routing rules).
- Built into the spec (no vendor annotations): **weighted traffic splitting** (canaries), header/query matching, redirects, rewrites, and cross-namespace routing (controlled by `ReferenceGrant`). It works the same across implementations (Envoy Gateway, Istio, Cilium, NGINX Gateway Fabric, Kong, Traefik, cloud LBs).

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: web
spec:
  parentRefs:
    - name: public-gateway          # a Gateway managed by the platform team
  hostnames: ["app.example.com"]
  rules:
    - matches:
        - path: {type: PathPrefix, value: /}
      backendRefs:
        - name: web-v1
          port: 80
          weight: 90                # 90% of traffic
        - name: web-v2
          port: 80
          weight: 10                # 10% canary
```

**Reading it simply:** for `app.example.com`, send 90% of traffic to `web-v1` and 10% to the new `web-v2`. That is a canary release written in a few lines.

### 10.8 ConfigMaps and Secrets (Almost always asked)

- **ConfigMap:** normal, non-secret configuration (key-values or whole files). Used as env vars, command arguments or mounted files.
- **Secret:** sensitive data. **Warning:** by default it is only **base64-encoded, not encrypted**. Base64 is like writing in a different alphabet: anyone can read it back.
- **How to protect Secrets:** **encryption at rest** for etcd (EncryptionConfiguration / KMS provider), strict RBAC on `get/list secrets`, and external secret managers through the **External Secrets Operator** or the **Secrets Store CSI driver** (Vault, AWS Secrets Manager, Azure Key Vault). In GitOps, use Sealed Secrets or SOPS if secrets must live in Git.
- **Secret types:** `Opaque`, `kubernetes.io/tls`, `kubernetes.io/dockerconfigjson` (for `imagePullSecrets`).
- **Update behaviour:** mounted files update eventually (not when using `subPath`). **Env vars never update** until the Pod restarts → `kubectl rollout restart`, or a checksum annotation in Helm to trigger a rollout.
- Mark rarely-changing ones `immutable: true` (safer and lighter on the API server).

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: web-config
data:
  db_host: "postgres.prod.svc.cluster.local"
  LOG_LEVEL: "info"
---
apiVersion: v1
kind: Secret
metadata:
  name: web-secret
type: Opaque
stringData:                   # plain text here; Kubernetes stores it base64-encoded
  db_password: "change-me"
```

```bash
kubectl create secret generic web-secret --from-literal=db_password=change-me
kubectl get secret web-secret -o jsonpath='{.data.db_password}' | base64 -d
```

The second command proves the point: anyone who can read the Secret can decode it in one line.

### 10.9 Storage (Almost always asked)

| Concept | Meaning |
| --- | --- |
| **Volume** | Storage attached to a Pod: `emptyDir` (scratch space for the Pod's life, shared by its containers), `configMap`/`secret`, `hostPath` (a node folder; avoid), `persistentVolumeClaim`, projected volumes |
| **PersistentVolume (PV)** | A piece of storage in the cluster (created by an admin or automatically) |
| **PersistentVolumeClaim (PVC)** | A user's **request** for storage (size, access mode, class) that gets bound to a PV |
| **StorageClass** | A "type" of storage + its provisioner (e.g. `ebs.csi.aws.com`); it enables **dynamic provisioning** |
| **CSI** | Container Storage Interface: the standard plugin API that storage vendors implement |

**Analogy:** PV = a parking space. PVC = your request for a parking space. StorageClass = a parking lot that builds new spaces on demand.

- **Access modes:** `ReadWriteOnce` (RWO, one node), `ReadOnlyMany` (ROX), `ReadWriteMany` (RWX, many nodes, e.g. NFS/EFS), `ReadWriteOncePod` (RWOP, exactly one Pod).
- **Reclaim policy:** `Delete` (default for dynamic volumes; the disk is deleted with the PVC) or `Retain` (keeps the disk and data for manual recovery). Use `Retain` for important data.
- `volumeBindingMode: WaitForFirstConsumer` creates the disk in the same zone as the Pod (important for zonal disks like EBS).

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp3
provisioner: ebs.csi.aws.com
parameters:
  type: gp3
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
spec:
  serviceName: postgres                  # headless Service gives stable DNS per Pod
  replicas: 1                            # real HA replication needs an operator (e.g. CloudNativePG)
  selector:
    matchLabels: {app: postgres}
  template:
    metadata:
      labels: {app: postgres}
    spec:
      containers:
        - name: postgres
          image: postgres:16
          ports:
            - containerPort: 5432
          env:
            - name: POSTGRES_PASSWORD
              valueFrom:
                secretKeyRef: {name: postgres-secret, key: password}
            - name: PGDATA                # subfolder avoids the "lost+found" issue on new disks
              value: /var/lib/postgresql/data/pgdata
          volumeMounts:
            - name: data
              mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:                  # one PVC per Pod: data-postgres-0, data-postgres-1 ...
    - metadata:
        name: data
      spec:
        accessModes: ["ReadWriteOnce"]
        storageClassName: gp3
        resources:
          requests:
            storage: 20Gi
```

**Reading it simply:** the StorageClass says "make gp3 EBS disks on demand, in the Pod's zone". The StatefulSet runs Postgres, and `volumeClaimTemplates` gives each Pod its own 20 GiB disk.

### 10.10 Health probes (Must know)

The kubelet keeps asking your container three questions:

| Probe | Question it asks | If it fails |
| --- | --- | --- |
| **Liveness** | "Are you alive, or stuck?" | The kubelet **restarts** the container |
| **Readiness** | "Can you take customers right now?" | The Pod is **removed from Service endpoints** (no restart) |
| **Startup** | "Has this slow app finished starting?" | Liveness/readiness wait until it passes; if it never passes, the container is restarted |

- **Methods:** `httpGet`, `tcpSocket`, `exec`, `grpc`. **Tuning:** `initialDelaySeconds`, `periodSeconds`, `timeoutSeconds`, `failureThreshold`, `successThreshold`.
- **Warning:** don't check external dependencies (DB, other services) in **liveness**. If the DB has a small hiccup, every Pod restarts at once (a cascading failure). Dependency checks belong in readiness, if anywhere.
- Slow-starting apps (big JVMs) need a **startup probe**. Otherwise liveness kills them before they finish booting, which looks like CrashLoopBackOff.

### 10.11 Resources and QoS (Almost always asked)

Think of a train. **Requests** are the seat you booked: guaranteed. **Limits** are the most space you are allowed to take.

- **requests** = the guaranteed amount. The **scheduler** places Pods based on requests.
- **limits** = the maximum allowed. CPU above the limit is **throttled** (slowed down, because CPU is compressible). Memory above the limit gets the container **OOMKilled** (exit code **137**, because memory cannot be squeezed).
- **Units:** CPU in cores or millicores (`500m` = half a core); memory in `Mi`/`Gi`.

| QoS class | When a Pod gets it | Who is evicted first when the node is short |
| --- | --- | --- |
| **Guaranteed** | Every container has CPU and memory requests **equal** to limits | Evicted last (VIP ticket) |
| **Burstable** | At least one request/limit is set, but not Guaranteed | Middle (normal ticket) |
| **BestEffort** | No requests or limits at all | Evicted first (no ticket) |

- **ResourceQuota** caps the total for a namespace. **LimitRange** sets defaults and min/max per Pod or container.
- **In-place Pod resize** (GA since v1.35): change the CPU/memory of a running Pod without recreating it (through the `resize` subresource).

### 10.12 Autoscaling (Almost always asked)

Think of a busy bank. **HPA** adds more cashiers (Pods). **VPA** gives each cashier a bigger desk (more CPU/memory). **Cluster Autoscaler / Karpenter** opens new rooms (nodes) when there is no space for more cashiers. **KEDA** adds cashiers based on events, such as the length of the queue.

| Autoscaler | What it scales | Notes |
| --- | --- | --- |
| **HPA** (Horizontal Pod Autoscaler) | Number of **Pods** | CPU/memory via metrics-server, or custom/external metrics (Prometheus Adapter, KEDA). Needs **requests** set to work out utilization %. Scale-to-zero reached beta in v1.37. |
| **VPA** (Vertical Pod Autoscaler) | **Requests/limits** of Pods | Great in recommendation mode. Don't let VPA and HPA fight over the same CPU/memory metric |
| **Cluster Autoscaler** | Number of **nodes** (in node groups) | Adds nodes when Pods are `Pending` for lack of space; removes underused nodes |
| **Karpenter** | **Nodes**, created directly and right-sized | Picks instance types that fit the waiting Pods quickly; packs Pods together to save cost |
| **KEDA** | Pods, based on **events** (queue length, Kafka lag, cron) | Can scale to zero |

**HPA formula (Asked often):** `desiredReplicas = ceil(currentReplicas × currentMetricValue / targetMetricValue)`

**Example:** 4 Pods at 90% CPU with a target of 60% → ceil(4 × 90 / 60) = **6 Pods**.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: web
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: web
  minReplicas: 3
  maxReplicas: 20
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

**Reading it simply:** keep between 3 and 20 Pods of `web`, adding or removing Pods to keep average CPU around 70%.

### 10.13 Controlling where Pods run (Almost always asked)

- **nodeSelector:** the simplest. The node must have the given labels.
- **Node affinity:** richer rules. `required...` (hard rule) or `preferred...` (soft wish), with `In`, `NotIn`, `Exists` operators.
- **Pod affinity / anti-affinity:** place Pods near or away from other Pods (e.g. spread replicas across nodes using `topologyKey: kubernetes.io/hostname`).
- **Taints and tolerations (Almost always asked):** a **taint** is a "No entry" board on a node; it **keeps Pods away**. A **toleration** is a special pass on a Pod that **allows** it onto that node, but does not force it there. Effects: `NoSchedule`, `PreferNoSchedule`, `NoExecute` (also evicts running Pods). To **dedicate** nodes (e.g. GPUs), use a taint + toleration **and** node affinity together.
- **Topology spread constraints:** spread Pods evenly across zones/nodes (`maxSkew`).
- **PriorityClass:** important Pods can push out less important ones.
- **PodDisruptionBudget (PDB):** limits how many Pods can be down during **voluntary** disruptions (node drains, upgrades).
- `kubectl cordon` (mark a node unschedulable) vs `kubectl drain` (cordon + evict its Pods, respecting PDBs) vs `kubectl uncordon` (allow scheduling again).

```yaml
# Pod spec excerpt: run only on GPU nodes, spread replicas across nodes
tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
            - key: node-type
              operator: In
              values: ["gpu"]
  podAntiAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchLabels: {app: trainer}
          topologyKey: kubernetes.io/hostname
```

```bash
kubectl taint nodes gpu-node-1 gpu=true:NoSchedule      # add a taint
kubectl taint nodes gpu-node-1 gpu=true:NoSchedule-     # remove it (trailing -)
```

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: web-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels: {app: web}
```

**Reading the PDB simply:** during a drain or upgrade, always keep at least 2 `web` Pods running.

### 10.14 Pod lifecycle, init and sidecar containers, graceful shutdown (Asked often)

- **Pod phases:** `Pending`, `Running`, `Succeeded`, `Failed`, `Unknown`.
- **Container states:** `Waiting` (reasons: `ContainerCreating`, `CrashLoopBackOff`, `ImagePullBackOff`, `ErrImagePull`, `CreateContainerConfigError`), `Running`, `Terminated` (reasons: `Completed`, `Error`, `OOMKilled`).
- `restartPolicy`: `Always` (Deployments), `OnFailure` / `Never` (Jobs).
- **Init containers** run one by one, to completion, before the app containers. Use them to wait for a dependency, run migrations or fetch config.
- **Sidecar containers** run next to the app (log shipper, proxy). **Native sidecars** (GA since v1.33) are init containers with `restartPolicy: Always`: they start before and stop after the main containers, and don't stop Jobs from completing.
- **Ephemeral containers:** `kubectl debug` attaches a debugging container to a running Pod (handy for distroless images that have no shell).
- **Static Pods:** run directly by the kubelet from `/etc/kubernetes/manifests` (this is how kubeadm runs control-plane components).

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-with-helpers
spec:
  initContainers:
    - name: wait-for-db                       # classic init container
      image: postgres:16
      command: ["sh", "-c", "until pg_isready -h postgres -p 5432; do sleep 2; done"]
    - name: log-shipper                       # native sidecar
      image: fluent/fluent-bit:3.2
      restartPolicy: Always
      volumeMounts:
        - {name: logs, mountPath: /var/log/app}
  containers:
    - name: app
      image: myrepo/app:1.0
      volumeMounts:
        - {name: logs, mountPath: /var/log/app}
  volumes:
    - name: logs
      emptyDir: {}
```

**Graceful termination sequence (Almost always asked).** It is like closing a shop: stop letting new customers in, finish serving the ones inside, then lock up. If someone still won't leave after 30 seconds, security removes them.

1. The Pod is marked `Terminating` and is **removed from Service endpoints** (this happens in parallel with the next steps).
2. The `preStop` hook runs (e.g. `sleep 5`, so load balancers stop sending traffic).
3. **SIGTERM** is sent to the containers. The app should stop taking new work and finish requests already in progress.
4. After `terminationGracePeriodSeconds` (default **30 s**), any process still running gets **SIGKILL**.

### 10.15 Security: RBAC, ServiceAccounts, Pod Security (Almost always asked)

**Every request passes three gates:** **Authentication** (who are you? certs, tokens, OIDC) → **Authorization** (are you allowed? RBAC) → **Admission control** (change or validate the request: Pod Security Admission, ValidatingAdmissionPolicy (CEL), OPA Gatekeeper, Kyverno).

**RBAC objects.** A Role is a list of permissions, like a job description. A RoleBinding gives that job to someone.

| Object | Scope | Purpose |
| --- | --- | --- |
| Role | One namespace | A set of permissions (verbs on resources) |
| ClusterRole | Whole cluster | Permissions for cluster-wide resources, or reusable in many namespaces |
| RoleBinding | One namespace | Gives a Role or ClusterRole to subjects inside one namespace |
| ClusterRoleBinding | Whole cluster | Gives a ClusterRole across the whole cluster |

**Subjects** are **Users**, **Groups** or **ServiceAccounts** (the identity a Pod uses). ServiceAccount tokens are short-lived, projected tokens. Map them to cloud IAM with **EKS Pod Identity / IRSA**, GKE Workload Identity or Azure Workload Identity, so Pods never need fixed cloud keys.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods", "pods/log"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: dev
  name: ci-bot-reads-pods
subjects:
  - kind: ServiceAccount
    name: ci-bot
    namespace: dev
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

**Reading it simply:** the `pod-reader` role can only read Pods and their logs in `dev`. The binding gives that role to the `ci-bot` ServiceAccount.

**Test permissions:** `kubectl auth can-i list pods -n dev --as=system:serviceaccount:dev:ci-bot`

- **Pod Security Standards:** `privileged`, `baseline`, `restricted`, enforced per namespace by **Pod Security Admission** labels (e.g. `pod-security.kubernetes.io/enforce: restricted`). PodSecurityPolicy was removed in v1.25.
- **securityContext essentials:** `runAsNonRoot`, `allowPrivilegeEscalation: false`, `readOnlyRootFilesystem: true`, drop `ALL` capabilities, `seccompProfile: RuntimeDefault`. **User namespaces** (`hostUsers: false`) are GA as of v1.36 and map root inside the container to a normal user on the host.

### 10.16 Networking model and NetworkPolicy (Almost always asked)

**Kubernetes networking rules:**

1. Every Pod gets its own IP, unique across the cluster.
2. All Pods can reach all other Pods **without NAT** (a flat network).
3. Containers inside the same Pod share the network namespace and talk over `localhost`.

**Four kinds of communication:**

| Path | Handled by |
| --- | --- |
| Container ↔ container (same Pod) | `localhost` |
| Pod ↔ Pod (any node) | The **CNI plugin** (VXLAN overlay like Flannel, BGP routing like Calico, eBPF like Cilium, or VPC-native IPs like the AWS VPC CNI) |
| Pod → Service | **kube-proxy** rules (DNAT from ClusterIP to Pod IPs) + **CoreDNS** for names |
| Outside → Service | NodePort / LoadBalancer / Ingress / Gateway |

**NetworkPolicy** = firewall rules for Pods (ingress/egress) using Pod selectors, namespace selectors and CIDRs. By default **everything can talk to everything**, like a building where every door is open. Policies work only if the CNI enforces them (Calico and Cilium yes; plain Flannel no). **Best practice:** **default deny** (lock every door), then allow only what is needed (hand out keys).

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: prod
spec:
  podSelector: {}                    # applies to every Pod in the namespace
  policyTypes: [Ingress, Egress]
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
  namespace: prod
spec:
  podSelector:
    matchLabels: {app: backend}
  policyTypes: [Ingress]
  ingress:
    - from:
        - podSelector:
            matchLabels: {app: frontend}
      ports:
        - protocol: TCP
          port: 8080
```

**Reading it simply:** the first policy blocks all traffic in and out of every Pod in `prod`. The second lets only `frontend` Pods talk to `backend` Pods on port 8080.

**Gotcha:** after a default-deny **egress** policy, allow DNS (UDP/TCP 53 to CoreDNS), or nothing can resolve names.

### 10.17 Operators and CRDs (Asked often)

- A **CRD** (CustomResourceDefinition) adds a new resource type to the Kubernetes API (e.g. `Certificate`, `PostgresCluster`, `KafkaTopic`).
- An **Operator** = CRDs + a custom controller that knows how a human expert would run some software (install, upgrade, backup, failover). Think of it as a robot employee trained for one job. Examples: Prometheus Operator, cert-manager, CloudNativePG, Strimzi (Kafka). Built with Kubebuilder or Operator SDK.

### 10.18 Service mesh

Istio, Linkerd and Cilium service mesh add an infrastructure layer for service-to-service traffic. It gives **mTLS** (encryption + identity between services), traffic control (retries, timeouts, circuit breaking, canary splits) and observability.

Think of a personal assistant standing next to every service, handling security, retries and record-keeping for its calls. The **data plane** is these assistants: sidecar proxies (Envoy) or the sidecar-less "ambient" mode. The **control plane** configures them. It is worth it when many services need the same security and traffic rules, but it adds operational complexity.

### 10.19 Managed Kubernetes, upgrades and etcd backup (Asked often)

- **Managed:** EKS (AWS), AKS (Azure), GKE (Google). The provider runs the control plane (HA API servers, etcd, upgrades); you manage the worker nodes (or use serverless options like Fargate / GKE Autopilot). **Self-managed:** kubeadm, kops, RKE2, k3s (lightweight). **Local:** kind, minikube, k3d, Docker Desktop.
- **Upgrade process (Asked often):**
  1. Read the release notes.
  2. Find deprecated APIs (`pluto`, `kubent`).
  3. Back up etcd.
  4. Upgrade the **control plane first**, **one minor version at a time**.
  5. Upgrade node pools (surge/blue-green node groups, or cordon + drain while respecting PDBs).
  6. Update add-ons (CNI, CoreDNS, kube-proxy, CSI drivers).
- **Version skew:** kubelets may be up to 3 minor versions older than the API server, never newer.

```bash
# etcd snapshot (kubeadm-style certificate paths)
ETCDCTL_API=3 etcdctl snapshot save /backup/etcd-$(date +%F).db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key
# restore with: etcdutl snapshot restore <file> --data-dir <new-dir>   (older: etcdctl snapshot restore)
```

For application-level backups (namespaces, PVs), use **Velero**.

### 10.20 kubectl cheat sheet (Almost always asked)

```bash
kubectl get pods -A -o wide                     # all namespaces, with node and IP
kubectl get all -n dev
kubectl describe pod <pod>                      # EVENTS at the bottom are gold
kubectl logs <pod> -c <container> --previous -f # --previous = logs of the crashed instance
kubectl exec -it <pod> -- sh
kubectl apply -f app.yaml; kubectl delete -f app.yaml
kubectl create deployment web --image=nginx:1.28 --replicas=3 --dry-run=client -o yaml > web.yaml
kubectl expose deployment web --port=80 --target-port=8080
kubectl scale deployment web --replicas=5
kubectl set image deployment/web web=myrepo/web:1.4.3
kubectl rollout status deployment/web
kubectl rollout history deployment/web
kubectl rollout undo deployment/web --to-revision=2
kubectl rollout restart deployment/web          # restart all Pods without changing the spec
kubectl top pods; kubectl top nodes             # needs metrics-server
kubectl get events -A --sort-by=.metadata.creationTimestamp
kubectl port-forward svc/web 8080:80
kubectl cordon <node>; kubectl drain <node> --ignore-daemonsets --delete-emptydir-data; kubectl uncordon <node>
kubectl config get-contexts; kubectl config use-context <ctx>
kubectl explain deployment.spec.strategy        # built-in field documentation
kubectl auth can-i create deployments -n prod --as=jane
kubectl debug -it <pod> --image=busybox:1.37 --target=<container>
kubectl get pod <pod> -o jsonpath='{.status.podIP}'
kubectl get endpointslices -l kubernetes.io/service-name=web
kubectl api-resources
```

### 10.21 Kubernetes troubleshooting (Must know)

**General flow, like a doctor's check-up:** `kubectl get` (what's wrong?) → `kubectl describe` (read the **Events**) → `kubectl logs` (`--previous`) → `exec`/`debug` into the Pod → check the node → check Services/DNS/NetworkPolicies → check **what changed recently** (rollout history, Git, CI).

| Symptom | Common causes | How to confirm / fix |
| --- | --- | --- |
| **Pending** | Not enough CPU/memory for the **requests**, nodeSelector/affinity matches no node, taint without toleration, PVC not bound, quota exceeded | `describe pod` events (e.g. "0/5 nodes are available: insufficient cpu"); lower requests, add nodes (Cluster Autoscaler/Karpenter), fix the PVC/StorageClass |
| **ImagePullBackOff / ErrImagePull** | Wrong image name/tag, private registry without `imagePullSecrets` or IAM permission, registry rate limits, no network route to the registry | `describe pod`; try `docker pull` with the same name; fix credentials |
| **CrashLoopBackOff** | The app crashes on start: bad config or env var, missing Secret/ConfigMap key, failing liveness probe, wrong command, permission issue, dependency down | `kubectl logs --previous`, check the exit code in `describe`, run the image locally, temporarily change the command to `sleep` and debug inside |
| **OOMKilled** (exit 137) | Memory limit too low, or a memory leak | Raise the limit, profile the app, match the JVM heap (`MaxRAMPercentage`) to the limit |
| **CreateContainerConfigError** | The ConfigMap/Secret (or key) it refers to doesn't exist | `describe pod`, create or fix the reference |
| **Running but not Ready** | Readiness probe failing (wrong path/port, slow start, dependency) | `describe pod` probe failures; test the endpoint with `exec` + `curl` |
| **Service unreachable** | **Service selector doesn't match Pod labels** (very common), wrong `targetPort`, no ready Pods, NetworkPolicy blocking, app bound to 127.0.0.1, DNS problem | `kubectl get endpointslices -l kubernetes.io/service-name=<svc>` (empty = selector/readiness issue), `nslookup <svc>` from a debug Pod |
| **Node NotReady** | kubelet stopped, disk/memory/PID pressure, CNI broken, expired certificates | `describe node` (Conditions), `systemctl status kubelet`, `journalctl -u kubelet` |
| **Evicted Pods** | Node pressure (disk/ephemeral storage, memory) | Node conditions; set requests/limits for ephemeral storage; clean disks |
| **Stuck Terminating** | Finalizers waiting on a controller, unreachable node | Check `metadata.finalizers`; force delete only as a last resort (`--grace-period=0 --force`) |

### 10.22 Kubernetes interview Q&A (Must know)

**Q: Why not create Pods directly?**

A bare Pod is not recreated if it dies or its node fails. A Deployment (through its ReplicaSet) keeps the replica count, does rolling updates and supports rollback.

**Q: How does Kubernetes self-heal, and what are its limits?**

Controllers keep reconciling: the kubelet restarts failed containers, ReplicaSets replace missing Pods, Pods from failed nodes are recreated elsewhere, and unready Pods are taken out of Service endpoints. Limits: it cannot fix bad code or config (CrashLoopBackOff), it needs spare capacity, and stateful data needs careful handling.

**Q: What happens if the control plane or etcd goes down?**

Running Pods keep running and serving traffic. But nothing can be scheduled, scaled, healed or changed, and `kubectl` fails. That is why we use HA control planes (3+ etcd members) and regular etcd backups.

**Q: What happens when a worker node dies?**

The node controller marks it `NotReady`/`Unknown` after its grace period (tens of seconds). Pods are evicted after their not-ready/unreachable tolerations expire (default 300 s), and ReplicaSets recreate them on healthy nodes. StatefulSet Pods are **not** replaced until the old Pod is confirmed gone, to avoid two Pods writing the same data.

**Q: When would you run multiple containers in one Pod?**

Only for tightly linked helpers that share network/storage: sidecars (proxies, log shippers), adapters, init containers. Otherwise, one main container per Pod.

**Q: Ingress (or Gateway) vs a LoadBalancer Service?**

A LoadBalancer Service creates one cloud LB per Service (L4, costly at scale). Ingress/Gateway gives one L7 entry point that routes many hosts/paths to many Services, with TLS.

**Q: `kubectl apply` vs `kubectl create`?**

`create` is imperative and fails if the object already exists. `apply` is declarative create-or-update (client-side last-applied annotation or server-side apply), ideal for Git-driven workflows.

**Q: How do you achieve zero-downtime deployments in K8s?**

RollingUpdate with `maxUnavailable: 0`, readiness probes, a `preStop` hook + graceful SIGTERM handling, PDBs, multiple replicas spread across nodes/zones, and backward-compatible changes.

**Q: How do you secure a cluster? (Almost always asked)**

Least-privilege RBAC, no cluster-admin for apps, private API endpoint + SSO, Pod Security Admission (`restricted`), NetworkPolicies (default deny), Secrets encryption + external secret managers, image scanning and signature checks at admission, policy engines (Kyverno/Gatekeeper), audit logging, runtime detection (Falco), regular upgrades, and CIS benchmark checks (kube-bench).

**Q: How does a Pod on node A reach a Pod on node B?**

Each Pod has an IP that is routable across the cluster. The CNI plugin routes traffic between nodes (overlay tunnels, BGP routes, eBPF or VPC-native routing) without NAT.

**Q: What is a headless Service?**

A Service with `clusterIP: None`. DNS returns the individual Pod IPs. StatefulSets use it to give each Pod a stable DNS name (`db-0.db.ns.svc.cluster.local`).

**Q: How do two containers in a Pod share files?**

Mount the same `emptyDir` volume in both.

**Q: What are finalizers?**

Keys on an object that block its deletion until a controller finishes cleanup (e.g. deleting a cloud load balancer, PVC protection).

**Q: DaemonSet vs Deployment?**

A DaemonSet runs exactly one Pod per (matching) node and follows nodes as they join or leave. A Deployment runs N replicas wherever the scheduler chooses.

## 11. Helm

Helm is the **package manager for Kubernetes**, like `apt` for Ubuntu or `npm` for Node. Instead of copying 10 YAML files for every environment, you write them once as templates and fill in different values.

### 11.1 Concepts (Asked often)

Think of a chart as a recipe with blanks, values as the ingredients you fill in, and a release as the dish you cooked. You can cook the same recipe many times.

- **Chart:** a package of Kubernetes manifests written as templates.
- **Release:** one installed copy of a chart in a namespace (you can install the same chart many times).
- **Revision:** every install, upgrade or rollback creates a new numbered revision.
- **Values:** the settings filled into the templates (`values.yaml`, `-f values-prod.yaml`, `--set key=value`).
- **Repository:** where charts are stored: classic HTTP repos or **OCI registries** (ECR, GHCR, Harbor).
- Helm 3 removed **Tiller** (Helm 2's in-cluster server), which improved security. Release data is now stored as Secrets in the release's namespace.

```
mychart/
  Chart.yaml          # chart name, chart version, appVersion, dependencies
  values.yaml         # default values
  values-prod.yaml    # environment overrides (convention)
  templates/          # Go-templated manifests
    deployment.yaml
    service.yaml
    _helpers.tpl      # named template snippets (labels, names)
    NOTES.txt         # message shown after install
  charts/             # sub-chart dependencies
  .helmignore
```

```yaml
# templates/deployment.yaml (excerpt)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "web.fullname" . }}
  labels:
    {{- include "web.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  template:
    spec:
      containers:
        - name: web
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

```yaml
# values.yaml
replicaCount: 3
image:
  repository: myrepo/web
  tag: ""             # empty -> falls back to the chart's appVersion
resources:
  requests: {cpu: 250m, memory: 256Mi}
  limits: {memory: 512Mi}
```

**Reading it simply:** everything inside `{{ }}` is a blank. Helm fills `{{ .Values.replicaCount }}` with `3` from `values.yaml`. For prod, you pass `values-prod.yaml` with different numbers, and the same template produces prod YAML.

### 11.2 Commands (Asked often)

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring --create-namespace

helm create web                                   # scaffold a new chart
helm lint ./web
helm template web ./web -f values-prod.yaml       # render locally: best debugging tool
helm upgrade --install web ./web -n prod -f values-prod.yaml \
     --set image.tag=1.4.2 --wait --rollback-on-failure    # Helm 3: --atomic
helm list -A
helm history web -n prod
helm rollback web 3 -n prod
helm get values web -n prod; helm get manifest web -n prod
helm uninstall web -n prod
helm package ./web
helm push web-0.1.0.tgz oci://123456789012.dkr.ecr.ap-south-1.amazonaws.com/charts
```

- **Hooks:** `pre-install`, `post-install`, `pre-upgrade` (e.g. a DB migration Job), `pre-delete`, `test`.
- **Restart Pods when a ConfigMap changes:** add an annotation such as `checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}` to the Pod template. When the ConfigMap changes, the checksum changes, so the Pods roll.

### 11.3 Helm 4 (released Nov 2025) (Asked often)

It is the first major version in 6 years. Helm 3 charts (`apiVersion: v2`) work unchanged, and Helm 3 is being phased out.

- **Server-side apply** by default for **new** installs (existing releases keep their old apply method).
- **New plugin system** with optional **WebAssembly** plugins; post-renderers are now plugins.
- `--wait` uses **kstatus** for better readiness checks; installs by OCI digest; values files can have multiple documents.
- **CLI changes that break old CI scripts:** `--atomic` → `--rollback-on-failure`, `--force` → `--force-replace` (old names still work, with deprecation warnings). `helm registry login` now takes a hostname, not a full URL.

### 11.4 Helm vs Kustomize (Asked often)

Helm fills blanks in templates. Kustomize takes plain YAML and applies small patches on top (overlays), with no templates at all.

|  | Helm | Kustomize |
| --- | --- | --- |
| Approach | Templates + values | Template-free **overlays/patches** on plain YAML |
| Packaging/versioning | Yes (charts, repos, releases, rollback) | No packaging; built into `kubectl apply -k` |
| Best for | Sharing reusable apps with many settings | Small per-environment differences in your own manifests |

Many teams use both (render a Helm chart, then patch it with Kustomize), and GitOps tools support both.

## 12. GitOps with Argo CD and Flux

GitOps means **Git is the master plan**, and an agent inside the cluster keeps the cluster matching that plan. Think of a strict site supervisor who keeps comparing the building with the blueprint and fixes any difference.

### 12.1 What GitOps is (Almost always asked)

GitOps is a way of working where **Git is the single source of truth for the desired state** of your systems. An **agent inside the cluster continuously pulls and reconciles** the live state to match Git.

**OpenGitOps principles:** 1) **Declarative**, 2) **Versioned and immutable**, 3) **Pulled automatically**, 4) **Continuously reconciled**.

**Push vs pull, in simple words.** In push-based CD, the CI server *delivers* changes into the cluster, so CI must hold the cluster's keys. In pull-based GitOps, the cluster *fetches* changes from Git itself, so the keys never leave the cluster.

|  | Push-based CD (classic) | Pull-based GitOps |
| --- | --- | --- |
| Who deploys | The CI server runs `kubectl`/`helm` against the cluster | An in-cluster agent (Argo CD/Flux) syncs from Git |
| Cluster credentials | Stored in CI (bigger attack surface) | Stay inside the cluster |
| Drift (manual changes) | Not detected | Detected and auto-corrected (self-heal) |
| Audit / rollback | CI logs | Git history; rollback = `git revert` |

### 12.2 Typical GitOps flow (Asked often)

1. A developer merges code → **CI** builds, tests, scans and pushes `myapp:<git-sha>`.
2. CI (or a bot like Argo CD Image Updater / Flux image automation) opens a PR to the **config repo**, updating the image tag in `envs/staging`.
3. **Argo CD** sees the change and syncs it to the staging cluster.
4. Promotion to prod = a reviewed PR that changes `envs/prod`.
5. Rollback = revert the commit.

**Best practices:** keep the **app repo** and the **config repo** separate. Use per-environment **folders/overlays**, not per-environment branches. Keep secrets out of Git (External Secrets Operator) or encrypted (Sealed Secrets, SOPS).

### 12.3 Argo CD

- **Application** CRD: a source (repo, path, revision) → a destination (cluster, namespace).
- **Sync** (manual or automatic), **prune** (delete resources removed from Git), **self-heal** (undo manual changes), **sync waves/hooks** for ordering, health checks, UI, SSO, RBAC, multi-cluster.
- **ApplicationSet:** generate many Applications from one template (per cluster, per environment, per folder).
- **Argo Rollouts:** progressive delivery (canary/blue-green with automatic metric analysis).

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: web-prod
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/acme/k8s-config.git
    targetRevision: main
    path: apps/web/overlays/prod
  destination:
    server: https://kubernetes.default.svc
    namespace: web
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

**Reading it simply:** watch the `apps/web/overlays/prod` folder on the `main` branch, and keep the `web` namespace exactly like it. Delete anything removed from Git, undo any manual change, and create the namespace if it is missing.

**Flux** (CNCF graduated, like Argo CD) is a toolkit of controllers with CRDs such as `GitRepository`, `Kustomization`, `HelmRelease`, plus image automation. Flux is CLI-first and lightweight; Argo CD has a rich UI.

**Q: Someone ran `kubectl edit` in prod on a GitOps-managed app. What happens?**

Argo CD marks the app **OutOfSync**. With self-heal on, it reverts the change to match Git. The right way to change anything is a PR to the config repo.

## 13. Terraform and Infrastructure as Code

Terraform lets you build cloud infrastructure from code files instead of clicking in a console. The single most important topic here is **state**; after that, `count` vs `for_each` and how you run Terraform safely in CI.

### 13.1 IaC concepts (Almost always asked)

Clicking in the console is like building a house without a blueprint: it works once, but nobody can rebuild it exactly. IaC is the blueprint.

- **Infrastructure as Code** = infrastructure written in version-controlled, machine-readable files, not created by clicking in consoles. **Benefits:** repeatable environments, code review, audit history, speed, consistency, disaster recovery, and the code documents itself.
- **Declarative vs imperative:** declarative describes the **end result** (Terraform, CloudFormation, Kubernetes YAML). Imperative describes the **steps** (shell scripts, CLI commands). Like telling a taxi driver "take me to the airport" vs giving every turn yourself. Ansible is in between: ordered tasks built from mostly declarative, idempotent modules.
- **Mutable vs immutable infrastructure:** mutable = update servers in place, which leads to drift and "snowflake" servers (each one slightly different). Immutable = replace them with a new image/container (golden images via Packer).
- **Idempotency (Asked often):** running the same code many times gives the same result, with no extra changes after the first run. Like pressing a lift button five times: the lift is still called once.
- **Configuration drift (Asked often):** the real infrastructure slowly differs from the code, usually because of manual console changes. Detect it with `terraform plan`; prevent it by limiting console write access.

### 13.2 Terraform basics (Almost always asked)

- Terraform is HashiCorp's IaC tool. It uses **HCL** and works with any API through **providers** (AWS, Azure, GCP, Kubernetes, GitHub, Datadog, Cloudflare and thousands more).
- **Licensing in 2026:** Terraform moved from MPL 2.0 to the **Business Source License 1.1** in Aug 2023 (from v1.6). HashiCorp is now part of **IBM** (deal closed Feb 2025). **OpenTofu** is the community fork under MPL 2.0, run by the Linux Foundation (CNCF sandbox); its `tofu` CLI is a near drop-in replacement. The two have started to differ (OpenTofu: native state encryption; Terraform: ephemeral values, Stacks). For internal use the BSL rarely matters, and **the concepts below apply to both**.

**Core workflow (Almost always asked)**

```bash
terraform init        # download providers/modules, configure the backend
terraform fmt         # format code (fmt -check in CI)
terraform validate    # syntax and internal consistency
terraform plan -out=tfplan   # preview: + create, ~ update in place, -/+ replace, - destroy
terraform apply tfplan       # apply exactly what was reviewed
terraform destroy            # delete everything in this configuration
```

**Remember the flow:** init (get ready) → fmt/validate (check) → plan (preview) → apply (do it) → destroy (clean up).

**Building blocks**

| Block | What it is for |
| --- | --- |
| `terraform {}` | Required Terraform version, required providers, backend |
| `provider` | Configures a provider (region, credentials, default tags) |
| `resource` | Something Terraform creates and manages |
| `data` | **Reads** existing information (an AMI ID, an existing VPC) without managing it |
| `variable` | An input (types, defaults, validation, `sensitive`) |
| `output` | A value given out after apply (or to other configurations/modules) |
| `locals` | Named expressions, to avoid repeating yourself |
| `module` | A reusable group of resources |

**Variable precedence, lowest → highest (Asked often):** the default in the `variable` block → `TF_VAR_name` environment variables → `terraform.tfvars` → `terraform.tfvars.json` → `*.auto.tfvars` (alphabetical) → `-var` / `-var-file` on the command line (the last one wins).

### 13.3 State (Must know: the most important Terraform topic)

**State is Terraform's memory.** Your code says `aws_instance.web`, but AWS only knows `i-0abc123`. The state file connects the two. Without it, Terraform would not know which real server belongs to which line of code.

- **`terraform.tfstate`** is a JSON file that maps every resource in your code to the **real object** (IDs, attributes). Terraform uses it to work out what changed, track dependencies and details, and avoid asking the cloud about everything every time.
- **Warning:** state can contain **secrets in plain text**. Never commit it to Git; encrypt it and restrict access.
- **Remote backends** (S3, Azure Blob, GCS, HCP Terraform, ...) give the team one shared, safe, access-controlled state.
- **State locking** stops two people or pipelines from applying at the same time and corrupting the state, like a "room occupied" lock. With S3, Terraform 1.10+ supports **native S3 locking** (`use_lockfile = true`); the older DynamoDB-table locking is deprecated. Clear a stale lock with `terraform force-unlock <LOCK_ID>`, only after confirming nobody is running.
- Turn on **versioning** on the state bucket so you can recover an older state.
- Never hand-edit state. Use the commands: `terraform state list | show | mv | rm`, `import` blocks, `moved` blocks, `removed` blocks.

```hcl
terraform {
  required_version = ">= 1.10"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }

  backend "s3" {
    bucket       = "acme-tfstate-prod"
    key          = "network/terraform.tfstate"
    region       = "ap-south-1"
    encrypt      = true
    use_lockfile = true      # S3-native locking (older setups used dynamodb_table)
  }
}
```

**Reading it simply:** use Terraform 1.10 or newer and AWS provider 6.x. Keep the state in an S3 bucket, encrypted, with locking turned on.

### 13.4 A realistic configuration (Asked often)

```hcl
provider "aws" {
  region = var.region
  default_tags {
    tags = { ManagedBy = "terraform", Project = "web" }
  }
}

variable "region" {
  type    = string
  default = "ap-south-1"
}

variable "environment" {
  type = string
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "environment must be dev, staging or prod."
  }
}

locals {
  name = "web-${var.environment}"
}

module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 6.0"                    # always pin module versions

  name            = local.name
  cidr            = "10.0.0.0/16"
  azs             = ["ap-south-1a", "ap-south-1b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = var.environment != "prod"   # one NAT per AZ in prod for HA
}

data "aws_ami" "al2023" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "name"
    values = ["al2023-ami-2023.*-x86_64"]
  }
}

resource "aws_security_group" "web" {
  name   = "${local.name}-sg"
  vpc_id = module.vpc.vpc_id

  ingress {
    description = "HTTPS from anywhere"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "web" {
  for_each = {                           # stable keys -> stable addresses
    a = module.vpc.private_subnets[0]
    b = module.vpc.private_subnets[1]
  }

  ami                    = data.aws_ami.al2023.id
  instance_type          = var.environment == "prod" ? "t3.large" : "t3.micro"
  subnet_id              = each.value
  vpc_security_group_ids = [aws_security_group.web.id]
  tags                   = { Name = "${local.name}-${each.key}" }

  lifecycle {
    create_before_destroy = true
    ignore_changes        = [ami]        # don't replace instances every time a new AMI appears
  }
}

output "web_private_ips" {
  value = { for k, inst in aws_instance.web : k => inst.private_ip }
}
```

**Reading it top to bottom:**

- **provider:** work in the chosen region and tag everything automatically.
- **variables:** region defaults to Mumbai (`ap-south-1`); environment must be dev, staging or prod, or Terraform refuses.
- **module "vpc":** builds a VPC with 2 private and 2 public subnets across 2 AZs. Prod gets one NAT per AZ; other environments share one NAT to save money.
- **data "aws\_ami":** looks up (reads) the latest Amazon Linux 2023 image.
- **security group:** allow HTTPS in from anywhere, allow all traffic out.
- **aws\_instance with for\_each:** one server per private subnet, named `a` and `b`. Prod gets bigger servers. New servers are created before old ones are destroyed.
- **output:** prints each server's private IP.

### 13.5 Must-know features (Almost always asked)

- **`count` vs `for_each` (Almost always asked):** `count` makes N copies numbered by index (`aws_instance.web[0]`). If you remove one from the middle, the **indexes shift**, and Terraform destroys and recreates the wrong resources. `for_each` uses map/set **keys** (`aws_instance.web["a"]`), so the addresses stay stable. Use `count` for a simple on/off switch: `count = var.create_bastion ? 1 : 0`.
- **Why indexes shifting is bad, with an example:** `count` is like roll numbers in a class. If student #2 leaves, everyone after them gets a new number, and Terraform thinks they are different people. `for_each` uses names, so nobody's identity changes.
- **Lifecycle meta-arguments:** `create_before_destroy`, `prevent_destroy` (protect databases), `ignore_changes`, `replace_triggered_by`; plus `precondition`/`postcondition` checks.
- **Dependencies:** implicit (one resource uses another's attribute) or explicit `depends_on` when there is no reference. Terraform builds a **dependency graph (DAG)** and creates independent resources in parallel (default parallelism 10).
- **`dynamic` blocks** generate repeated nested blocks (e.g. many ingress rules from a list).
- **Modules (Asked often):** a root module + child modules; inputs (variables) → outputs. Sources: local path, Git, Terraform/OpenTofu registry, S3. Good modules do one job, are documented and versioned, have sensible defaults, and don't configure providers themselves.
- **Workspaces:** several state files for one configuration (`terraform workspace new dev`). Many teams prefer **separate folders/backends per environment** (clearer isolation, smaller damage if something goes wrong), or Terragrunt to reduce duplication.
- **Provisioners** (`local-exec`, `remote-exec`): last resort. Prefer `user_data`/cloud-init, Packer-built images or Ansible.
- **Replace a resource:** `terraform apply -replace='aws_instance.web["a"]'` (`terraform taint` is deprecated).
- **Refresh:** `terraform apply -refresh-only` updates the state to match reality without changing infrastructure (`terraform refresh` is deprecated).
- **Import existing resources:** `import` blocks (+ `terraform plan -generate-config-out=generated.tf`) or `terraform import <address> <id>`.
- **Rename without destroying:** `moved { from = aws_instance.old  to = aws_instance.web }` or `terraform state mv`.
- **`.terraform.lock.hcl`** pins exact provider versions and checksums. **Commit it.**
- **Useful functions:** `cidrsubnet`, `lookup`, `merge`, `concat`, `length`, `join`, `split`, `format`, `templatefile`, `file`, `try`, `coalesce`, `toset`, `jsonencode`.
- **Secrets:** never hardcode them. Pass them as sensitive variables from a secret manager/CI, or read them with data sources. `sensitive = true` only hides values in the CLI output; **they are still in state**. Terraform's ephemeral values (1.10+) and OpenTofu's state encryption reduce this risk.

### 13.6 Terraform in CI/CD (Almost always asked)

1. **On a PR:** `fmt -check`, `validate`, `tflint`, a security scan (**Checkov** or **Trivy**, which absorbed tfsec), a cost estimate (**Infracost**), then `plan -out=tfplan` posted as a PR comment.
2. **Review the plan.** Watch for `-/+` replacements and `-` destroys on critical resources!
3. **After merge:** `apply tfplan` (with manual approval for prod), using **OIDC** credentials and remote state with locking.
4. **Scheduled drift detection:** `terraform plan -detailed-exitcode` (exit 0 = no changes, 1 = error, 2 = changes/drift) → send an alert.
5. **Policy as code:** OPA/Conftest, Sentinel (HCP Terraform), Checkov custom policies.

**Orchestration tools:** Atlantis, HCP Terraform, Spacelift, env0, Scalr, or plain CI pipelines.

### 13.7 Terraform vs other tools (Asked often)

| Tool | Notes |
| --- | --- |
| **Terraform / OpenTofu** | Multi-cloud, HCL, explicit state, huge provider ecosystem |
| **CloudFormation** | AWS only, YAML/JSON, AWS manages the state, StackSets, drift detection |
| **AWS CDK** | Real programming languages that generate CloudFormation |
| **Bicep / ARM** | Azure-native |
| **Pulumi** | IaC in Python/TypeScript/Go/C#, multi-cloud |
| **Crossplane** | Kubernetes-native: manage cloud resources as CRDs with continuous reconciliation |

**Terraform vs Ansible (Almost always asked).** Terraform **builds the house** (servers, networks, databases). Ansible **furnishes and maintains it** (packages, files, services).

|  | Terraform | Ansible |
| --- | --- | --- |
| Main job | **Provisioning** infrastructure (VPCs, VMs, DBs, DNS) | **Configuration management** of existing machines (packages, files, services) + orchestration |
| Style | Declarative, **state-based** | Ordered tasks with idempotent modules, **no state file** |
| Typical model | Immutable (replace) | Mutable (configure in place) |
| Together | Terraform creates the servers → Ansible configures them (or Packer + Ansible bake images) | - |

### 13.8 Terraform interview Q&A (Almost always asked)

**Q: Someone changed a security group manually in the console. What happens on the next plan?**

The plan shows the drift and proposes changing it back to match the code. Decide: re-apply the code (most common), or update the code to keep the change. Then find out why manual changes were possible at all.

**Q: How do you rename a resource without destroying it?**

Add a `moved` block (or use `terraform state mv`), then confirm the plan shows no destroy.

**Q: The plan wants to destroy and recreate your production database. What do you do?**

Stop. Find the attribute marked "forces replacement" in the plan. Fix the code (revert the change, or use `ignore_changes` if that attribute is managed elsewhere). Protect critical resources with `prevent_destroy` and deletion protection. Never apply a plan you don't understand.

**Q: An apply failed halfway. Now what?**

Terraform is not transactional: resources that were created successfully are already recorded in state. Fix the error and run plan/apply again; it continues from the current state.

**Q: How do you manage dev/staging/prod?**

Reusable modules + a separate root configuration and backend (state) per environment, with environment-specific `tfvars`. Or workspaces for very similar environments. Or Terragrunt.

**Q: How do you handle a stuck state lock?**

Confirm no apply is running (check CI jobs, ask the team), then `terraform force-unlock <LOCK_ID>`.

**Q: Data source vs resource?**

A resource is created and managed by Terraform. A data source only reads something that already exists.

**Q: How do you destroy only one resource?**

`terraform destroy -target=<address>` (use `-target` rarely; it is for exceptional situations), or remove it from the code and apply.

## 14. Ansible and Configuration Management

Ansible configures servers that already exist: installs packages, copies files, starts services. Think of a teacher giving the same instructions to a whole class at once; the students (servers) need nothing special installed to listen.

### 14.1 Concepts (Almost always asked)

- **Agentless:** it connects over **SSH** to Linux and **WinRM** to Windows. **Push-based**, written in Python, playbooks in **YAML**.
- **Control node** (where Ansible runs) → **managed nodes** (the targets; they only need SSH + Python).
- **Inventory:** the list of hosts and groups. Static (INI/YAML) or **dynamic** (plugins like `amazon.aws.aws_ec2` pull the host list from the cloud).
- **Modules:** units of work (`apt`, `dnf`, `copy`, `template`, `service`, `user`, `file`, `git`, `docker_container`, `k8s`, ...). Most are **idempotent**: they check the current state and change only what is needed. The task result says `ok` (already fine) or `changed` (it fixed something).
- **Task → Play → Playbook:** a task calls one module (one instruction). A play maps tasks to a group of hosts (instructions for one group). A playbook is a list of plays (the full lesson plan).
- **Roles:** a standard folder structure to package reusable automation, like a ready-made kit. **Collections** bundle roles/modules/plugins and are shared through **Ansible Galaxy**.
- **Handlers:** tasks that run **only when notified** by a task that changed something, and only once, at the end of the play. Example: reload nginx only if its config actually changed.
- **Facts:** system information gathered automatically (`ansible_facts['os_family']`). Turn off with `gather_facts: false` for speed.
- **Templates:** Jinja2 (`.j2`) files filled in with variables.
- **Ansible Vault:** encrypts secrets in files or variables.
- **Variable precedence:** about 22 levels. Key points: role `defaults/` are the lowest, and `-e` extra vars are always the highest.

### 14.2 Inventory and playbook example (Asked often)

```ini
# inventory.ini
[web]
web1 ansible_host=10.0.1.10
web2 ansible_host=10.0.1.11

[db]
db1 ansible_host=10.0.2.10

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_ed25519
```

```yaml
# site.yml
- name: Configure web servers
  hosts: web
  become: true                 # sudo
  serial: 1                    # rolling: one host at a time
  vars:
    app_port: 8080
  tasks:
    - name: Install nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true

    - name: Deploy nginx site config
      ansible.builtin.template:
        src: templates/app.conf.j2
        dest: /etc/nginx/conf.d/app.conf
        mode: "0644"
      notify: Reload nginx

    - name: Ensure nginx is running and enabled
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true

  handlers:
    - name: Reload nginx
      ansible.builtin.service:
        name: nginx
        state: reloaded
```

**Reading it simply:** on the `web` group, as sudo, one server at a time: install nginx, put the site config in place, and make sure nginx is running and starts at boot. The "Reload nginx" handler runs only if the config file actually changed.

**Role folder structure:**

```
roles/nginx/
  tasks/main.yml        handlers/main.yml     templates/nginx.conf.j2
  files/                vars/main.yml         defaults/main.yml
  meta/main.yml
```

### 14.3 Commands (Asked often)

```bash
ansible all -i inventory.ini -m ping                          # connectivity test
ansible web -i inventory.ini -m apt -a "name=nginx state=present" -b   # ad-hoc command
ansible-playbook -i inventory.ini site.yml --check --diff     # dry run with diffs
ansible-playbook -i inventory.ini site.yml --limit web1 --tags nginx -v
ansible-inventory -i inventory.ini --graph
ansible-vault create secrets.yml; ansible-vault encrypt vars.yml; ansible-vault edit secrets.yml
ansible-playbook site.yml --ask-vault-pass        # or --vault-password-file
ansible-galaxy init roles/nginx; ansible-galaxy collection install amazon.aws
ansible-lint site.yml
```

### 14.4 Useful features

- `when:` conditions, `loop:` (replaces `with_items`), `register:` to save a task's output.
- `block` / `rescue` / `always` for error handling (like try / catch / finally); `failed_when`, `changed_when`, `ignore_errors`.
- `delegate_to` (run a task on another host, e.g. remove a node from a load balancer), `run_once`.
- `async` + `poll` for long tasks; `serial` for rolling updates; `forks` (default 5) for parallelism.
- **`command` vs `shell`:** `command` does not go through a shell (no pipes, redirects or env var expansion), so it is safer. `shell` supports them. Prefer purpose-built modules for idempotency; for raw commands use `creates:`/`removes:` or `changed_when`.
- **At scale / with a UI:** **AWX** (open source) / Red Hat Ansible Automation Platform.

### 14.5 Ansible vs Chef vs Puppet vs Salt

|  | Ansible | Chef | Puppet | SaltStack |
| --- | --- | --- | --- | --- |
| Agent | **Agentless** (SSH) | Agent | Agent | Agent (agentless mode exists) |
| Model | Push | Pull | Pull | Push/pull |
| Language | YAML | Ruby DSL | Puppet DSL | YAML |
| Learning curve | Easiest | Steeper | Steeper | Medium |

### 14.6 Ansible interview Q&A

**Q: What are handlers and why use them?**

Tasks triggered through `notify` only when something changed, and run once at the end of the play. Example: restart a service only if its config file changed.

**Q: How do you keep secrets safe?**

Ansible Vault (encrypted files/vars), or lookups from an external secret manager (HashiCorp Vault, AWS Secrets Manager). Add `no_log: true` on sensitive tasks.

**Q: How do you make playbooks faster?**

Increase `forks`, turn on SSH **pipelining**, disable or cache facts, use `async` for long tasks, use the `free` strategy where order doesn't matter, and avoid unnecessary `shell` loops.

**Q: How do you run a playbook on one host only?**

`--limit web1`.

**Q: `vars/` vs `defaults/` in a role?**

`defaults/` have the lowest precedence (meant to be overridden by users). `vars/` have high precedence (internal constants of the role).

**Q: Is Ansible idempotent?**

Its modules are designed to be. Raw `shell`/`command` tasks are not, unless you add `creates`, `removes` or `changed_when`.

## 15. Cloud Computing with AWS

AWS is the cloud asked about most often, and its ideas map directly to Azure and GCP (see the table in 15.10). The favourites are IAM, VPC networking, Security Group vs NACL, and designing a highly available 3-tier app.

### 15.1 Cloud basics (Almost always asked)

**Cloud computing** = renting computers, storage, networking and services over the internet, and **paying only for what you use**. Like renting a car instead of buying one.

**Service models (Asked often).** The classic way to remember them is **pizza**. IaaS: you get a kitchen and oven; you make the pizza. PaaS: the base is ready; you add toppings. SaaS: the pizza is delivered; you just eat.

| Model | You manage | Provider manages | Examples |
| --- | --- | --- | --- |
| **IaaS** | OS, runtime, app, data | Hardware, virtualization, network | EC2, Azure VMs, Compute Engine |
| **PaaS** | App and data | Everything below the app | Elastic Beanstalk, Azure App Service, Google App Engine |
| **SaaS** | Just use it (and your data/settings) | Everything | Gmail, Salesforce, Microsoft 365 |
| **FaaS / Serverless** | Function code | Servers, scaling | Lambda, Azure Functions, Cloud Run functions |
| **CaaS** | Containers | Orchestration infrastructure | ECS, EKS, AKS, GKE |

- **Deployment models:** public, private, hybrid, multi-cloud.
- **Shared Responsibility Model (Almost always asked):** AWS is responsible for security **OF** the cloud (data centers, hardware, network, hypervisor, internals of managed services). You are responsible for security **IN** the cloud (your data, IAM, OS patching on EC2, security group rules, encryption settings, app code). The line moves with the service: you patch the OS on EC2, but not on Lambda or RDS. It is like renting a flat: the landlord looks after the building; you lock your own door.
- **Global infrastructure:** **Regions** (e.g. `ap-south-1` Mumbai, `ap-south-2` Hyderabad) contain several isolated **Availability Zones** (one or more data centers each, with fast links). Think of a region as a city and AZs as separate buildings with their own power, so a fire in one does not affect the others. **Edge locations** serve CloudFront. **High availability = spread across AZs; disaster recovery = across regions.**
- **Scalability vs elasticity:** scalability = handling growth by adding resources. Elasticity = scaling up *and down* automatically with demand. **Vertical** scaling = a bigger machine; **horizontal** = more machines.
- **CapEx → OpEx:** from buying hardware upfront to paying for what you use.

### 15.2 IAM (Must know)

IAM decides **who can do what** in your AWS account.

| Concept | Meaning |
| --- | --- |
| **User** | A person or app with long-term credentials (avoid access keys for apps) |
| **Group** | A set of users who share permissions |
| **Role** | An identity that is **assumed** temporarily (via STS) by EC2, Lambda, EKS Pods, other accounts, CI pipelines (OIDC) or federated users. Gives **temporary credentials** |
| **Policy** | A JSON document: `Effect`, `Action`, `Resource`, optional `Condition` |
| Identity-based vs resource-based policy | Attached to a user/role vs attached to a resource (S3 bucket policy, KMS key policy, SQS policy) |

**Simple picture:** a user is an employee with a permanent ID card. A role is a visitor badge you pick up for one job and return. A policy is the list of doors that badge opens.

**Policy evaluation (Asked often):** by default everything is **denied**. An explicit **Allow** gives access. An explicit **Deny always wins**, like a "No entry" sign that beats any pass.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::my-app-bucket",
        "arn:aws:s3:::my-app-bucket/*"
      ]
    }
  ]
}
```

**Reading it simply:** allow reading files from `my-app-bucket` and listing what is inside it. Nothing else.

**Best practices:** **least privilege**, MFA everywhere, don't use the root account (lock it away), roles instead of access keys, IAM Identity Center (SSO) for humans, IAM Access Analyzer, **SCPs** (Service Control Policies in AWS Organizations) as account-level guardrails, and permission boundaries for delegated admin.

### 15.3 EC2 (Almost always asked)

- EC2 = virtual servers. **Instance families:** `t` (burstable), `m` (general purpose), `c` (compute), `r` (memory), `i` (storage-optimized), `g`/`p` (GPU). A `g` after the generation number means **Graviton (ARM)**, e.g. `m7g`, usually better price-performance.
- **AMI** (the machine image), **key pairs**, **user data** (a script that runs on first boot), **instance metadata** at `169.254.169.254` (enforce **IMDSv2**, which needs a session token and blocks many SSRF attacks).

**Pricing models (Almost always asked).** On-Demand is like a taxi on the meter. Savings Plans / Reserved are like a monthly pass. Spot is a cheap standby seat that can be taken back with 2 minutes' notice.

| Model | Discount | Best for |
| --- | --- | --- |
| On-Demand | None | Short-term, unpredictable work |
| Savings Plans / Reserved Instances | Up to \~72% for 1-3 year commitments | Steady, always-on usage |
| Spot | Up to \~90%, but AWS can take it back with a **2-minute warning** | Work that can handle interruption: batch, CI runners, stateless workers, K8s nodes with Karpenter |
| Dedicated Hosts / Instances | Not a discount option | Licensing or compliance needs |

- **Placement groups:** cluster (low latency, same rack), spread (separate hardware), partition (large distributed systems).
- **Instance store** (temporary, lost when the instance stops) vs **EBS** (persistent network disk).

### 15.4 Storage (Almost always asked)

**Simple picture:** EBS is a hard disk plugged into one computer. EFS is a shared network folder many computers can open. S3 is a huge online storage for files (objects) that you reach over the internet.

|  | EBS | EFS | S3 |
| --- | --- | --- | --- |
| Type | Block storage | Managed NFS file system | Object storage |
| Attach to | One EC2 instance (in one AZ); multi-attach only for io1/io2 | Thousands of instances across AZs | Reached over an HTTPS API from anywhere |
| Scope | One AZ | Regional (multi-AZ) | Regional, 11 nines durability |
| Use | Boot disks, databases | Shared files, Kubernetes RWX volumes | Backups, static assets, data lakes, logs, Terraform state |

**S3 essentials (Asked often):**

- Buckets, objects, keys.
- **Storage classes:** Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant Retrieval, Glacier Flexible Retrieval, Glacier Deep Archive.
- **Lifecycle rules** to move or expire data; **versioning**; replication (cross-region/same-region).
- Encryption (SSE-S3 by default for new objects, SSE-KMS, client-side); **Block Public Access**.
- Presigned URLs for temporary access; static website hosting; strong read-after-write consistency.

### 15.5 VPC networking (Must know; very frequently asked)

A **VPC** is your own private, gated colony inside AWS. **Subnets** are streets in that colony. A **public subnet** has a gate to the main road (Internet Gateway). A **private subnet** has no direct gate; it goes out through a **one-way exit door** (NAT Gateway).

```
VPC 10.0.0.0/16  (region ap-south-1)
|
|-- AZ ap-south-1a
|     public subnet   10.0.101.0/24 : ALB node, NAT Gateway      route 0.0.0.0/0 -> Internet Gateway
|     private subnet  10.0.1.0/24   : app servers / EKS nodes    route 0.0.0.0/0 -> NAT Gateway (same AZ)
|     db subnet       10.0.21.0/24  : RDS primary                no internet route
|
|-- AZ ap-south-1b
|     public subnet   10.0.102.0/24 : ALB node, NAT Gateway
|     private subnet  10.0.2.0/24   : app servers / EKS nodes
|     db subnet       10.0.22.0/24  : RDS standby
|
Internet Gateway attached to the VPC
```

- **VPC:** your isolated virtual network in one region, defined by a CIDR block.
- **Subnet:** a range inside one AZ. A subnet is **public** if its route table has `0.0.0.0/0 → Internet Gateway`; otherwise it is **private**.
- **Internet Gateway (IGW):** lets resources with public IPs talk to the internet (both directions).
- **NAT Gateway:** lets private-subnet resources make **outgoing** connections (updates, external APIs) while staying unreachable from outside. It lives in one AZ, so production uses **one per AZ**. It charges per hour **and per GB processed** (a classic surprise on the bill).
- **Route tables** decide where traffic goes.

**Security Group vs NACL (Must know).** A Security Group is a guard at each flat's door: it remembers who went out and lets them back in (stateful). A NACL is a guard at the colony gate with no memory: he checks everyone in both directions against a numbered rule list (stateless).

|  | Security Group | Network ACL |
| --- | --- | --- |
| Level | Instance / network interface | Subnet |
| State | **Stateful** (reply traffic is allowed automatically) | **Stateless** (you must allow reply traffic, incl. ephemeral ports 1024-65535) |
| Rules | **Allow only** | Allow **and Deny** |
| Evaluation | All rules checked together | In **number order**; first match wins |
| Default | New SG: deny all inbound, allow all outbound | Default NACL allows all; a new custom NACL denies all until you add rules |
| Typical use | Main access control; can refer to other SGs (e.g. "allow DB port from app SG") | Rough subnet-level guardrails, blocking specific IPs |

**Connectivity options:**

- **VPC Peering:** a private 1:1 link between two VPCs. **Not transitive** (A↔B and B↔C does not mean A↔C), and CIDRs must not overlap.
- **Transit Gateway:** a hub-and-spoke router for many VPCs and on-prem networks.
- **VPC Endpoints (Asked often):** reach AWS services privately, without the internet or NAT. **Gateway endpoints** (S3, DynamoDB; free) and **Interface endpoints / PrivateLink** (most other services). They also cut NAT data charges.
- **Site-to-Site VPN** (encrypted over the internet) and **Direct Connect** (a dedicated private line) to on-premises.
- **Bastion host vs SSM Session Manager:** Session Manager needs no open SSH port, is controlled by IAM and is logged. Prefer Session Manager.
- **VPC Flow Logs** record ACCEPT/REJECT traffic details; great for troubleshooting connectivity.
- **Elastic IP** = a fixed public IPv4. AWS charges for all public IPv4 addresses (since Feb 2024), so don't leave them idle.

### 15.6 Load balancing and Auto Scaling (Almost always asked)

**ALB vs NLB in simple words:** an ALB reads the letter inside the envelope (HTTP path, host, headers) before deciding where to send it. An NLB only reads the address on the envelope (IP and port), so it is extremely fast.

| Load balancer | Layer | Highlights |
| --- | --- | --- |
| **ALB** | L7 | HTTP/HTTPS/gRPC, routing by host/path/header, WebSockets; targets: instances, IPs, Lambda; works with WAF and Cognito |
| **NLB** | L4 | TCP/UDP/TLS, ultra-low latency, millions of requests/sec, a fixed IP per AZ (Elastic IP support) |
| **GWLB** | L3 | Inserts third-party virtual appliances (firewalls, IDS) transparently |
| CLB | L4/L7 | Old; avoid for new designs |

**Concepts:** listeners and rules, **target groups**, **health checks**, cross-zone load balancing, **deregistration delay** (connection draining), sticky sessions.

**Auto Scaling Group (ASG):** keeps min/desired/max instances from a **launch template**, spread across AZs, and replaces unhealthy ones (EC2 or ELB health checks). **Policies:** **target tracking** (e.g. keep average CPU at 50%), step scaling, scheduled scaling, predictive scaling. **Extras:** lifecycle hooks, **instance refresh** (rolling replacement for new AMIs), warm pools.

### 15.7 Databases (Almost always asked)

- **RDS:** managed MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, with automatic backups, point-in-time restore and patching.
  - **Multi-AZ (Asked often)** = a **synchronous** standby copy in another AZ with **automatic failover** → **high availability**. The classic standby does not serve reads. Think of a substitute goalkeeper waiting on the bench.
  - **Read Replicas (Asked often)** = **asynchronous** copies for **read scaling**. They can be in another region and can be promoted (useful for DR). Think of extra helpers who only answer questions (reads) to reduce the load.
- **Aurora:** cloud-native MySQL/PostgreSQL-compatible engine. Storage is copied 6 ways across 3 AZs; up to 15 read replicas; Aurora Serverless v2.
- **DynamoDB:** serverless NoSQL key-value/document DB with single-digit-millisecond latency. Design around the **partition key**; on-demand or provisioned capacity; DAX cache; global tables (multi-region).
- **ElastiCache:** managed in-memory cache (Valkey/Redis OSS, Memcached) for sessions, caching, leaderboards, rate limiting.

### 15.8 Other services you should know (Asked often)

| Service | In one line |
| --- | --- |
| **Route 53** | DNS + health checks. Routing policies: simple, **weighted** (canary), **latency**, **failover** (DR), geolocation, geoproximity, multivalue. Alias records at the apex |
| **CloudFront** | CDN; use Origin Access Control (OAC) to keep S3 origins private |
| **Lambda** | Event-driven functions; max 15-minute run; pay per request and duration; watch cold starts |
| **API Gateway** | Managed APIs in front of Lambda/HTTP backends (auth, throttling) |
| **SQS** | Message queue for decoupling. Standard (at-least-once, best-effort order) vs FIFO (ordered, exactly-once processing); visibility timeout; dead-letter queues |
| **SNS** | Pub/sub notifications; fan-out to SQS, Lambda, email, SMS |
| **EventBridge** | Event bus and scheduler for event-driven designs |
| **ECS** | AWS's own container orchestrator (tasks, services) on EC2 or **Fargate** |
| **EKS** | Managed Kubernetes (control plane managed; EKS Auto Mode can also manage nodes and core add-ons) |
| **Fargate** | Serverless compute for containers (no nodes to manage) |
| **ECR** | Container registry with image scanning and lifecycle policies |
| **CloudWatch** | Metrics, logs (Logs Insights), alarms, dashboards, Container Insights |
| **CloudTrail** | **Audit log of API calls**: who did what, when, from where |
| **AWS Config** | History of resource configuration + compliance rules |
| **Systems Manager (SSM)** | Session Manager, Parameter Store, Patch Manager, Run Command |
| **Secrets Manager vs Parameter Store** | Secrets Manager: built-in automatic rotation, costs more. Parameter Store: config + SecureString, cheap/free standard tier, no built-in rotation |
| **KMS** | Managed encryption keys (envelope encryption), used by most AWS services |
| **ACM** | Free public TLS certificates for ALB/CloudFront, with auto-renewal |
| **WAF / Shield** | Web application firewall / DDoS protection |
| **GuardDuty / Inspector / Security Hub** | Threat detection / vulnerability scanning / central security findings |
| **Organizations / Control Tower** | Multi-account management, SCP guardrails, landing zones |
| **AWS Backup** | Central, policy-based backups across services |

**CloudWatch vs CloudTrail (Asked often):** CloudWatch = **performance monitoring** (metrics, logs, alarms), like a health monitor showing heartbeat and BP. CloudTrail = **auditing** (records every API call for security and compliance), like CCTV recording who did what.

### 15.9 Well-Architected Framework: 6 pillars (Asked often)

1. **Operational Excellence:** IaC, small reversible changes, observability, learning from failures.
2. **Security:** least privilege, traceability, encryption in transit and at rest, defense in depth.
3. **Reliability:** multi-AZ, automatic recovery, tested backups, scaling, managing quotas.
4. **Performance Efficiency:** the right resource types, serverless, caching, measure and experiment.
5. **Cost Optimization:** pay for what you use, right-size, commitments, measure spend per team.
6. **Sustainability:** maximize utilization, efficient instance types (e.g. Graviton), reduce waste.

### 15.10 AWS vs Azure vs GCP mapping (Asked often)

| Category | AWS | Azure | GCP |
| --- | --- | --- | --- |
| Virtual machines | EC2 | Virtual Machines | Compute Engine |
| Autoscaling groups | Auto Scaling Groups | VM Scale Sets | Managed Instance Groups |
| Managed Kubernetes | EKS | AKS | GKE |
| Serverless functions | Lambda | Azure Functions | Cloud Run functions |
| Serverless containers | Fargate / App Runner | Container Apps | Cloud Run |
| Object storage | S3 | Blob Storage | Cloud Storage |
| Block storage | EBS | Managed Disks | Persistent Disk |
| File storage | EFS | Azure Files | Filestore |
| Relational DB | RDS / Aurora | Azure SQL / Azure Database for PostgreSQL | Cloud SQL / AlloyDB / Spanner |
| NoSQL | DynamoDB | Cosmos DB | Firestore / Bigtable |
| Virtual network | VPC | Virtual Network (VNet) | VPC (global) |
| Network firewall rules | Security Groups / NACLs | Network Security Groups | VPC firewall rules |
| L7 load balancer | ALB | Application Gateway | Cloud Load Balancing (HTTP(S)) |
| DNS | Route 53 | Azure DNS | Cloud DNS |
| CDN | CloudFront | Azure Front Door | Cloud CDN |
| Identity | IAM | Microsoft Entra ID + Azure RBAC | Cloud IAM |
| Secrets | Secrets Manager | Key Vault | Secret Manager |
| Monitoring | CloudWatch | Azure Monitor | Cloud Monitoring / Logging |
| Audit logs | CloudTrail | Activity Log | Cloud Audit Logs |
| Native IaC | CloudFormation / CDK | ARM / Bicep | Infrastructure Manager |
| CI/CD | CodePipeline / CodeBuild | Azure DevOps Pipelines | Cloud Build / Cloud Deploy |
| Container registry | ECR | ACR | Artifact Registry |
| Messaging | SQS / SNS | Service Bus / Event Grid | Pub/Sub |

### 15.11 Cost optimization, FinOps (Almost always asked)

- **Right-size** instances (use CloudWatch/Compute Optimizer data).
- **Savings Plans** for steady load; **Spot** for work that can handle interruption.
- Autoscaling and **schedules** (stop dev/test at night and on weekends); Graviton.
- S3 lifecycle policies (move old data to IA/Glacier).
- Delete unattached EBS volumes, old snapshots, idle load balancers and unused Elastic IPs.
- **VPC endpoints** to cut NAT data charges.
- Mandatory **tagging** so costs can be assigned to teams; AWS Budgets + Cost Anomaly Detection.
- In Kubernetes: right-size requests, use Karpenter consolidation, track cost per namespace with OpenCost/Kubecost.

### 15.12 Classic question: design a highly available 3-tier web app on AWS (Must know)

Walk through it from the user's browser down to the database, layer by layer.

1. **DNS:** Route 53 (health checks; failover/latency routing if multi-region).
2. **Edge:** CloudFront for static assets and caching + **AWS WAF** + Shield.
3. **Web/LB tier:** an **ALB** in public subnets across **at least 2 AZs**, HTTPS with an ACM certificate.
4. **App tier:** an **Auto Scaling Group** (or EKS/ECS) in **private subnets** across AZs. Stateless app instances; sessions kept in **ElastiCache**.
5. **Data tier:** **RDS Multi-AZ** (+ read replicas for read-heavy load) in isolated DB subnets. Security groups allow DB access only from the app SG.
6. **Storage:** S3 for uploads and static content (private, served through CloudFront OAC).
7. **Outbound internet:** a NAT Gateway per AZ; VPC endpoints for S3/DynamoDB.
8. **Security:** least-privilege IAM roles, Secrets Manager, KMS encryption, SSM Session Manager instead of SSH, GuardDuty.
9. **Observability:** CloudWatch metrics/logs/alarms (or Prometheus/Grafana), CloudTrail, ALB access logs.
10. **Resilience:** AWS Backup, cross-region snapshot copies for DR, regular restore tests.
11. **Delivery:** everything in Terraform; CI/CD with blue-green or canary deployments.

## 16. Monitoring, Logging and Observability

You can't fix what you can't see. Interviewers check if you know the three pillars, Prometheus (metric types and basic PromQL), and how to write alerts that don't wake people up for nothing.

### 16.1 Concepts (Almost always asked)

- **Monitoring** = collecting fixed metrics and checks to know **when** something is wrong (problems you already expected).
- **Observability** = being able to understand **why** the system behaves a certain way from its outputs, even for questions you never expected.

Think of a car. **Monitoring** is the warning light on the dashboard: it tells you *that* something is wrong. **Observability** is the mechanic's full diagnostic tool: it tells you *why*.

**Three pillars (Almost always asked).** Metrics are like your pulse and BP readings over time. Logs are like a diary of what happened. Traces are like the GPS route of one request through all the services.

| Pillar | What it is | Strength | Tools |
| --- | --- | --- | --- |
| **Metrics** | Numbers over time (CPU, request rate, latency) | Cheap, easy to combine, perfect for alerts and trends | Prometheus, CloudWatch, Datadog |
| **Logs** | Timestamped event records | Rich detail and context for debugging | ELK/EFK, Loki, CloudWatch Logs, Splunk |
| **Traces** | The path of one request across services (spans with timings) | Shows *where* the delay or error happens in microservices | Jaeger, Grafana Tempo, Zipkin, X-Ray |

Continuous **profiling** is often called the fourth pillar.

**What to measure: three frameworks (Almost always asked)**

- **Four Golden Signals** (Google SRE): **Latency, Traffic, Errors, Saturation**.
- **RED** (for services that handle requests): **Rate, Errors, Duration**.
- **USE** (for resources like CPU, disk, network): **Utilization, Saturation, Errors**.

**Easy way to remember:** RED is for **services** (how are requests doing?). USE is for **machines** (how are resources doing?).

### 16.2 Prometheus (Must know)

- An open-source (CNCF graduated) monitoring system and time-series database, with the **PromQL** query language and alerting.
- **Pull model:** Prometheus **scrapes** (collects from) HTTP `/metrics` endpoints at a fixed interval, finding targets through service discovery (Kubernetes, EC2, Consul, files). It is like a census officer visiting each house, instead of each house sending its data. For short-lived batch jobs, use the **Pushgateway**.
- **Components:**
  - The Prometheus server (scraper + time-series database + query engine).
  - **Exporters:** `node_exporter` (host metrics), `blackbox_exporter` (probing URLs/ports), DB exporters, **kube-state-metrics** (state of Kubernetes objects), **cAdvisor** in the kubelet (container metrics).
  - Client libraries to add metrics to your own code.
  - **Alertmanager:** removes duplicates, groups, routes, silences and inhibits alerts → Slack, PagerDuty, email.
  - **Grafana** for dashboards.
- **In Kubernetes,** the usual install is **kube-prometheus-stack** (Prometheus Operator with `ServiceMonitor`/`PodMonitor` CRDs, Alertmanager, Grafana, exporters).
- **Long-term storage / HA / multi-cluster:** Thanos, Grafana Mimir, Cortex, VictoriaMetrics (via `remote_write`).

**Metric types (Almost always asked).** A **counter** is like a car's odometer: it only goes up. A **gauge** is like the speedometer: it goes up and down. A **histogram** is like sorting exam marks into ranges (0-40, 40-60, ...) so you can work out percentiles later. A **summary** is like each school working out its own topper percentile: you cannot combine those across schools.

| Type | Behaviour | Example |
| --- | --- | --- |
| **Counter** | Only goes up (resets to 0 on restart); use `rate()` on it | `http_requests_total` |
| **Gauge** | Goes up and down | Memory in use, queue length, temperature |
| **Histogram** | Counts values in buckets; quantiles are calculated at query time with `histogram_quantile()`; can be combined | Request latency |
| **Summary** | Quantiles calculated in the client; **cannot be combined** across instances | Latency in a single process |

**Data model:** metric name + **labels** = one unique time series.

**Warning: high-cardinality labels** (user ID, request ID, full URL) create millions of series and can bring Prometheus down. Use labels with a small, fixed set of values.

**PromQL you should be able to write (Almost always asked)**

```promql
# Requests per second over the last 5 minutes
rate(http_requests_total[5m])

# Requests per second by status code
sum by (status) (rate(http_requests_total[5m]))

# Error ratio (5xx / all)
sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m]))

# 95th percentile latency
histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))

# CPU usage % per node
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Memory used % per node
100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)

# Disk predicted to fill within 4 hours
predict_linear(node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"}[6h], 4 * 3600) < 0

# Containers restarting a lot (kube-state-metrics)
increase(kube_pod_container_status_restarts_total[15m]) > 3

# Scrape targets that are down
up == 0
```

**Three functions people mix up:** `rate()` = the per-second average over the window (smooth; use it for alerts). `irate()` = from the last two samples only (spiky; for fast-moving graphs). `increase()` = the total increase over the window.

**Alert rule example**

```yaml
groups:
  - name: web-alerts
    rules:
      - alert: HighErrorRate
        expr: |
          sum(rate(http_requests_total{status=~"5.."}[5m]))
            / sum(rate(http_requests_total[5m])) > 0.05
        for: 10m                       # must be true for 10 minutes (avoids flapping)
        labels:
          severity: critical
        annotations:
          summary: "5xx error rate above 5% for 10 minutes"
          runbook_url: "https://wiki.example.com/runbooks/high-error-rate"
```

**Reading it simply:** if more than 5% of requests return 5xx errors, and this stays true for 10 minutes, fire a critical alert with a link to the runbook.

**Pull vs push:** pull makes it easy to know if a target is down (`up == 0`) and keeps control in one place. Push suits short-lived jobs and targets behind firewalls/NAT.

### 16.3 Grafana

Grafana builds dashboards over many data sources (Prometheus, Loki, Tempo, Elasticsearch, CloudWatch, SQL). Features: variables/templating, alerting, annotations (mark deployments on graphs), and dashboards as code (JSON, Terraform provider).

### 16.4 Logging (Almost always asked)

- **Centralized logging is essential**, because containers and Pods are temporary (their logs vanish with them) and requests cross many services.
- **ELK / Elastic Stack:** **E**lasticsearch (store, index, search) + **L**ogstash (collect, parse, transform) + **K**ibana (explore, visualize); Beats (Filebeat) ship the logs. **EFK** swaps Logstash for **Fluentd/Fluent Bit** (lighter; runs as a Kubernetes DaemonSet). **OpenSearch** is the AWS-led fork.
- **Grafana Loki** indexes only **labels**, not the full text, so it is much cheaper to run. It is like a library that indexes only book titles, not every word inside. Queried with **LogQL**. Collectors: Grafana Alloy (Promtail is deprecated), Fluent Bit, OpenTelemetry Collector.

```logql
{namespace="prod", app="web"} |= "error"
sum by (app) (count_over_time({namespace="prod"} |= "error" [5m]))
```

- **Kubernetes logging pattern:** apps write to **stdout/stderr** → the container runtime writes files under `/var/log/containers` on the node → a **node-level agent (DaemonSet)** ships them to the backend.
- **Best practices:** structured **JSON** logs, consistent log levels, include **trace/correlation IDs**, never log secrets or personal data, and set retention and sampling to control cost.

### 16.5 Tracing and OpenTelemetry (Almost always asked)

- **Distributed tracing:** a **trace** is one request's full journey. It is made of **spans** (single operations with a start time and duration). The trace context travels between services in headers (W3C `traceparent`). Think of a courier tracking number that gets scanned at every hub.
- **OpenTelemetry (OTel)** is the CNCF vendor-neutral standard (APIs, SDKs, auto-instrumentation, the **OTLP** protocol and the **Collector**) for creating and exporting **traces, metrics and logs**. Instrument once, then send the data to any backend (Jaeger, Tempo, Prometheus, Datadog, New Relic, Honeycomb, Elastic). Like a universal charger that fits every phone.
- **Collector pipeline:** **receivers** (e.g. OTLP) → **processors** (batching, sampling, filtering, adding attributes) → **exporters** (backends).
- **Commercial all-in-one APM/observability:** Datadog, New Relic, Dynatrace, Elastic Observability, Grafana Cloud.

### 16.6 Alerting best practices (Almost always asked)

1. Alert on **symptoms users feel** (error rate, latency, SLO burn rate), not on every cause (CPU 80% on one node).
2. Every page must be **actionable** and link to a **runbook**.
3. Use severities and routing: page only for urgent issues; send the rest to tickets/chat.
4. Avoid **alert fatigue**: remove duplicates, group, tune thresholds, use `for:` durations, delete alerts nobody acts on. Too many false alarms and people start ignoring the real ones.
5. Dashboards are for exploring; alerts are for action.

## 17. Site Reliability Engineering

SRE puts **numbers** on reliability. The must-know items are SLI/SLO/SLA, error budgets, the "nines" table, and how you run an incident.

### 17.1 SLI, SLO, SLA and error budgets (Must know)

Think of pizza delivery. The **SLI** is the actual delivery time you measure. The **SLO** is your internal target ("we deliver within 25 minutes"). The **SLA** is the promise to customers with a penalty ("30 minutes or it's free").

| Term | Meaning | Example |
| --- | --- | --- |
| **SLI** (Indicator) | A measured number that shows service health from the user's point of view | % of HTTP requests that succeed in under 300 ms |
| **SLO** (Objective) | Your internal **target** for an SLI over a time window | 99.9% of requests succeed within 300 ms over 30 days |
| **SLA** (Agreement) | A **contract** with customers, with penalties (credits) if missed | 99.5% monthly availability, or a 10% credit |

- The SLA is usually **looser** than the SLO, which gives you a safety buffer.
- **Error budget = 1 − SLO.** For 99.9% over 30 days: 0.1% of 43,200 minutes = **43.2 minutes** of allowed "badness". Treat it like a monthly data pack you are allowed to use up.
- **Error budget policy:** budget left → ship features and take reasonable risks. Budget used up → freeze risky releases and focus on reliability work. This turns the Dev vs Ops argument into a shared, data-based decision.

**Availability "nines" (Almost always asked)**

| Availability | Downtime per year | per 30-day month | per week |
| --- | --- | --- | --- |
| 99% | 3.65 days | 7.2 hours | 1.68 hours |
| 99.9% | 8.76 hours | 43.2 minutes | 10.1 minutes |
| 99.95% | 4.38 hours | 21.6 minutes | 5.04 minutes |
| 99.99% | 52.6 minutes | 4.32 minutes | 1.01 minutes |
| 99.999% | 5.26 minutes | 25.9 seconds | 6.05 seconds |

**Trick to remember:** every extra "9" cuts the allowed downtime by 10 times. 99.9% ≈ 43 minutes a month; 99.99% ≈ 4 minutes a month.

- **Serial dependencies multiply:** two services at 99.9% in a chain → about 99.8% overall. Like bulbs wired in series: if one fails, the whole chain goes dark.
- **Redundancy helps:** two independent 99% replicas in parallel → 1 − (0.01 × 0.01) = 99.99%. Both must fail at the same time for an outage.
- **Burn-rate alerting:** alert when the error budget is being used up much faster than is sustainable. Like spending half your monthly salary in two days. Example: a burn rate of 14.4 for 1 hour uses 2% of a 30-day budget, which is worth paging someone. Use multiple windows (a long one to be sure it's real, a short one so the alert clears quickly).

### 17.2 Toil (Asked often)

Toil is work that is **manual, repetitive, automatable, tactical, has no lasting value, and grows as the service grows**. It is like washing dishes by hand every day instead of buying a dishwasher. Google SRE limits operational work to about **50%** of an SRE's time; the rest goes to engineering that removes toil.

### 17.3 Incident management (Almost always asked)

Run an incident like a fire brigade: a commander, someone talking to the public, firefighters, and a note-taker. **Put out the fire first; investigate the cause later.**

1. **Detect** (alerts, users, synthetic checks) and **declare** an incident with a severity (SEV1 = critical customer impact ... SEV4 = minor).
2. **Assign roles:** **Incident Commander** (coordinates, decides), **Communications lead** (status page, stakeholders), **Operations/subject experts** (hands on keyboard), **Scribe** (writes the timeline).
3. **Mitigate first, root-cause later:** roll back, fail over, scale out, turn off a feature flag, shed load.
4. **Resolve** and keep monitoring.
5. **Blameless postmortem (Asked often):** summary, impact, timeline, root cause(s) and contributing factors (5 Whys / fishbone), what went well, what went badly, where we got lucky, and **action items with owners and dates**. Focus on systems and processes, not on blaming people. Share it widely.

**Time metrics:** **MTTD** (mean time to detect), **MTTA** (to acknowledge), **MTTR** (to recover/restore; always say which "R" you mean), **MTBF** (mean time between failures).

**On-call hygiene:** fair rotations, escalation policies, runbooks, handoff notes, a manageable number of pages, and follow-up on every page.

### 17.4 Chaos engineering (Asked often)

Chaos engineering is a **fire drill for your system**: you break things on purpose, in a controlled way, to find weak spots before real failures do.

**Steps:** define the **steady state** (normal behaviour) → form a **hypothesis** ("if one AZ goes down, users won't notice") → **inject** a failure (kill Pods, add latency, drop a node or a whole AZ) → **observe** → fix the weaknesses.

Start in staging, keep the **blast radius** (how much can break) small, and run **game days**. **Tools:** Chaos Monkey, LitmusChaos, Chaos Mesh, **AWS Fault Injection Service**, Gremlin.

### 17.5 Reliability patterns (Almost always asked)

- **Timeouts** on every network call. Never wait forever.
- **Retries with exponential backoff + jitter** (only for idempotent operations): wait 1 s, then 2 s, then 4 s, plus a little randomness, so thousands of clients don't all retry at the same moment.
- **Circuit breakers:** stop calling a dependency that keeps failing, like the circuit breaker in your house that trips to protect the wiring.
- **Bulkheads:** isolate resource pools, like the separate compartments in a ship that stop one leak from sinking it.
- **Rate limiting and load shedding:** refuse some requests to protect the rest.
- **Graceful degradation:** serve cached or partial results instead of failing completely.
- **Idempotency keys**, **queues** to absorb spikes, health checks + redundancy across AZs, capacity planning and load testing.

## 18. DevSecOps and Supply Chain Security

Security is no longer a separate team checking things at the very end. Expect questions on which security checks go where in the pipeline, SAST vs DAST vs SCA, supply chain security and secrets management.

### 18.1 What DevSecOps means (Asked often)

DevSecOps means security is built into every stage of the DevOps lifecycle, owned by everyone, and enforced through **automated guardrails** in pipelines, not a manual gate at the end. This is "shift left", plus protection while the app runs (sometimes called "shield right").

Think of a car. Security is not a guard checking the car at the factory exit. It is the seatbelts, airbags and brakes built into the car from the first day.

### 18.2 Security checks in the pipeline (Almost always asked)

| Stage | Check | What it finds | Tools |
| --- | --- | --- | --- |
| Pre-commit / PR | **Secret scanning** | Keys, tokens, passwords in code | gitleaks, trufflehog, GitHub push protection |
| Build | **SAST** (static application security testing) | Risky patterns in **your own source code** (SQL injection, XSS) | SonarQube, Semgrep, CodeQL, Checkmarx |
| Build | **SCA** (software composition analysis) | Known CVEs and license problems in **open-source dependencies** | Dependabot, Snyk, OWASP Dependency-Check, Trivy, Renovate (updates) |
| Build | **IaC scanning** | Misconfigurations (public S3 buckets, open security groups) | Checkov, Trivy (includes the former tfsec), KICS |
| Build | **Container image scanning** | OS/package CVEs inside images | Trivy, Grype, Docker Scout, ECR scanning |
| Test | **DAST** (dynamic testing of the running app, black-box) | Runtime issues: auth flaws, injection, misconfigured headers | OWASP ZAP (now ZAP by Checkmarx), Burp Suite |
| Deploy | **Policy as code / admission control** | Non-compliant manifests, unsigned images | OPA Gatekeeper, Kyverno, Conftest |
| Runtime | **Runtime threat detection** | Suspicious behaviour (a shell in a container, crypto-mining) | Falco, Tetragon, GuardDuty |

**SAST vs DAST vs SCA (Asked often).** Picture a building:

- **SAST** reads the building's blueprint to find weak points, without the building existing yet. It reads your code without running it (white-box, early, may give false alarms).
- **DAST** tries to break into the finished building from outside. It attacks the running app (black-box, later, finds real runtime issues).
- **SCA** checks that the bricks you bought from suppliers are not on a recall list. It checks third-party dependencies against vulnerability databases.

### 18.3 Software supply chain security (Almost always asked; hot topic)

Attacks like SolarWinds (2020), Log4Shell (2021), the xz-utils backdoor (2024) and the compromised `tj-actions/changed-files` GitHub Action (2025) showed that **everything you depend on** can be attacked.

Think of food safety: you must know every ingredient and where it came from. The **SBOM** is the ingredients label, **signing** is the tamper-proof seal, and **provenance** is the certificate of where and how it was made.

- **SBOM** (Software Bill of Materials): a machine-readable list of every component in your software (formats: **SPDX**, **CycloneDX**; generate with Syft or Trivy). When a new CVE appears, you can instantly find which services are affected.
- **Signing and verification:** **Sigstore**. `cosign` signs images; keyless signing uses your OIDC identity via the Fulcio CA and is recorded in the Rekor transparency log. Check signatures at deploy time with admission policies (Kyverno, Sigstore policy-controller).
- **SLSA** (Supply-chain Levels for Software Artifacts): a framework for build integrity with **provenance attestations** (who built what, from which source, how) and increasing levels of assurance.
- **Pin dependencies** (lockfiles, image digests, CI actions by commit SHA), use private mirrors/registries, minimal base images, reproducible builds, and least-privilege CI tokens.

### 18.4 Secrets management (Almost always asked)

- **Never** store secrets in Git, images, plain env files in repos, or logs.
- Use a **secret manager:** HashiCorp Vault (dynamic short-lived secrets, leases, revocation, encryption as a service, Kubernetes auth), AWS Secrets Manager / Parameter Store, Azure Key Vault, GCP Secret Manager.
- **In Kubernetes:** **External Secrets Operator** or the **Secrets Store CSI driver**; encrypt etcd; RBAC on Secrets.
- Prefer **short-lived credentials:** IAM roles, OIDC federation for CI, workload identity for Pods. Rotate regularly and automatically. A key that expires in one hour is far less dangerous if it leaks.

### 18.5 Other security fundamentals

- **Least privilege** and **zero trust** ("never trust, always verify"): authenticate and authorize every request, even inside the network, and use mTLS between services. Like airport security that checks everyone at every gate, even staff.
- **CIS Benchmarks** for hardening Linux, Docker and Kubernetes (`kube-bench`, `docker-bench-security`).
- **OWASP Top 10** web application risks (broken access control, injection, security misconfiguration, vulnerable components, etc.).
- **CVE** = a public ID for a vulnerability. **CVSS** = its severity score (0-10). Define patch SLAs (e.g. critical issues fixed within 7 days).
- Encryption in transit (TLS everywhere) and at rest (KMS); audit logging (CloudTrail, Kubernetes audit logs); regular patching and dependency updates.

## 19. Platform Engineering, AI and 2026 Trends

These topics show the interviewer that you follow where DevOps is going. Expect at least one question on platform engineering or AI.

### 19.1 Platform engineering and Internal Developer Platforms (Almost always asked)

Think of a shopping mall. The mall management provides electricity, water, security and ready-to-use shops. Each shop owner just moves in and starts selling. The **platform team** is the mall management, and **developers** are the shop owners.

- A dedicated **platform team** builds an **Internal Developer Platform (IDP)** and treats it as a **product**, with developers as its customers.
- It offers **self-service golden paths / paved roads**. Example: "create a new service" generates a repo with CI/CD, Dockerfile, Helm chart, dashboards, alerts and security scanning already connected. A golden path is like a well-paved road with signboards: you *can* go off-road, but the paved road is the easiest way.
- A **developer portal** (Backstage, a CNCF project from Spotify; also Port, Cortex) gives a software catalog, templates, docs and ownership info.
- **Goals:** reduce developers' **cognitive load** (how much they must think about), standardize with guardrails instead of gates, and deliver faster. Measure it with DORA metrics plus developer-experience surveys.
- **DORA 2025:** about 90% of organizations have adopted at least one internal platform. A **high-quality platform is what lets organizations turn AI-assisted coding into real delivery improvements**.

### 19.2 AI in DevOps (Asked often; expect at least one question)

- **What is happening:** AI coding assistants and agents write code, Dockerfiles, Terraform, pipelines and Kubernetes manifests. AI helps with incident triage (summarizing alerts and logs, suggesting likely causes), anomaly detection (AIOps), ChatOps, and plain-English questions over observability data. Protocols such as **MCP (Model Context Protocol)** let AI agents use tools like Kubernetes, CI systems and cloud APIs.
- **DORA 2025 takeaway:** AI is an **amplifier**. Teams with good foundations get faster. Teams with weak testing, large batches and poor platforms get more instability. Small batches, strong version control and fast feedback matter more now, not less.
- **Risks to mention:** made-up (hallucinated) or insecure configs, leaking secrets/code into prompts, agents with too many permissions, unreviewed changes.
- **Controls to mention:** mandatory code review, automated tests and policy checks, least-privilege credentials for agents, human approval for production actions, audit logs.
- **MLOps / LLMOps basics:** versioned data and models, training pipelines, model registries, serving (KServe, vLLM), GPU scheduling on Kubernetes (Dynamic Resource Allocation is stable; workload-aware/**gang scheduling** APIs reached beta in v1.37), and monitoring for drift and cost.

**Interview tip:** a balanced answer wins here. Say AI makes you faster, then immediately say how you keep it safe (review, tests, least privilege, human approval).

### 19.3 Other trends worth naming

| Trend | In one line |
| --- | --- |
| Gateway API | Replacing Ingress as the standard for Kubernetes traffic (Ingress NGINX retired in 2026) |
| OpenTofu vs Terraform | Open-source fork vs the IBM-owned BSL tool; the concepts are identical |
| eBPF | Runs safe, sandboxed programs inside the Linux kernel; powers Cilium networking, Tetragon security and low-overhead observability |
| OpenTelemetry everywhere | Vendor-neutral instrumentation is now the default choice |
| FinOps | Engineers own cloud cost; cost is visible per team/service |
| Supply chain security | SBOMs, signing, provenance, pinned dependencies |
| GitOps | The default way to run Kubernetes deployments at scale |
| Kubernetes for AI | DRA for GPUs, gang scheduling, in-place resize, rootless kubelet (beta in v1.37) |
| Helm 4 | Server-side apply, Wasm plugins, renamed CLI flags |

## 20. System Design for DevOps

Design questions test whether you can combine everything into a working system. Start with the vocabulary (HA, DR, RTO, RPO), then practise the four designs below out loud.

### 20.1 HA vs fault tolerance vs disaster recovery (Almost always asked)

Think of travel. **High availability** is a car with a spare tyre: a short stop, then you continue. **Fault tolerance** is a plane with several engines: if one fails, it keeps flying with no interruption. **Disaster recovery** is your plan after a flood destroys the whole house: insurance and a backup home in another city.

| Term | Meaning | Example |
| --- | --- | --- |
| **High availability (HA)** | Keep downtime very low through redundancy and automatic failover (a brief blip is okay) | App across 3 AZs behind an ALB; RDS Multi-AZ |
| **Fault tolerance** | Keep working with **zero** interruption when a part fails (full redundancy, more expensive) | Active-active systems, RAID 1, dual power supplies |
| **Disaster recovery (DR)** | Recover from a major event (region outage, data corruption, ransomware) within agreed targets | Cross-region backups and a standby environment |

- **RTO** (Recovery Time Objective) **(Almost always asked)** = the maximum acceptable **downtime** ("we must be back within 1 hour").
- **RPO** (Recovery Point Objective) **(Almost always asked)** = the maximum acceptable **data loss**, measured in time ("we can lose at most 5 minutes of data").

**Memory trick:** RTO = **T**ime to come back. RPO = the **P**oint you go back to (how old your last good copy is). If your shop burns down, RTO is how long it stays closed; RPO is how many minutes of sales records you lose.

**DR strategies (Almost always asked).** The faster you want to recover, the more you pay.

| Strategy | How it works | Typical RTO / RPO |
| --- | --- | --- |
| **Backup and restore** | Regular backups copied to another region; rebuild everything with IaC when needed | Hours / hours |
| **Pilot light** | Core data is copied continuously (e.g. a DB replica); everything else is off or minimal until disaster | Tens of minutes / minutes |
| **Warm standby** | A smaller but fully working copy runs in the DR region; scale it up on failover | Minutes / seconds-minutes |
| **Multi-site active-active** | Full production in two or more regions, all serving traffic at the same time | Near zero / near zero |

**Easy pictures:** backup and restore = keeping photos of your furniture so you can rebuy it. Pilot light = the small gas flame that is always on, ready to light the full burner. Warm standby = a second, smaller shop already open. Active-active = two full shops, both serving customers.

**Backups (Asked often):** follow the **3-2-1 rule** (3 copies, on 2 different media/services, 1 off-site) plus an **immutable** copy (S3 Object Lock / vault lock) against ransomware. Automate them (AWS Backup, Velero for Kubernetes, DB point-in-time recovery) and **test restores regularly**. An untested backup is only a hope.

### 20.2 Scaling building blocks (Asked often)

- **Vertical scaling** (a bigger machine): simple, but has limits and often needs downtime. **Horizontal scaling** (more machines): needs **stateless** services and a load balancer; move sessions/state out to Redis, a DB or S3.
- **Caching layers:** CDN (edge), in-memory cache (Redis), application caches. Define TTLs and how to invalidate.
- **Database scaling:** read replicas, connection pooling (PgBouncer, RDS Proxy), indexes and query tuning, partitioning/sharding, choosing the right store (SQL vs NoSQL).
- **Asynchronous processing:** queues (SQS, Kafka, RabbitMQ) absorb spikes and decouple services.
- **Protect the system:** rate limiting, autoscaling with sensible limits, load shedding, circuit breakers.
- **Remove single points of failure (SPOFs):** multiple AZs, multiple instances, redundant NAT gateways, replicated databases.
- **Reduce blast radius:** separate accounts/projects per environment, namespaces, cell-based architecture, canary releases, feature flags.

### 20.3 Design: CI/CD for 50 microservices on Kubernetes (Almost always asked)

1. **Repos:** one repo per service (or a monorepo with path-based triggers) + a separate **GitOps config repo**.
2. **CI (shared templates):** build → unit tests → SAST/SCA/secret scan → build the image with BuildKit cache → Trivy scan → SBOM + cosign signature → push to ECR, tagged with the git SHA.
3. **CD (GitOps):** a bot updates the image tag in the config repo for `dev`; Argo CD syncs; automated integration tests run; promotion to `staging` and `prod` happens through PRs (prod needs approval).
4. **Progressive delivery:** Argo Rollouts canary (5% → 25% → 100%) with automatic analysis of error rate and latency from Prometheus; automatic rollback on failure.
5. **Standardization:** one golden Helm chart (probes, resources, PDB, HPA, NetworkPolicy, ServiceMonitor) that every service configures through values.
6. **Guardrails:** Kyverno/Gatekeeper policies (no `:latest`, must have requests/limits, signed images only, non-root).
7. **Observability:** OpenTelemetry tracing, Prometheus metrics, Loki logs, SLO dashboards per service, deployment annotations on graphs.
8. **Secrets:** External Secrets Operator + AWS Secrets Manager; OIDC for CI to AWS.

### 20.4 Design: zero-downtime release with a database schema change (Almost always asked)

Use **expand and contract**: never break the old version while the new one rolls out.

1. **Expand:** add the new column/table in a backward-compatible way (nullable, no renames or drops). The old code keeps working.
2. Deploy app version N+1 that **writes to both** the old and new structures and reads the old one (or reads the new one with a fallback).
3. **Backfill** existing data in batches.
4. Deploy version N+2 that **reads from the new** structure.
5. **Contract:** in a later release, remove the old column and the dual-write code.

Combine this with rolling/canary deployments, readiness probes and a tested rollback. Rollback works because every step is backward compatible.

### 20.5 Design: multi-region active-passive setup (Asked often)

- The primary region serves all traffic. The secondary region has a **warm standby**: a smaller ASG/EKS node group, Aurora Global Database or a cross-region read replica, S3 Cross-Region Replication, ECR replication.
- **Route 53 failover routing** with health checks (or a global load balancer) switches traffic.
- Infrastructure is defined in Terraform for both regions; secrets and config are replicated; there are runbooks and **regular failover drills** (game days).
- **Know the trade-offs:** replication lag (RPO), DNS TTL and failover time (RTO), the cost of idle capacity, and data consistency when you fail back.

### 20.6 Design: centralized logging for 500 Pods (Asked often)

**The flow:** apps log JSON to stdout → **Fluent Bit DaemonSet** (parse, add Kubernetes details, filter noise, buffer) → **Loki** (cost-efficient, label-indexed, object storage backend) or **OpenSearch/Elasticsearch** (full-text search) → **Grafana/Kibana**.

**Add:** retention tiers (hot for 7 days, archive to S3 for 90+ days), sampling of debug logs, per-namespace rate limits, PII scrubbing, and alerts on log-based error rates.

## 21. Scenario-Based Troubleshooting Questions

In scenario questions, the interviewer cares more about **how you think** than about one perfect answer. Use the same method for every scenario, and think out loud.

**The method for any scenario (Must know).** Think like a doctor in an emergency room: first stop the bleeding, then find the disease, then make sure it doesn't come back.

1. **Clarify:** what changed? When did it start? Who or what is affected?
2. **Assess the impact:** how many users, and how serious is it?
3. **Mitigate first:** roll back, scale out, fail over. Get users working again.
4. **Investigate with data:** metrics, logs, traces, events.
5. **Fix the root cause.**
6. **Prevent it from happening again:** alerts, tests, automation, a postmortem.

### Scenario 1: The website returns 502 errors (Must know)

**Remember:** 502 means the proxy or load balancer got a **bad answer or no answer from the app behind it**. So look at the app first, not the load balancer.

1. **Scope:** all users or some? All endpoints? Since when? Was there a recent deployment or config change?
2. Check LB/ingress logs and target health (ALB target groups, `kubectl get endpointslices`).
3. Are the app processes/Pods running? (`systemctl status`, `kubectl get pods`, restarts, OOMKilled, CrashLoopBackOff.)
4. Is the app listening on the expected port and interface (`ss -tulnp`; `0.0.0.0` vs `127.0.0.1`)? Does the LB/Service `targetPort` match?
5. Check app logs for crashes, exceptions and connection resets. Check for keep-alive timeout mismatches between the proxy and the app.
6. **Mitigate:** roll back the last deployment if the timing matches; scale out if it is overloaded.
7. **Prevent:** readiness probes, health checks, canary releases, alerts on the 5xx rate.

### Scenario 2: A Linux server is very slow (Almost always asked)

A slow server means one resource is the bottleneck: **CPU, disk, memory or network**. Check them one by one, like a mechanic checking a slow car.

1. `uptime` (load average vs `nproc`), `top`/`htop` (which process? `%us`, `%sy`, `%wa`).
2. **High CPU:** find the process (`ps aux --sort=-%cpu`) and check whether it is expected (traffic spike, cron job, runaway loop, crypto-miner). Dig deeper with `pidstat`, `strace`, `perf`, or a thread dump for Java (`jstack`).
3. **High I/O wait:** `iostat -xz 1`, `iotop`. Look for a disk at 100% utilization, a backup job, heavy logging, or swap activity.
4. **Memory pressure:** `free -h`, `vmstat 1` (is it swapping?), OOM messages in `dmesg -T`.
5. **Network:** `ss -s` (connection counts), retransmits, saturated bandwidth.
6. **Mitigate:** restart or kill a runaway process carefully (SIGTERM first), scale out, move batch jobs to off-peak hours. **Long term:** right-size, fix the code, and add monitoring and alerts on these signals.

### Scenario 3: "No space left on device" (Must know)

There are three possible reasons: the **space** is full, the **inodes** are full, or **deleted files are still held open** by a process.

1. `df -h` to find the full filesystem; `df -i` to check **inodes**.
2. Find the big folders and files: `du -xh / --max-depth=1 | sort -rh | head`, then drill down; `find / -xdev -size +500M`.
3. **Usual culprits:** app/system logs, Docker images/containers/build cache (`docker system df`), old backups, core dumps, `/tmp`.
4. **Deleted files still held open:** `lsof +L1` → restart that process (or truncate its file descriptor) to release the space.
5. **Free space safely:** compress/rotate logs, `docker system prune`, delete old artifacts. **Don't** delete random files, or live log files from under running processes without truncating them properly.
6. **Prevent:** logrotate, Docker log limits, disk usage alerts (e.g. at 80%) and `predict_linear` alerts, separate volumes for data and logs, and extending the volume (EBS resize + `growpart` + `resize2fs`/`xfs_growfs`).

### Scenario 4: You can't SSH into an EC2 instance (Almost always asked)

Check it like a delivery person who can't reach a house: do I have the right key? Is the gate open? Is there a road? Is the house okay?

1. **Right key?** Correct key and **username** (`ec2-user` for Amazon Linux, `ubuntu` for Ubuntu); key permissions `chmod 400 key.pem`.
2. **Gate open?** The **security group** allows port 22 from your IP; the **NACL** allows inbound 22 **and outbound ephemeral ports**.
3. **Is there a road?** The instance has a public IP / Elastic IP, and its subnet routes to an **Internet Gateway** (or you are using a bastion/VPN).
4. **House okay?** EC2 status checks; console screenshot / system log (kernel panic, full disk, sshd failed).
5. Use **SSM Session Manager** or the **EC2 Serial Console** to get in without SSH.
6. **Last resort:** stop the instance, detach the root volume, attach it to a rescue instance, fix the problem (`authorized_keys`, `sshd_config`, full disk), and reattach it.

### Scenario 5: A Kubernetes Pod is in CrashLoopBackOff (Must know)

**CrashLoopBackOff** means the container keeps starting and crashing, and Kubernetes waits a little longer before each restart. Your job is to find **why it crashes**.

1. `kubectl describe pod` (events, last state, **exit code**, reason).
2. `kubectl logs <pod> --previous` (logs of the container that crashed).
3. **Read the exit code:** 1 = app error, 137 = OOMKilled/SIGKILL, 126/127 = command problem, 143 = SIGTERM (maybe the liveness probe is killing it).
4. **Common causes:** missing env var/ConfigMap/Secret, wrong DB credentials or endpoint, a dependency that is down, a liveness probe that is too aggressive (needs a startup probe), wrong command/entrypoint, read-only filesystem or permission issues.
5. **Debug interactively:** `kubectl debug`, or temporarily change the command to `sleep infinity` and exec in.
6. **Check what changed:** `kubectl rollout history`, recent config commits. Roll back if needed (`kubectl rollout undo`).

### Scenario 6: Pods stuck in Pending (Almost always asked)

**Pending** means the scheduler cannot find a node for the Pod. The events in `kubectl describe pod` tell you why:

- **Not enough CPU/memory:** requests are too high or the cluster is full. Check Cluster Autoscaler/Karpenter logs and the node group's max size.
- **No node matches** the nodeSelector/affinity.
- **Taints without tolerations.**
- **PVC not bound:** StorageClass problem or zone mismatch (use `WaitForFirstConsumer`).
- **ResourceQuota exceeded.**
- **Too many Pods per node:** IP exhaustion with the AWS VPC CNI.

### Scenario 7: A deployment just broke production (Must know)

**Golden rule:** restoring the service is more important than finding the bug. Roll back first; investigate later.

1. **Mitigate immediately:** roll back (`kubectl rollout undo`, `helm rollback`, revert the GitOps commit, switch blue-green back, turn off the feature flag).
2. **Communicate:** declare an incident; update stakeholders and the status page.
3. **Verify recovery** with metrics (error rate and latency back to normal).
4. **Investigate** in a non-production environment: compare the releases, logs and traces.
5. **Postmortem:** why didn't the tests or the canary catch it? Add the missing test, canary analysis or alert.

### Scenario 8: It works locally but fails in Docker/CI (Asked often)

Something is different between your laptop and Docker/CI. Go through the usual differences:

- Environment variables and config files.
- OS/library versions (Alpine's musl vs glibc).
- File paths and **case-sensitive filesystems**.
- Files missing because of `.dockerignore`.
- Network access from CI (proxies, private registries).
- The app binding to `localhost` instead of `0.0.0.0`.
- Timezone/locale.
- CPU architecture (ARM Mac vs x86 CI: build multi-arch images with `docker buildx`).
- Secrets not available in CI.
- Caches hiding problems on your laptop.

**Best trick:** run the exact CI image on your own machine to reproduce the problem.

### Scenario 9: The CI pipeline is suddenly slow or flaky (Asked often)

**Slow:** compare with recent good runs and find which stage got slower. Common causes: cache misses (did the cache key change?), a slow or rate-limited dependency registry, runner capacity (queue time), new heavy tests, a Docker layer cache broken by an early `COPY . .`, network issues.

**Flaky:** find the flaky tests from the run history, quarantine them, fix shared-state or time-dependency problems, and add retries only with tracking.

### Scenario 10: Two engineers ran `terraform apply` at the same time (Asked often)

Remote state **with locking** prevents this. If it happened, the backend was not locking.

1. Stop all applies.
2. If the state is corrupted, restore the last good version from the versioned bucket.
3. Run `terraform plan` to see the real drift, and fix it (import, or fix the code).
4. **Enforce:** a locking backend, applies only from CI, branch protection, and plan review.

### Scenario 11: Pods keep getting OOMKilled (Almost always asked)

1. **Confirm it:** `kubectl describe pod` (reason `OOMKilled`, exit code 137) and memory graphs (`container_memory_working_set_bytes`).
2. **Memory climbs steadily until the kill → it is a leak.** Like a tap left running into a bucket, it will overflow whatever the bucket size. Take heap dumps/profiles and fix the code.
3. **Memory need is steady but simply too high → raise the limit** (and the request).
4. **For JVMs:** match the heap to the container limit (`-XX:MaxRAMPercentage`), and remember non-heap memory (threads, metaspace, buffers).
5. Use VPA recommendations, and alert before the limit is reached.

### Scenario 12: Latency doubled but nothing was deployed (Asked often)

"Nothing was deployed" does not mean nothing changed. Check the golden signals for each component:

- **More traffic?** Scale out.
- **A slow dependency?** DB slow queries, a lower cache hit ratio, a third-party API.
- **Use traces** to find the slow span.
- **Saturation:** CPU throttling (CPU limits!), connection pool exhaustion, GC pauses, noisy neighbours, disk I/O, network/DNS latency.
- **Changes outside deployments:** feature flags, infra changes, certificate renewals, cloud provider incidents.

### Scenario 13: DNS resolution fails inside Kubernetes Pods (Asked often)

Start from a debug Pod: `nslookup kubernetes.default` and `nslookup <svc>.<ns>.svc.cluster.local`. Then check:

- The CoreDNS Pods (`kubectl -n kube-system get pods -l k8s-app=kube-dns`) and their logs.
- The `kube-dns` Service and its endpoints.
- NetworkPolicies blocking egress on port 53.
- The Pod's `/etc/resolv.conf` (search domains, `ndots`).
- DNS problems on the node itself.

Under heavy load, scale CoreDNS or add NodeLocal DNSCache.

### Scenario 14: The production database was accidentally deleted or corrupted (Almost always asked)

1. **Stop the bleeding:** block further writes and changes; declare an incident.
2. **Restore:** RDS **point-in-time recovery** to just before the event (this creates a new instance), or the latest snapshot. Then point the app at it (DNS/config). Know your RTO/RPO.
3. **Validate** the data; replay or reconcile missing transactions if possible.
4. **Prevent:** deletion protection, `prevent_destroy` in Terraform, least privilege (nobody has delete rights in prod by default), separate prod accounts, automated backups with cross-region copies, regular restore drills.

### Scenario 15: Cut the cloud bill by 30% (Almost always asked)

Treat it like cutting household expenses: first see where the money goes, then cancel what you don't use, then get bulk discounts on what you do use.

1. **Visibility first:** Cost Explorer by service/tag/account; find the top 5 cost drivers.
2. **Quick wins:** delete idle resources (unattached EBS, old snapshots, idle LBs, unused Elastic IPs), stop dev/test outside office hours, S3 lifecycle policies, log retention limits.
3. **Right-size** over-provisioned instances, databases and Kubernetes requests.
4. **Commitments** (Savings Plans) for the steady baseline; **Spot** for fault-tolerant work; **Graviton**.
5. **Architecture:** VPC endpoints instead of NAT data processing, caching/CDN to reduce egress, serverless for spiky, low-volume work.
6. **Governance:** mandatory tags, budgets and anomaly alerts, showback/chargeback per team.

### Scenario 16: Migrate a monolith running on VMs to Kubernetes (Asked often)

1. **Assess:** dependencies, state, config, startup behaviour.
2. **Make it 12-factor friendly:** config through env vars, logs to stdout, stateless sessions.
3. **Containerize and test.**
4. **Set up the platform:** cluster via Terraform, CI/CD, observability, secrets.
5. **Run it next to the old system** and **shift traffic gradually** (weighted DNS/LB).
6. Keep the database managed outside the cluster at first.
7. **Later, split the monolith** with the **strangler fig pattern**: peel off one feature at a time behind the same entry point. (A strangler fig is a plant that slowly grows around a tree until it replaces it.)

### Scenario 17: You get paged at 2 AM (Almost always asked)

Go step by step, and don't try to be a hero:

1. Acknowledge the page.
2. Open the alert's dashboard and runbook.
3. Assess customer impact and severity.
4. Escalate or declare an incident if needed.
5. Mitigate (roll back, fail over, scale).
6. Post status updates at regular intervals.
7. Confirm recovery, then hand off with notes.
8. Next day: run a postmortem, and fix the alert or runbook if it was noisy or unclear.

### Scenario 18: An expired TLS certificate caused an outage (Asked often)

**Mitigate:** renew or reissue the certificate (ACM, Let's Encrypt, cert-manager) and redeploy.

**Prevent:** automatic issuance and renewal, expiry alerts (e.g. at 30/14/7 days; blackbox\_exporter's `probe_ssl_earliest_cert_expiry`), an inventory of every certificate (including internal ones), and no manual certificate handling.

### Scenario 19: 5xx errors come only from some Pods (Asked often)

Compare the failing Pods with the healthy ones:

- **Which nodes are they on?** Could be a node-specific problem: disk, network, kernel.
- **Different image versions?** A stuck rollout.
- **Different config?** A ConfigMap update picked up by only some Pods.
- **Resource throttling** on those Pods?

**Tools:** `kubectl get pods -o wide`, logs per Pod, metrics by Pod label. **Mitigate** by cordoning the bad node or deleting the bad Pods, and make sure readiness probes would have removed them.

### Scenario 20: Marketing expects 10x traffic for a sale next week (Asked often)

Prepare like a shop before a big festival sale: extra staff, extra stock, clear queues, and no renovation work during the sale.

- **Load test** at the expected peak plus a margin; find the bottlenecks (DB, cache, third-party limits).
- **Pre-scale:** raise ASG/HPA minimums, warm the caches, pre-warm load balancers if needed, raise service quotas.
- Use a **CDN** for static content and **queues** for non-critical work.
- Prepare **feature flags** to switch off expensive features if needed.
- Set up a **war-room dashboard** and on-call coverage.
- **Freeze risky deployments** during the event.

### Scenario 21: An S3 bucket with customer data was made public (Asked often)

**Immediately:** block public access (account-level S3 Block Public Access), check what was exposed using access logs/CloudTrail, involve security/legal as per policy, and rotate any exposed secrets.

**Prevent:** account-wide Block Public Access, SCPs, AWS Config rules and alerts, IaC scanning (Checkov/Trivy) in CI, and least privilege on bucket policies.

## 22. Rapid-Fire Questions

These are 90 one-line answers for quick rounds and warm-ups. **How to practise:** cover the Answer column and test yourself. If you miss one, go back to its section and read it again.

| # | Question | Answer |
| --- | --- | --- |
| 1 | What is idempotency? | Doing something many times gives the same result as doing it once |
| 2 | CI vs CD? | CI = merge and test every change automatically. CD = every change is ready to release (Delivery) or goes to prod automatically (Deployment) |
| 3 | What is a webhook? | An HTTP message sent automatically when an event happens (e.g. push → trigger a build) |
| 4 | Jenkins pipeline file? | `Jenkinsfile` |
| 5 | GitHub Actions workflow location? | `.github/workflows/` |
| 6 | Keyless cloud login from CI? | OIDC federation (short-lived credentials) |
| 7 | `git fetch` vs `git pull`? | fetch only downloads; pull = fetch + merge/rebase |
| 8 | Safely undo a pushed commit? | `git revert <sha>` |
| 9 | Recover "lost" commits? | `git reflog` |
| 10 | Find the commit that introduced a bug? | `git bisect` |
| 11 | Show listening ports? | `ss -tulnp` |
| 12 | Check disk space / inodes? | `df -h` / `df -i` |
| 13 | Zombie process? | A finished child whose parent has not collected its exit status |
| 14 | Load average? | Average number of running + waiting (uninterruptible) processes over 1/5/15 minutes |
| 15 | SUID? | The program runs with the file owner's permissions |
| 16 | Hard vs soft link? | Same inode vs a shortcut to a path |
| 17 | Cron for every 15 minutes? | `*/15 * * * *` |
| 18 | TCP handshake? | SYN → SYN-ACK → ACK |
| 19 | DNS port? | 53 (UDP; TCP for large answers and zone transfers) |
| 20 | 502 vs 504? | Bad/no answer from the app vs the app took too long (timeout) |
| 21 | 401 vs 403? | Not logged in vs logged in but not allowed |
| 22 | Private IPv4 ranges? | 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 |
| 23 | Usable IPs in an AWS /24? | 251 (AWS reserves 5) |
| 24 | VM vs container? | A VM virtualizes hardware and has its own OS; a container shares the host kernel |
| 25 | Which Linux features make containers? | Namespaces (isolation) + cgroups (limits) + overlay filesystem |
| 26 | CMD vs ENTRYPOINT? | CMD = defaults you can override; ENTRYPOINT = the fixed program |
| 27 | COPY vs ADD? | COPY just copies; ADD can also extract tars and download URLs. Prefer COPY |
| 28 | Why multi-stage builds? | Smaller, more secure runtime images |
| 29 | Exit code 137? | SIGKILL, usually OOMKilled |
| 30 | Exit code 143? | SIGTERM (graceful stop) |
| 31 | Default Docker network? | bridge |
| 32 | Where is Kubernetes state stored? | etcd |
| 33 | Smallest deployable unit in Kubernetes? | Pod |
| 34 | Who assigns Pods to nodes? | kube-scheduler |
| 35 | Who restarts containers when liveness fails? | kubelet |
| 36 | Default Service type? | ClusterIP |
| 37 | NodePort range? | 30000-32767 |
| 38 | Kubernetes API server port? | 6443 |
| 39 | One Pod on every node? | DaemonSet |
| 40 | Stable identity + its own storage per Pod? | StatefulSet |
| 41 | What happens when a readiness probe fails? | The Pod is removed from Service endpoints (no restart) |
| 42 | Are Secrets encrypted by default? | No, only base64-encoded; turn on encryption at rest |
| 43 | What replaced PodSecurityPolicy? | Pod Security Admission (Pod Security Standards) |
| 44 | Recommended replacement for Ingress NGINX? | Gateway API (or another maintained controller) |
| 45 | `kubectl drain`? | Cordon + evict Pods (respecting PDBs) |
| 46 | Taint vs affinity? | A taint (on the node) pushes Pods away; affinity (on the Pod) pulls it toward nodes |
| 47 | What does HPA need? | Resource requests + metrics-server (or custom metrics) |
| 48 | Roll back a Deployment? | `kubectl rollout undo deployment/<name>` |
| 49 | CRD / Operator? | A custom API type / a controller that automates running an app |
| 50 | Helm release? | One installed copy of a chart |
| 51 | Helm 4 replacement for `--atomic`? | `--rollback-on-failure` |
| 52 | GitOps in one line? | Git is the source of truth; an agent in the cluster keeps the cluster matching it |
| 53 | GitOps tools? | Argo CD, Flux |
| 54 | Preview Terraform changes? | `terraform plan` |
| 55 | Why Terraform state? | It maps code to real resources, tracks details, and lets Terraform work out what changed |
| 56 | Why lock state? | So two applies at the same time can't corrupt it |
| 57 | `count` vs `for_each`? | Number-based vs name-based (stable addresses) |
| 58 | Rename a resource without destroying it? | `moved` block / `terraform state mv` |
| 59 | `.terraform.lock.hcl`? | Locks provider versions and checksums; commit it |
| 60 | OpenTofu license? | MPL 2.0 (Terraform: BSL 1.1) |
| 61 | How does Ansible connect? | SSH (WinRM for Windows); agentless |
| 62 | Ansible handler? | A task that runs once at the end of a play, only when a change notifies it |
| 63 | Encrypt secrets in Ansible? | `ansible-vault` |
| 64 | How fast is a blue-green rollback? | Instant: switch traffic back |
| 65 | Canary? | Slowly move traffic to the new version while watching metrics |
| 66 | Separate deploy from release? | Feature flags |
| 67 | SG vs NACL? | Stateful, instance-level, allow-only vs stateless, subnet-level, numbered allow + deny |
| 68 | Outbound internet for private subnets? | NAT Gateway |
| 69 | RDS Multi-AZ vs read replica? | Synchronous standby for HA vs async copy for read scaling |
| 70 | CloudWatch vs CloudTrail? | Monitoring vs audit trail of API calls |
| 71 | Spot interruption notice? | 2 minutes |
| 72 | S3 durability? | 11 nines (99.999999999%) |
| 73 | Shared responsibility in one line? | AWS secures the cloud; you secure what you put in it |
| 74 | Prometheus collection model? | Pull (scrapes `/metrics`) |
| 75 | Counter vs gauge? | Only goes up vs goes up and down |
| 76 | p95 latency in PromQL? | `histogram_quantile(0.95, ...)` |
| 77 | Golden signals? | Latency, traffic, errors, saturation |
| 78 | Three pillars of observability? | Metrics, logs, traces |
| 79 | SLI vs SLO vs SLA? | Measurement vs internal target vs customer contract |
| 80 | 99.9% monthly error budget? | About 43.2 minutes |
| 81 | Toil? | Manual, repetitive, automatable work with no lasting value |
| 82 | RTO vs RPO? | Max downtime you can accept vs max data loss you can accept |
| 83 | SAST vs DAST? | Checks the source code vs tests the running app |
| 84 | SBOM? | A list of every software component inside your app |
| 85 | cosign? | Signs and verifies container images (Sigstore) |
| 86 | Five DORA metrics? | Deployment frequency, change lead time, failed deployment recovery time, change fail rate, deployment rework rate |
| 87 | 12-factor rule for config? | Store config in the environment |
| 88 | Pets vs cattle? | Hand-cared servers vs identical, replaceable ones |
| 89 | Latest Kubernetes release (Oct 2026)? | v1.37 "Garhwal" |
| 90 | What is shift-left? | Doing testing and security earlier in the lifecycle |

## 23. How to Answer in Interviews

Knowing the answer is half the job; saying it clearly is the other half. This section gives you ready-made structures for the most common types of questions.

### 23.1 Self-introduction (60-90 seconds)

"Tell me about yourself" is usually the first question, so prepare it word for word. Fill in the brackets with your own details and practise until it takes 60-90 seconds.

> "I'm **\[name\]**, a DevOps engineer with **\[X years\]** of experience (or: a recent graduate who has built **\[project\]**). I work mainly with **\[AWS, Kubernetes, Terraform, GitHub Actions/Jenkins\]**. In my current role I **\[own the CI/CD platform for N services / manage EKS clusters / automated infra with Terraform\]**. Recently I **\[a measurable achievement: cut pipeline time 25 → 9 minutes, reduced cloud cost 28%, reduced failed deployments with canary releases\]**. I enjoy **\[automation / reliability / developer experience\]**, and I'm looking for a role where **\[connect to the job\]**."

**Tip:** numbers make you memorable. "Cut pipeline time from 25 to 9 minutes" sticks much better than "improved the pipeline".

### 23.2 Explaining your project (Must know: the most important answer)

Draw it if you can, and tell it like a story in this order:

1. **Business context:** what the product does, and its scale (users, requests, number of services).
2. **Architecture:** cloud, network layout, compute (EKS/ECS/EC2), data stores, entry points.
3. **Delivery flow:** commit → CI (build, test, scan) → registry → CD (Helm/Argo CD) → environments → deployment strategy.
4. **Infrastructure:** Terraform modules, state backend, environments.
5. **Operations:** monitoring, logging, alerting, on-call, incidents you handled.
6. **Security:** IAM/OIDC, secrets management, scanning, network policies.
7. **Your role and impact, with numbers.**
8. **A challenge you solved.** The interviewer will dig here, so pick a real one you can explain in depth.

**Important:** only claim what you actually did. Interviewers keep asking "how exactly?" until they find the edge of your knowledge.

### 23.3 Structure for technical questions

For any "What is X?" question, use this 4-step pattern: **Definition → how it works → example from your work → trade-offs / gotchas.**

**Example: "What is a readiness probe?"**

1. **Definition:** a check that tells Kubernetes whether a Pod can receive traffic right now.
2. **How it works:** the kubelet runs it regularly. If it fails, the Pod is removed from the Service endpoints (no restart).
3. **Example from your work:** "We used `/ready`, which checks the DB connection pool, so Pods didn't get traffic during warm-up."
4. **Gotcha:** "Don't put dependency checks in liveness; it causes cascading restarts."

### 23.4 Behavioural questions with STAR (Asked often)

For "Tell me about a time when..." questions, use **STAR**: **S**ituation → **T**ask → **A**ction (what *you* did) → **R**esult (numbers + what you learned). It keeps your story short and clear.

**Example: "Tell me about a production incident you handled"**

- **S:** checkout errors jumped to 20% right after a Friday release.
- **T:** I was on call and had to restore service.
- **A:** I matched the spike to the deployment on our Grafana dashboard and rolled back with `helm rollback` within 6 minutes. I posted updates in the incident channel, then reproduced the bug in staging: a missing env var in the new chart version.
- **R:** full recovery in under 10 minutes. In the postmortem I added a CI check for required env vars and introduced canary releases with automatic rollback. We had no similar incident afterwards.

**Common behavioural questions:** a time you disagreed with a developer or manager; an outage you caused; how you prioritize many requests; an automation you are proud of; how you handle pressure; how you learn new tools; how you helped onboard others.

**Tip:** prepare 4-5 real stories in advance. One good story can answer several of these questions.

### 23.5 When you don't know the answer

Say so honestly, then think out loud: "I haven't used X directly, but I'd expect it to work like Y because...; I'd confirm by reading the docs or testing in a sandbox." Interviewers value honest reasoning far more than a bluff.

### 23.6 What interviewers expect by level

| Level | What they focus on |
| --- | --- |
| Fresher / 0-2 yrs | Linux, networking, Git, scripting basics; Docker; Kubernetes objects; one CI tool; AWS core services; a real hands-on project |
| Mid / 2-5 yrs | Designing pipelines, Terraform modules and state, Kubernetes troubleshooting, monitoring and alerting, security basics, real incidents |
| Senior / 5+ yrs | Architecture and trade-offs, multi-account/multi-cluster strategy, reliability (SLOs, DR), cost, platform engineering, mentoring, influencing teams |

### 23.7 Smart questions to ask the interviewer

At the end you will hear "Do you have any questions for us?" Always ask one or two; it shows real interest.

- How do deployments to production work today, and how often do you deploy?
- What does on-call look like (rotation size, pages per week)?
- What is the biggest reliability or delivery pain point the team wants this hire to fix?
- How is the platform/DevOps team organized relative to the product teams?
- How do you measure success for this role in the first 6 months?

## 24. Portfolio Project to Build

One end-to-end project you built yourself beats ten certificates in interviews. Put it on GitHub with an architecture diagram, so you can show it and talk about it with confidence.

**Goal:** a small web app (e.g. a Node.js or Python API + a database), delivered end to end. Think of it as a mini version of a real company's setup. Each step below uses one part of these notes, so building it is also the best revision you can do.

1. **App + Dockerfile:** multi-stage build, non-root user, health endpoints (`/healthz`, `/ready`), structured JSON logs, a Prometheus metrics endpoint.
2. **CI (GitHub Actions):** lint → unit tests → Trivy scan (fail on critical) → build → push to GHCR/ECR tagged with the git SHA. Use OIDC to AWS (no stored keys).
3. **Infrastructure (Terraform):** a VPC (public/private subnets across 2 AZs), EKS (or k3s on a small VM to save money), ECR, and remote state in S3 with locking.
4. **Kubernetes packaging:** a Helm chart with Deployment, Service, HPA, PDB, probes, resources, NetworkPolicy, and a Gateway API `HTTPRoute`.
5. **GitOps:** Argo CD watching a config repo with `dev` and `prod` overlays; promotion through PRs.
6. **Observability:** kube-prometheus-stack (Prometheus, Grafana, Alertmanager) + Loki; a dashboard with RED metrics and an SLO; an alert sent to Slack/email.
7. **Progressive delivery (bonus):** an Argo Rollouts canary with automated analysis.
8. **Security (bonus):** Kyverno policies (no `:latest`, non-root, limits required), External Secrets Operator, cosign image signing.
9. **Chaos test (bonus):** delete Pods/nodes during a load test (k6) and show that the app stays up.
10. **README:** architecture diagram, how to run it, design decisions and trade-offs, what you would improve, and screenshots of your dashboards.

**Cost warning:** EKS control planes, NAT Gateways, load balancers and public IPv4 addresses are billed by the hour, even when nothing is running on them. Do most of the work locally with **kind/k3d/minikube**, set an AWS Budget alert, and run `terraform destroy` when you are done.

## 25. Last-Day Revision Sheet

Read this the night before (or the morning of) your interview. Every line points back to a section above, so if a line feels unclear, revise that section.

### 25.1 Most-asked "X vs Y" (one line each)

| Pair | Key difference |
| --- | --- |
| DevOps vs SRE | A culture and set of practices vs a concrete engineering way of doing it, with SLOs and error budgets |
| Continuous Delivery vs Deployment | Manual approval before prod vs fully automatic |
| merge vs rebase | Merge commit keeps history as it happened vs a straight line with rewritten history |
| reset vs revert | Rewrites history vs adds a new "undo" commit |
| Process vs thread | Own memory vs shared memory inside a process |
| SIGTERM vs SIGKILL | Polite stop vs forced kill |
| TCP vs UDP | Reliable and ordered vs fast, best effort |
| L4 vs L7 load balancer | Routes by IP/port vs routes by HTTP details (host, path, headers) |
| Forward vs reverse proxy | Sits in front of clients vs sits in front of servers |
| VM vs container | Own kernel vs shared kernel |
| Image vs container | Template (the recipe) vs running instance (the dish) |
| CMD vs ENTRYPOINT | Defaults you can override vs the fixed program |
| Volume vs bind mount | Managed by Docker vs a host folder |
| Deployment vs StatefulSet | Interchangeable replicas vs stable identity + own storage |
| Liveness vs readiness | Fails → restart vs fails → stop sending traffic |
| Requests vs limits | Guaranteed for scheduling vs hard cap (CPU throttled / memory OOMKilled) |
| ClusterIP vs NodePort vs LoadBalancer | Internal only vs a port on every node vs a cloud load balancer |
| Ingress vs Gateway API | One limited resource + annotations vs role-based, portable, richer routing |
| ConfigMap vs Secret | Plain config vs sensitive data (only base64; needs encryption at rest) |
| HPA vs VPA vs Cluster Autoscaler | Number of Pods vs size of Pods vs number of nodes |
| Helm vs Kustomize | Templates + packaging vs overlays/patches |
| Push CD vs GitOps | CI pushes using cluster credentials vs an in-cluster agent pulls from Git |
| Terraform vs Ansible | Builds infrastructure, with state vs configures existing machines |
| Terraform vs CloudFormation | Multi-cloud + its own state vs AWS only + AWS-managed state |
| count vs for\_each | Number-based vs name-based |
| Security Group vs NACL | Stateful, instance-level vs stateless, subnet-level |
| Multi-AZ vs read replica | HA failover vs read scaling |
| S3 vs EBS vs EFS | Object vs block vs shared file storage |
| CloudWatch vs CloudTrail | Metrics/logs vs API audit |
| Monitoring vs observability | Catches known failures vs explains unknown ones |
| Metrics vs logs vs traces | Numbers over time vs events vs request journeys |
| SLO vs SLA | Internal target vs external contract |
| RTO vs RPO | How much downtime you can accept vs how much data loss you can accept |
| SAST vs DAST vs SCA | Your code vs the running app vs your dependencies |
| Blue-green vs canary | Instant full switch vs gradual traffic shift |

### 25.2 Numbers to remember

| Item | Value |
| --- | --- |
| NodePort range | 30000-32767 |
| Kubernetes API / etcd / kubelet ports | 6443 / 2379-2380 / 10250 |
| Default Pod termination grace period | 30 s |
| `docker stop` default timeout | 10 s |
| Exit codes | 137 = SIGKILL/OOM, 143 = SIGTERM, 127 = not found, 126 = not executable |
| AWS reserved IPs per subnet | 5 |
| Spot interruption notice | 2 minutes |
| Lambda max duration | 15 minutes |
| 99.9% availability | About 43.2 min/month, about 8.76 h/year |
| 99.99% availability | About 4.32 min/month, about 52.6 min/year |
| Kubernetes support window | Latest 3 minor versions, about 14 months each |
| Helm revisions kept by default | 10 (`--history-max`) |
| Ansible default forks | 5 |
| Terraform default parallelism | 10 |

### 25.3 Commands to have at your fingertips

Practise typing these until you don't need to look them up. Hands-on rounds often start with exactly these.

```bash
# Linux
top; free -h; df -h; df -i; du -sh *; ss -tulnp; lsof -i :8080; journalctl -u <svc> -f
ps aux --sort=-%mem | head; dmesg -T | tail; find / -xdev -size +500M
# Networking
dig <name>; curl -v <url>; nc -zv <host> <port>; traceroute <host>
# Git
git log --oneline --graph; git revert <sha>; git reflog; git stash; git rebase -i HEAD~3
# Docker
docker logs -f <c>; docker exec -it <c> sh; docker inspect <c>; docker system df
# Kubernetes
kubectl get pods -A -o wide; kubectl describe pod <p>; kubectl logs <p> --previous
kubectl rollout undo deploy/<d>; kubectl top pods; kubectl get events --sort-by=.metadata.creationTimestamp
# Helm / Terraform
helm upgrade --install <r> <chart> --rollback-on-failure; helm rollback <r> <rev>
terraform init; terraform plan -out=tfplan; terraform apply tfplan; terraform state list
```

## 26. References and Further Learning

These are the places to go deeper. When in doubt, the official docs are always the source of truth.

**Official documentation**

- Kubernetes [docs](https://kubernetes.io/docs/) and [blog](https://kubernetes.io/blog/) (v1.37 "Garhwal" release post, 26 Aug 2026; Ingress NGINX retirement announcement, Nov 2025)
- [Gateway API](https://gateway-api.sigs.k8s.io/)
- [Helm](https://helm.sh/docs/) (Helm 4 overview and release post, Nov 2025)
- [Terraform](https://developer.hashicorp.com/terraform) and [OpenTofu](https://opentofu.org/docs/)
- [Ansible](https://docs.ansible.com/)
- [AWS docs](https://docs.aws.amazon.com/) and the [Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Prometheus](https://prometheus.io/docs/), [OpenTelemetry](https://opentelemetry.io/docs/) and [Grafana](https://grafana.com/docs/)
- [Argo CD](https://argo-cd.readthedocs.io/) and [Flux](https://fluxcd.io/flux/)
- [DORA research and metrics](https://dora.dev/) (metrics history, 2025 State of AI-assisted Software Development report)
- [Google SRE books](https://sre.google/books/) (free online)
- Security: [OWASP](https://owasp.org/), [SLSA](https://slsa.dev/), [Sigstore](https://www.sigstore.dev/)

**Books**

- *The Phoenix Project* and *The DevOps Handbook* (Gene Kim et al.)
- *Accelerate* (Forsgren, Humble, Kim)
- *Site Reliability Engineering* and *The Site Reliability Workbook* (Google)
- *Kubernetes Up & Running*; *Terraform: Up & Running*

**Practice and roadmaps**

- [DevOps roadmap](https://roadmap.sh/devops)
- [Killercoda](https://killercoda.com/): free browser-based Kubernetes and Linux labs
- [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- Useful certifications: CKA / CKAD / CKS, AWS Solutions Architect Associate / DevOps Engineer Professional, HashiCorp Terraform Associate.

**Final tip:** read a section, then do it hands-on the same day. The things you have broken and fixed yourself are the ones you will explain confidently in an interview. Good luck!
