# **Linux Process Management Commands**

This document explains various Linux commands to manage and monitor processes.

---

## 🌲 **1. List Processes: `ps aux`**
- **ps** — The process status tool that shows information about running processes.

- **a** — Lists processes of all users, not just the current user’s processes. This ensures you see everyone’s processes, including system and other users’ services.

- **u** — Outputs processes in a user-oriented format. It adds information about the user/owner of the process, and displays CPU, memory usage, and start time clearly.

- **x** — Shows processes that are not attached to a terminal (for instance, background daemons or services running independently).

**Example Output:**
```
USER       PID  %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 167500  1100 ?        Ss   Sep25   0:05 /sbin/init
vibhu     1234  1.2  1.5 274532 15632 ?        Sl   10:15   0:12 /usr/bin/python3 script.py
mysql     2001  0.5  2.0 450000 20988 ?        Ssl  Sep25   1:02 /usr/sbin/mysqld
```

![alt text](<../../images/Screenshot from 2025-09-27 21-12-46.png>)
---

## 🌳 **2. Process Tree: `pstree -p`**
 It visualizes the parent-child relationship between processes as a tree.

 **Example Output:**
```
systemd(1)─┬─NetworkManager(778)
           ├─sshd(895)─┬─sshd(1023)───bash(1024)───pstree(1101)
           ├─mysqld(2001)
           └─python3(1234)
```
![alt text](<../../images/Screenshot from 2025-09-27 21-16-29.png>)
---

## 📊 **3. Real-Time Monitoring: `top`**
 The top command shows an ongoing, dynamically updating list of processes sorted by CPU usage by default.

 **Example Output:**
```
top - 10:40:46 up  22 min,  1 user,  load average: 0.22, 0.38, 0.27
Tasks: 211 total,   1 running, 210 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.3 sy,  0.0 ni, 99.5 id,  0.0 wa,  0.0 hi,  0.1 si,  0.0 st
MiB Mem :   5776.9 total,   2161.6 free,   1138.6 used,   2799.3 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.   4638.2 avail Mem 

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
  1992 sameerb+  20   0 4967692 411396 141592 S   3.6  7.0   0:59.64 gnome-s+
 12065 sameerb+  20   0  553448  52396  42096 S   0.5  0.9   0:00.63 gnome-t+
  1394 root      20   0  316824   8788   7380 S   0.3  0.1   0:00.39 upowerd
  2568 sameerb+  20   0 2947568  67764  51612 S   0.3  1.1   0:01.47 gjs
 11299 root      20   0       0      0      0 I   0.3  0.0   0:01.92 kworker+
 12078 sameerb+  20   0   14500   5924   3748 R   0.3  0.1   0:00.06 top
     1 root      20   0  23212  14144   9664 S   0.0  0.2   0:09.05 systemd
     2 root      20   0       0      0      0 S   0.0  0.0   0:00.02 kthreadd
     3 root      20   0       0      0      0 I   0.0  0.0   0:00.00 pool_wo+
     4 root      20   0       0      0      0 I   0.0  0.0   0:00.00 kworker+
     5 root      0  -20       0      0      0 I   0.0  0.0   0:00.00 kworker+
     6 root      0  -20       0      0      0 I   0.0  0.0   0:00.00 kworker+
     7 root      0  -20       0      0      0 I   0.0  0.0   0:00.00 kworker+
     8 root      0  -20       0      0      0 I   0.0  0.0   0:00.00 kworker+
    10 root      0  -20       0      0      0 I   0.0  0.0   0:00.00 kworker+
    11 root      0  -20       0      0      0 I   0.0  0.0   0:00.00 kworker+
    13 root      0  -20       0      0      0 I   0.0  0.0   0:00.00 kworker+

```
👉 Press `q` to quit.


![alt text](<../../images/Screenshot from 2025-09-27 22-07-33.png>)
---

## ⚡ **4. Adjust Process Priority- nice and renice **

- nice command Used to start a process with a specific priority:
 ```
nice -n 10 sleep 300 &
[1] 3050
```
- renice command Changes the priority of an existing running process.  
```
renice -n -5 -p 3050
3050 (process ID) old priority 10, new priority -5
```
![alt text](<../../images/Screenshot from 2025-09-27 22-20-56.png>)
---

## 🔧 **5. CPU Affinity: `taskset`**
- Bind Process to Specific CPU Cores (CPU Affinity):
```
taskset -cp 3050
pid 3050's current affinity list: 0-3
```
- Restrict to core 1 only:
```
taskset -cp 1 3050
pid 3050's current affinity list: 1
```
![alt text](<../../images/Screenshot from 2025-09-27 22-24-30.png>)
---

## 📂 **6. I/O Scheduling Priority: `ionice`**
```
 Controls the priority of a process’s disk I/O operations.
 ionice -c 3 -p 3050
  
  Makes process 3050 perform disk IO only when no other tasks need it.
```
  👉 Class 3 (idle) → Process only gets I/O when system is idle.

---

## 📑 **7. File Descriptors: `lsof`**


```bash
lsof -p 4139 | head -5
```

-  Shows all files and resources that a process has opened (including network sockets, directories, regular files).

![alt text](<../../images/Screenshot from 2025-09-27 22-37-18.png>)
---

#### 8. 🐛 Trace System Calls of a Process

```bash
strace -p 4139
```

-  strace monitors all system calls executed by a process.

- Useful for debugging why a process is stuck or what system resources it is using.

#### 9. 📡 Find Process Using a Port

```bash
sudo fuser -n tcp 4259
```
-  Find Process Using a Network Port.Finds process ID(s) that are listening on or using a TCP port.

-  When a service fails to start due to a busy port, this command reveals what process occupies that port.

-  sudo fuser -n tcp 4259
  
- Output: 4259/tcp: 3321 (process 3321 is using port 4259).

#### 10. 📊 Per-Process Statistics

```bash
pidstat -p 4139 2 3
```
- Tracks CPU usage and other stats for specified processes repeatedly.

- Detect intermittent performance problems or CPU spikes.

- Reports stats every 2 seconds, 3 times, for process 4139.

![alt text](<../../images/Screenshot from 2025-09-27 22-51-55.png>)
---

 #### 11. 🔐 Control Groups (cgroups) for Resource Limits

 -  Kernel feature to group processes and control CPU, memory, and I/O usage.

 1. Create a cgroup:

- Before proceeding to create control groups (cgroups), it is necessary to first install the relevant cgroup tools on your Linux system. These tools, such as cgcreate, cgset, and cgexec, are essential utilities that allow for the easy definition, configuration, and management of resource limits—like CPU time, memory, and disk I/O—for processes within the kernel's cgroup framework.  This prerequisite installation ensures that the commands you will use to set up and manage cgroups are available and functional.

```bash
sudo cgcreate -g cpu,memory:/testgroup
```

 ![alt text](<../../images/Screenshot from 2025-09-27 22-56-40.png>) 

 2. Limit CPU and Memory:

 - First indentify if your system is running on cgroup v1 or cgroup v2
  - If running on cgroup v1,files like cpu.cfs_quota_us and memory.limit_in_bytes exist but if on v2 cpu.max,memory.max exist
- cgroup v1 or cgroup v2 can be identifed using 

```bash
mount | grep cgroup
```

 ![alt text](<../../images/Screenshot from 2025-09-27 23-00-56.png>) 
- My system is running on cgroup v2

```bash
echo 50000 | sudo tee /sys/fs/cgroup/cpu/testgroup/cpu.cfs_quota_us 
echo 100M | sudo tee /sys/fs/cgroup/memory/testgroup/memory.limit_in_bytes  
```

- Add process (PID 4139) to cgroup:

```bash
 echo 3050 | sudo tee /sys/fs/cgroup/cpu/testgroup/tasks  
 ```

- cgroups Used for resource fairness, preventing any one process or group from exhausting system resources, especially important in containerized environments.
