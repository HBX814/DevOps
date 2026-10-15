# DevOps Complete Notes: Concepts + Interview Preparation (2026 Edition)

> **Last updated:** October 2026
> **Who this is for:** Freshers and experienced engineers preparing for DevOps / Cloud / SRE / Platform Engineer interviews.
> **How each topic is written:** Concept -> How it works -> Commands / Code -> Interview Q&A -> Gotchas.
> ⭐ = asked very frequently. ⭐⭐ = almost guaranteed to be asked.

---

## What's New in 2026 (mention these to sound current)

| Area | What changed | Interview angle |
|---|---|---|
| Kubernetes | Latest release is **v1.37 "Garhwal"** (26 Aug 2026), named after the Himalayan region of Uttarakhand. v1.38 is expected around Dec 2026. | Shows you follow the ecosystem |
| Kubernetes | **v1.35** removed cgroup v1 support (nodes must run cgroup v2) and made **in-place Pod resize** GA. **v1.37** made the `metrics.k8s.io` API stable and moved **HPA scale-to-zero** to beta. | Resource management and autoscaling questions |
| Ingress | The community **Ingress NGINX** controller was **retired in March 2026** (no more releases or security fixes). Recommended path: **Gateway API** or another maintained controller. | "How would you expose apps today?" |
| Helm | **Helm 4** (Nov 2025), first major release in 6 years: server-side apply for new installs, WebAssembly plugins, kstatus-based `--wait`, `--atomic` renamed to `--rollback-on-failure`. Helm 3 charts still work. | Helm questions, CI scripts |
| Terraform | Terraform is under the **Business Source License** since v1.6; **IBM completed its HashiCorp acquisition in Feb 2025**. **OpenTofu** is the open-source (MPL 2.0) fork under the Linux Foundation / CNCF. | "Terraform or OpenTofu?" |
| DORA | DORA now tracks **5 delivery metrics** (added *deployment rework rate*). The 2025 report found **AI acts as an amplifier** of existing strengths and weaknesses, and that **platform engineering** is the foundation for getting value from AI. | "How do you measure DevOps success?" |

---

## Study Roadmap

| Week | Topics | Hands-on goal |
|---|---|---|
| 1 | Linux, Networking, Git, Bash | Troubleshoot a VM, write 5 useful scripts |
| 2 | CI/CD (Jenkins + GitHub Actions), Docker | A pipeline that builds, tests, scans and pushes an image |
| 3 | Kubernetes, Helm, GitOps | Deploy the app on kind/minikube with Helm + Argo CD |
| 4 | Terraform, Ansible, AWS | Provision a VPC + EKS (or EC2) with Terraform |
| 5 | Monitoring, SRE, DevSecOps, scenarios | Prometheus/Grafana dashboards + alerts, mock interviews |

---

## Table of Contents

1. [DevOps Fundamentals](#1-devops-fundamentals)
2. [Linux for DevOps](#2-linux-for-devops)
3. [Networking for DevOps](#3-networking-for-devops)
4. [Git and Version Control](#4-git-and-version-control)
5. [Shell Scripting and Python](#5-shell-scripting-and-python)
6. [Build Tools and Artifact Management](#6-build-tools-and-artifact-management)
7. [CI/CD Pipelines](#7-cicd-pipelines)
8. [Deployment Strategies](#8-deployment-strategies)
9. [Docker and Containers](#9-docker-and-containers)
10. [Kubernetes](#10-kubernetes)
11. [Helm](#11-helm)
12. [GitOps with Argo CD and Flux](#12-gitops-with-argo-cd-and-flux)
13. [Terraform and Infrastructure as Code](#13-terraform-and-infrastructure-as-code)
14. [Ansible and Configuration Management](#14-ansible-and-configuration-management)
15. [Cloud Computing with AWS](#15-cloud-computing-with-aws)
16. [Monitoring, Logging and Observability](#16-monitoring-logging-and-observability)
17. [Site Reliability Engineering](#17-site-reliability-engineering)
18. [DevSecOps and Supply Chain Security](#18-devsecops-and-supply-chain-security)
19. [Platform Engineering, AI and 2026 Trends](#19-platform-engineering-ai-and-2026-trends)
20. [System Design for DevOps](#20-system-design-for-devops)
21. [Scenario-Based Troubleshooting Questions](#21-scenario-based-troubleshooting-questions)
22. [Rapid-Fire Questions](#22-rapid-fire-questions)
23. [How to Answer in Interviews](#23-how-to-answer-in-interviews)
24. [Portfolio Project to Build](#24-portfolio-project-to-build)
25. [Last-Day Revision Sheet](#25-last-day-revision-sheet)
26. [References and Further Learning](#26-references-and-further-learning)

---

## 1. DevOps Fundamentals

### 1.1 What is DevOps? ⭐⭐

**Memorize this 20-second answer:**
> DevOps is a culture and a set of practices that brings development and operations together to deliver software **faster, more reliably and more frequently**. It relies on **automation** (CI/CD, Infrastructure as Code, automated testing, monitoring), **small and frequent changes**, **shared ownership** of production, and **fast feedback** from users and systems.

- DevOps is **not a tool** and not only a job title. Tools (Jenkins, Docker, Kubernetes, Terraform) *enable* the practices.
- **The problem it solves, the "wall of confusion":** developers are rewarded for *change* (features), operations for *stability*. Throwing work "over the wall" causes slow releases, "works on my machine" bugs, manual and risky deployments, and blame games.
- **Benefits:** faster time to market, fewer failed releases, faster recovery, better collaboration, repeatable environments, scalability, happier teams.

### 1.2 CALMS framework

| Letter | Meaning | Example |
|---|---|---|
| **C** | Culture: shared responsibility, collaboration, blameless learning | Developers join on-call ("you build it, you run it") |
| **A** | Automation: builds, tests, infra, deployments | CI/CD pipelines, Terraform |
| **L** | Lean: small batches, limit work in progress, remove waste | Trunk-based development |
| **M** | Measurement: measure flow and outcomes | DORA metrics, SLOs |
| **S** | Sharing: knowledge, tools, learnings | Blameless postmortems, internal docs |

### 1.3 The Three Ways (from *The Phoenix Project* / *The DevOps Handbook*)

1. **Flow:** fast left-to-right flow of work from Dev -> Ops -> customer (small batches, fewer hand-offs, automation).
2. **Feedback:** fast right-to-left feedback (automated tests, monitoring, alerts, peer review) so problems are found early.
3. **Continual learning and experimentation:** blameless culture, safe experiments, chaos engineering, improving daily work.

### 1.4 DevOps lifecycle (the infinity loop) ⭐

`Plan -> Code -> Build -> Test -> Release -> Deploy -> Operate -> Monitor -> (feedback) -> Plan`

| Phase | What happens | Common tools |
|---|---|---|
| Plan | Backlog, sprints, requirements | Jira, Azure Boards, Linear |
| Code | Version control, code review | Git, GitHub, GitLab, Bitbucket |
| Build | Compile, package, build images | Maven, Gradle, npm, Docker |
| Test | Automated tests, code quality | JUnit, PyTest, Selenium, SonarQube |
| Release | Version and store artifacts | Nexus, Artifactory, ECR, GHCR |
| Deploy | Push to environments | Jenkins, GitHub Actions, GitLab CI, Argo CD, Helm |
| Operate | Run and configure infrastructure | Kubernetes, Terraform, Ansible, AWS |
| Monitor | Metrics, logs, traces, alerts | Prometheus, Grafana, ELK/Loki, OpenTelemetry, Datadog |

### 1.5 Waterfall vs Agile vs DevOps ⭐

| | Waterfall | Agile | DevOps |
|---|---|---|---|
| Approach | Sequential phases | Iterative sprints | Continuous flow to production |
| Release cadence | Months/years | Every sprint (potentially) | Many times per day possible |
| Focus | Plan everything up front | Business <-> Dev collaboration | Dev <-> Ops collaboration + automation |
| Feedback | At the end | Each sprint | Continuous, from production |
| Change | Expensive | Welcome | Small, frequent, low-risk |

**Key line:** Agile made *development* iterative; DevOps extends that to *delivery and operations*, so what Agile teams build actually reaches users quickly and safely.

### 1.6 CI vs Continuous Delivery vs Continuous Deployment ⭐⭐

| Practice | What it means | Human gate before prod? |
|---|---|---|
| **Continuous Integration (CI)** | Developers merge to the shared mainline frequently (at least daily); every merge triggers an automated build and tests | n/a |
| **Continuous Delivery** | Every change that passes the pipeline is **releasable**; production deployment is a **business decision** (one click / approval) | **Yes** |
| **Continuous Deployment** | Every change that passes all stages is deployed to production **automatically** | **No** |

Memory trick: *Delivery = ready to deploy at any time. Deployment = actually deployed automatically.*

### 1.7 DORA metrics: how DevOps performance is measured ⭐⭐

DORA (DevOps Research and Assessment, now part of Google Cloud) currently uses **five software delivery metrics** in two groups:

| Group | Metric | What it measures |
|---|---|---|
| Throughput | **Deployment frequency** | How often you deploy to production |
| Throughput | **Change lead time** | Time from code commit to running in production |
| Throughput | **Failed deployment recovery time** (formerly MTTR / time to restore) | How fast you recover when a deployment causes a failure |
| Instability | **Change fail rate** | % of deployments that cause a failure needing remediation (rollback, hotfix) |
| Instability | **Deployment rework rate** (added 2024) | % of deployments that are unplanned, done to fix a production incident |

- "Elite" teams historically: deploy on demand (multiple times a day), lead time under a day, recovery under an hour, low change fail rate.
- **Speed and stability are not a trade-off:** the research shows top performers are good at both.
- **Reliability** (meeting availability/latency targets) is tracked separately as *operational* performance.
- **2025 report ("State of AI-assisted Software Development"):** about 90% of respondents use AI at work; AI adoption now correlates with higher throughput, but instability is still a concern. AI **amplifies** what's already there, so strong foundations (small batches, good version control, quality internal platforms) matter more than ever.
- Use metrics to improve **teams and systems**, never to rank individuals.

### 1.8 DevOps vs SRE vs Platform Engineering ⭐

| | DevOps | SRE | Platform Engineering |
|---|---|---|---|
| What it is | Culture and practices | Google's engineering approach to operations ("class SRE implements interface DevOps") | A team that builds an **Internal Developer Platform (IDP)** as a product |
| Focus | Collaboration, automation, flow | Reliability using SLOs, error budgets, toil reduction | Self-service "golden paths" that reduce developer cognitive load |
| Typical work | CI/CD, IaC, monitoring | SLOs, incident response, capacity, automation | Templates, portals (Backstage), paved CI/CD, infra APIs |

Note: many roles titled "DevOps Engineer" in 2026 are effectively platform engineering roles.

### 1.9 Key principles and buzzwords

- **Shift left:** move testing, security and quality checks earlier in the lifecycle, where fixes are cheaper.
- **Infrastructure as Code (IaC):** infrastructure defined in version-controlled code (Terraform, CloudFormation).
- **Immutable infrastructure:** never patch servers in place; build a new image/container and replace.
- **Pets vs cattle:** pets are hand-crafted servers you nurse back to health; cattle are identical, disposable instances replaced automatically.
- **You build it, you run it:** the team that writes a service also operates it.
- **Everything as code:** pipelines, config, policies, dashboards, docs.
- **Small batch size:** smaller changes = easier review, lower risk, faster recovery.
- **Feature flags:** decouple deploying code from releasing features.
- **Value stream mapping:** visualize every step from idea to production to find bottlenecks and waiting time.

### 1.10 The Twelve-Factor App ⭐

| # | Factor | One-line meaning |
|---|---|---|
| 1 | Codebase | One codebase in version control, many deploys |
| 2 | Dependencies | Declare and isolate dependencies explicitly |
| 3 | Config | Store config in the environment, not in code |
| 4 | Backing services | Treat DBs, caches, queues as attached resources (swap via URL) |
| 5 | Build, release, run | Strictly separate build, release and run stages |
| 6 | Processes | Run as stateless, share-nothing processes |
| 7 | Port binding | Export services by binding to a port |
| 8 | Concurrency | Scale out via the process model (more instances) |
| 9 | Disposability | Fast startup and graceful shutdown |
| 10 | Dev/prod parity | Keep environments as similar as possible |
| 11 | Logs | Treat logs as event streams (write to stdout) |
| 12 | Admin processes | Run one-off admin tasks as separate processes |

### 1.11 Monolith vs Microservices ⭐

| | Monolith | Microservices |
|---|---|---|
| Deployment | One unit | Each service deployed independently |
| Scaling | Scale the whole app | Scale only the hot services |
| Tech stack | Usually one | Can differ per service |
| Failure impact | One bug can take down everything | Better isolation (if designed well) |
| Complexity | Simple to start, harder as it grows | Operationally complex: networking, observability, data consistency |
| Best for | Small teams, early products | Large orgs, many teams, independent release cycles |

Microservices challenges to mention: service discovery, network latency and failures (retries, timeouts, circuit breakers), distributed tracing, data consistency (sagas, eventual consistency), versioned APIs, more CI/CD pipelines.

### 1.12 Fundamentals interview Q&A

**Q: Is DevOps a role or a culture?**
Primarily a culture and set of practices. In industry, a "DevOps engineer" usually builds and runs the automation and platform (CI/CD, IaC, Kubernetes, observability) that lets product teams practice DevOps. A strong answer mentions both.

**Q: How is DevOps different from Agile?**
Agile improves collaboration between business and developers with iterative development. DevOps extends collaboration to operations and automates the path to production so changes are released frequently and reliably.

**Q: How do you measure whether DevOps is working?**
DORA metrics (deployment frequency, lead time, change fail rate, failed deployment recovery time, rework rate), SLO attainment, business outcomes (feature adoption, customer satisfaction) and developer experience surveys. Track trends over time per team.

**Q: What are the biggest challenges in a DevOps transformation?**
Cultural resistance, legacy systems, skill gaps, tool sprawl, siloed goals, security/compliance concerns. Start small with one value stream, automate the biggest pain point, show improvement with metrics, then scale.

**Q: What does "shift left" mean in practice?**
Running unit tests, static analysis, dependency scanning, IaC scanning and secret scanning in the developer's PR pipeline (or even pre-commit) instead of at the end before release.

---

## 2. Linux for DevOps

> Almost every DevOps interview starts with Linux. Servers, containers and Kubernetes nodes all run Linux.

### 2.1 Linux architecture

```
User applications (nginx, java, python ...)
Shell (bash, zsh) + system libraries (glibc)      <- user space
--------------------------------------------------------------
Kernel: process & memory management, filesystems, <- kernel space
networking, device drivers, system calls
--------------------------------------------------------------
Hardware (CPU, RAM, disk, network card)
```

- **Kernel** = core of the OS that talks to hardware. **System calls** (`open`, `read`, `fork`, `exec`, ...) are how programs request kernel services.
- **Distributions:** Ubuntu/Debian (`apt`), RHEL/Rocky/AlmaLinux/Fedora/Amazon Linux (`dnf`/`yum`), Alpine (`apk`, tiny, popular for containers).

### 2.2 Boot process ⭐

1. **BIOS/UEFI:** power-on self-test (POST), finds the boot device.
2. **Bootloader (GRUB2):** loads the kernel and the **initramfs** into memory.
3. **Kernel:** initializes hardware, mounts the initramfs (a temporary root filesystem with drivers needed to find the real disk), then mounts the real root filesystem.
4. **systemd (PID 1):** the first user-space process; starts services in parallel.
5. **Default target:** `multi-user.target` (servers) or `graphical.target` (desktops). Old SysV runlevel 3 is roughly multi-user, runlevel 5 graphical.
6. **Login:** getty on the console, sshd for remote logins.

`systemctl get-default`, `systemctl set-default multi-user.target`

### 2.3 Filesystem hierarchy ⭐

| Path | Purpose |
|---|---|
| `/` | Root of everything ("everything is a file") |
| `/etc` | System configuration (`/etc/passwd`, `/etc/hosts`, `/etc/fstab`, `/etc/ssh/sshd_config`) |
| `/var` | Variable data: logs (`/var/log`), caches, spool, `/var/lib/docker` |
| `/home`, `/root` | User home directories; root's home |
| `/bin`, `/sbin`, `/usr/bin`, `/usr/sbin` | User and system binaries (merged into `/usr` on modern distros) |
| `/lib`, `/usr/lib` | Shared libraries, kernel modules |
| `/opt` | Optional / third-party software |
| `/tmp` | Temporary files (often cleared on reboot) |
| `/proc` | Virtual filesystem with process and kernel info (`/proc/cpuinfo`, `/proc/meminfo`, `/proc/<pid>/`) |
| `/sys` | Virtual filesystem for devices and kernel settings (cgroups live in `/sys/fs/cgroup`) |
| `/dev` | Device files (`/dev/sda`, `/dev/null`) |
| `/boot` | Kernel, initramfs, GRUB config |
| `/mnt`, `/media` | Mount points |
| `/run` | Runtime data since boot (PID files, sockets) |

### 2.4 File permissions ⭐⭐

```
-rwxr-x---  1 alice devs  4096 Oct 11 10:00 deploy.sh
| owner=rwx | group=r-x | others=---
first char = type: - file, d directory, l symlink
```

| Permission | Value | On a file | On a directory |
|---|---|---|---|
| r | 4 | Read contents | List names (`ls`) |
| w | 2 | Modify contents | Create, delete, rename files inside |
| x | 1 | Execute | Enter it (`cd`) and access files |

- `chmod 750 deploy.sh` -> owner 7 (rwx), group 5 (r-x), others 0 (---). Symbolic: `chmod u+x,g-w,o= file`.
- `chown alice:devs file`, `chown -R appuser:appuser /opt/app`.
- **umask** removes bits from the defaults (files 666, dirs 777). umask `022` -> files `644`, dirs `755`.

**Special permissions** ⭐

| Bit | Octal | Effect | Example |
|---|---|---|---|
| SUID | 4000 | Program runs with the **file owner's** privileges | `/usr/bin/passwd` (`-rwsr-xr-x`) |
| SGID | 2000 | On a file: runs with the group's privileges. On a directory: new files inherit the directory's group | Shared team folders |
| Sticky bit | 1000 | On a directory: only the file's owner (or root) can delete/rename it | `/tmp` (`drwxrwxrwt`) |

Fine-grained permissions: ACLs with `setfacl -m u:bob:rw file` and `getfacl file`.

### 2.5 Users, groups and sudo

- `/etc/passwd` (accounts), `/etc/shadow` (password hashes, root only), `/etc/group` (groups).
- UID 0 = root. Service/system accounts usually have UID < 1000.

```bash
useradd -m -s /bin/bash deploy      # create user with home dir and shell
passwd deploy                        # set password
usermod -aG docker deploy            # add to a supplementary group (-a = append!)
id deploy; groups deploy             # check membership
userdel -r deploy                    # delete user + home
visudo                               # safely edit /etc/sudoers
```

Gotcha: `usermod -G docker user` **without `-a`** removes the user from all other supplementary groups.

### 2.6 Processes ⭐⭐

- **Process** = running instance of a program with its own PID and memory. **Thread** = lighter unit inside a process sharing its memory.
- Every process has a parent (PPID). PID 1 is `systemd` on a host (and **your app** inside a container).

**Process states** (`STAT` column in `ps`): `R` running/runnable, `S` interruptible sleep (waiting for an event), `D` uninterruptible sleep (usually waiting on disk/NFS I/O, cannot be killed), `T` stopped, `Z` zombie.

| Term | Meaning | What to do |
|---|---|---|
| **Zombie** | Child exited but the parent hasn't read its exit status (`wait()`). Uses no CPU/RAM, only a PID slot | Fix or restart the parent. If the parent dies, PID 1 adopts and reaps it. You cannot `kill` a zombie |
| **Orphan** | Parent died while the child is still running | Adopted by PID 1 (normal) |
| **Daemon** | Background service without a terminal | Managed by systemd |

**Signals** ⭐

| Signal | Number | Meaning |
|---|---|---|
| SIGHUP | 1 | Hang up; many daemons reload config |
| SIGINT | 2 | Interrupt (Ctrl+C) |
| SIGKILL | 9 | Force kill; cannot be caught, no cleanup |
| SIGTERM | 15 | Polite "please terminate"; app can clean up (default for `kill`) |
| SIGSTOP / SIGCONT | 19 / 18 | Pause / resume |

Always try SIGTERM before SIGKILL. Docker and Kubernetes send SIGTERM first, then SIGKILL after a grace period.

```bash
ps aux --sort=-%cpu | head                 # top CPU consumers
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

### 2.7 CPU, memory and load average ⭐⭐

**Load average** (`uptime`, `top`): the average number of processes that are **running or waiting** (R + D states) over the last **1, 5 and 15 minutes**.
- Compare with the CPU core count (`nproc`). Load 4.0 on a 4-core machine means fully busy; 8.0 means work is queueing.
- **High load but low CPU usage** -> processes stuck in `D` state -> usually **I/O wait** (slow disk/NFS). Check `%wa` in `top`, `iostat -xz 1`, `iotop`.

**Memory** (`free -h`):
- Linux uses spare RAM for **buff/cache**; that's healthy, not a leak. Look at the **`available`** column.
- **Swap** = disk used as overflow memory (slow). Constant swapping (`vmstat 1`, columns `si`/`so`) means memory is undersized.
- **OOM killer:** when memory runs out, the kernel kills a process (highest `oom_score`). Check `dmesg -T | grep -i "killed process"` or `journalctl -k`.

Toolbox: `top`, `htop`, `vmstat 1`, `iostat -xz 1`, `mpstat -P ALL 1`, `pidstat 1`, `sar` (history), `free -h`, `nproc`, `lscpu`, `strace -p <pid>` (system calls), `perf top`.

### 2.8 Disk and storage ⭐⭐

```bash
df -h                     # space used per filesystem
df -i                     # inode usage (can be 100% even when space is free!)
du -sh /var/log/*         # size of each item
du -xh / --max-depth=1 2>/dev/null | sort -rh | head   # biggest top-level directories
find / -xdev -type f -size +500M 2>/dev/null           # files bigger than 500 MB
lsblk; blkid              # block devices and UUIDs
mount /dev/xvdf /data     # mount (persist it in /etc/fstab using the UUID)
lsof +L1                  # deleted files still held open by processes
```

- **Inode** = metadata record for a file: type, permissions, owner, size, timestamps, pointers to data blocks. It stores **everything except the filename** (names live in directory entries). Millions of tiny files can exhaust inodes and cause "No space left on device" while `df -h` still shows free space.
- **Deleted-but-open files:** if a process keeps a deleted (e.g. rotated) log open, the space isn't released. Find with `lsof +L1`, then restart the process or truncate it via `: > /proc/<pid>/fd/<fd>`.
- **LVM:** Physical Volume (PV) -> Volume Group (VG) -> Logical Volume (LV). Grow storage online: `lvextend -r -L +20G /dev/vg0/data` (`-r` also resizes the filesystem).
- **RAID:** 0 = striping (speed, no redundancy), 1 = mirroring, 5 = striping + parity (survives one disk), 10 = mirror + stripe.

### 2.9 Hard link vs soft link ⭐⭐

| | Hard link | Soft (symbolic) link |
|---|---|---|
| What it is | Another name for the **same inode** | A small file containing a **path** to the target |
| If the original is deleted | Still works (data remains until the last link is removed) | Breaks (dangling link) |
| Across filesystems | No | Yes |
| Can link directories | No | Yes |
| Command | `ln file hardlink` | `ln -s target linkname` |
| Inode number | Same as the original | Different |

### 2.10 systemd and services ⭐

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

Minimal unit file `/etc/systemd/system/myapp.service`:

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

`restart` stops and starts the process (brief downtime). `reload` asks the process to re-read its config without stopping (if supported, e.g. nginx).

### 2.11 Text processing: grep, awk, sed, find ⭐⭐

```bash
grep -rin "error" /var/log/app/         # recursive, case-insensitive, with line numbers
grep -v "DEBUG" app.log                  # exclude lines
grep -c "500" access.log                 # count matching lines
grep -E "timeout|refused" app.log        # extended regex (OR)
tail -f app.log | grep --line-buffered ERROR

awk '{print $1}' access.log              # print first column
awk -F: '{print $1}' /etc/passwd         # custom delimiter
awk '$9 >= 500 {print $7}' access.log    # URLs that returned 5xx (combined log format)
awk '{sum += $10} END {print sum}' access.log   # sum a column (bytes sent)

sed -i 's/old/new/g' config.txt          # in-place replace
sed -n '10,20p' file                     # print lines 10-20
sed -i '/^#/d' file                      # delete comment lines

cut -d',' -f2 data.csv
sort file | uniq -c | sort -nr           # count duplicates
wc -l file; tr 'a-z' 'A-Z' < file
find /var/log/app -name "*.log" -mtime +7 -delete       # delete logs older than 7 days
find . -type f -name "*.tmp" -print0 | xargs -0 rm -f
```

⭐ **Classic one-liner: top 10 client IPs in an nginx/apache access log**
```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```
⭐ Count HTTP status codes: `awk '{print $9}' access.log | sort | uniq -c | sort -nr`

### 2.12 Networking commands

```bash
ip a; ip r                          # addresses and routes (ifconfig/route are legacy)
ss -tulnp                           # listening TCP/UDP ports + owning process (netstat is legacy)
ss -tulnp | grep :8080              # who is using port 8080? (or: lsof -i :8080)
ping -c 4 host; traceroute host; mtr host
dig example.com +short; dig +trace example.com; nslookup example.com
curl -v https://api.example.com/health       # DNS, TCP, TLS and headers in one view
curl -s -o /dev/null -w "%{http_code} %{time_total}\n" https://example.com
nc -zv db.internal 5432             # is the TCP port reachable?
tcpdump -i eth0 port 443 -nn        # capture packets
cat /etc/resolv.conf /etc/hosts     # DNS servers, static host entries
```

### 2.13 Package management

| Family | Install | Update everything | List installed |
|---|---|---|---|
| Debian/Ubuntu | `apt install nginx` | `apt update && apt upgrade` | `dpkg -l` |
| RHEL/Rocky/Amazon Linux | `dnf install nginx` (or `yum`) | `dnf upgrade` | `rpm -qa` |
| Alpine | `apk add nginx` | `apk upgrade` | `apk info` |

### 2.14 SSH ⭐⭐

```bash
ssh-keygen -t ed25519 -C "me@laptop"     # key pair: ~/.ssh/id_ed25519 (+ .pub)
ssh-copy-id user@server                   # append public key to the server's authorized_keys
ssh -i key.pem ec2-user@203.0.113.10
ssh -J bastion user@private-host          # jump through a bastion host
ssh -L 5432:db.internal:5432 bastion      # local port forward (tunnel to a private DB)
scp file user@host:/tmp/
rsync -avz ./dist/ user@host:/var/www/
```

- **How key authentication works:** the server stores your **public key** in `~/.ssh/authorized_keys`. It sends a challenge; your client signs it with the **private key** (which never leaves your machine); the server verifies the signature with the public key.
- Required permissions: `~/.ssh` = 700, private key and `authorized_keys` = 600, otherwise SSH refuses them.
- Hardening (`/etc/ssh/sshd_config`): `PermitRootLogin no`, `PasswordAuthentication no`, `AllowUsers`/`AllowGroups`, fail2ban, restrict port 22 to known IPs, or avoid open SSH entirely with **AWS SSM Session Manager**.

### 2.15 Cron jobs ⭐

```
# minute(0-59) hour(0-23) day-of-month(1-31) month(1-12) day-of-week(0-7; 0 and 7 = Sunday)
0 2 * * *     /opt/scripts/backup.sh >> /var/log/backup.log 2>&1   # every day at 02:00
*/5 * * * *   /opt/scripts/healthcheck.sh                           # every 5 minutes
0 9 * * 1-5   /opt/scripts/report.sh                                # weekdays at 09:00
0 0 1 * *     /opt/scripts/monthly.sh                               # 1st of every month
```

- `crontab -e` (edit), `crontab -l` (list). System-wide: `/etc/crontab`, `/etc/cron.d/`.
- Gotcha: cron runs with a **minimal environment and PATH**, so use absolute paths and redirect output to a log.
- Modern alternative: **systemd timers** (journald logging, dependencies, missed-run handling).

### 2.16 Logs

- `/var/log/syslog` (Debian/Ubuntu) or `/var/log/messages` (RHEL): general system log.
- `/var/log/auth.log` or `/var/log/secure`: logins, sudo, SSH.
- `dmesg -T`: kernel ring buffer (hardware errors, OOM kills). `journalctl`: systemd journal.
- **logrotate** (`/etc/logrotate.d/`) rotates, compresses and deletes old logs so disks don't fill up.

### 2.17 Shell basics you must know

- `>` overwrite, `>>` append, `2>` stderr, `2>&1` stderr to wherever stdout points, `&>` both, `/dev/null` discards output.
- `|` pipe, `&&` run next only on success, `||` run next only on failure, `;` run regardless.
- `~/.bashrc` (interactive non-login shells) vs `~/.bash_profile` (login shells); `source ~/.bashrc` reloads.
- `export VAR=value` makes a variable visible to child processes; `env` / `printenv` list them.

### 2.18 Linux interview Q&A ⭐

**Q: What happens when you type `ls -l` and press Enter?**
The shell reads and parses the line, expands aliases and globs, searches `$PATH` for `ls`, then **forks** a child process which **execs** `/usr/bin/ls`. `ls` uses system calls (`openat`, `getdents64`, `statx`) to read directory entries and inode metadata, writes the output to stdout (the terminal) and exits. The shell **waits** for the child, stores its exit code in `$?` and prints the prompt again.

**Q: Which process is using port 8080?** `ss -tulnp | grep :8080` or `lsof -i :8080`.

**Q: "No space left on device" but `df -h` shows free space. Why?**
Inodes are exhausted (`df -i`), usually millions of small files (sessions, cache). Also check for space held by deleted-but-open files (`lsof +L1`).

**Q: `kill -9` vs `kill -15`?** SIGTERM (15) lets the app shut down cleanly; SIGKILL (9) is immediate and uncatchable. Use 9 only when 15 fails.

**Q: How do you check the OS and kernel version?** `cat /etc/os-release`, `uname -r`, `hostnamectl`.

**Q: What is swap, and is it bad?** Disk space used as extra memory. A little swap is a safety net; constant swapping means the machine needs more RAM (or the app has a leak).

**Q: How do you run a script automatically at boot?** A systemd service with `WantedBy=multi-user.target` (preferred), or `@reboot` in cron.

**Q: `chmod 755` vs `chmod 644`?** 755 = owner rwx, group/others r-x (scripts, directories). 644 = owner rw, others read-only (regular files).

**Q: What is `/proc`?** A virtual filesystem generated by the kernel in memory that exposes process and kernel information (`/proc/meminfo`, `/proc/<pid>/status`).

**Q: Find files modified in the last 24 hours / 30 minutes.** `find /path -type f -mtime -1` / `find /path -type f -mmin -30`.

**Q: What does `cmd > out.log 2>&1` do?** Sends stdout to `out.log`, then points stderr (fd 2) at the same place as stdout (fd 1). Order matters: `2>&1 > out.log` would leave stderr on the terminal.

---

## 3. Networking for DevOps

### 3.1 OSI and TCP/IP models ⭐⭐

| # | OSI layer | What it does | Examples | DevOps relevance |
|---|---|---|---|---|
| 7 | Application | Network services for apps | HTTP, DNS, SSH, SMTP | L7 load balancers, Ingress/Gateway, API gateways |
| 6 | Presentation | Encoding, encryption, compression | TLS, JSON, JPEG | TLS termination |
| 5 | Session | Opens and maintains sessions | RPC, session handling | Sticky sessions |
| 4 | Transport | End-to-end delivery, ports | TCP, UDP | L4 load balancers (NLB), security group rules |
| 3 | Network | Logical addressing and routing | IP, ICMP, routers | VPCs, subnets, route tables, CIDR |
| 2 | Data Link | Frames inside a network, MAC addresses | Ethernet, ARP, switches, VLANs | Bridges, CNI internals |
| 1 | Physical | Bits on the wire/air | Cables, fiber, radio | - |

Mnemonic (7 -> 1): **"All People Seem To Need Data Processing."**
TCP/IP model (4 layers): Application (OSI 5-7) -> Transport (4) -> Internet (3) -> Link / Network Access (1-2).

### 3.2 TCP vs UDP ⭐⭐

| | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (handshake) | Connectionless |
| Reliability | Guaranteed delivery, ordering, retransmission | Best effort, no ordering |
| Flow / congestion control | Yes | No |
| Header size | 20+ bytes | 8 bytes |
| Speed | More overhead | Faster, lower latency |
| Used by | HTTP/1.1 and HTTP/2, SSH, databases, SMTP | DNS queries, video/voice, gaming, DHCP, **QUIC / HTTP/3** |

**TCP 3-way handshake:** `SYN` -> `SYN-ACK` -> `ACK`. Closing: `FIN` -> `ACK` -> `FIN` -> `ACK` (the side that closes first waits in `TIME_WAIT`).

### 3.3 IP addressing, CIDR and subnetting ⭐⭐

- IPv4 = 32 bits (e.g. `10.0.1.25`); IPv6 = 128 bits.
- **CIDR** `/n` = the first *n* bits are the network part. Number of addresses = **2^(32 - n)**.

| CIDR | Subnet mask | Addresses | Typical use |
|---|---|---|---|
| /32 | 255.255.255.255 | 1 | One host (e.g. your IP in a security group) |
| /28 | 255.255.255.240 | 16 | Tiny subnet |
| /24 | 255.255.255.0 | 256 | Common subnet |
| /20 | 255.255.240.0 | 4,096 | Large subnet (EKS nodes + pods) |
| /16 | 255.255.0.0 | 65,536 | Whole VPC |
| /0 | 0.0.0.0 | everything | "Anywhere" (`0.0.0.0/0`) |

- **Private ranges (RFC 1918)** ⭐: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`. Not routable on the internet; they need NAT to reach it.
- Special: `127.0.0.0/8` loopback; `169.254.0.0/16` link-local (cloud **instance metadata** lives at `169.254.169.254`); a server binding to `0.0.0.0` listens on all interfaces.
- Classic networks lose 2 addresses per subnet (network + broadcast). **AWS reserves 5 IPs in every subnet** (first four + last), so a `/24` gives **251 usable** addresses. ⭐
- Example: split `10.0.0.0/16` into `/24`s -> 256 subnets: `10.0.0.0/24`, `10.0.1.0/24`, ..., `10.0.255.0/24`. Terraform: `cidrsubnet("10.0.0.0/16", 8, 1)` = `10.0.1.0/24`.

### 3.4 DNS ⭐⭐

**Resolution flow for `www.example.com`:**
1. Browser cache -> OS cache -> `/etc/hosts`.
2. Query goes to a **recursive resolver** (ISP, `8.8.8.8`, `1.1.1.1`, or the VPC resolver).
3. Resolver asks a **root server** -> "ask the `.com` TLD servers".
4. Asks the **TLD server** -> "ask example.com's authoritative name servers".
5. Asks the **authoritative server** -> gets the record (e.g. an A record `203.0.113.10`).
6. Resolver caches the answer for the record's **TTL** and returns it.

| Record | Purpose |
|---|---|
| A / AAAA | Name -> IPv4 / IPv6 address |
| CNAME | Alias to another name (not allowed at the zone apex such as `example.com`) |
| MX | Mail servers |
| TXT | Free text: domain verification, SPF, DKIM, DMARC |
| NS | Authoritative name servers for the zone |
| SOA | Zone metadata (primary NS, serial, timers) |
| PTR | Reverse lookup (IP -> name) |
| SRV | Service location (host + port) |
| CAA | Which certificate authorities may issue certs for the domain |
| Alias (Route 53) | CNAME-like, but works at the apex and points to AWS resources (ALB, CloudFront, S3) |

- DNS uses **UDP 53** (TCP 53 for large responses and zone transfers).
- **TTL** matters for migrations: lower the TTL a day before switching IPs so clients pick up the change quickly.
- Debug with `dig name`, `dig +trace name`, `dig @8.8.8.8 name`, `nslookup name`.

### 3.5 HTTP and HTTPS ⭐⭐

**Methods:** GET (read), POST (create), PUT (replace; idempotent), PATCH (partial update), DELETE (idempotent), HEAD (headers only), OPTIONS (CORS preflight).

**Status codes** ⭐⭐

| Code | Meaning | Typical DevOps cause |
|---|---|---|
| 200 / 201 / 204 | OK / Created / No Content | - |
| 301 / 302 / 304 | Moved permanently / Found (temporary) / Not modified (cache) | HTTP -> HTTPS redirects |
| 400 | Bad Request | Malformed payload |
| 401 | Unauthorized = **not authenticated** | Missing or expired token |
| 403 | Forbidden = authenticated but **not allowed** | IAM/RBAC, WAF block, S3 bucket policy |
| 404 | Not Found | Wrong path, wrong ingress rule |
| 429 | Too Many Requests | Rate limiting |
| 500 | Internal Server Error | Application bug / unhandled exception |
| **502** | **Bad Gateway**: proxy/LB got an **invalid or no response** from the upstream | App crashed, wrong upstream port, app not listening, connection reset |
| **503** | **Service Unavailable** | No healthy targets, overload, maintenance mode |
| **504** | **Gateway Timeout**: upstream **didn't respond in time** | Slow DB query, mismatched timeouts, traffic silently dropped by a firewall/security group |

**HTTP versions:** HTTP/1.1 (keep-alive, one request at a time per connection), HTTP/2 (binary, many streams multiplexed over one TCP connection, header compression), HTTP/3 (runs over **QUIC on UDP**, faster setup, no TCP head-of-line blocking).

**TLS handshake (TLS 1.3, simplified)** ⭐
1. **ClientHello:** supported versions and cipher suites, key share, SNI (the hostname).
2. **ServerHello:** chosen cipher + key share, then the server's **certificate** (+ intermediate chain) and a signature proving it owns the private key.
3. The client **verifies the certificate**: chains to a trusted CA, hostname matches, not expired.
4. Both sides derive the same **symmetric session keys** (ECDHE); application data is now encrypted.

- Asymmetric crypto handles authentication and key exchange; fast **symmetric** ciphers (AES-GCM, ChaCha20) encrypt the bulk data.
- TLS 1.3 needs 1 round trip (TLS 1.2 needed 2). Certificates come from Let's Encrypt, AWS ACM, or cert-manager in Kubernetes. **Monitor expiry**: expired certificates cause many outages.

### 3.6 Ports to remember ⭐

| Port | Service | Port | Service |
|---|---|---|---|
| 22 | SSH | 3306 | MySQL |
| 25 | SMTP | 5432 | PostgreSQL |
| 53 | DNS | 6379 | Redis |
| 80 / 443 | HTTP / HTTPS | 27017 | MongoDB |
| 123 | NTP (UDP) | 9092 | Kafka |
| 389 / 636 | LDAP / LDAPS | 2049 | NFS |
| 3389 | RDP | 8080 | Jenkins / alternate HTTP |
| 6443 | Kubernetes API server | 2379-2380 | etcd |
| 10250 | kubelet API | 30000-32767 | Kubernetes NodePort range |
| 9090 | Prometheus | 9100 | node_exporter |
| 3000 | Grafana | 9093 | Alertmanager |
| 9200 / 5601 | Elasticsearch / Kibana | 9000 | SonarQube |
| 8081 | Nexus | 8200 | HashiCorp Vault |

### 3.7 Load balancers and proxies ⭐⭐

- A **load balancer** spreads traffic across healthy backends using **health checks** -> high availability and horizontal scaling.
- **L4 LB** (transport): routes on IP/port, very fast, doesn't parse HTTP (AWS NLB).
- **L7 LB** (application): routes on host/path/headers/cookies, terminates TLS, redirects, integrates with a WAF (AWS ALB, NGINX, HAProxy, Envoy, Traefik).
- **Algorithms:** round robin, weighted round robin, least connections, least response time, IP hash / consistent hashing.
- **Sticky sessions** pin a user to one backend. Better: stateless apps with sessions in Redis/DB.
- **Forward proxy** sits in front of **clients** (corporate proxy; hides clients, filters outbound traffic). **Reverse proxy** sits in front of **servers** (hides servers; load balancing, TLS, caching, compression; e.g. NGINX, Envoy).
- **API gateway** = reverse proxy + authentication, rate limiting, request transformation, API keys (Kong, AWS API Gateway).

### 3.8 NAT, firewalls, VPN, CDN

- **NAT** rewrites private source IPs to a public IP so private machines can reach the internet (outbound only). AWS: **NAT Gateway**.
- **Stateful firewall** tracks connections, so return traffic is automatically allowed (AWS **Security Groups**). **Stateless** firewall checks every packet, so return traffic must be allowed explicitly (AWS **NACLs**).
- Linux firewalls: `iptables` / `nftables`, `ufw` (Ubuntu), `firewalld` (RHEL).
- **VPN** = encrypted tunnel between networks (site-to-site) or between a user and a network.
- **CDN** caches content at edge locations near users (CloudFront, Cloudflare, Akamai): lower latency, less origin load, DDoS absorption.

### 3.9 "What happens when you type https://www.google.com in a browser?" ⭐⭐⭐

1. Browser checks its caches and the HSTS list (forces HTTPS).
2. **DNS** resolution (browser -> OS -> resolver -> root -> TLD -> authoritative) returns an IP.
3. **TCP 3-way handshake** to port 443 (or a **QUIC** handshake over UDP for HTTP/3).
4. **TLS handshake**: certificate validation, session keys.
5. Browser sends an **HTTP GET** with headers and cookies.
6. The request reaches a **CDN/edge** -> **load balancer** -> **web/app servers**, which may call caches, databases and other services.
7. Server returns an **HTTP response** (status, headers, HTML).
8. Browser **parses the HTML**, fetches CSS/JS/images (often in parallel over HTTP/2 or HTTP/3), builds the DOM and CSSOM, runs JavaScript and **renders** the page.
9. The connection is kept alive for reuse.

### 3.10 Network troubleshooting checklist ⭐

1. **DNS:** does the name resolve correctly? `dig name`
2. **Reachability:** `ping` (ICMP may be blocked), `traceroute` / `mtr`
3. **Port open?** `nc -zv host port`, `curl -v`
4. **Service listening?** On the server: `ss -tulnp`. Bound to `0.0.0.0` or only `127.0.0.1`?
5. **Firewalls:** iptables/ufw, security groups, NACLs, Kubernetes NetworkPolicies
6. **Routing:** route tables, NAT/IGW, peering routes
7. **TLS:** `openssl s_client -connect host:443 -servername host` (expiry, chain, hostname)
8. **Logs:** application logs and LB/proxy logs (502 vs 504 tells you a lot)

Rule of thumb: **"It's always DNS"**, so check it first.

### 3.11 Networking interview Q&A

**Q: Difference between 502 and 504?** 502 = the upstream returned an invalid response or none at all (crash, wrong port, reset). 504 = the upstream was too slow and the proxy timed out.

**Q: Why can't a CNAME exist at the zone apex?** The apex must hold SOA and NS records, and a CNAME can't coexist with other records at the same name. Use an ALIAS/ANAME record (Route 53 Alias) instead.

**Q: How many usable IPs are in a /26 subnet on AWS?** 64 - 5 = **59**.

**Q: What is a default gateway?** The router IP a host sends traffic to when the destination is outside its own subnet.

**Q: Why do we need NAT if we have an Internet Gateway?** The IGW gives internet access to resources with public IPs. Private-subnet resources have no public IP; a NAT Gateway (in a public subnet) lets them make outbound connections without being reachable from the internet.

---

## 4. Git and Version Control

### 4.1 Concepts

- **Distributed VCS:** every clone contains the full history (unlike centralized SVN).
- **Areas:** Working directory -> (`git add`) -> **Staging area / index** -> (`git commit`) -> **Local repository** -> (`git push`) -> **Remote** (GitHub/GitLab/Bitbucket).
- **Commit** = snapshot of the project + metadata (author, message, parent commit), identified by a SHA hash.
- **Branch** = a lightweight movable pointer to a commit. **HEAD** = pointer to your current branch/commit. **Detached HEAD** = HEAD points to a commit directly, not to a branch.
- **Tag** = fixed pointer to a commit, used for releases (`v1.4.2`). Annotated tags also store author, date and message.

### 4.2 Everyday commands ⭐

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
git restore file.txt                  # discard working-directory changes
git restore --staged file.txt         # unstage
git log -p -- path/file; git blame path/file
git reflog                            # history of HEAD movements (rescue tool)
git bisect start; git bisect bad; git bisect good v1.0   # binary search for a bad commit
git cherry-pick <sha>
git clean -fd                         # delete untracked files and directories
```

### 4.3 The most-asked Git differences ⭐⭐⭐

| Question | Answer |
|---|---|
| `git fetch` vs `git pull` | `fetch` downloads remote changes into remote-tracking branches (`origin/main`) **without touching your branch**. `pull` = `fetch` + `merge` (or `rebase` with `--rebase`). |
| `merge` vs `rebase` | Merge joins histories with a **merge commit** (non-linear, preserves history, safe on shared branches). Rebase **replays your commits on top** of another branch: linear history, but **commit IDs are rewritten**. Golden rule: **never rebase a branch other people have pulled**. |
| `reset` vs `revert` | `reset` moves the branch pointer backwards (rewrites history; local use). `revert` adds a **new commit that undoes** an old one (safe on shared branches). |
| `reset --soft / --mixed / --hard` | soft: undo the commit, keep changes **staged**. mixed (default): keep changes **unstaged**. hard: **discard** changes. |
| Fast-forward vs 3-way merge | Fast-forward just moves the pointer when histories haven't diverged; a 3-way merge creates a merge commit when both sides have new commits. |
| `cherry-pick` | Apply one specific commit to the current branch (e.g. a hotfix into a release branch). |
| `stash` | Temporarily shelve uncommitted work so you can switch context. |
| Squash | Combine several commits into one (`git rebase -i`, or "Squash and merge" on a PR). |
| Fork vs clone | Fork = server-side copy in your account (GitHub feature). Clone = local copy on your machine. |
| `git rm --cached file` | Stop tracking a file but keep it on disk (then add it to `.gitignore`). |

### 4.4 Merge conflicts

They happen when the same lines changed on both branches:

```
<<<<<<< HEAD
timeout = 30
=======
timeout = 60
>>>>>>> feature/x
```

Edit the file to the correct result -> `git add file` -> `git commit` (or `git rebase --continue`). Prevent them with small PRs, frequent syncing with `main`, and trunk-based development.

### 4.5 Branching strategies ⭐⭐

| Strategy | How it works | Best for | Downsides |
|---|---|---|---|
| **GitFlow** | `main` (prod), `develop`, `feature/*`, `release/*`, `hotfix/*` | Versioned, scheduled releases (mobile apps, on-prem products) | Heavy, long-lived branches, merge pain, slows CD |
| **GitHub Flow** | `main` is always deployable; short-lived feature branch -> PR -> review -> merge -> deploy | Web apps / SaaS | Needs good automated tests |
| **Trunk-Based Development** | Everyone integrates into `main` at least daily via very short-lived branches; unfinished work hidden behind **feature flags** | High-performing CI/CD teams (DORA research links it to better delivery performance) | Requires testing discipline and feature flags |
| Release branches | Cut `release/1.4` from main for stabilization and hotfixes | Supporting several versions in parallel | Back-porting effort |

Anti-pattern: **environment branches** (`dev`, `qa`, `prod` as branches) cause drift and painful merges. Promote the same artifact through environments instead.

### 4.6 Good practices

- **Branch protection:** required reviews, required status checks (CI must pass), no force-push to `main`, signed commits, `CODEOWNERS`.
- **Conventional commits** (`feat:`, `fix:`, `chore:`) enable automated changelogs and semantic versioning.
- **Git hooks:** `pre-commit` (lint, format, **secret scanning**), `commit-msg`, `pre-push`; server-side `pre-receive`. The `pre-commit` framework makes them shareable.
- **`.gitignore`:** build outputs, `node_modules`, `.env`, `*.tfstate`, keys.
- Large binaries -> Git LFS.

### 4.7 Git interview Q&A

**Q: How do you undo the last commit?** Not pushed: `git reset --soft HEAD~1` (keep changes) or `git reset --hard HEAD~1` (discard). Already pushed to a shared branch: `git revert <sha>`.

**Q: You committed and pushed an AWS key. What do you do?** ⭐⭐
1. **Revoke/rotate the key immediately**. Assume it's compromised; bots scan public repos within minutes.
2. Check audit logs (e.g. CloudTrail) for misuse.
3. Remove it from history (`git filter-repo` or BFG), force-push, ask collaborators to re-clone.
4. Prevent recurrence: secret scanning and push protection, pre-commit hooks (gitleaks), secrets in a secret manager, OIDC instead of static keys in CI.

**Q: I lost commits after `git reset --hard`.** `git reflog` -> find the SHA -> `git reset --hard <sha>` (or `git branch rescue <sha>`).

**Q: How do you find which commit introduced a bug?** `git bisect` (binary search between a known good and a known bad commit).

**Q: `git checkout` vs `git switch` / `git restore`?** `checkout` did two jobs (switching branches and restoring files); Git 2.23 split them into `switch` and `restore` for clarity.

---

## 5. Shell Scripting and Python

### 5.1 Bash essentials ⭐⭐

```bash
#!/usr/bin/env bash
set -euo pipefail          # -e: exit on error, -u: error on unset variables,
                           # -o pipefail: a pipeline fails if ANY command in it fails
# set -x                   # debug mode: print each command (or run: bash -x script.sh)

NAME="world"               # no spaces around =
echo "Hello, ${NAME}"      # always quote variables
TODAY=$(date +%F)          # command substitution
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

**Special variables** ⭐

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1` ... `$9` | Positional arguments |
| `$#` | Number of arguments |
| `$@` | All arguments as separate words (use `"$@"`) |
| `$*` | All arguments as one string |
| `$?` | Exit status of the last command (0 = success) |
| `$$` | PID of the current shell |
| `$!` | PID of the last background job |

**Test operators:** `-f` file exists, `-d` directory, `-e` exists, `-r`/`-w`/`-x` permissions, `-z` empty string, `-n` non-empty, `-eq -ne -lt -le -gt -ge` numeric, `==` / `!=` strings.

**Exit codes worth memorizing** ⭐ (you'll see them in Docker and Kubernetes)

| Code | Meaning |
|---|---|
| 0 | Success |
| 1 | General error |
| 2 | Misuse of a builtin / bad arguments |
| 126 | Found but not executable (permissions) |
| 127 | Command not found (typo, missing binary, wrong PATH) |
| 130 | Terminated by Ctrl+C (128 + 2) |
| **137** | Killed by **SIGKILL** (128 + 9); in containers usually **OOMKilled** |
| **143** | Terminated by **SIGTERM** (128 + 15); a normal graceful stop |

Quotes: `'$HOME'` is literal; `"$HOME"` expands the variable.

### 5.2 Scripts interviewers love ⭐⭐

**1. Disk usage alert**
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

**2. Restart a service if it is down**
```bash
#!/usr/bin/env bash
SERVICE=${1:-nginx}
if ! systemctl is-active --quiet "$SERVICE"; then
  echo "$(date '+%F %T') $SERVICE is down - restarting" >> /var/log/service-watch.log
  systemctl restart "$SERVICE"
fi
```

**3. Website health check**
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

**4. Backup with 7-day retention**
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

**5. Run a command on many servers**
```bash
#!/usr/bin/env bash
while IFS= read -r host; do
  echo "== $host =="
  ssh -n -o ConnectTimeout=5 "$host" 'uptime; df -h /' || echo "FAILED: $host"
done < servers.txt
```
(`ssh -n` stops ssh from reading the loop's stdin, a classic bug where the loop only processes the first host.)

**6. Handy one-liners**
```bash
ps aux --sort=-%mem | head -6                        # top memory processes
grep -c "ERROR" app.log                              # count errors
find /var/log -name "*.log" -size +100M               # large logs
docker ps -q -f status=exited | xargs -r docker rm    # remove exited containers
kubectl get pods -A | grep -v Running                 # pods that aren't Running
```

### 5.3 Python for DevOps

Use Python when logic gets complex: APIs, JSON/YAML processing, cloud automation, proper error handling and tests.

| Library | Use |
|---|---|
| `subprocess`, `os`, `pathlib`, `shutil` | Run commands, work with files |
| `requests` | REST APIs, webhooks |
| `json`, `yaml` (PyYAML) | Parse config |
| `boto3` | AWS automation |
| `kubernetes` | Kubernetes API client |
| `paramiko`, `fabric` | SSH automation |
| `argparse`, `logging` | Build CLI tools |

**Example: find unattached EBS volumes (cost saving)** ⭐
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

**Example: count HTTP status codes in an access log**
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

**Q: `$@` vs `$*`?** Quoted `"$@"` keeps each argument separate (safe with spaces); `"$*"` joins all arguments into one string.

**Q: How do you debug a shell script?** `bash -x script.sh` or `set -x`; `shellcheck` for static analysis.

**Q: How do you keep a script running after logout?** `nohup ... &`, `tmux`/`screen`, or (best for services) a systemd unit.

**Q: Bash or Python?** Bash for gluing commands and short tasks; Python for complex logic, APIs, data processing, maintainability and unit tests.

**Q: What does `set -o pipefail` change?** Without it, `false | true` returns 0 (the status of the last command). With it, the pipeline returns the failing command's status.

---

## 6. Build Tools and Artifact Management

### 6.1 Build tools

| Ecosystem | Tool | Manifest | Typical CI command |
|---|---|---|---|
| Java | Maven | `pom.xml` | `mvn -B clean verify` |
| Java/Kotlin | Gradle | `build.gradle(.kts)` | `./gradlew build` |
| Node.js | npm / yarn / pnpm | `package.json` + lockfile | `npm ci && npm run build` |
| Python | pip / poetry / uv | `requirements.txt` / `pyproject.toml` | `pip install -r requirements.txt` |
| Go | go | `go.mod` | `go build ./...` |
| .NET | dotnet | `*.csproj` | `dotnet publish -c Release` |

**Maven lifecycle** ⭐: `validate -> compile -> test -> package -> verify -> install -> deploy`
- Running a phase runs all earlier phases (`mvn package` also compiles and tests).
- `install` copies the artifact to the local repository (`~/.m2`); `deploy` uploads it to a remote repository (Nexus/Artifactory).
- `clean` is a separate lifecycle (deletes `target/`). `-DskipTests` skips running tests.
- `SNAPSHOT` versions (`1.0-SNAPSHOT`) are mutable development builds; release versions are immutable.

`npm ci` vs `npm install`: `npm ci` installs exactly what the lockfile says, fails if it's out of sync, and is faster and reproducible, so use it in CI.

### 6.2 Artifacts ⭐

- **Artifact** = the output of a build: JAR/WAR, Docker image, npm package, Python wheel, binary, Helm chart.
- **Artifact repositories:** Nexus, JFrog Artifactory, GitHub Packages, AWS CodeArtifact. **Container registries:** Docker Hub, Amazon ECR, Azure ACR, Google Artifact Registry, GHCR, Harbor (self-hosted).
- ⭐ **Build once, deploy many:** build the artifact **once**, then **promote the same artifact** (same image digest) from dev -> staging -> prod, injecting environment-specific config at deploy time. Rebuilding per environment means production runs something you never tested.
- **Semantic versioning** `MAJOR.MINOR.PATCH` (e.g. `2.4.1`): MAJOR = breaking change, MINOR = backward-compatible feature, PATCH = bug fix. Pre-release: `1.0.0-rc.1`.
- **Image tags:** don't deploy `latest`. Use immutable tags such as the **git SHA** and/or semver (`myapp:1.4.2`, `myapp:3f9c2ab`), and pin by **digest** (`myapp@sha256:...`) for maximum safety. Turn on tag immutability in the registry.
- Retention/cleanup policies (e.g. ECR lifecycle policies) stop registries from growing forever.

### 6.3 Code quality and testing

- **SonarQube:** static analysis for bugs, vulnerabilities, code smells, duplication and coverage. A **Quality Gate** (e.g. "coverage on new code >= 80%, no new critical issues") can fail the pipeline.
- **Linters:** ESLint, ruff/pylint, `shellcheck` (bash), `hadolint` (Dockerfiles), `tflint` (Terraform), `yamllint`, `kubeconform` / `kube-linter` (Kubernetes manifests).
- **Testing pyramid** ⭐: many fast **unit** tests -> fewer **integration** tests -> few slow **end-to-end/UI** tests.
- Other test types: **smoke** (quick post-deploy sanity check), **regression**, **performance/load** (JMeter, k6, Gatling, Locust), **security** (SAST/DAST, see section 18), **contract tests** (Pact) between microservices.
- **Flaky tests** (randomly pass/fail) destroy trust in CI: track them, quarantine them, fix the root cause (timing, shared state, external dependencies).

---

## 7. CI/CD Pipelines

### 7.1 What a pipeline looks like ⭐⭐

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

### 7.2 CI/CD best practices ⭐⭐

1. **Pipeline as code** (Jenkinsfile, `.github/workflows/*.yml`, `.gitlab-ci.yml`), versioned and reviewed like application code.
2. **Fail fast:** run cheap checks (lint, unit tests) first.
3. **Build once, promote the same artifact** through environments.
4. **Keep CI fast** (aim for < 10 minutes): dependency and Docker layer caching, parallel jobs, test splitting, build only what changed (monorepos).
5. **Ephemeral, isolated runners/agents** (containers or pods) with a clean state every build.
6. **Secrets** never in code or logs: use the CI secret store or a vault, and prefer **OIDC short-lived cloud credentials** over static access keys.
7. **Security scanning built in** (SAST, SCA, secrets, images, IaC) with sensible failure thresholds.
8. **Automated deployment verification and rollback** (health checks, metrics, canary analysis).
9. **Everything versioned and immutable:** images, charts, IaC modules.
10. **Fast notifications** (Slack/Teams) and visible pipeline status.
11. **Trunk-based development + feature flags** keep `main` always releasable.

### 7.3 Jenkins ⭐⭐ (very common in Indian service companies)

**Architecture**
- **Controller** (formerly "master"): web UI, configuration, scheduling, plugins; stores everything in `JENKINS_HOME`.
- **Agents** (formerly "slaves"): machines/containers that run builds; they connect over **SSH** or as **inbound agents** (JNLP/WebSocket). **Executors** = how many builds an agent runs concurrently.
- Best practice: set the controller's executors to **0** (never build on the controller) and use **dynamic agents** (Kubernetes plugin pods, Docker, EC2 fleets).

**Job types:** Freestyle (UI-configured, legacy), **Pipeline** (Jenkinsfile), **Multibranch Pipeline** (auto-creates a pipeline for every branch/PR containing a Jenkinsfile), Organization Folders.

**Declarative vs Scripted pipelines** ⭐⭐

| Declarative | Scripted |
|---|---|
| `pipeline { ... }` with a fixed structure | `node { ... }` with plain Groovy |
| Easy to read, syntax validation, built-in `post`, `when`, `options` | Very flexible (loops, complex logic) but harder to maintain |
| Recommended default | For edge cases, or use `script { }` blocks inside declarative |

**Features to mention:** triggers (GitHub/GitLab **webhooks** preferred over Poll SCM), parameters, `credentials()` / `withCredentials` (secrets masked in logs), **Shared Libraries** (reusable Groovy steps across repos via `@Library('my-lib') _`), `parallel` stages, `input` approvals, `stash`/`unstash`, `archiveArtifacts`, `junit` reports, `post { always | success | failure | cleanup }`, **Configuration as Code (JCasC)**.

**Sample declarative Jenkinsfile** ⭐
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
Tip: in real pipelines, put long manual approvals outside an agent (or use the stage-level `input` directive) so the waiting build doesn't hold an executor.

**Jenkins Q&A**
- **How do you trigger builds on every push?** A webhook from GitHub/GitLab to Jenkins (Multibranch / GitHub plugin), not polling.
- **How do you store secrets?** Jenkins Credentials (or the Vault plugin); use `withCredentials`; never echo them.
- **How do you back up Jenkins?** Back up `JENKINS_HOME` (jobs, config, plugins, credentials), or better, make it reproducible with JCasC + Jenkinsfiles in Git.
- **A build is stuck in the queue. Why?** No free executors, offline agents, label mismatch, controller has 0 executors and no agents are connected, throttling plugins.
- **How do you scale Jenkins?** Distributed/ephemeral agents (Kubernetes plugin), separate controllers per team/domain, keep the controller light.
- **What is a Shared Library?** A versioned Git repo of Groovy code providing reusable pipeline steps (`vars/`) and classes (`src/`) for many pipelines.

### 7.4 GitHub Actions ⭐⭐

| Term | Meaning |
|---|---|
| Workflow | YAML file in `.github/workflows/` |
| Event / trigger | `push`, `pull_request`, `schedule` (cron), `workflow_dispatch` (manual), `release`, `workflow_call` (reusable workflow) |
| Job | Group of steps on one runner; jobs run **in parallel** unless linked with `needs:` |
| Step | A shell command (`run:`) or an action (`uses:`) |
| Action | Reusable unit (e.g. `actions/checkout`) |
| Runner | Machine that runs jobs: GitHub-hosted (`ubuntu-latest`) or self-hosted (autoscaled on Kubernetes with Actions Runner Controller) |
| Secrets / variables | Encrypted secrets and plain config at repo, org or environment level |
| Environment | e.g. `production` with protection rules (required reviewers, wait timers) |
| Matrix | Run the same job across versions/OSes |

**OIDC to the cloud** ⭐⭐: the job requests a short-lived **OIDC token** from GitHub; AWS/Azure/GCP trusts GitHub's OIDC provider and exchanges the token for **temporary credentials** for a specific role. No long-lived keys stored as secrets. Requires `permissions: id-token: write`.

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

**Security tips:** pin third-party actions to a commit SHA (tags can be moved by an attacker), set minimal `permissions:`, be very careful with `pull_request_target` (it runs with the base repo's secrets), never print secrets, protect production with environment reviewers, keep actions updated with Dependabot.

### 7.5 GitLab CI (quick view)

`.gitlab-ci.yml` defines `stages` and jobs with `image`, `script`, `rules`, `artifacts`, `cache`, `needs` (DAG), `environment` and `include` (templates). Runners (shared/group/project) use executors such as shell, docker or kubernetes. GitLab also ships a built-in container registry and security scanning templates.

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

### 7.6 Choosing a CI/CD tool

| Tool | Strengths | Watch-outs |
|---|---|---|
| Jenkins | Free, huge plugin ecosystem, full control, on-prem friendly | Plugin upkeep, controller maintenance, Groovy complexity |
| GitHub Actions | Native to GitHub, marketplace, simple YAML, OIDC | Minutes cost at scale, third-party action supply-chain risk |
| GitLab CI | All-in-one platform (SCM, registry, security scans) | Best when code lives on GitLab |
| Azure DevOps Pipelines | Strong Azure/Microsoft integration, Boards, Artifacts | |
| CircleCI / Buildkite / Tekton / Argo Workflows | SaaS speed / hybrid agents / Kubernetes-native pipelines | |
| Argo CD / Flux | **CD only (GitOps)**: pull-based sync into Kubernetes | Pair with a CI tool |

### 7.7 CI/CD interview Q&A ⭐⭐

**Q: Your pipeline takes 40 minutes. How do you speed it up?**
Measure each stage first. Then cache dependencies and Docker layers (BuildKit cache), run independent jobs in parallel, split tests across runners, build/test only affected services (path filters in monorepos), use smaller base images, use bigger or self-hosted runners, move slow E2E suites later or to nightly runs, and order stages to fail fast.

**Q: How do you handle database migrations in CD?**
Version migrations with Flyway/Liquibase/Alembic, run them as a separate step/job before the app rollout, and keep them **backward compatible** with the **expand -> migrate -> contract** pattern (add new column, deploy code that writes both, backfill, switch reads, remove old column in a later release). Never ship a destructive change in the same release that stops using the old schema.

**Q: What is a webhook?** An HTTP callback: e.g. GitHub sends a POST request to Jenkins when a push or PR event happens.

**Q: Artifact vs cache?** An artifact is an output you keep or pass between jobs/releases (JAR, test report, image). A cache stores reusable inputs (dependencies) purely to speed things up and may be evicted.

**Q: How would you design CI/CD for 30 microservices?**
Shared, versioned pipeline templates (Jenkins shared library or reusable workflows), a pipeline per service triggered by path changes, a standard Helm chart, contract tests between services, a GitOps config repo per environment, and automated canary analysis for production.

---

## 8. Deployment Strategies

| Strategy | How it works | Downtime | Rollback | Extra cost | Risk |
|---|---|---|---|---|---|
| **Recreate** | Stop all old instances, then start the new ones | Yes | Redeploy old version | None | High |
| **Rolling update** | Replace instances gradually (Kubernetes default via `maxSurge`/`maxUnavailable`) | No | Gradual roll back | Low | Medium; both versions run side by side for a while |
| **Blue-Green** ⭐ | Two identical environments; switch all traffic from Blue (old) to Green (new) at the LB/DNS | No | **Instant** (switch back) | 2x during release | Low, but the DB schema must work for both |
| **Canary** ⭐ | Send a small % of traffic (1% -> 10% -> 50% -> 100%) to the new version, watch metrics, then promote | No | Fast (shift traffic back) | Low-medium | Lowest; small blast radius |
| **A/B testing** | Route by user attributes to compare **business** metrics (conversion) | No | - | Medium | A product experiment more than a safety mechanism |
| **Shadow / dark launch** | Mirror real traffic to the new version; users only see the old version's responses | No | n/a | High | Real-load testing; beware side effects like double writes |
| **Feature flags** | Ship code "off", enable at runtime for some or all users | No | Flip the flag | Low | Decouples **deploy** from **release** |

Tools: **Argo Rollouts** / **Flagger** (automated canary and blue-green on Kubernetes with metric analysis), service mesh or **Gateway API** weighted routes, AWS ALB weighted target groups, AWS CodeDeploy, Route 53 weighted records. Feature flags: LaunchDarkly, Unleash, Flagsmith, **OpenFeature** (vendor-neutral standard).

**Zero-downtime checklist** ⭐⭐: at least 2 replicas, **readiness probes**, graceful shutdown (handle **SIGTERM**, finish in-flight requests, `preStop` hook), connection draining on the LB, `maxUnavailable: 0`, **PodDisruptionBudgets**, backward-compatible DB migrations (expand/contract) and APIs.

**Rollback options:** redeploy the previous immutable image tag, `kubectl rollout undo`, `helm rollback`, switch blue-green back, turn off the feature flag, or `git revert` in a GitOps repo. For databases, prefer **roll-forward fixes** and migrations designed so the old version keeps working.

**Q: Blue-green or canary?** Blue-green for an instant all-or-nothing switch when you can afford double capacity; canary to limit blast radius and validate with real traffic and metrics. Many teams combine canary + automated analysis + feature flags.

---

## 9. Docker and Containers

### 9.1 VM vs container ⭐⭐⭐

```
VIRTUAL MACHINES                         CONTAINERS
[App A][App B][App C]                    [App A][App B][App C]
[Libs ][Libs ][Libs ]                    [Libs ][Libs ][Libs ]
[GuestOS][GuestOS][GuestOS]              [ Container runtime  ]
[      Hypervisor      ]                 [ Host OS + ONE shared kernel ]
[   Hardware / Host OS ]                 [      Hardware       ]
```

| | Virtual machine | Container |
|---|---|---|
| Virtualizes | Hardware (via a hypervisor) | The operating system (process isolation) |
| OS | Full guest OS per VM | Shares the **host kernel** |
| Size | GBs | MBs |
| Startup | Minutes | Seconds or less |
| Isolation | Strong (separate kernels) | Weaker (shared kernel), so harden it |
| Density | Tens per host | Hundreds per host |
| Typical use | Different OS/kernel, strong isolation, legacy apps | Microservices, CI jobs, portable packaging |

Hypervisors: **Type 1** (bare metal: VMware ESXi, KVM, Hyper-V, Xen; used by clouds) vs **Type 2** (runs on a host OS: VirtualBox, VMware Workstation).

**Why containers?** The image packages the app plus all dependencies, which solves "works on my machine", gives identical dev -> prod behavior, fast startup, efficient resource use and immutable deployments.

### 9.2 How containers actually work ⭐⭐ (impresses interviewers)

A container is a **normal Linux process** with isolation and limits applied:
- **Namespaces = isolation (what the process can SEE):** `pid` (own process tree, the app is PID 1), `net` (own interfaces/IP/ports), `mnt` (own filesystem view), `uts` (own hostname), `ipc` (shared memory), `user` (maps container root to an unprivileged host user), `cgroup`, `time`.
- **cgroups = limits (what the process can USE):** CPU, memory, block I/O, number of PIDs. **cgroup v2** is now the standard (Kubernetes v1.35 dropped cgroup v1).
- **Union filesystem (OverlayFS):** read-only image layers stacked together + a thin **writable container layer** (copy-on-write).
- **Extra security layers:** Linux capabilities, seccomp (system call filtering), AppArmor/SELinux.

### 9.3 Docker architecture ⭐⭐

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

- **OCI** (Open Container Initiative) standardizes image, runtime and distribution formats, so images built with Docker run on containerd, CRI-O, Podman and Kubernetes.
- **Kubernetes no longer uses Docker Engine** (dockershim was removed in v1.24). The kubelet talks to **containerd** or **CRI-O** through the **CRI**. Docker-built images still work because they are OCI images.

### 9.4 Image vs container

- **Image** = read-only template (a stack of layers + metadata such as the default command). Think **class**.
- **Container** = a running (or stopped) instance of an image with its own writable layer. Think **object**.
- Image reference: `registry/repository:tag` (a mutable label) vs `@sha256:<digest>` (immutable content hash).

### 9.5 Dockerfile instructions and the classic differences ⭐⭐⭐

| Instruction | Purpose |
|---|---|
| `FROM` | Base image (each build stage starts with one) |
| `WORKDIR` | Set the working directory |
| `COPY` / `ADD` | Copy files into the image |
| `RUN` | Execute a command **at build time** (creates a layer) |
| `ENV` | Environment variable available at build **and run** time |
| `ARG` | Build-time variable (`--build-arg`); not available at runtime |
| `EXPOSE` | **Documents** a port; does not publish it |
| `USER` | User to run as |
| `CMD` | Default command/arguments **when the container starts** |
| `ENTRYPOINT` | Fixed executable when the container starts |
| `HEALTHCHECK` | Command Docker runs to check container health |
| `VOLUME`, `LABEL`, `STOPSIGNAL`, `SHELL`, `ONBUILD` | Miscellaneous |

| Question | Answer |
|---|---|
| **CMD vs ENTRYPOINT** | `ENTRYPOINT` is the executable that always runs; `CMD` provides default arguments (or a default command) that are **easily overridden** by `docker run image <args>`. Combined: `ENTRYPOINT ["python","app.py"]` + `CMD ["--port","8000"]`. Override the entrypoint with `--entrypoint`. |
| **Exec form vs shell form** | `CMD ["node","server.js"]` (exec form) runs the app as **PID 1**, so it **receives SIGTERM** and can shut down gracefully. `CMD node server.js` (shell form) wraps it in `/bin/sh -c`, which may not forward signals, so stops are slow or forced. Prefer exec form. |
| **COPY vs ADD** | `COPY` just copies local files. `ADD` can also auto-extract local tar archives and fetch URLs, which is less predictable. Prefer `COPY`. |
| **ARG vs ENV** | `ARG` exists only during the build; `ENV` persists into running containers. Neither is safe for secrets (visible in image history); use BuildKit secrets: `RUN --mount=type=secret,id=npm_token ...` |
| **RUN vs CMD** | `RUN` executes while **building** the image; `CMD` executes when the **container starts**. |
| **EXPOSE vs `-p`** | `EXPOSE` is documentation; `-p 8080:80` actually publishes host port 8080 to container port 80. |

### 9.6 Layer caching ⭐⭐

Each instruction creates a layer. Docker reuses cached layers **until the first instruction whose inputs changed**; everything after it is rebuilt. Order instructions from **least to most frequently changing**:

```dockerfile
# Bad: any source change re-installs every dependency
COPY . .
RUN npm ci

# Good: dependencies stay cached until package*.json changes
COPY package*.json ./
RUN npm ci
COPY . .
```

### 9.7 Multi-stage builds ⭐⭐

Build with a full SDK image, then copy only the output into a small runtime image: **smaller, faster to pull, more secure** (no compilers or source code in production).

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
(`-XX:MaxRAMPercentage` sizes the JVM heap relative to the container's memory limit, which helps avoid OOMKills.)

### 9.8 Image optimization and security best practices ⭐⭐

1. Small base images: `alpine`, `-slim`, **distroless**, `scratch` (for static binaries), Chainguard/Wolfi images.
2. **Multi-stage builds.**
3. **`.dockerignore`**: exclude `.git`, `node_modules`, `.env`, test data, secrets.
4. Install and clean up in the same layer: `apt-get update && apt-get install -y --no-install-recommends pkg && rm -rf /var/lib/apt/lists/*`.
5. Pin base image versions (ideally digests) and rebuild regularly to pick up security patches.
6. Run as **non-root** (`USER`), drop capabilities, use a read-only root filesystem where possible.
7. Never bake secrets into images (they stay in layer history even if deleted in a later layer).
8. One main process per container; log to **stdout/stderr**.
9. **Scan** images (Trivy, Grype, Docker Scout, ECR scanning) and **sign** them (cosign).
10. Add a `HEALTHCHECK` for Docker/Compose (Kubernetes uses its own probes instead).

### 9.9 Docker networking ⭐⭐

| Driver | Description | Use case |
|---|---|---|
| `bridge` (default) | Private network on the host (`docker0`); containers get internal IPs; publish ports with `-p` | Single-host apps |
| User-defined bridge | Same, **plus automatic DNS by container name** and better isolation | Multi-container apps on one host (Compose does this) |
| `host` | Shares the host's network stack (no isolation, no `-p` needed) | Maximum network performance (Linux) |
| `none` | No networking | Isolated batch jobs |
| `overlay` | Spans multiple hosts | Docker Swarm |
| `macvlan` | Container gets its own MAC/IP on the physical LAN | Legacy apps that need to appear on the LAN |

```bash
docker network create appnet
docker run -d --name db  --network appnet -e POSTGRES_PASSWORD=secret postgres:16
docker run -d --name api --network appnet -p 8080:3000 myapi:1.0   # api reaches the DB at host "db"
```

### 9.10 Docker storage ⭐⭐

The container's writable layer is **ephemeral**: data disappears when the container is removed.

| Type | What it is | Use |
|---|---|---|
| **Volume** | Managed by Docker (`/var/lib/docker/volumes`), e.g. `-v pgdata:/var/lib/postgresql/data` | Persistent data such as databases (preferred) |
| **Bind mount** | A host path mapped in, e.g. `-v /home/me/src:/app` | Local development, injecting config files |
| **tmpfs** | In-memory only | Sensitive or scratch data |

### 9.11 Docker Compose

Define multi-container apps in `compose.yaml`; run with `docker compose up -d`, `docker compose logs -f`, `docker compose down -v`. (Compose v2 is the `docker compose` plugin; the old Python `docker-compose` v1 is deprecated.)

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

### 9.12 Commands cheat sheet ⭐

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
docker system df; docker system prune -a    # reclaim disk (careful on shared hosts)
docker network ls; docker volume ls
```

**More classic differences** ⭐

| Question | Answer |
|---|---|
| `docker stop` vs `docker kill` | `stop` sends SIGTERM, waits (10 s by default), then SIGKILL. `kill` sends SIGKILL immediately by default. |
| `docker save` vs `docker export` | `save` writes an **image** with all layers and history (`docker load` to import). `export` writes a **container's filesystem**, flattened, without history (`docker import`). |
| `docker exec` vs `docker attach` | `exec` starts a new process inside the container (e.g. a shell). `attach` connects to the main process's stdin/stdout (Ctrl+C may stop it). |
| `docker run` vs `docker create` + `start` | `run` = create + start in one step. |
| Restart policies | `no`, `on-failure[:N]`, `always`, `unless-stopped`. |
| `docker commit` | Creates an image from a container's changes; avoid it for production (not reproducible). Use Dockerfiles. |

### 9.13 Troubleshooting containers ⭐⭐

| Symptom | Likely cause | What to check |
|---|---|---|
| Container exits immediately | The main process finished or crashed (a container lives only as long as PID 1) | `docker logs`, `docker inspect --format '{{.State.ExitCode}} {{.State.OOMKilled}}' <name>`, run it with `-it ... sh`, make sure the process runs in the foreground (e.g. `nginx -g 'daemon off;'`) |
| Exit code **137** | SIGKILL, often **out of memory** | `OOMKilled: true` in inspect; raise `--memory` or fix the leak |
| Exit 127 / 126 | Command not found / not executable | Typo in CMD, binary missing from a slim image, missing `chmod +x`, Windows (CRLF) line endings in scripts |
| App unreachable from the host | Port not published, or the app listens on **127.0.0.1 inside the container** | `docker port`, bind the app to `0.0.0.0`, check host firewall |
| Containers can't resolve each other by name | They're on the default bridge network | Use a user-defined network or Compose |
| Host disk full | Old images/containers, build cache, huge JSON log files | `docker system df`, prune, configure log rotation (`max-size`, `max-file`) |
| Permission denied on a volume | UID/GID mismatch between the container user and host files | Align UID/GID, `chown`, or use named volumes |

### 9.14 Container security checklist

Non-root user, minimal and patched base images, scanning in CI, signed images, no `--privileged`, `--cap-drop ALL` (add back only what's needed), read-only filesystem, resource limits (`--memory`, `--cpus`), **never mount `/var/run/docker.sock` into untrusted containers** (that's root on the host), secrets as mounted files rather than env vars, rootless Docker or **Podman** (daemonless, rootless by default). To build images in Kubernetes-based CI without a Docker daemon, use BuildKit (rootless mode) or Buildah.

### 9.15 Docker interview Q&A

**Q: Docker vs Kubernetes?** Docker builds and runs containers on one host. Kubernetes orchestrates containers across a cluster: scheduling, scaling, self-healing, service discovery, rolling updates.

**Q: How would you shrink a 1.2 GB image?** Multi-stage build, slim/distroless base, `.dockerignore`, production-only dependencies, clean package caches in the same layer, inspect layers with `docker history` or `dive`.

**Q: Can a container run a different kernel than the host?** No. Containers share the host kernel; that's why Linux containers on macOS/Windows run inside a lightweight Linux VM.

**Q: What is a dangling image?** An untagged image (`<none>:<none>`) left behind after rebuilds; remove with `docker image prune`.

**Q: How do you pass secrets to a container?** As mounted files from a secret manager (Docker/Compose secrets, Kubernetes Secrets as volumes, Vault agent), never as `ENV` in the Dockerfile.

**Q: Why does my container take 10 seconds to stop?** The app isn't handling SIGTERM (often because of shell-form `CMD`), so Docker waits for the timeout and sends SIGKILL. Use exec form and handle SIGTERM, or use an init like `tini` (`docker run --init`).

---

## 10. Kubernetes

> Current release: **v1.37 "Garhwal"** (Aug 2026). Kubernetes ships about 3 minor releases a year; the latest 3 minor versions are supported with roughly 14 months of patches each.

### 10.1 What and why

Kubernetes (K8s) is an open-source **container orchestration** platform (born from Google's Borg, now a CNCF project) that automates **deployment, scaling, self-healing, service discovery, load balancing, rolling updates, config/secret management and storage**.

**The core idea, declarative desired state + reconciliation** ⭐⭐: you declare *what* you want (e.g. "3 replicas of image v2") in YAML; **controllers** run continuous control loops that compare **desired state** (stored in etcd) with **actual state** and act to close the gap. This loop is why Kubernetes self-heals.

### 10.2 Architecture ⭐⭐⭐

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

**Control plane components**

| Component | Role |
|---|---|
| **kube-apiserver** | Front door of the cluster (REST API). Every request goes through **authentication -> authorization (RBAC) -> admission control -> validation**, then is persisted to etcd. The **only** component that talks to etcd. Scales horizontally. |
| **etcd** | Distributed, strongly consistent key-value store (Raft consensus) holding all cluster state. Run an odd number of members (3 or 5) for quorum. **Back it up.** |
| **kube-scheduler** | Watches for unscheduled Pods and picks a node: **filtering** (enough resources? taints/tolerations, affinity, node selectors) then **scoring** (spread, utilization), then **binds** the Pod to the node. |
| **kube-controller-manager** | Runs built-in controllers: Node, ReplicaSet, Deployment, StatefulSet, Job, EndpointSlice, ServiceAccount, Namespace, ... Each is a reconcile loop. |
| **cloud-controller-manager** | Talks to the cloud provider: creates load balancers for `LoadBalancer` Services, manages node lifecycle and routes. |

**Node components**

| Component | Role |
|---|---|
| **kubelet** | Agent on every node. Receives Pod specs, starts containers through the **CRI**, mounts volumes, runs probes, reports Pod and node status. |
| **kube-proxy** | Implements Service virtual IPs using iptables, **nftables** (GA since v1.33) or IPVS (deprecated in v1.35). Some CNIs (e.g. Cilium with eBPF) replace it entirely. |
| **Container runtime** | containerd or CRI-O. |

### 10.3 What happens when you run `kubectl apply -f deployment.yaml`? ⭐⭐⭐

1. `kubectl` reads your kubeconfig and sends an HTTPS request to the **API server**.
2. API server: **authentication** (certificate/token/OIDC) -> **authorization** (RBAC) -> **mutating admission** -> schema validation -> **validating admission** -> stores the Deployment in **etcd**.
3. The **Deployment controller** notices the new Deployment (via a watch) and creates a **ReplicaSet**.
4. The **ReplicaSet controller** creates the required number of **Pod** objects (no node assigned yet, status `Pending`).
5. The **scheduler** sees the unscheduled Pods, filters and scores nodes, and **binds** each Pod to a node.
6. The **kubelet** on that node sees the Pod, pulls the image via the runtime, the **CNI** plugin assigns a Pod IP, the **CSI** driver mounts volumes, init containers run, then app containers start.
7. The kubelet runs the **probes** and reports status; the Pod becomes `Running` and `Ready`.
8. The **EndpointSlice controller** adds the ready Pod's IP to the Service's endpoints; **kube-proxy** updates its rules and traffic starts flowing.

### 10.4 Workload objects ⭐⭐

| Object | What it does | Typical use |
|---|---|---|
| **Pod** | Smallest deployable unit: one or more containers sharing a **network namespace** (same IP, talk over `localhost`) and volumes. Ephemeral. | Never create bare Pods in production |
| **ReplicaSet** | Keeps N identical Pods running (via a label selector) | Managed by Deployments |
| **Deployment** | Manages ReplicaSets: rolling updates, rollbacks, scaling | **Stateless** apps (web, APIs) |
| **StatefulSet** | Stable, unique Pod names (`db-0`, `db-1`), **stable per-Pod storage** (`volumeClaimTemplates`), ordered start/stop, stable DNS via a headless Service | Databases, Kafka, ZooKeeper, Elasticsearch |
| **DaemonSet** | One Pod on every (matching) node | Log collectors (Fluent Bit), node-exporter, CNI agents, kube-proxy |
| **Job** | Runs Pods to completion (`completions`, `parallelism`, `backoffLimit`) | Batch work, migrations |
| **CronJob** | Creates Jobs on a cron schedule | Backups, reports, cleanups |

**Deployment vs StatefulSet** ⭐⭐

| | Deployment | StatefulSet |
|---|---|---|
| Pod names | Random suffix (`web-7d9f8-abc12`) | Ordinal and stable (`db-0`, `db-1`) |
| Storage | Usually shared or none; replicas are interchangeable | Each Pod gets **its own PVC**, which follows it on reschedule |
| Ordering | Pods start/stop in parallel | Ordered create, scale and delete (by default) |
| Network identity | Through the Service only | Stable per-Pod DNS: `db-0.db.<namespace>.svc.cluster.local` |
| Use | Stateless | Stateful, clustered software |

**Namespaces** partition a cluster (names, RBAC, quotas, network policies). Defaults: `default`, `kube-system`, `kube-public`, `kube-node-lease`.
**Labels** are identifying key-values used by selectors (`app: web`); **annotations** are non-identifying metadata used by tools (`prometheus.io/scrape: "true"`).

### 10.5 A production-style Deployment + Service ⭐

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

Note on CPU limits: many teams set CPU **requests** but no CPU **limit** (to avoid throttling), and set memory limit = request for predictability. Be ready to discuss the trade-off.

### 10.6 Services ⭐⭐⭐

**Why:** Pods are ephemeral and their IPs change. A **Service** gives a **stable virtual IP and DNS name** and load-balances across all ready Pods matching its **label selector** (tracked in **EndpointSlices**; the old Endpoints API is deprecated since v1.33).

| Type | Behavior | Use |
|---|---|---|
| **ClusterIP** (default) | Internal virtual IP, reachable only inside the cluster | Service-to-service traffic |
| **NodePort** | Opens a static port (30000-32767) on **every node**, forwarding to the Service | Dev/test, or behind your own LB |
| **LoadBalancer** | Provisions a cloud load balancer (via cloud-controller-manager or an LB controller) -> NodePort/Pods | Exposing a single service externally |
| **ExternalName** | DNS CNAME to an external hostname (no proxying) | Reaching an external DB by an internal name |
| **Headless** (`clusterIP: None`) | No virtual IP; DNS returns the individual Pod IPs | StatefulSets, client-side load balancing |

- DNS name: `<service>.<namespace>.svc.cluster.local` (just `<service>` inside the same namespace).
- `port` = the Service's port; `targetPort` = the container's port; `nodePort` = the port opened on each node. ⭐

### 10.7 Ingress and Gateway API ⭐⭐ (2026 update)

**Ingress** = L7 HTTP(S) routing rules (host/path) to Services, plus TLS termination, through **one** entry point. It needs an **Ingress controller** to work (Traefik, HAProxy, Kong, NGINX Inc's controller, AWS Load Balancer Controller, cloud-native controllers, etc.). One shared entry point is much cheaper than a `LoadBalancer` Service per app.

> ⚠️ **2026 change:** the community **Ingress NGINX** controller (`kubernetes/ingress-nginx`) was **retired in March 2026**: no further releases, bug fixes or security patches. Existing installs keep working, but SIG Network recommends migrating to **Gateway API** or another maintained controller. Check whether you run it: `kubectl get pods -A -l app.kubernetes.io/name=ingress-nginx`.

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

**Gateway API ("Ingress 2.0")** splits responsibilities by role:
- **GatewayClass** (infrastructure provider: which implementation) -> **Gateway** (cluster operator: listeners, ports, TLS) -> **HTTPRoute / GRPCRoute / TLSRoute / TCPRoute** (app teams: routing rules).
- Built into the spec (no vendor annotations): **weighted traffic splitting** (canaries), header/query matching, redirects, rewrites, cross-namespace routing (controlled by `ReferenceGrant`). Portable across implementations (Envoy Gateway, Istio, Cilium, NGINX Gateway Fabric, Kong, Traefik, cloud LBs).

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

### 10.8 ConfigMaps and Secrets ⭐⭐

- **ConfigMap:** non-sensitive configuration (key-values or whole files). Consumed as env vars, command arguments or mounted files.
- **Secret:** sensitive data. ⚠️ Stored **base64-encoded, not encrypted** by default. Protect Secrets with: **encryption at rest** for etcd (EncryptionConfiguration / KMS provider), tight RBAC on `get/list secrets`, and external secret managers via **External Secrets Operator** or the **Secrets Store CSI driver** (Vault, AWS Secrets Manager, Azure Key Vault). In GitOps, use Sealed Secrets or SOPS if secrets must live in Git.
- Secret types: `Opaque`, `kubernetes.io/tls`, `kubernetes.io/dockerconfigjson` (for `imagePullSecrets`).
- **Update behavior:** mounted files update eventually (not when using `subPath`); **env vars never update** until the Pod restarts -> `kubectl rollout restart`, or a checksum annotation in Helm to trigger a rollout.
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

### 10.9 Storage ⭐⭐

| Concept | Meaning |
|---|---|
| **Volume** | Storage attached to a Pod: `emptyDir` (scratch space for the Pod's lifetime, shared between its containers), `configMap`/`secret`, `hostPath` (node path; avoid), `persistentVolumeClaim`, projected volumes |
| **PersistentVolume (PV)** | A piece of storage in the cluster (provisioned by an admin or dynamically) |
| **PersistentVolumeClaim (PVC)** | A user's **request** for storage (size, access mode, class) that binds to a PV |
| **StorageClass** | Describes a "type" of storage + provisioner (e.g. `ebs.csi.aws.com`) and enables **dynamic provisioning** |
| **CSI** | Container Storage Interface: the standard plugin API storage vendors implement |

Analogy: PV = a parking space, PVC = your request for a space, StorageClass = a parking lot that builds new spaces on demand.

- **Access modes:** `ReadWriteOnce` (RWO, one node), `ReadOnlyMany` (ROX), `ReadWriteMany` (RWX, many nodes, e.g. NFS/EFS), `ReadWriteOncePod` (RWOP, exactly one Pod).
- **Reclaim policy:** `Delete` (default for dynamic volumes; deletes the disk with the PVC) or `Retain` (keeps the disk/data for manual recovery). Use `Retain` for important data.
- `volumeBindingMode: WaitForFirstConsumer` creates the disk in the same zone where the Pod gets scheduled (important for zonal disks like EBS).

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

### 10.10 Health probes ⭐⭐⭐

| Probe | Question it answers | On failure |
|---|---|---|
| **Liveness** | "Is the container alive, or stuck?" | kubelet **restarts** the container |
| **Readiness** | "Can it serve traffic right now?" | Pod is **removed from Service endpoints** (no restart) |
| **Startup** | "Has a slow app finished starting?" | Liveness/readiness are held off until it succeeds; if it never succeeds, the container is restarted |

- Mechanisms: `httpGet`, `tcpSocket`, `exec`, `grpc`. Tuning: `initialDelaySeconds`, `periodSeconds`, `timeoutSeconds`, `failureThreshold`, `successThreshold`.
- ⚠️ Don't check external dependencies (DB, other services) in **liveness**: if the DB blips, every Pod restarts at once (cascading failure). Dependency checks belong in readiness, if anywhere.
- Slow-starting apps (big JVMs) need a **startup probe**, otherwise liveness kills them before they finish booting (looks like CrashLoopBackOff).

### 10.11 Resources and QoS ⭐⭐

- **requests** = guaranteed amount; the **scheduler** places Pods based on requests.
- **limits** = maximum allowed. CPU above the limit is **throttled** (CPU is compressible). Memory above the limit gets the container **OOMKilled** (exit code **137**; memory is incompressible).
- Units: CPU in cores or millicores (`500m` = half a core); memory in `Mi`/`Gi`.

| QoS class | Condition | Eviction order under node pressure |
|---|---|---|
| **Guaranteed** | Every container has CPU and memory requests **equal** to limits | Evicted last |
| **Burstable** | At least one request/limit set, but not Guaranteed | Middle |
| **BestEffort** | No requests or limits at all | Evicted first |

- **ResourceQuota** caps the total for a namespace; **LimitRange** sets defaults and min/max per Pod/container.
- **In-place Pod resize** (GA since v1.35): change CPU/memory of a running Pod without recreating it (through the `resize` subresource).

### 10.12 Autoscaling ⭐⭐

| Autoscaler | Scales | Notes |
|---|---|---|
| **HPA** (Horizontal Pod Autoscaler) | Number of **Pods** | CPU/memory via metrics-server, or custom/external metrics (Prometheus Adapter, KEDA). Needs **requests** set to compute utilization %. Scale-to-zero support reached beta in v1.37. |
| **VPA** (Vertical Pod Autoscaler) | **Requests/limits** of Pods | Great in recommendation mode; don't let VPA and HPA fight over the same CPU/memory metric |
| **Cluster Autoscaler** | Number of **nodes** (in node groups) | Adds nodes when Pods are `Pending` for lack of capacity; removes underused nodes |
| **Karpenter** | **Nodes**, provisioned directly and right-sized | Picks instance types to fit pending Pods quickly; consolidates to save cost |
| **KEDA** | Pods, based on **events** (queue length, Kafka lag, cron) | Can scale to zero |

**HPA formula** ⭐: `desiredReplicas = ceil(currentReplicas x currentMetricValue / targetMetricValue)`
Example: 4 Pods at 90% CPU with target 60% -> ceil(4 x 90 / 60) = **6 Pods**.

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

### 10.13 Controlling where Pods run ⭐⭐

- **nodeSelector:** simplest; the node must carry the given labels.
- **Node affinity:** richer rules; `required...` (hard) or `preferred...` (soft), with `In`, `NotIn`, `Exists` operators.
- **Pod affinity / anti-affinity:** place Pods near or away from other Pods (e.g. spread replicas across nodes using `topologyKey: kubernetes.io/hostname`).
- **Taints and tolerations** ⭐⭐: a **taint** on a node **repels** Pods; a **toleration** on a Pod **allows** (but doesn't force) it onto that node. Effects: `NoSchedule`, `PreferNoSchedule`, `NoExecute` (also evicts running Pods). To **dedicate** nodes (e.g. GPUs), combine a taint + toleration **and** node affinity.
- **Topology spread constraints:** spread Pods evenly across zones/nodes (`maxSkew`).
- **PriorityClass:** important Pods can preempt less important ones.
- **PodDisruptionBudget (PDB):** limits how many Pods can be down during **voluntary** disruptions (node drains, upgrades).
- `kubectl cordon` (mark unschedulable) vs `kubectl drain` (cordon + evict Pods, respecting PDBs) vs `kubectl uncordon`.

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

### 10.14 Pod lifecycle, init and sidecar containers, graceful shutdown ⭐

- **Pod phases:** `Pending`, `Running`, `Succeeded`, `Failed`, `Unknown`.
- **Container states:** `Waiting` (reasons: `ContainerCreating`, `CrashLoopBackOff`, `ImagePullBackOff`, `ErrImagePull`, `CreateContainerConfigError`), `Running`, `Terminated` (reasons: `Completed`, `Error`, `OOMKilled`).
- `restartPolicy`: `Always` (Deployments), `OnFailure` / `Never` (Jobs).
- **Init containers** run one by one, to completion, before app containers (wait for a dependency, run migrations, fetch config).
- **Sidecar containers** run alongside the app (log shipper, proxy). **Native sidecars** (GA since v1.33) are init containers with `restartPolicy: Always`: they start before and stop after the main containers and don't block Jobs from completing.
- **Ephemeral containers:** `kubectl debug` attaches a debugging container to a running Pod (handy for distroless images).
- **Static Pods:** managed directly by the kubelet from `/etc/kubernetes/manifests` (how kubeadm runs control-plane components).

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

**Graceful termination sequence** ⭐⭐
1. Pod is marked `Terminating` and is **removed from Service endpoints** (in parallel with the next steps).
2. The `preStop` hook runs (e.g. `sleep 5` so load balancers stop sending traffic).
3. **SIGTERM** is sent to the containers; the app should stop accepting work and finish in-flight requests.
4. After `terminationGracePeriodSeconds` (default **30 s**), any remaining processes get **SIGKILL**.

### 10.15 Security: RBAC, ServiceAccounts, Pod Security ⭐⭐

- Request flow: **Authentication** (who are you? certs, tokens, OIDC) -> **Authorization** (are you allowed? RBAC) -> **Admission control** (mutate/validate: Pod Security Admission, ValidatingAdmissionPolicy (CEL), OPA Gatekeeper, Kyverno).
- **RBAC objects:**

| Object | Scope | Purpose |
|---|---|---|
| Role | Namespace | Set of permissions (verbs on resources) |
| ClusterRole | Cluster-wide | Permissions for cluster-scoped resources, or reusable across namespaces |
| RoleBinding | Namespace | Grants a Role or ClusterRole to subjects within one namespace |
| ClusterRoleBinding | Cluster-wide | Grants a ClusterRole across the whole cluster |

Subjects are **Users**, **Groups** or **ServiceAccounts** (the identity Pods use). ServiceAccount tokens are short-lived, projected tokens. Map them to cloud IAM with **EKS Pod Identity / IRSA**, GKE Workload Identity or Azure Workload Identity, so Pods never need static cloud keys.

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
Test permissions: `kubectl auth can-i list pods -n dev --as=system:serviceaccount:dev:ci-bot`

- **Pod Security Standards:** `privileged`, `baseline`, `restricted`, enforced per namespace by **Pod Security Admission** labels (e.g. `pod-security.kubernetes.io/enforce: restricted`). PodSecurityPolicy was removed in v1.25.
- **securityContext essentials:** `runAsNonRoot`, `allowPrivilegeEscalation: false`, `readOnlyRootFilesystem: true`, drop `ALL` capabilities, `seccompProfile: RuntimeDefault`. **User namespaces** (`hostUsers: false`) are GA as of v1.36 and map container root to an unprivileged host user.

### 10.16 Networking model and NetworkPolicy ⭐⭐

**Kubernetes networking rules:**
1. Every Pod gets its own IP, unique across the cluster.
2. All Pods can reach all other Pods **without NAT** (flat network).
3. Containers inside the same Pod share the network namespace and talk via `localhost`.

**Four kinds of communication:**

| Path | Handled by |
|---|---|
| Container <-> container (same Pod) | `localhost` |
| Pod <-> Pod (any node) | **CNI plugin** (VXLAN overlay like Flannel, BGP routing like Calico, eBPF like Cilium, or VPC-native IPs like the AWS VPC CNI) |
| Pod -> Service | **kube-proxy** rules (DNAT ClusterIP -> Pod IPs) + **CoreDNS** for names |
| External -> Service | NodePort / LoadBalancer / Ingress / Gateway |

**NetworkPolicy** = firewall rules for Pods (ingress/egress) using Pod selectors, namespace selectors and CIDRs. By default **everything can talk to everything**. Policies only work if the CNI enforces them (Calico, Cilium yes; plain Flannel no). Best practice: **default deny**, then allow explicitly.

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
⚠️ Gotcha: after a default-deny **egress** policy, allow DNS (UDP/TCP 53 to CoreDNS) or nothing can resolve names.

### 10.17 Operators and CRDs ⭐

- A **CRD** (CustomResourceDefinition) adds a new resource type to the Kubernetes API (e.g. `Certificate`, `PostgresCluster`, `KafkaTopic`).
- An **Operator** = CRDs + a custom controller that encodes human operational knowledge (install, upgrade, backup, failover) for complex software. Examples: Prometheus Operator, cert-manager, CloudNativePG, Strimzi (Kafka). Built with Kubebuilder or Operator SDK.

### 10.18 Service mesh

Istio, Linkerd, Cilium service mesh: an infrastructure layer for service-to-service traffic providing **mTLS**, traffic management (retries, timeouts, circuit breaking, canary splits) and observability. Data plane = sidecar proxies (Envoy) or sidecar-less "ambient" mode; control plane configures them. Worth it when many services need consistent security and traffic policy; it adds operational complexity.

### 10.19 Managed Kubernetes, upgrades and etcd backup ⭐

- **Managed:** EKS (AWS), AKS (Azure), GKE (Google). The provider runs the control plane (HA API servers, etcd, upgrades); you manage worker nodes (or use serverless options like Fargate / GKE Autopilot). **Self-managed:** kubeadm, kops, RKE2, k3s (lightweight). **Local:** kind, minikube, k3d, Docker Desktop.
- **Upgrade process** ⭐: read release notes; find deprecated APIs (`pluto`, `kubent`); back up etcd; upgrade the **control plane first**, **one minor version at a time**; then upgrade node pools (surge/blue-green node groups or cordon + drain, respecting PDBs); update add-ons (CNI, CoreDNS, kube-proxy, CSI drivers). Version skew: kubelets may be up to 3 minor versions older than the API server, never newer.

```bash
# etcd snapshot (kubeadm-style certificate paths)
ETCDCTL_API=3 etcdctl snapshot save /backup/etcd-$(date +%F).db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key
# restore with: etcdutl snapshot restore <file> --data-dir <new-dir>   (older: etcdctl snapshot restore)
```
For application-level backups (namespaces, PVs) use **Velero**.

### 10.20 kubectl cheat sheet ⭐⭐

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

### 10.21 Kubernetes troubleshooting ⭐⭐⭐

**General flow:** `kubectl get` -> `kubectl describe` (read the **Events**) -> `kubectl logs` (`--previous`) -> `exec`/`debug` into the Pod -> check the node -> check Services/DNS/NetworkPolicies -> check **what changed recently** (rollout history, Git, CI).

| Symptom | Common causes | How to confirm / fix |
|---|---|---|
| **Pending** | Not enough CPU/memory for the **requests**, nodeSelector/affinity matches no node, taint without toleration, PVC not bound, quota exceeded | `describe pod` events (e.g. "0/5 nodes are available: insufficient cpu"); lower requests, add nodes (Cluster Autoscaler/Karpenter), fix the PVC/StorageClass |
| **ImagePullBackOff / ErrImagePull** | Wrong image name/tag, private registry without `imagePullSecrets` or IAM permission, registry rate limits, no network route to the registry | `describe pod`; try `docker pull` with the same name; fix credentials |
| **CrashLoopBackOff** | App crashes on start: bad config or env var, missing Secret/ConfigMap key, failing liveness probe, wrong command, permission issue, dependency down | `kubectl logs --previous`, check the exit code in `describe`, run the image locally, temporarily override the command with `sleep` to debug inside |
| **OOMKilled** (exit 137) | Memory limit too low, or a leak | Raise the limit, profile the app, align JVM heap (`MaxRAMPercentage`) with the limit |
| **CreateContainerConfigError** | Referenced ConfigMap/Secret or key doesn't exist | `describe pod`, create or fix the reference |
| **Running but not Ready** | Readiness probe failing (wrong path/port, slow start, dependency) | `describe pod` probe failures; test the endpoint with `exec` + `curl` |
| **Service unreachable** | Service selector doesn't match Pod labels ⭐, wrong `targetPort`, no ready Pods, NetworkPolicy blocking, app bound to 127.0.0.1, DNS problem | `kubectl get endpointslices -l kubernetes.io/service-name=<svc>` (empty = selector/readiness issue), `nslookup <svc>` from a debug Pod |
| **Node NotReady** | kubelet stopped, disk/memory/PID pressure, CNI broken, expired certificates | `describe node` (Conditions), `systemctl status kubelet`, `journalctl -u kubelet` |
| **Evicted Pods** | Node pressure (disk/ephemeral storage, memory) | Node conditions; set requests/limits for ephemeral storage; clean disks |
| **Stuck Terminating** | Finalizers waiting on a controller, unreachable node | Inspect `metadata.finalizers`; force delete only as a last resort (`--grace-period=0 --force`) |

### 10.22 Kubernetes interview Q&A ⭐⭐⭐

**Q: Why not create Pods directly?** A bare Pod isn't recreated if it dies or its node fails. A Deployment (through its ReplicaSet) keeps the replica count, does rolling updates and supports rollback.

**Q: How does Kubernetes self-heal, and what are its limits?** Controllers constantly reconcile: kubelet restarts failed containers, ReplicaSets replace missing Pods, Pods from failed nodes are recreated elsewhere, unready Pods leave Service endpoints. Limits: it can't fix bad code or config (CrashLoopBackOff), it needs spare capacity, and stateful data needs careful handling.

**Q: What happens if the control plane or etcd goes down?** Running Pods keep running and serving traffic, but nothing can be scheduled, scaled, healed or changed, and `kubectl` fails. Hence HA control planes (3+ etcd members) and regular etcd backups.

**Q: What happens when a worker node dies?** The node controller marks it `NotReady`/`Unknown` after its grace period (tens of seconds). Pods are evicted after their not-ready/unreachable tolerations expire (default 300 s), and ReplicaSets recreate them on healthy nodes. StatefulSet Pods are **not** replaced until the old Pod is confirmed gone (to avoid two Pods writing the same data).

**Q: When would you run multiple containers in one Pod?** Only for tightly coupled helpers that share network/storage: sidecars (proxies, log shippers), adapters, init containers. Otherwise, one main container per Pod.

**Q: Ingress (or Gateway) vs a LoadBalancer Service?** A LoadBalancer Service creates one cloud LB per Service (L4, costly at scale). Ingress/Gateway gives one L7 entry point that routes many hosts/paths to many Services with TLS.

**Q: `kubectl apply` vs `kubectl create`?** `create` is imperative and fails if the object exists. `apply` is declarative create-or-update (client-side last-applied annotation or server-side apply), ideal for Git-driven workflows.

**Q: How do you achieve zero-downtime deployments in K8s?** RollingUpdate with `maxUnavailable: 0`, readiness probes, `preStop` hook + graceful SIGTERM handling, PDBs, multiple replicas spread across nodes/zones, backward-compatible changes.

**Q: How do you secure a cluster?** ⭐⭐ Least-privilege RBAC, no cluster-admin for apps, private API endpoint + SSO, Pod Security Admission (`restricted`), NetworkPolicies (default deny), Secrets encryption + external secret managers, image scanning and signature verification at admission, policy engines (Kyverno/Gatekeeper), audit logging, runtime detection (Falco), regular upgrades, CIS benchmark checks (kube-bench).

**Q: How does a Pod on node A reach a Pod on node B?** Each Pod has a cluster-routable IP; the CNI plugin routes traffic between nodes (overlay tunnels, BGP routes, eBPF or VPC-native routing) without NAT.

**Q: What is a headless Service?** A Service with `clusterIP: None`; DNS returns individual Pod IPs. StatefulSets use it to give each Pod a stable DNS name (`db-0.db.ns.svc.cluster.local`).

**Q: How do two containers in a Pod share files?** Mount the same `emptyDir` volume in both.

**Q: What are finalizers?** Keys on an object that block its deletion until a controller finishes cleanup (e.g. deleting a cloud load balancer, PVC protection).

**Q: DaemonSet vs Deployment?** A DaemonSet runs exactly one Pod per (matching) node and follows nodes as they join/leave; a Deployment runs N replicas anywhere the scheduler chooses.

---

## 11. Helm

### 11.1 Concepts ⭐

Helm is the **package manager for Kubernetes**.
- **Chart:** a package of templated Kubernetes manifests.
- **Release:** an installed instance of a chart in a namespace (you can install the same chart many times).
- **Revision:** each install/upgrade/rollback creates a new numbered revision.
- **Values:** configuration injected into templates (`values.yaml`, `-f values-prod.yaml`, `--set key=value`).
- **Repository:** where charts are stored: classic HTTP repos or **OCI registries** (ECR, GHCR, Harbor).
- Helm 3 removed **Tiller** (Helm 2's in-cluster server component), improving security. Release data is stored as Secrets in the release's namespace.

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

### 11.2 Commands ⭐

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
- **Trigger a rollout when a ConfigMap changes:** add an annotation such as `checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}` to the Pod template.

### 11.3 Helm 4 (released Nov 2025) ⭐

First major version in 6 years; Helm 3 charts (`apiVersion: v2`) work unchanged, and Helm 3 is being phased out.
- **Server-side apply** by default for **new** installs (existing releases keep their previous apply method).
- **New plugin system** with optional **WebAssembly** plugins; post-renderers are now plugins.
- `--wait` uses **kstatus** for richer readiness checks; OCI digest-based installs; multi-document values files.
- CLI changes that break old CI scripts: `--atomic` -> `--rollback-on-failure`, `--force` -> `--force-replace` (old names still work with deprecation warnings); `helm registry login` takes a hostname, not a full URL.

### 11.4 Helm vs Kustomize ⭐

| | Helm | Kustomize |
|---|---|---|
| Approach | Templates + values | Template-free **overlays/patches** on plain YAML |
| Packaging/versioning | Yes (charts, repos, releases, rollback) | No packaging; built into `kubectl apply -k` |
| Best for | Distributing reusable apps, many config knobs | Small per-environment differences of your own manifests |

Many teams use both (render a Helm chart, patch with Kustomize), and GitOps tools support both.

---

## 12. GitOps with Argo CD and Flux

### 12.1 What GitOps is ⭐⭐

GitOps = an operating model where **Git is the single source of truth for the desired state** of your systems, and an **agent inside the cluster continuously pulls and reconciles** the live state to match Git.

**OpenGitOps principles:** 1) **Declarative**, 2) **Versioned and immutable**, 3) **Pulled automatically**, 4) **Continuously reconciled**.

| | Push-based CD (classic) | Pull-based GitOps |
|---|---|---|
| Who deploys | CI server runs `kubectl`/`helm` against the cluster | In-cluster agent (Argo CD/Flux) syncs from Git |
| Cluster credentials | Stored in CI (bigger attack surface) | Stay inside the cluster |
| Drift | Not detected | Detected and auto-corrected (self-heal) |
| Audit / rollback | CI logs | Git history; rollback = `git revert` |

### 12.2 Typical GitOps flow ⭐

1. Developer merges code -> **CI** builds, tests, scans and pushes `myapp:<git-sha>`.
2. CI (or a bot like Argo CD Image Updater / Flux image automation) opens a PR to the **config repo** updating the image tag in `envs/staging`.
3. **Argo CD** detects the change and syncs it to the staging cluster.
4. Promotion to prod = a reviewed PR changing `envs/prod`.
5. Rollback = revert the commit.

Best practices: separate **app repo** and **config repo**; use per-environment **folders/overlays**, not per-environment branches; keep secrets out of Git (External Secrets Operator) or encrypted (Sealed Secrets, SOPS).

### 12.3 Argo CD

- **Application** CRD: source (repo, path, revision) -> destination (cluster, namespace).
- **Sync** (manual or automated), **prune** (delete resources removed from Git), **self-heal** (revert manual changes), **sync waves/hooks** for ordering, health assessment, UI, SSO, RBAC, multi-cluster.
- **ApplicationSet:** generate many Applications from one template (per cluster, per environment, per directory).
- **Argo Rollouts:** progressive delivery (canary/blue-green with automated metric analysis).

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

**Flux** (CNCF graduated, like Argo CD): a toolkit of controllers with CRDs such as `GitRepository`, `Kustomization`, `HelmRelease`, plus image automation. CLI-first and lightweight; Argo CD is UI-rich.

**Q: Someone ran `kubectl edit` in prod on a GitOps-managed app. What happens?** Argo CD marks the app **OutOfSync**; with self-heal on, it reverts the change to match Git. The right fix is a PR to the config repo.

---

## 13. Terraform and Infrastructure as Code

### 13.1 IaC concepts ⭐⭐

- **Infrastructure as Code** = defining infrastructure in version-controlled, machine-readable files instead of clicking in consoles. Benefits: repeatable environments, code review, audit history, speed, consistency, disaster recovery, self-documentation.
- **Declarative** (describe the end state: Terraform, CloudFormation, Kubernetes YAML) vs **imperative** (describe the steps: shell scripts, CLI commands). Ansible is in between: ordered tasks built from mostly declarative, idempotent modules.
- **Mutable infrastructure** (update servers in place -> config drift, "snowflake" servers) vs **immutable infrastructure** (replace with a new image/container; golden images via Packer).
- **Idempotency** ⭐: running the same code many times produces the same result, with no unintended changes after the first run.
- **Configuration drift** ⭐: real infrastructure diverges from code (usually manual console changes). Detect with `terraform plan`; prevent by limiting console write access.

### 13.2 Terraform basics ⭐⭐

- IaC tool by HashiCorp using **HCL**; works with any API through **providers** (AWS, Azure, GCP, Kubernetes, GitHub, Datadog, Cloudflare, thousands more).
- **Licensing in 2026:** Terraform moved from MPL 2.0 to the **Business Source License 1.1** in Aug 2023 (from v1.6) and HashiCorp is now part of **IBM** (deal closed Feb 2025). **OpenTofu** is the community fork under MPL 2.0, governed by the Linux Foundation (CNCF sandbox), with the `tofu` CLI as a near drop-in replacement. The tools have started to diverge (OpenTofu: native state encryption; Terraform: ephemeral values, Stacks). For internal use the BSL rarely matters; **the concepts below apply to both**.

**Core workflow** ⭐⭐
```bash
terraform init        # download providers/modules, configure the backend
terraform fmt         # format code (fmt -check in CI)
terraform validate    # syntax and internal consistency
terraform plan -out=tfplan   # preview: + create, ~ update in place, -/+ replace, - destroy
terraform apply tfplan       # apply exactly what was reviewed
terraform destroy            # delete everything in this configuration
```

**Building blocks**

| Block | Purpose |
|---|---|
| `terraform {}` | Required Terraform version, required providers, backend |
| `provider` | Configures a provider (region, credentials, default tags) |
| `resource` | Something Terraform creates and manages |
| `data` | **Reads** existing information (an AMI ID, an existing VPC) without managing it |
| `variable` | Input parameter (types, defaults, validation, `sensitive`) |
| `output` | Value exported after apply (or to other configurations/modules) |
| `locals` | Named expressions to avoid repetition |
| `module` | Reusable group of resources |

**Variable precedence (lowest -> highest)** ⭐: default in the `variable` block -> `TF_VAR_name` environment variables -> `terraform.tfvars` -> `terraform.tfvars.json` -> `*.auto.tfvars` (alphabetical) -> `-var` / `-var-file` on the command line (last one wins).

### 13.3 State ⭐⭐⭐ (the most important Terraform topic)

- **`terraform.tfstate`** is a JSON file mapping every resource in your code to the **real-world object** (IDs, attributes). Terraform uses it to compute diffs, track dependencies and metadata, and avoid querying everything every time.
- ⚠️ State can contain **secrets in plain text**: never commit it to Git; encrypt and restrict access.
- **Remote backends** (S3, Azure Blob, GCS, HCP Terraform, ...) give teams a shared, durable, access-controlled state.
- **State locking** prevents two people/pipelines from applying at the same time and corrupting state. With S3, Terraform 1.10+ supports **native S3 locking** (`use_lockfile = true`); the older DynamoDB-table locking is deprecated. A stale lock can be cleared with `terraform force-unlock <LOCK_ID>`, only after confirming nobody is running.
- Enable **versioning** on the state bucket so you can recover a previous state.
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

### 13.4 A realistic configuration ⭐

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

### 13.5 Must-know features ⭐⭐

- **`count` vs `for_each`** ⭐⭐: `count` makes N copies addressed by index (`aws_instance.web[0]`); removing an item from the middle **shifts indexes** and Terraform destroys/recreates the wrong resources. `for_each` uses map/set **keys** (`aws_instance.web["a"]`), so addresses stay stable. Use `count` for simple on/off: `count = var.create_bastion ? 1 : 0`.
- **Lifecycle meta-arguments:** `create_before_destroy`, `prevent_destroy` (protect databases), `ignore_changes`, `replace_triggered_by`; plus `precondition`/`postcondition` checks.
- **Dependencies:** implicit (one resource references another's attribute) or explicit `depends_on` when no reference exists. Terraform builds a **dependency graph (DAG)** and creates independent resources in parallel (default parallelism 10).
- **`dynamic` blocks** generate repeated nested blocks (e.g. many ingress rules from a list).
- **Modules** ⭐: root module + child modules; inputs (variables) -> outputs; sources: local path, Git, Terraform/OpenTofu registry, S3. Good modules are single-purpose, documented, versioned, have sensible defaults and don't configure providers themselves.
- **Workspaces:** several state files for one configuration (`terraform workspace new dev`). Many teams prefer **separate directories/backends per environment** (clearer isolation, smaller blast radius) or Terragrunt to reduce duplication.
- **Provisioners** (`local-exec`, `remote-exec`): last resort. Prefer `user_data`/cloud-init, Packer-baked images or Ansible.
- **Replacing a resource:** `terraform apply -replace='aws_instance.web["a"]'` (`terraform taint` is deprecated).
- **Refresh:** `terraform apply -refresh-only` updates state to match reality without changing infrastructure (`terraform refresh` is deprecated).
- **Import existing resources:** `import` blocks (+ `terraform plan -generate-config-out=generated.tf`) or `terraform import <address> <id>`.
- **Refactoring without destroying:** `moved { from = aws_instance.old  to = aws_instance.web }` or `terraform state mv`.
- **`.terraform.lock.hcl`** pins exact provider versions/checksums: **commit it**.
- Useful functions: `cidrsubnet`, `lookup`, `merge`, `concat`, `length`, `join`, `split`, `format`, `templatefile`, `file`, `try`, `coalesce`, `toset`, `jsonencode`.
- **Secrets:** never hardcode; pass via sensitive variables from a secret manager/CI, or read with data sources. `sensitive = true` only hides values in CLI output, **they are still in state**. Terraform's ephemeral values (1.10+) and OpenTofu's state encryption reduce exposure.

### 13.6 Terraform in CI/CD ⭐⭐

1. On a PR: `fmt -check`, `validate`, `tflint`, security scan (**Checkov** or **Trivy**, which absorbed tfsec), cost estimate (**Infracost**), then `plan -out=tfplan` posted as a PR comment.
2. Review the plan (watch for `-/+` replacements and `-` destroys on critical resources!).
3. After merge: `apply tfplan` (manual approval for prod), using **OIDC** credentials and remote state with locking.
4. Scheduled **drift detection**: `terraform plan -detailed-exitcode` (exit 0 = no changes, 1 = error, 2 = changes/drift) -> alert.
5. Policy as code: OPA/Conftest, Sentinel (HCP Terraform), Checkov custom policies.

Orchestration tools: Atlantis, HCP Terraform, Spacelift, env0, Scalr, or plain CI pipelines.

### 13.7 Terraform vs other tools ⭐

| Tool | Notes |
|---|---|
| **Terraform / OpenTofu** | Multi-cloud, HCL, explicit state, huge provider ecosystem |
| **CloudFormation** | AWS only, YAML/JSON, AWS manages state, StackSets, drift detection |
| **AWS CDK** | Real programming languages that synthesize CloudFormation |
| **Bicep / ARM** | Azure-native |
| **Pulumi** | IaC in Python/TypeScript/Go/C#, multi-cloud |
| **Crossplane** | Kubernetes-native: manage cloud resources as CRDs with continuous reconciliation |

**Terraform vs Ansible** ⭐⭐

| | Terraform | Ansible |
|---|---|---|
| Main job | **Provisioning** infrastructure (VPCs, VMs, DBs, DNS) | **Configuration management** of existing machines (packages, files, services) + orchestration |
| Style | Declarative, **state-based** | Ordered tasks with idempotent modules, **no state file** |
| Typical model | Immutable (replace) | Mutable (configure in place) |
| Together | Terraform creates the servers -> Ansible configures them (or Packer + Ansible bake images) | |

### 13.8 Terraform interview Q&A ⭐⭐

**Q: Someone changed a security group manually in the console. What happens on the next plan?** The plan shows the drift and proposes reverting to the code. Decide: re-apply the code (most common), or update the code to adopt the change. Then find out why manual changes were possible.

**Q: How do you rename a resource without destroying it?** Add a `moved` block (or `terraform state mv`), then confirm the plan shows no destroy.

**Q: The plan wants to destroy and recreate your production database. What do you do?** Stop. Find the attribute marked "forces replacement" in the plan. Fix the code (revert the change, or use `ignore_changes` if the attribute is managed elsewhere), and protect critical resources with `prevent_destroy` and deletion protection. Never apply a plan you don't understand.

**Q: An apply failed halfway. Now what?** Terraform isn't transactional: successfully created resources are already recorded in state. Fix the error and run plan/apply again; it continues from the current state.

**Q: How do you manage dev/staging/prod?** Reusable modules + a separate root configuration and backend (state) per environment, with environment-specific `tfvars`; or workspaces for very similar environments; or Terragrunt.

**Q: How do you handle a stuck state lock?** Confirm no apply is running (check CI jobs, ask the team), then `terraform force-unlock <LOCK_ID>`.

**Q: Data source vs resource?** A resource is created and managed by Terraform; a data source only reads something that already exists.

**Q: How do you destroy only one resource?** `terraform destroy -target=<address>` (use `-target` sparingly; it's for exceptional situations), or remove it from code and apply.

---

## 14. Ansible and Configuration Management

### 14.1 Concepts ⭐⭐

- **Agentless** (connects over **SSH** to Linux, **WinRM** to Windows), **push-based**, written in Python, playbooks in **YAML**.
- **Control node** (where Ansible runs) -> **managed nodes** (targets; just need SSH + Python).
- **Inventory:** list of hosts and groups; static (INI/YAML) or **dynamic** (plugins like `amazon.aws.aws_ec2` pull hosts from the cloud).
- **Modules:** units of work (`apt`, `dnf`, `copy`, `template`, `service`, `user`, `file`, `git`, `docker_container`, `k8s`, ...). Most are **idempotent**: they check current state and only change what's needed (task result `ok` vs `changed`).
- **Task -> Play -> Playbook:** a task calls one module; a play maps tasks to a group of hosts; a playbook is a list of plays.
- **Roles:** standard structure to package reusable automation. **Collections** bundle roles/modules/plugins; shared via **Ansible Galaxy**.
- **Handlers:** tasks that run **only when notified** by a task that changed something, once, at the end of the play (e.g. reload nginx only if its config changed).
- **Facts:** system info gathered automatically (`ansible_facts['os_family']`); disable with `gather_facts: false` for speed.
- **Templates:** Jinja2 (`.j2`) files rendered with variables.
- **Ansible Vault:** encrypts secrets in files or variables.
- **Variable precedence:** about 22 levels; key points: role `defaults/` are the lowest, `-e` extra vars are always the highest.

### 14.2 Inventory and playbook example ⭐

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

```
roles/nginx/
  tasks/main.yml        handlers/main.yml     templates/nginx.conf.j2
  files/                vars/main.yml         defaults/main.yml
  meta/main.yml
```

### 14.3 Commands ⭐

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

- `when:` conditions, `loop:` (replaces `with_items`), `register:` to capture output.
- `block` / `rescue` / `always` for error handling; `failed_when`, `changed_when`, `ignore_errors`.
- `delegate_to` (run a task on another host, e.g. remove a node from a load balancer), `run_once`.
- `async` + `poll` for long-running tasks; `serial` for rolling updates; `forks` (default 5) for parallelism.
- **`command` vs `shell`:** `command` doesn't go through a shell (no pipes, redirects or env var expansion), so it's safer; `shell` supports them. Prefer purpose-built modules for idempotency; for raw commands use `creates:`/`removes:` or `changed_when`.
- Scale/UI: **AWX** (open source) / Red Hat Ansible Automation Platform.

### 14.5 Ansible vs Chef vs Puppet vs Salt

| | Ansible | Chef | Puppet | SaltStack |
|---|---|---|---|---|
| Agent | **Agentless** (SSH) | Agent | Agent | Agent (agentless mode exists) |
| Model | Push | Pull | Pull | Push/pull |
| Language | YAML | Ruby DSL | Puppet DSL | YAML |
| Learning curve | Easiest | Steeper | Steeper | Medium |

### 14.6 Ansible interview Q&A

**Q: What are handlers and why use them?** Tasks triggered via `notify` only when something changed, run once at the end of the play: e.g. restart a service only if its config file changed.

**Q: How do you keep secrets safe?** Ansible Vault (encrypted files/vars), or lookups from an external secret manager (HashiCorp Vault, AWS Secrets Manager); `no_log: true` on sensitive tasks.

**Q: How do you make playbooks faster?** Increase `forks`, enable SSH **pipelining**, disable or cache facts, use `async` for long tasks, use the `free` strategy where order doesn't matter, avoid unnecessary `shell` loops.

**Q: How do you run a playbook on one host only?** `--limit web1`.

**Q: `vars/` vs `defaults/` in a role?** `defaults/` are lowest precedence (meant to be overridden by users); `vars/` are high precedence (internal role constants).

**Q: Is Ansible idempotent?** Its modules are designed to be; raw `shell`/`command` tasks are not unless you add `creates`, `removes` or `changed_when`.

---

## 15. Cloud Computing with AWS

> AWS is the most commonly asked cloud. The concepts map directly to Azure and GCP (mapping table in 15.10).

### 15.1 Cloud basics ⭐⭐

- **Cloud computing** = on-demand compute, storage, networking and services over the internet with **pay-as-you-go** pricing.
- **Service models** ⭐:

| Model | You manage | Provider manages | Examples |
|---|---|---|---|
| **IaaS** | OS, runtime, app, data | Hardware, virtualization, network | EC2, Azure VMs, Compute Engine |
| **PaaS** | App and data | Everything below the app | Elastic Beanstalk, Azure App Service, Google App Engine |
| **SaaS** | Just use it (and your data/settings) | Everything | Gmail, Salesforce, Microsoft 365 |
| **FaaS / Serverless** | Function code | Servers, scaling | Lambda, Azure Functions, Cloud Run functions |
| **CaaS** | Containers | Orchestration infrastructure | ECS, EKS, AKS, GKE |

- **Deployment models:** public, private, hybrid, multi-cloud.
- **Shared Responsibility Model** ⭐⭐: AWS is responsible for security **OF** the cloud (data centers, hardware, network, hypervisor, managed-service internals). You are responsible for security **IN** the cloud (your data, IAM, OS patching on EC2, security group rules, encryption settings, app code). The line moves with the service: you patch the OS on EC2, but not on Lambda or RDS.
- **Global infrastructure:** **Regions** (e.g. `ap-south-1` Mumbai, `ap-south-2` Hyderabad) contain multiple isolated **Availability Zones** (one or more data centers each, low-latency links). **Edge locations** serve CloudFront. **High availability = spread across AZs; disaster recovery = across regions.**
- **Scalability** (handle growth by adding resources) vs **elasticity** (automatically scale up *and down* with demand). **Vertical** scaling = bigger machine; **horizontal** = more machines.
- CapEx (buy hardware upfront) -> OpEx (pay for what you use).

### 15.2 IAM ⭐⭐⭐

| Concept | Meaning |
|---|---|
| **User** | A person/app with long-term credentials (avoid access keys for apps) |
| **Group** | Collection of users sharing permissions |
| **Role** | An identity **assumed** temporarily (via STS) by EC2, Lambda, EKS Pods, other accounts, CI pipelines (OIDC) or federated users. **Temporary credentials** |
| **Policy** | JSON document: `Effect`, `Action`, `Resource`, optional `Condition` |
| Identity-based vs resource-based policy | Attached to a user/role vs attached to a resource (S3 bucket policy, KMS key policy, SQS policy) |

**Policy evaluation** ⭐: by default everything is **implicitly denied**; an explicit **Allow** grants access; an explicit **Deny always wins**.

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

Best practices: **least privilege**, MFA everywhere, don't use the root account (lock it away), roles instead of access keys, IAM Identity Center (SSO) for humans, IAM Access Analyzer, **SCPs** (Service Control Policies in AWS Organizations) as account-level guardrails, permission boundaries for delegated admin.

### 15.3 EC2 ⭐⭐

- Virtual servers. **Instance families:** `t` (burstable), `m` (general purpose), `c` (compute), `r` (memory), `i` (storage-optimized), `g`/`p` (GPU). A `g` after the generation number means **Graviton (ARM)**, e.g. `m7g`, usually better price-performance.
- **AMI** (machine image), **key pairs**, **user data** (bootstrap script on first boot), **instance metadata** at `169.254.169.254` (enforce **IMDSv2**, which requires a session token and blocks many SSRF attacks).
- **Pricing models** ⭐⭐:

| Model | Discount | Best for |
|---|---|---|
| On-Demand | None | Short-term, unpredictable workloads |
| Savings Plans / Reserved Instances | Up to ~72% for 1-3 year commitments | Steady baseline usage |
| Spot | Up to ~90%, but can be reclaimed with a **2-minute warning** | Fault-tolerant work: batch, CI runners, stateless workers, K8s nodes with Karpenter |
| Dedicated Hosts / Instances | n/a | Licensing or compliance needs |

- **Placement groups:** cluster (low latency, same rack), spread (separate hardware), partition (large distributed systems).
- **Instance store** (ephemeral, lost on stop) vs **EBS** (persistent network block storage).

### 15.4 Storage ⭐⭐

| | EBS | EFS | S3 |
|---|---|---|---|
| Type | Block storage | Managed NFS file system | Object storage |
| Attach to | One EC2 instance (in one AZ); multi-attach only for io1/io2 | Thousands of instances across AZs | Accessed over HTTPS API from anywhere |
| Scope | Single AZ | Regional (multi-AZ) | Regional, 11 nines durability |
| Use | Boot disks, databases | Shared files, Kubernetes RWX volumes | Backups, static assets, data lakes, logs, Terraform state |

**S3 essentials** ⭐: buckets, objects, keys; **storage classes** (Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant Retrieval, Glacier Flexible Retrieval, Glacier Deep Archive); **lifecycle rules** to move/expire data; **versioning**; replication (cross-region/same-region); encryption (SSE-S3 by default for new objects, SSE-KMS, client-side); **Block Public Access**; presigned URLs for temporary access; static website hosting; strong read-after-write consistency.

### 15.5 VPC networking ⭐⭐⭐ (very frequently asked)

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

- **VPC:** your isolated virtual network in a region, defined by a CIDR block.
- **Subnet:** a range inside one AZ. A subnet is **public** if its route table has `0.0.0.0/0 -> Internet Gateway`; otherwise it's **private**.
- **Internet Gateway (IGW):** lets resources with public IPs talk to the internet (both directions).
- **NAT Gateway:** lets private-subnet resources make **outbound** connections (updates, external APIs) while staying unreachable from outside. It lives in one AZ, so production uses **one per AZ**. It charges per hour **and per GB processed** (a classic surprise on the bill).
- **Route tables** decide where traffic goes.

**Security Group vs NACL** ⭐⭐⭐

| | Security Group | Network ACL |
|---|---|---|
| Level | Instance / network interface | Subnet |
| State | **Stateful** (return traffic automatically allowed) | **Stateless** (allow return traffic explicitly, incl. ephemeral ports 1024-65535) |
| Rules | **Allow only** | Allow **and Deny** |
| Evaluation | All rules evaluated together | In **number order**; first match wins |
| Default | New SG: deny all inbound, allow all outbound | Default NACL allows all; a new custom NACL denies all until rules are added |
| Typical use | Main access control; can reference other SGs (e.g. "allow DB port from app SG") | Coarse subnet guardrails, blocking specific IPs |

**Connectivity options:**
- **VPC Peering:** private 1:1 connection between two VPCs; **not transitive**; CIDRs must not overlap.
- **Transit Gateway:** hub-and-spoke router for many VPCs and on-prem networks.
- **VPC Endpoints** ⭐: reach AWS services privately without the internet or NAT. **Gateway endpoints** (S3, DynamoDB; free) and **Interface endpoints / PrivateLink** (most other services). They also cut NAT data charges.
- **Site-to-Site VPN** (encrypted over the internet) and **Direct Connect** (dedicated private link) to on-premises.
- **Bastion host** vs **SSM Session Manager** (no inbound SSH port, IAM-controlled, logged): prefer Session Manager.
- **VPC Flow Logs** record ACCEPT/REJECT traffic metadata, great for troubleshooting connectivity.
- **Elastic IP** = static public IPv4. AWS charges for all public IPv4 addresses (since Feb 2024), so don't leave them idle.

### 15.6 Load balancing and Auto Scaling ⭐⭐

| Load balancer | Layer | Highlights |
|---|---|---|
| **ALB** | L7 | HTTP/HTTPS/gRPC, host/path/header routing, WebSockets, targets: instances, IPs, Lambda; integrates with WAF and Cognito |
| **NLB** | L4 | TCP/UDP/TLS, ultra-low latency, millions of requests/sec, static IP per AZ (Elastic IP support) |
| **GWLB** | L3 | Inserts third-party virtual appliances (firewalls, IDS) transparently |
| CLB | L4/L7 | Legacy; avoid for new designs |

Concepts: listeners and rules, **target groups**, **health checks**, cross-zone load balancing, **deregistration delay** (connection draining), sticky sessions.

**Auto Scaling Group (ASG):** min/desired/max capacity from a **launch template**, spread across AZs, replaces unhealthy instances (EC2 or ELB health checks). Policies: **target tracking** (e.g. keep average CPU at 50%), step scaling, scheduled scaling, predictive scaling. Extras: lifecycle hooks, **instance refresh** (rolling replacement for new AMIs), warm pools.

### 15.7 Databases ⭐⭐

- **RDS:** managed MySQL, PostgreSQL, MariaDB, Oracle, SQL Server: automated backups, point-in-time restore, patching.
  - **Multi-AZ** ⭐ = a **synchronous** standby in another AZ with **automatic failover** -> **high availability** (the classic standby doesn't serve reads).
  - **Read Replicas** ⭐ = **asynchronous** copies for **read scaling**; can be cross-region and promoted (useful for DR).
- **Aurora:** cloud-native MySQL/PostgreSQL-compatible engine; storage replicated 6 ways across 3 AZs; up to 15 read replicas; Aurora Serverless v2.
- **DynamoDB:** serverless NoSQL key-value/document DB with single-digit-millisecond latency; design around the **partition key**; on-demand or provisioned capacity; DAX cache; global tables (multi-region).
- **ElastiCache:** managed in-memory cache (Valkey/Redis OSS, Memcached) for sessions, caching, leaderboards, rate limiting.

### 15.8 Other services you should know ⭐

| Service | One-liner |
|---|---|
| **Route 53** | DNS + health checks. Routing policies: simple, **weighted** (canary), **latency**, **failover** (DR), geolocation, geoproximity, multivalue. Alias records at the apex |
| **CloudFront** | CDN; use Origin Access Control (OAC) to keep S3 origins private |
| **Lambda** | Event-driven functions; max 15-minute run; pay per request and duration; watch cold starts |
| **API Gateway** | Managed APIs in front of Lambda/HTTP backends (auth, throttling) |
| **SQS** | Message queue for decoupling; Standard (at-least-once, best-effort order) vs FIFO (ordered, exactly-once processing); visibility timeout; dead-letter queues |
| **SNS** | Pub/sub notifications; fan-out to SQS, Lambda, email, SMS |
| **EventBridge** | Event bus and scheduler for event-driven architectures |
| **ECS** | AWS's own container orchestrator (tasks, services) on EC2 or **Fargate** |
| **EKS** | Managed Kubernetes (control plane managed; EKS Auto Mode can also manage nodes and core add-ons) |
| **Fargate** | Serverless compute for containers (no nodes to manage) |
| **ECR** | Container registry with image scanning and lifecycle policies |
| **CloudWatch** | Metrics, logs (Logs Insights), alarms, dashboards, Container Insights |
| **CloudTrail** | **Audit log of API calls**: who did what, when, from where |
| **AWS Config** | Resource configuration history and compliance rules |
| **Systems Manager (SSM)** | Session Manager, Parameter Store, Patch Manager, Run Command |
| **Secrets Manager vs Parameter Store** | Secrets Manager: built-in automatic rotation, higher cost. Parameter Store: config + SecureString, cheap/free standard tier, no built-in rotation |
| **KMS** | Managed encryption keys (envelope encryption) used by most AWS services |
| **ACM** | Free public TLS certificates for ALB/CloudFront with auto-renewal |
| **WAF / Shield** | Web application firewall / DDoS protection |
| **GuardDuty / Inspector / Security Hub** | Threat detection / vulnerability scanning / central security findings |
| **Organizations / Control Tower** | Multi-account management, SCP guardrails, landing zones |
| **AWS Backup** | Central, policy-based backups across services |

**CloudWatch vs CloudTrail** ⭐: CloudWatch = **performance monitoring** (metrics, logs, alarms). CloudTrail = **auditing** (records API activity for security and compliance).

### 15.9 Well-Architected Framework (6 pillars) ⭐

1. **Operational Excellence:** IaC, small reversible changes, observability, learning from failures.
2. **Security:** least privilege, traceability, encryption in transit/at rest, defense in depth.
3. **Reliability:** multi-AZ, automatic recovery, tested backups, scaling, quotas management.
4. **Performance Efficiency:** right resource types, serverless, caching, measure and experiment.
5. **Cost Optimization:** pay for what you use, right-size, commitments, measure spend per team.
6. **Sustainability:** maximize utilization, efficient instance types (e.g. Graviton), reduce waste.

### 15.10 AWS vs Azure vs GCP mapping ⭐

| Category | AWS | Azure | GCP |
|---|---|---|---|
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

### 15.11 Cost optimization (FinOps) ⭐⭐

Right-size instances (use CloudWatch/Compute Optimizer data); Savings Plans for steady load; **Spot** for fault-tolerant work; autoscaling and **schedules** (stop dev/test at night and weekends); Graviton; S3 lifecycle policies (IA/Glacier); delete unattached EBS volumes, old snapshots, idle load balancers and unused Elastic IPs; **VPC endpoints** to cut NAT data charges; mandatory **tagging** for cost allocation; AWS Budgets + Cost Anomaly Detection; in Kubernetes, right-size requests, use Karpenter consolidation, track costs per namespace with OpenCost/Kubecost.

### 15.12 Classic question: design a highly available 3-tier web app on AWS ⭐⭐⭐

1. **DNS:** Route 53 (health checks, failover/latency routing if multi-region).
2. **Edge:** CloudFront for static assets and caching + **AWS WAF** + Shield.
3. **Web/LB tier:** **ALB** in public subnets across **at least 2 AZs**, HTTPS with an ACM certificate.
4. **App tier:** an **Auto Scaling Group** (or EKS/ECS) in **private subnets** across AZs; stateless app instances; sessions in **ElastiCache**.
5. **Data tier:** **RDS Multi-AZ** (+ read replicas for read-heavy load) in isolated DB subnets; security groups allow DB access only from the app SG.
6. **Storage:** S3 for uploads and static content (private, via CloudFront OAC).
7. **Outbound internet:** NAT Gateway per AZ; VPC endpoints for S3/DynamoDB.
8. **Security:** least-privilege IAM roles, Secrets Manager, KMS encryption, SSM Session Manager instead of SSH, GuardDuty.
9. **Observability:** CloudWatch metrics/logs/alarms (or Prometheus/Grafana), CloudTrail, ALB access logs.
10. **Resilience:** AWS Backup, cross-region snapshot copies for DR, regular restore tests.
11. **Delivery:** everything in Terraform; CI/CD with blue-green or canary deployments.

---

## 16. Monitoring, Logging and Observability

### 16.1 Concepts ⭐⭐

- **Monitoring** = collecting predefined metrics and checks to know **when** something is wrong (known unknowns).
- **Observability** = being able to understand **why** a system behaves a certain way from its outputs, including questions you didn't anticipate (unknown unknowns).
- **Three pillars** ⭐⭐:

| Pillar | What | Strength | Tools |
|---|---|---|---|
| **Metrics** | Numeric time series (CPU, request rate, latency) | Cheap, aggregatable, perfect for alerts and trends | Prometheus, CloudWatch, Datadog |
| **Logs** | Timestamped event records | Rich detail and context for debugging | ELK/EFK, Loki, CloudWatch Logs, Splunk |
| **Traces** | The path of one request across services (spans with timings) | Find *where* latency or errors happen in microservices | Jaeger, Grafana Tempo, Zipkin, X-Ray |

(Continuous **profiling** is often called the fourth pillar.)

**Frameworks for what to measure** ⭐⭐
- **Four Golden Signals** (Google SRE): **Latency, Traffic, Errors, Saturation**.
- **RED** (for request-driven services): **Rate, Errors, Duration**.
- **USE** (for resources like CPU, disk, network): **Utilization, Saturation, Errors**.

### 16.2 Prometheus ⭐⭐⭐

- Open-source (CNCF graduated) monitoring system and time-series database with **PromQL** and alerting.
- **Pull model:** Prometheus **scrapes** HTTP `/metrics` endpoints on an interval, discovering targets via service discovery (Kubernetes, EC2, Consul, files). For short-lived batch jobs, use the **Pushgateway**.
- **Components:** Prometheus server (scraper + TSDB + query engine); **exporters** (`node_exporter` for host metrics, `blackbox_exporter` for probing URLs/ports, DB exporters, **kube-state-metrics** for Kubernetes object state, **cAdvisor** in the kubelet for container metrics); client libraries to instrument code; **Alertmanager** (deduplicates, groups, routes, silences and inhibits alerts -> Slack, PagerDuty, email); **Grafana** for dashboards.
- In Kubernetes, the usual install is **kube-prometheus-stack** (Prometheus Operator with `ServiceMonitor`/`PodMonitor` CRDs, Alertmanager, Grafana, exporters).
- Long-term storage / HA / multi-cluster: Thanos, Grafana Mimir, Cortex, VictoriaMetrics (via `remote_write`).

**Metric types** ⭐⭐

| Type | Behavior | Example |
|---|---|---|
| **Counter** | Only goes up (resets to 0 on restart); use `rate()` on it | `http_requests_total` |
| **Gauge** | Goes up and down | Memory in use, queue length, temperature |
| **Histogram** | Counts observations in buckets; quantiles computed at query time with `histogram_quantile()`; aggregatable | Request latency |
| **Summary** | Quantiles computed in the client; **can't be aggregated** across instances | Latency in a single process |

**Data model:** metric name + **labels** = a unique time series. ⚠️ **High-cardinality labels** (user ID, request ID, full URL) create millions of series and can take Prometheus down.

**PromQL you should be able to write** ⭐⭐
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
`rate()` = per-second average over the window (smooth, use for alerts); `irate()` = from the last two samples (spiky, for fast-moving graphs); `increase()` = total increase over the window.

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

**Pull vs push:** pull makes it easy to know if a target is down (`up == 0`) and keeps control centralized; push suits short-lived jobs and targets behind firewalls/NAT.

### 16.3 Grafana

Dashboards over many data sources (Prometheus, Loki, Tempo, Elasticsearch, CloudWatch, SQL). Features: variables/templating, alerting, annotations (mark deployments on graphs), dashboards as code (JSON, Terraform provider).

### 16.4 Logging ⭐⭐

- Centralized logging is essential because containers and Pods are ephemeral and requests cross many services.
- **ELK / Elastic Stack:** **E**lasticsearch (store, index, search) + **L**ogstash (ingest, parse, transform) + **K**ibana (explore, visualize); Beats (Filebeat) ship logs. **EFK** swaps Logstash for **Fluentd/Fluent Bit** (lighter; runs as a Kubernetes DaemonSet). **OpenSearch** is the AWS-led fork.
- **Grafana Loki:** indexes only **labels**, not the full text, so it's much cheaper to run; queried with **LogQL**; collectors: Grafana Alloy (Promtail is deprecated), Fluent Bit, OpenTelemetry Collector.
  ```logql
  {namespace="prod", app="web"} |= "error"
  sum by (app) (count_over_time({namespace="prod"} |= "error" [5m]))
  ```
- **Kubernetes logging pattern:** apps write to **stdout/stderr** -> the container runtime writes files under `/var/log/containers` on the node -> a **node-level agent (DaemonSet)** ships them to the backend.
- **Best practices:** structured **JSON** logs, consistent log levels, include **trace/correlation IDs**, never log secrets or personal data, set retention and sampling to control cost.

### 16.5 Tracing and OpenTelemetry ⭐⭐

- **Distributed tracing:** a **trace** is one request's journey; it consists of **spans** (individual operations with start time and duration). The trace context is propagated between services in headers (W3C `traceparent`).
- **OpenTelemetry (OTel)** is the CNCF vendor-neutral standard (APIs, SDKs, auto-instrumentation, the **OTLP** protocol and the **Collector**) for generating and exporting **traces, metrics and logs**. Instrument once and send data to any backend (Jaeger, Tempo, Prometheus, Datadog, New Relic, Honeycomb, Elastic).
- **Collector pipeline:** **receivers** (e.g. OTLP) -> **processors** (batching, sampling, filtering, adding attributes) -> **exporters** (backends).
- Commercial all-in-one APM/observability: Datadog, New Relic, Dynatrace, Elastic Observability, Grafana Cloud.

### 16.6 Alerting best practices ⭐⭐

1. Alert on **symptoms that users feel** (error rate, latency, SLO burn rate), not on every cause (CPU 80% on one node).
2. Every page must be **actionable** and link to a **runbook**.
3. Use severities and routing: page only for urgent issues, send the rest to tickets/chat.
4. Avoid **alert fatigue**: deduplicate, group, tune thresholds, use `for:` durations, delete alerts nobody acts on.
5. Dashboards for exploration; alerts for action.

---

## 17. Site Reliability Engineering

### 17.1 SLI, SLO, SLA and error budgets ⭐⭐⭐

| Term | Meaning | Example |
|---|---|---|
| **SLI** (Indicator) | A measured metric of service health from the user's point of view | % of HTTP requests that succeed in under 300 ms |
| **SLO** (Objective) | Internal **target** for an SLI over a window | 99.9% of requests succeed within 300 ms over 30 days |
| **SLA** (Agreement) | **Contract** with customers, with penalties (credits) if missed | 99.5% monthly availability or 10% credit |

- SLA is usually **looser** than the SLO, giving a safety buffer.
- **Error budget = 1 - SLO.** For 99.9% over 30 days: 0.1% of 43,200 minutes = **43.2 minutes** of allowed "badness".
- **Error budget policy:** budget left -> ship features and take reasonable risks; budget exhausted -> freeze risky releases and prioritize reliability work. It turns the Dev vs Ops argument into a shared, data-driven decision.

**Availability "nines"** ⭐⭐

| Availability | Downtime per year | per 30-day month | per week |
|---|---|---|---|
| 99% | 3.65 days | 7.2 hours | 1.68 hours |
| 99.9% | 8.76 hours | 43.2 minutes | 10.1 minutes |
| 99.95% | 4.38 hours | 21.6 minutes | 5.04 minutes |
| 99.99% | 52.6 minutes | 4.32 minutes | 1.01 minutes |
| 99.999% | 5.26 minutes | 25.9 seconds | 6.05 seconds |

- **Serial dependencies multiply:** two services at 99.9% in series -> about 99.8% overall.
- **Redundancy helps:** two independent 99% replicas in parallel -> 1 - (0.01 x 0.01) = 99.99%.
- **Burn-rate alerting:** alert when the error budget is being consumed much faster than sustainable. E.g. a burn rate of 14.4 for 1 hour uses 2% of a 30-day budget, which is worth paging. Use multiple windows (a long one for significance, a short one so the alert clears quickly).

### 17.2 Toil ⭐

Toil is work that is **manual, repetitive, automatable, tactical, has no lasting value and grows linearly with the service**. Google SRE caps operational work at about **50%** of an SRE's time; the rest goes to engineering that removes toil.

### 17.3 Incident management ⭐⭐

1. **Detect** (alerts, users, synthetic checks) and **declare** an incident with a severity (SEV1 = critical customer impact ... SEV4 = minor).
2. **Assign roles:** **Incident Commander** (coordinates, decides), **Communications lead** (status page, stakeholders), **Operations/subject experts** (hands on keyboard), **Scribe** (timeline).
3. **Mitigate first, root-cause later:** roll back, fail over, scale out, disable a feature flag, shed load.
4. **Resolve** and monitor.
5. **Blameless postmortem** ⭐: summary, impact, timeline, root cause(s) and contributing factors (5 Whys / fishbone), what went well, what went badly, where we got lucky, **action items with owners and dates**. Focus on systems and processes, not people. Share it widely.

**Time metrics:** **MTTD** (mean time to detect), **MTTA** (to acknowledge), **MTTR** (to recover/restore; always say which "R" you mean), **MTBF** (mean time between failures).

**On-call hygiene:** fair rotations, escalation policies, runbooks, handoff notes, a manageable page load, follow-up on every page.

### 17.4 Chaos engineering ⭐

Deliberately inject failures in controlled experiments to build confidence: define **steady state** -> form a **hypothesis** -> **inject** failure (kill Pods, add latency, drop a node or a whole AZ) -> **observe** -> fix weaknesses. Start in staging, keep the **blast radius** small, run **game days**. Tools: Chaos Monkey, LitmusChaos, Chaos Mesh, **AWS Fault Injection Service**, Gremlin.

### 17.5 Reliability patterns ⭐⭐

Timeouts on every network call; **retries with exponential backoff + jitter** (only for idempotent operations); **circuit breakers** (stop calling a failing dependency); **bulkheads** (isolate resource pools); rate limiting and load shedding; **graceful degradation** (serve cached/partial results); idempotency keys; queues to absorb spikes; health checks + redundancy across AZs; capacity planning and load testing.

---

## 18. DevSecOps and Supply Chain Security

### 18.1 What DevSecOps means ⭐

Security integrated into every stage of the DevOps lifecycle, owned by everyone, and enforced through **automated guardrails** in pipelines rather than a manual gate at the end ("shift left", plus runtime protection, sometimes called "shield right").

### 18.2 Security checks in the pipeline ⭐⭐

| Stage | Check | What it finds | Tools |
|---|---|---|---|
| Pre-commit / PR | **Secret scanning** | Keys, tokens, passwords in code | gitleaks, trufflehog, GitHub push protection |
| Build | **SAST** (static application security testing) | Vulnerable patterns in **your source code** (SQL injection, XSS) | SonarQube, Semgrep, CodeQL, Checkmarx |
| Build | **SCA** (software composition analysis) | Known CVEs and license issues in **open-source dependencies** | Dependabot, Snyk, OWASP Dependency-Check, Trivy, Renovate (updates) |
| Build | **IaC scanning** | Misconfigurations (public S3 buckets, open security groups) | Checkov, Trivy (includes former tfsec), KICS |
| Build | **Container image scanning** | OS/package CVEs in images | Trivy, Grype, Docker Scout, ECR scanning |
| Test | **DAST** (dynamic testing of the running app, black-box) | Runtime issues: auth flaws, injection, misconfigured headers | OWASP ZAP (now ZAP by Checkmarx), Burp Suite |
| Deploy | **Policy as code / admission control** | Non-compliant manifests, unsigned images | OPA Gatekeeper, Kyverno, Conftest |
| Runtime | **Runtime threat detection** | Suspicious behavior (shell in container, crypto-mining) | Falco, Tetragon, GuardDuty |

**SAST vs DAST vs SCA** ⭐: SAST reads your code without running it (white-box, early, may have false positives). DAST attacks the running application from outside (black-box, later, finds real runtime issues). SCA checks third-party dependencies against vulnerability databases.

### 18.3 Software supply chain security ⭐⭐ (hot topic)

Attacks like SolarWinds (2020), Log4Shell (2021), the xz-utils backdoor (2024) and the compromised `tj-actions/changed-files` GitHub Action (2025) showed that **what you depend on** is an attack surface.

- **SBOM** (Software Bill of Materials): a machine-readable inventory of every component in your software (formats: **SPDX**, **CycloneDX**; generate with Syft or Trivy). When a new CVE drops, you can instantly find affected services.
- **Signing and verification:** **Sigstore** - `cosign` signs images (keyless signing using your OIDC identity via the Fulcio CA, recorded in the Rekor transparency log). Verify signatures at deploy time with admission policies (Kyverno, Sigstore policy-controller).
- **SLSA** (Supply-chain Levels for Software Artifacts): a framework for build integrity with **provenance attestations** (who built what, from which source, how) and increasing assurance levels.
- **Pin dependencies** (lockfiles, image digests, CI actions by commit SHA), use private mirrors/registries, minimal base images, reproducible builds, and least-privilege CI tokens.

### 18.4 Secrets management ⭐⭐

- **Never** store secrets in Git, images, plain env files in repos, or logs.
- Use a **secret manager:** HashiCorp Vault (dynamic short-lived secrets, leases, revocation, encryption as a service, Kubernetes auth), AWS Secrets Manager / Parameter Store, Azure Key Vault, GCP Secret Manager.
- In Kubernetes: **External Secrets Operator** or the **Secrets Store CSI driver**; encrypt etcd; RBAC on Secrets.
- Prefer **short-lived credentials**: IAM roles, OIDC federation for CI, workload identity for Pods. Rotate regularly and automatically.

### 18.5 Other security fundamentals

- **Least privilege** and **zero trust** ("never trust, always verify": authenticate and authorize every request, even inside the network; mTLS between services).
- **CIS Benchmarks** for hardening Linux, Docker and Kubernetes (`kube-bench`, `docker-bench-security`).
- **OWASP Top 10** web application risks (broken access control, injection, security misconfiguration, vulnerable components, etc.).
- **CVE** = a public identifier for a vulnerability; **CVSS** = its severity score (0-10). Define patch SLAs (e.g. criticals fixed within 7 days).
- Encryption in transit (TLS everywhere) and at rest (KMS); audit logging (CloudTrail, Kubernetes audit logs); regular patching and dependency updates.

---

## 19. Platform Engineering, AI and 2026 Trends

### 19.1 Platform engineering and Internal Developer Platforms ⭐⭐

- A dedicated **platform team** builds an **Internal Developer Platform (IDP)** and treats it as a **product** with developers as customers.
- It offers **self-service golden paths / paved roads**: e.g. "create a new service" generates a repo with CI/CD, Dockerfile, Helm chart, dashboards, alerts and security scanning already wired in.
- A **developer portal** (Backstage, a CNCF project from Spotify; also Port, Cortex) provides a software catalog, templates, docs and ownership info.
- Goals: reduce developers' **cognitive load**, standardize with guardrails instead of gates, increase delivery speed. Measure with DORA metrics plus developer-experience surveys.
- DORA's 2025 research reports that about 90% of organizations have adopted at least one internal platform, and that a **high-quality platform is what lets organizations turn AI-assisted coding into real delivery improvements**.

### 19.2 AI in DevOps ⭐ (expect at least one question)

- **What's happening:** AI coding assistants and agents write code, Dockerfiles, Terraform, pipelines and Kubernetes manifests; AI helps with incident triage (summarizing alerts and logs, suggesting likely causes), anomaly detection (AIOps), ChatOps, and natural-language queries over observability data. Protocols such as **MCP (Model Context Protocol)** let AI agents use tools like Kubernetes, CI systems and cloud APIs.
- **DORA 2025 takeaway:** AI is an **amplifier**: teams with good foundations get faster; teams with weak testing, large batches and poor platforms get more instability. Small batches, strong version control and fast feedback matter more, not less.
- **Risks to mention:** hallucinated or insecure configs, leaking secrets/code into prompts, over-privileged agents, unreviewed changes. **Controls:** mandatory code review, automated tests and policy checks, least-privilege credentials for agents, human approval for production actions, audit logs.
- **MLOps / LLMOps basics:** versioned data and models, training pipelines, model registries, serving (KServe, vLLM), GPU scheduling on Kubernetes (Dynamic Resource Allocation is stable; workload-aware/**gang scheduling** APIs reached beta in v1.37), monitoring for drift and cost.

### 19.3 Other trends worth naming

| Trend | One-liner |
|---|---|
| Gateway API | Replacing Ingress as the standard for Kubernetes traffic (Ingress NGINX retired in 2026) |
| OpenTofu vs Terraform | Open-source fork vs IBM-owned BSL tool; concepts identical |
| eBPF | Run sandboxed programs in the Linux kernel; powers Cilium networking, Tetragon security, low-overhead observability |
| OpenTelemetry everywhere | Vendor-neutral instrumentation is now the default choice |
| FinOps | Engineers own cloud cost; cost visible per team/service |
| Supply chain security | SBOMs, signing, provenance, pinned dependencies |
| GitOps | Default way to run Kubernetes deployments at scale |
| Kubernetes for AI | DRA for GPUs, gang scheduling, in-place resize, rootless kubelet (beta in v1.37) |
| Helm 4 | Server-side apply, Wasm plugins, renamed CLI flags |

---

## 20. System Design for DevOps

### 20.1 HA vs fault tolerance vs disaster recovery ⭐⭐

| Term | Meaning | Example |
|---|---|---|
| **High availability (HA)** | Minimize downtime through redundancy and automatic failover (a brief blip is acceptable) | App across 3 AZs behind an ALB; RDS Multi-AZ |
| **Fault tolerance** | Keep working with **zero** interruption when a component fails (full redundancy, more expensive) | Active-active systems, RAID 1, dual power supplies |
| **Disaster recovery (DR)** | Recover from a major event (region outage, data corruption, ransomware) within agreed targets | Cross-region backups and standby environment |

- **RTO** (Recovery Time Objective) ⭐⭐ = the maximum acceptable **downtime** ("we must be back within 1 hour").
- **RPO** (Recovery Point Objective) ⭐⭐ = the maximum acceptable **data loss**, measured in time ("we can lose at most 5 minutes of data").

**DR strategies** (cost goes up as RTO/RPO go down) ⭐⭐

| Strategy | How it works | Typical RTO / RPO |
|---|---|---|
| **Backup and restore** | Regular backups copied to another region; rebuild everything with IaC when needed | Hours / hours |
| **Pilot light** | Core data continuously replicated (e.g. DB replica); other infrastructure off or minimal until disaster | Tens of minutes / minutes |
| **Warm standby** | A scaled-down but fully working copy runs in the DR region; scale it up on failover | Minutes / seconds-minutes |
| **Multi-site active-active** | Full production in two or more regions serving traffic simultaneously | Near zero / near zero |

**Backups** ⭐: follow the **3-2-1 rule** (3 copies, 2 different media/services, 1 off-site) plus an **immutable** copy (S3 Object Lock / vault lock) against ransomware. Automate (AWS Backup, Velero for Kubernetes, DB point-in-time recovery) and **test restores regularly**: an untested backup is only a hope.

### 20.2 Scaling building blocks ⭐

- **Vertical scaling** (bigger machine): simple, but has limits and often needs downtime. **Horizontal scaling** (more machines): needs **stateless** services and a load balancer; externalize sessions/state to Redis, a DB or S3.
- **Caching layers:** CDN (edge), in-memory cache (Redis), application caches; define TTLs and invalidation.
- **Database scaling:** read replicas, connection pooling (PgBouncer, RDS Proxy), indexing and query tuning, partitioning/sharding, choosing the right store (SQL vs NoSQL).
- **Asynchronous processing:** queues (SQS, Kafka, RabbitMQ) absorb spikes and decouple services.
- **Protect the system:** rate limiting, autoscaling with sensible limits, load shedding, circuit breakers.
- **Remove single points of failure (SPOFs):** multiple AZs, multiple instances, redundant NAT gateways, replicated databases.
- **Reduce blast radius:** separate accounts/projects per environment, namespaces, cell-based architecture, canary releases, feature flags.

### 20.3 Design: CI/CD for 50 microservices on Kubernetes ⭐⭐

1. **Repos:** one repo per service (or a monorepo with path-based triggers) + a separate **GitOps config repo**.
2. **CI (shared templates):** build -> unit tests -> SAST/SCA/secret scan -> build image with BuildKit cache -> Trivy scan -> SBOM + cosign signature -> push to ECR tagged with the git SHA.
3. **CD (GitOps):** a bot updates the image tag in the config repo for `dev`; Argo CD syncs; automated integration tests run; promotion to `staging` and `prod` via PRs (prod requires approval).
4. **Progressive delivery:** Argo Rollouts canary (5% -> 25% -> 100%) with automated analysis of error rate and latency from Prometheus; auto-rollback on failure.
5. **Standardization:** one golden Helm chart (probes, resources, PDB, HPA, NetworkPolicy, ServiceMonitor) that services configure via values.
6. **Guardrails:** Kyverno/Gatekeeper policies (no `:latest`, must have requests/limits, signed images only, non-root).
7. **Observability:** OpenTelemetry tracing, Prometheus metrics, Loki logs, SLO dashboards per service, deployment annotations on graphs.
8. **Secrets:** External Secrets Operator + AWS Secrets Manager; OIDC for CI to AWS.

### 20.4 Design: zero-downtime release with a database schema change ⭐⭐

Use **expand and contract**:
1. **Expand:** add the new column/table in a backward-compatible way (nullable, no renames/drops). Old code keeps working.
2. Deploy app version N+1 that **writes to both** old and new structures and reads the old one (or reads new with a fallback).
3. **Backfill** existing data in batches.
4. Deploy version N+2 that **reads from the new** structure.
5. **Contract:** in a later release, remove the old column and the dual-write code.
Combine with rolling/canary deployments, readiness probes and a tested rollback (which works because every step is backward compatible).

### 20.5 Design: multi-region active-passive setup ⭐

- Primary region serves all traffic; secondary region has a **warm standby** (smaller ASG/EKS node group, Aurora Global Database or cross-region read replica, S3 Cross-Region Replication, ECR replication).
- **Route 53 failover routing** with health checks (or a global load balancer) switches traffic.
- Infrastructure defined in Terraform for both regions; secrets and config replicated; runbooks and **regular failover drills** (game days).
- Know the trade-offs: replication lag (RPO), DNS TTL and failover time (RTO), cost of idle capacity, data consistency on failback.

### 20.6 Design: centralized logging for 500 Pods ⭐

Apps log JSON to stdout -> **Fluent Bit DaemonSet** (parse, enrich with Kubernetes metadata, filter noise, buffer) -> **Loki** (cost-efficient, label-indexed, object storage backend) or **OpenSearch/Elasticsearch** (full-text search) -> **Grafana/Kibana**. Add retention tiers (hot 7 days, archive to S3 for 90+ days), sampling of debug logs, per-namespace rate limits, PII scrubbing, and alerts on log-based error rates.

---

## 21. Scenario-Based Troubleshooting Questions

> How to answer any scenario: **clarify** (what changed? when did it start? who/what is affected?) -> **assess impact** -> **mitigate first** (rollback, scale, failover) -> **investigate** with data (metrics, logs, traces, events) -> **fix the root cause** -> **prevent recurrence** (alerts, tests, automation, postmortem). Think out loud; interviewers grade the process.

### Scenario 1: The website returns 502 errors ⭐⭐⭐
1. Scope: all users or some? All endpoints? Since when? Was there a recent deployment or config change?
2. 502 means the **proxy/LB got a bad or no response from the upstream**. Check LB/ingress logs and target health (ALB target groups, `kubectl get endpointslices`).
3. Are the app processes/Pods running? (`systemctl status`, `kubectl get pods`, restarts, OOMKilled, CrashLoopBackOff.)
4. Is the app listening on the expected port and interface (`ss -tulnp`; `0.0.0.0` vs `127.0.0.1`)? Does the LB/Service `targetPort` match?
5. Check app logs for crashes, exceptions, connection resets; check keep-alive timeout mismatches between proxy and app.
6. **Mitigate:** roll back the last deployment if it correlates; scale out if overloaded.
7. **Prevent:** readiness probes, health checks, canary releases, alerts on 5xx rate.

### Scenario 2: A Linux server is very slow ⭐⭐
1. `uptime` (load average vs `nproc`), `top`/`htop` (which process? `%us`, `%sy`, `%wa`).
2. **High CPU:** identify the process (`ps aux --sort=-%cpu`), check whether it's expected (traffic spike, cron job, runaway loop, crypto-miner). Use `pidstat`, `strace`, `perf` or a thread dump for Java (`jstack`).
3. **High I/O wait:** `iostat -xz 1`, `iotop`; look for a disk at 100% util, a backup job, heavy logging, swap activity.
4. **Memory pressure:** `free -h`, `vmstat 1` (swapping?), OOM messages in `dmesg -T`.
5. **Network:** `ss -s` (connection counts), retransmits, saturated bandwidth.
6. Mitigate: restart/kill a runaway process carefully (SIGTERM first), scale out, move batch jobs off-peak. Long term: right-size, fix code, add monitoring and alerts on these signals.

### Scenario 3: "No space left on device" ⭐⭐⭐
1. `df -h` to find the full filesystem; `df -i` to check **inodes**.
2. Find big directories/files: `du -xh / --max-depth=1 | sort -rh | head`, then drill down; `find / -xdev -size +500M`.
3. Usual culprits: application/system logs, Docker images/containers/build cache (`docker system df`), old backups, core dumps, `/tmp`.
4. **Deleted files still held open:** `lsof +L1` -> restart that process (or truncate its file descriptor) to release space.
5. Free space safely: compress/rotate logs, `docker system prune`, delete old artifacts. **Don't** delete random files or live log files from under running processes without truncating properly.
6. Prevent: logrotate, Docker log limits, disk usage alerts (e.g. at 80%) and `predict_linear` alerts, separate volumes for data and logs, extend the volume (EBS resize + `growpart` + `resize2fs`/`xfs_growfs`).

### Scenario 4: You can't SSH into an EC2 instance ⭐⭐
1. Correct key and **username** (`ec2-user` for Amazon Linux, `ubuntu` for Ubuntu), key permissions `chmod 400 key.pem`.
2. **Security group** allows port 22 from your IP; **NACL** allows inbound 22 **and outbound ephemeral ports**.
3. Instance has a public IP / Elastic IP and its subnet routes to an **Internet Gateway** (or you're using a bastion/VPN).
4. Instance health: EC2 status checks; console screenshot / system log (kernel panic, full disk, sshd failed).
5. Use **SSM Session Manager** or **EC2 Serial Console** to get in without SSH.
6. Last resort: stop the instance, detach the root volume, attach it to a rescue instance, fix (`authorized_keys`, `sshd_config`, full disk), reattach.

### Scenario 5: A Kubernetes Pod is in CrashLoopBackOff ⭐⭐⭐
1. `kubectl describe pod` (events, last state, **exit code**, reason).
2. `kubectl logs <pod> --previous` (logs of the crashed container).
3. Interpret the exit code: 1 = app error, 137 = OOMKilled/SIGKILL, 126/127 = command problem, 143 = SIGTERM (maybe liveness probe killing it).
4. Common causes: missing env var/ConfigMap/Secret, wrong DB credentials or endpoint, dependency unavailable, liveness probe too aggressive (needs a startup probe), wrong command/entrypoint, read-only filesystem or permission issues.
5. Debug interactively: `kubectl debug`, or temporarily override the command with `sleep infinity` and exec in.
6. Check what changed: `kubectl rollout history`, recent config commits; roll back if needed (`kubectl rollout undo`).

### Scenario 6: Pods stuck in Pending ⭐⭐
`kubectl describe pod` events tell you why: insufficient CPU/memory (requests too high or cluster full -> check Cluster Autoscaler/Karpenter logs, node group max size), no node matching nodeSelector/affinity, taints without tolerations, unbound PVC (StorageClass, zone mismatch -> `WaitForFirstConsumer`), ResourceQuota exceeded, or too many Pods per node (IP exhaustion with the AWS VPC CNI).

### Scenario 7: A deployment just broke production ⭐⭐⭐
1. **Mitigate immediately:** roll back (`kubectl rollout undo`, `helm rollback`, revert the GitOps commit, switch blue-green back, disable the feature flag). Restoring service beats finding the bug.
2. Communicate: declare an incident, update stakeholders/status page.
3. Verify recovery with metrics (error rate, latency back to baseline).
4. Investigate in a non-production environment: diff the releases, logs, traces.
5. Postmortem: why didn't tests/canary catch it? Add the missing test, canary analysis or alert.

### Scenario 8: It works locally but fails in Docker/CI ⭐
Differences to check: environment variables and config files, OS/library versions (Alpine musl vs glibc), file paths and **case-sensitive filesystems**, missing files due to `.dockerignore`, network access from CI (proxies, private registries), the app binding to `localhost` instead of `0.0.0.0`, timezone/locale, CPU architecture (ARM Mac vs x86 CI: build multi-arch images with `docker buildx`), secrets not available in CI, and caches hiding problems locally. Reproduce by running the exact CI image locally.

### Scenario 9: The CI pipeline is suddenly slow or flaky ⭐
Compare with recent successful runs: which stage got slower? Cache misses (cache key changed?), dependency registry slow or rate-limited, runner capacity (queue time), new heavy tests, Docker layer cache invalidated by an early `COPY . .`, network issues. For flakiness: identify flaky tests from history, quarantine them, fix shared state/time-dependency issues, add retries only with tracking.

### Scenario 10: Two engineers ran `terraform apply` at the same time ⭐
Remote state **with locking** prevents this; if it happened, the backend wasn't locking. Steps: stop all applies, restore the last good state version from the versioned bucket if state is corrupted, run `terraform plan` to see the real drift and reconcile (import or fix), then enforce: locking backend, applies only from CI, branch protection, and plan review.

### Scenario 11: Pods keep getting OOMKilled ⭐⭐
Confirm with `kubectl describe pod` (reason `OOMKilled`, exit code 137) and memory graphs (`container_memory_working_set_bytes`). If usage climbs steadily until the kill, it's a **leak**: take heap dumps/profiles and fix the code. If it's a steady need, raise the limit (and request). For JVMs, align heap with the container limit (`-XX:MaxRAMPercentage`), remembering non-heap memory (threads, metaspace, buffers). Use VPA recommendations, and alert before the limit is reached.

### Scenario 12: Latency doubled but nothing was deployed ⭐
Look at the golden signals by component: is traffic higher (scale out)? Is a dependency slow (DB slow queries, cache hit ratio dropped, third-party API)? Use **traces** to find the slow span. Check saturation: CPU throttling (CPU limits!), connection pool exhaustion, GC pauses, noisy neighbors, disk I/O, network/DNS latency. Check for config changes outside deployments (feature flags, infra changes, certificate renewals, cloud provider incidents).

### Scenario 13: DNS resolution fails inside Kubernetes Pods ⭐
From a debug Pod: `nslookup kubernetes.default` and `nslookup <svc>.<ns>.svc.cluster.local`. Check CoreDNS Pods (`kubectl -n kube-system get pods -l k8s-app=kube-dns`) and their logs, the `kube-dns` Service and its endpoints, NetworkPolicies blocking port 53 egress, the Pod's `/etc/resolv.conf` (search domains, `ndots`), and node-level DNS issues. Under heavy load, scale CoreDNS or add NodeLocal DNSCache.

### Scenario 14: The production database was accidentally deleted or corrupted ⭐⭐
1. Stop the bleeding: block further writes/changes; declare an incident.
2. Restore: RDS **point-in-time recovery** to just before the event (creates a new instance), or the latest snapshot; then repoint the app (DNS/config). Know your RTO/RPO.
3. Validate data integrity; replay or reconcile missing transactions if possible.
4. Prevent: deletion protection, `prevent_destroy` in Terraform, least privilege (no one has delete rights in prod by default), separate prod accounts, automated backups with cross-region copies, regular restore drills.

### Scenario 15: Cut the cloud bill by 30% ⭐⭐
1. **Visibility first:** Cost Explorer by service/tag/account; find the top 5 cost drivers.
2. **Quick wins:** delete idle resources (unattached EBS, old snapshots, idle LBs, unused Elastic IPs), stop dev/test outside office hours, S3 lifecycle policies, log retention limits.
3. **Right-size** over-provisioned instances, databases and Kubernetes requests.
4. **Commitments** (Savings Plans) for the steady baseline; **Spot** for fault-tolerant workloads; **Graviton**.
5. **Architecture:** VPC endpoints instead of NAT data processing, caching/CDN to reduce egress, serverless for spiky low-volume workloads.
6. **Governance:** mandatory tags, budgets and anomaly alerts, showback/chargeback per team.

### Scenario 16: Migrate a monolith running on VMs to Kubernetes ⭐
Assess (dependencies, state, config, startup behavior) -> make it 12-factor friendly (config via env, logs to stdout, stateless sessions) -> containerize and test -> set up the platform (cluster via Terraform, CI/CD, observability, secrets) -> deploy alongside the old system and **shift traffic gradually** (weighted DNS/LB) -> keep the DB managed outside the cluster at first -> later, split the monolith with the **strangler fig pattern** (peel off one capability at a time behind the same entry point).

### Scenario 17: You get paged at 2 AM ⭐⭐
Acknowledge -> check the alert's dashboard and runbook -> assess customer impact and severity -> escalate/declare an incident if needed (don't be a hero) -> mitigate (rollback, failover, scale) -> communicate status at regular intervals -> confirm recovery -> hand off with notes -> next day: postmortem and fix the alert/runbook if it was noisy or unclear.

### Scenario 18: An expired TLS certificate caused an outage ⭐
Mitigate by renewing/reissuing (ACM, Let's Encrypt, cert-manager) and redeploying. Prevent: automated issuance and renewal, alerts on expiry (e.g. 30/14/7 days, blackbox_exporter's `probe_ssl_earliest_cert_expiry`), an inventory of all certificates (including internal ones), and avoiding manual certificate handling.

### Scenario 19: 5xx errors come only from some Pods ⭐
Compare failing vs healthy Pods: which nodes are they on (node-specific problem: disk, network, kernel)? Different image versions (a stuck rollout)? Different config (a ConfigMap update picked up by some Pods only)? Resource throttling on those Pods? Use `kubectl get pods -o wide`, logs per Pod, metrics by Pod label. Mitigate by cordoning the bad node or deleting the bad Pods; ensure readiness probes would have removed them.

### Scenario 20: Marketing expects 10x traffic for a sale next week ⭐
Load test at the expected peak plus margin; find bottlenecks (DB, cache, third-party limits); pre-scale (raise ASG/HPA minimums, warm caches, pre-warm load balancers if needed, raise service quotas); enable a CDN for static content; add queues for non-critical work; prepare feature flags to shed expensive features; set up a war-room dashboard and on-call coverage; freeze risky deployments during the event.

### Scenario 21: An S3 bucket with customer data was made public ⭐
Immediately block public access (account-level S3 Block Public Access), assess exposure using access logs/CloudTrail, involve security/legal per policy, rotate any exposed secrets. Prevent: account-wide Block Public Access, SCPs, AWS Config rules and alerts, IaC scanning (Checkov/Trivy) in CI, least privilege on bucket policies.

---

## 22. Rapid-Fire Questions

| # | Question | Answer |
|---|---|---|
| 1 | What is idempotency? | Repeating an operation gives the same result as doing it once |
| 2 | CI vs CD? | CI = integrate and test every change; CD = keep every change releasable (Delivery) or deploy it automatically (Deployment) |
| 3 | What is a webhook? | An HTTP callback sent when an event happens (e.g. push -> trigger build) |
| 4 | Jenkins pipeline file? | `Jenkinsfile` |
| 5 | GitHub Actions workflow location? | `.github/workflows/` |
| 6 | Keyless cloud auth from CI? | OIDC federation (short-lived credentials) |
| 7 | `git fetch` vs `git pull`? | fetch downloads only; pull = fetch + merge/rebase |
| 8 | Safely undo a pushed commit? | `git revert <sha>` |
| 9 | Recover "lost" commits? | `git reflog` |
| 10 | Find the commit that introduced a bug? | `git bisect` |
| 11 | Show listening ports? | `ss -tulnp` |
| 12 | Check disk space / inodes? | `df -h` / `df -i` |
| 13 | Zombie process? | Exited child whose parent hasn't collected its exit status |
| 14 | Load average? | Avg number of runnable + uninterruptible processes over 1/5/15 minutes |
| 15 | SUID? | Executable runs with the file owner's privileges |
| 16 | Hard vs soft link? | Same inode vs a pointer to a path |
| 17 | Cron for every 15 minutes? | `*/15 * * * *` |
| 18 | TCP handshake? | SYN -> SYN-ACK -> ACK |
| 19 | DNS port? | 53 (UDP; TCP for large responses and zone transfers) |
| 20 | 502 vs 504? | Bad/no response from upstream vs upstream timeout |
| 21 | 401 vs 403? | Not authenticated vs not authorized |
| 22 | Private IPv4 ranges? | 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 |
| 23 | Usable IPs in an AWS /24? | 251 (AWS reserves 5) |
| 24 | VM vs container? | VM virtualizes hardware with its own OS; container shares the host kernel |
| 25 | Which Linux features make containers? | Namespaces (isolation) + cgroups (limits) + overlay filesystem |
| 26 | CMD vs ENTRYPOINT? | CMD = overridable defaults; ENTRYPOINT = fixed executable |
| 27 | COPY vs ADD? | COPY copies; ADD also extracts tars/fetches URLs. Prefer COPY |
| 28 | Why multi-stage builds? | Smaller, more secure runtime images |
| 29 | Exit code 137? | SIGKILL, usually OOMKilled |
| 30 | Exit code 143? | SIGTERM (graceful stop) |
| 31 | Default Docker network? | bridge |
| 32 | Where is Kubernetes state stored? | etcd |
| 33 | Smallest deployable unit in Kubernetes? | Pod |
| 34 | Who assigns Pods to nodes? | kube-scheduler |
| 35 | Who restarts containers on liveness failure? | kubelet |
| 36 | Default Service type? | ClusterIP |
| 37 | NodePort range? | 30000-32767 |
| 38 | Kubernetes API server port? | 6443 |
| 39 | One Pod per node? | DaemonSet |
| 40 | Stable identity + per-Pod storage? | StatefulSet |
| 41 | Readiness probe failure effect? | Pod removed from Service endpoints (no restart) |
| 42 | Are Secrets encrypted by default? | No, only base64-encoded; enable encryption at rest |
| 43 | What replaced PodSecurityPolicy? | Pod Security Admission (Pod Security Standards) |
| 44 | Recommended successor to Ingress NGINX? | Gateway API (or another maintained controller) |
| 45 | `kubectl drain`? | Cordon + evict Pods (respecting PDBs) |
| 46 | Taint vs affinity? | Taint (node) repels Pods; affinity (Pod) attracts it to nodes |
| 47 | HPA needs? | Resource requests + metrics-server (or custom metrics) |
| 48 | Roll back a Deployment? | `kubectl rollout undo deployment/<name>` |
| 49 | What is a CRD / Operator? | Custom API type / controller that automates an app's operations |
| 50 | Helm release? | An installed instance of a chart |
| 51 | Helm 4 replacement for `--atomic`? | `--rollback-on-failure` |
| 52 | GitOps in one line? | Git is the source of truth; an in-cluster agent continuously reconciles to it |
| 53 | GitOps tools? | Argo CD, Flux |
| 54 | Preview Terraform changes? | `terraform plan` |
| 55 | Why Terraform state? | Maps code to real resources, tracks metadata, enables diffs |
| 56 | Why lock state? | Prevent concurrent applies from corrupting it |
| 57 | `count` vs `for_each`? | Index-based vs key-based (stable addresses) |
| 58 | Rename a resource without destroying it? | `moved` block / `terraform state mv` |
| 59 | `.terraform.lock.hcl`? | Provider version/checksum lock; commit it |
| 60 | OpenTofu license? | MPL 2.0 (Terraform: BSL 1.1) |
| 61 | Ansible transport? | SSH (WinRM for Windows); agentless |
| 62 | Ansible handler? | Task run once at the end of a play, only when notified by a change |
| 63 | Encrypt secrets in Ansible? | `ansible-vault` |
| 64 | Blue-green rollback speed? | Instant: switch traffic back |
| 65 | Canary? | Gradually shift traffic to the new version while watching metrics |
| 66 | Decouple deploy from release? | Feature flags |
| 67 | SG vs NACL? | Stateful, instance-level, allow-only vs stateless, subnet-level, ordered allow+deny |
| 68 | Outbound internet for private subnets? | NAT Gateway |
| 69 | RDS Multi-AZ vs read replica? | Synchronous standby for HA vs async copy for read scaling |
| 70 | CloudWatch vs CloudTrail? | Monitoring vs API audit trail |
| 71 | Spot interruption notice? | 2 minutes |
| 72 | S3 durability? | 11 nines (99.999999999%) |
| 73 | Shared responsibility in one line? | Provider secures the cloud; you secure what you put in it |
| 74 | Prometheus collection model? | Pull (scrape `/metrics`) |
| 75 | Counter vs gauge? | Only increases vs goes up and down |
| 76 | p95 latency in PromQL? | `histogram_quantile(0.95, ...)` |
| 77 | Golden signals? | Latency, traffic, errors, saturation |
| 78 | Three pillars of observability? | Metrics, logs, traces |
| 79 | SLI vs SLO vs SLA? | Measurement vs internal target vs customer contract |
| 80 | 99.9% monthly error budget? | ~43.2 minutes |
| 81 | Toil? | Manual, repetitive, automatable work with no lasting value |
| 82 | RTO vs RPO? | Max acceptable downtime vs max acceptable data loss |
| 83 | SAST vs DAST? | Analyze source code vs test the running app |
| 84 | SBOM? | Inventory of all software components |
| 85 | cosign? | Signs and verifies container images (Sigstore) |
| 86 | Five DORA metrics? | Deployment frequency, change lead time, failed deployment recovery time, change fail rate, deployment rework rate |
| 87 | 12-factor rule for config? | Store config in the environment |
| 88 | Pets vs cattle? | Hand-maintained servers vs identical, replaceable instances |
| 89 | Latest Kubernetes release (Oct 2026)? | v1.37 "Garhwal" |
| 90 | What is shift-left? | Doing testing and security earlier in the lifecycle |

---

## 23. How to Answer in Interviews

### 23.1 Self-introduction (60-90 seconds)

"I'm **[name]**, a DevOps engineer with **[X years]** of experience (or: a recent graduate who has built **[project]**). I work mainly with **[AWS, Kubernetes, Terraform, GitHub Actions/Jenkins]**. In my current role I **[own the CI/CD platform for N services / manage EKS clusters / automated infra with Terraform]**. Recently I **[a measurable achievement: cut pipeline time 25 -> 9 minutes, reduced cloud cost 28%, reduced failed deployments with canary releases]**. I enjoy **[automation / reliability / developer experience]**, and I'm looking for a role where **[connect to the job]**."

### 23.2 Explaining your project (the most important answer) ⭐⭐⭐

Draw it if you can. Cover, in order:
1. **Business context:** what the product does, scale (users, requests, number of services).
2. **Architecture:** cloud, network layout, compute (EKS/ECS/EC2), data stores, entry points.
3. **Delivery flow:** commit -> CI (build, test, scan) -> registry -> CD (Helm/Argo CD) -> environments -> deployment strategy.
4. **Infrastructure:** Terraform modules, state backend, environments.
5. **Operations:** monitoring, logging, alerting, on-call, incidents you handled.
6. **Security:** IAM/OIDC, secrets management, scanning, network policies.
7. **Your role and impact with numbers.**
8. **A challenge you solved** (the interviewer will dig here, so pick a real one you can explain in depth).

> Only claim what you actually did. Interviewers follow up with "how exactly?" questions until they find the edge of your knowledge.

### 23.3 Structure for technical questions

**Definition -> how it works -> example from your work -> trade-offs / gotchas.**
Example, "What is a readiness probe?": define it -> how the kubelet runs it and what happens on failure -> "we used `/ready` that checks the DB connection pool so Pods didn't get traffic during warm-up" -> "don't put dependency checks in liveness, it causes cascading restarts".

### 23.4 Behavioral questions with STAR ⭐

**S**ituation -> **T**ask -> **A**ction (what *you* did) -> **R**esult (numbers + what you learned).

Example, "Tell me about a production incident you handled":
- *S:* Checkout errors jumped to 20% right after a Friday release.
- *T:* I was on call and had to restore service.
- *A:* Correlated the spike with the deployment on our Grafana dashboard, rolled back with `helm rollback` within 6 minutes, posted updates in the incident channel, then reproduced the bug in staging: a missing env var in the new chart version.
- *R:* Full recovery in under 10 minutes. In the postmortem I added a CI check that validates required env vars and introduced canary releases with automated rollback; we had no similar incident afterwards.

Common behavioral questions: a time you disagreed with a developer/manager; an outage you caused; how you prioritize many requests; an automation you're proud of; how you handle pressure; how you learn new tools; how you onboarded others.

### 23.5 When you don't know the answer

Say so honestly, then reason aloud: "I haven't used X directly, but I'd expect it to work like Y because...; I'd verify by reading the docs / testing in a sandbox." Interviewers value honest reasoning far more than a bluff.

### 23.6 What interviewers expect by level

| Level | Focus |
|---|---|
| Fresher / 0-2 yrs | Linux, networking, Git, scripting basics; Docker; Kubernetes objects; one CI tool; AWS core services; a real hands-on project |
| Mid / 2-5 yrs | Designing pipelines, Terraform modules and state, Kubernetes troubleshooting, monitoring and alerting, security basics, real incidents |
| Senior / 5+ yrs | Architecture and trade-offs, multi-account/multi-cluster strategy, reliability (SLOs, DR), cost, platform engineering, mentoring, influencing teams |

### 23.7 Smart questions to ask the interviewer

- How do deployments to production work today, and how often do you deploy?
- What does on-call look like (rotation size, pages per week)?
- What's the biggest reliability or delivery pain point the team wants this hire to fix?
- How is the platform/DevOps team organized relative to product teams?
- How do you measure success for this role in the first 6 months?

---

## 24. Portfolio Project to Build

> One end-to-end project you built yourself beats ten certificates in interviews. Document it on GitHub with an architecture diagram.

**Goal:** a small web app (e.g. a Node.js or Python API + database) delivered end to end.

1. **App + Dockerfile:** multi-stage build, non-root user, health endpoints (`/healthz`, `/ready`), structured JSON logs, Prometheus metrics endpoint.
2. **CI (GitHub Actions):** lint -> unit tests -> Trivy scan (fail on critical) -> build -> push to GHCR/ECR tagged with the git SHA; OIDC to AWS (no stored keys).
3. **Infrastructure (Terraform):** VPC (public/private subnets across 2 AZs), EKS (or k3s on a small VM to save money), ECR, remote state in S3 with locking.
4. **Kubernetes packaging:** a Helm chart with Deployment, Service, HPA, PDB, probes, resources, NetworkPolicy, and a Gateway API `HTTPRoute`.
5. **GitOps:** Argo CD watching a config repo with `dev` and `prod` overlays; promotion via PR.
6. **Observability:** kube-prometheus-stack (Prometheus, Grafana, Alertmanager) + Loki; a dashboard with RED metrics and an SLO; an alert routed to Slack/email.
7. **Progressive delivery (bonus):** Argo Rollouts canary with automated analysis.
8. **Security (bonus):** Kyverno policies (no `:latest`, non-root, require limits), External Secrets Operator, cosign image signing.
9. **Chaos test (bonus):** delete Pods/nodes during a load test (k6) and show the app stays up.
10. **README:** architecture diagram, how to run, design decisions and trade-offs, what you'd improve, screenshots of dashboards.

⚠️ **Cost warning:** EKS control planes, NAT Gateways, load balancers and public IPv4 addresses are billed by the hour. Use **kind/k3d/minikube** locally for most of the work, set an AWS Budget alert, and run `terraform destroy` when you're done.

---

## 25. Last-Day Revision Sheet

### 25.1 Most-asked "X vs Y" (one line each)

| Pair | Key difference |
|---|---|
| DevOps vs SRE | Culture/practices vs a concrete engineering implementation with SLOs and error budgets |
| Continuous Delivery vs Deployment | Manual approval to prod vs fully automatic |
| merge vs rebase | Merge commit preserves history vs linear rewritten history |
| reset vs revert | Rewrite history vs add an undo commit |
| Process vs thread | Separate memory vs shared memory within a process |
| SIGTERM vs SIGKILL | Graceful vs forced |
| TCP vs UDP | Reliable/ordered vs fast/best-effort |
| L4 vs L7 load balancer | IP/port routing vs HTTP-aware routing |
| Forward vs reverse proxy | Fronts clients vs fronts servers |
| VM vs container | Own kernel vs shared kernel |
| Image vs container | Template vs running instance |
| CMD vs ENTRYPOINT | Overridable defaults vs fixed executable |
| Volume vs bind mount | Docker-managed vs host path |
| Deployment vs StatefulSet | Interchangeable replicas vs stable identity + storage |
| Liveness vs readiness | Restart vs stop sending traffic |
| Requests vs limits | Scheduling guarantee vs hard cap (CPU throttle / memory OOMKill) |
| ClusterIP vs NodePort vs LoadBalancer | Internal vs node port vs cloud LB |
| Ingress vs Gateway API | Single limited resource + annotations vs role-based, portable, richer routing |
| ConfigMap vs Secret | Plain config vs sensitive data (base64, needs encryption at rest) |
| HPA vs VPA vs Cluster Autoscaler | Pod count vs Pod size vs node count |
| Helm vs Kustomize | Templates + packaging vs overlays/patches |
| Push CD vs GitOps | CI pushes with cluster creds vs in-cluster agent pulls from Git |
| Terraform vs Ansible | Provisioning with state vs configuring existing machines |
| Terraform vs CloudFormation | Multi-cloud + own state vs AWS-only + managed state |
| count vs for_each | Index-based vs key-based |
| Security Group vs NACL | Stateful instance-level vs stateless subnet-level |
| Multi-AZ vs read replica | HA failover vs read scaling |
| S3 vs EBS vs EFS | Object vs block vs shared file |
| CloudWatch vs CloudTrail | Metrics/logs vs API audit |
| Monitoring vs observability | Known failures vs understanding unknown ones |
| Metrics vs logs vs traces | Numbers over time vs events vs request journeys |
| SLO vs SLA | Internal target vs external contract |
| RTO vs RPO | Downtime tolerance vs data-loss tolerance |
| SAST vs DAST vs SCA | Your code vs running app vs dependencies |
| Blue-green vs canary | Instant full switch vs gradual traffic shift |

### 25.2 Numbers to remember

| Item | Value |
|---|---|
| NodePort range | 30000-32767 |
| Kubernetes API / etcd / kubelet ports | 6443 / 2379-2380 / 10250 |
| Default Pod termination grace period | 30 s |
| `docker stop` default timeout | 10 s |
| Exit codes | 137 = SIGKILL/OOM, 143 = SIGTERM, 127 = not found, 126 = not executable |
| AWS reserved IPs per subnet | 5 |
| Spot interruption notice | 2 minutes |
| Lambda max duration | 15 minutes |
| 99.9% availability | ~43.2 min/month, ~8.76 h/year |
| 99.99% availability | ~4.32 min/month, ~52.6 min/year |
| Kubernetes support window | Latest 3 minor versions, ~14 months each |
| Helm revisions kept by default | 10 (`--history-max`) |
| Ansible default forks | 5 |
| Terraform default parallelism | 10 |

### 25.3 Commands to have at your fingertips

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

---

## 26. References and Further Learning

**Official documentation (always the source of truth)**
- Kubernetes docs and blog: https://kubernetes.io/docs/ and https://kubernetes.io/blog/
  (v1.37 "Garhwal" release post, 26 Aug 2026; Ingress NGINX retirement announcement, Nov 2025)
- Gateway API: https://gateway-api.sigs.k8s.io/
- Helm: https://helm.sh/docs/ (Helm 4 overview and release post, Nov 2025)
- Terraform: https://developer.hashicorp.com/terraform | OpenTofu: https://opentofu.org/docs/
- Ansible: https://docs.ansible.com/
- AWS docs: https://docs.aws.amazon.com/ | Well-Architected Framework: https://aws.amazon.com/architecture/well-architected/
- Prometheus: https://prometheus.io/docs/ | OpenTelemetry: https://opentelemetry.io/docs/ | Grafana: https://grafana.com/docs/
- Argo CD: https://argo-cd.readthedocs.io/ | Flux: https://fluxcd.io/flux/
- DORA research and metrics: https://dora.dev/ (metrics history, 2025 State of AI-assisted Software Development report)
- Google SRE books (free online): https://sre.google/books/
- Security: OWASP https://owasp.org/ | SLSA https://slsa.dev/ | Sigstore https://www.sigstore.dev/

**Books**
- *The Phoenix Project* and *The DevOps Handbook* (Gene Kim et al.)
- *Accelerate* (Forsgren, Humble, Kim)
- *Site Reliability Engineering* and *The Site Reliability Workbook* (Google)
- *Kubernetes Up & Running*; *Terraform: Up & Running*

**Practice and roadmaps**
- DevOps roadmap: https://roadmap.sh/devops
- Free browser-based Kubernetes/Linux labs: https://killercoda.com/
- *Kubernetes The Hard Way*: https://github.com/kelseyhightower/kubernetes-the-hard-way
- Useful certifications: CKA / CKAD / CKS, AWS Solutions Architect Associate / DevOps Engineer Professional, HashiCorp Terraform Associate.

> **Final tip:** read a section, then do it hands-on the same day. Concepts you've broken and fixed yourself are the ones you'll explain confidently in an interview. Good luck! 🚀
