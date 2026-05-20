## Project URL

https://roadmap.sh/projects/server-stats

creation of command :

#!/bin/bash

echo "======================================"
echo "        SERVER PERFORMANCE STATS       "
echo "======================================"
echo "Timestamp: $(date)"
echo ""

# --------------------------------------
# CPU Usage
# --------------------------------------
echo "---- CPU Usage ----"
CPU_IDLE=$(top -bn1 | grep "Cpu(s)" | awk '{print $8}' | cut -d. -f1)
CPU_USAGE=$((100 - CPU_IDLE))
echo "Total CPU Usage: ${CPU_USAGE}%"
echo ""

# --------------------------------------
# Memory Usage
# --------------------------------------
echo "---- Memory Usage ----"
MEM_TOTAL=$(free -m | awk '/Mem:/ {print $2}')
MEM_USED=$(free -m | awk '/Mem:/ {print $3}')
MEM_FREE=$(free -m | awk '/Mem:/ {print $4}')
MEM_PERCENT=$(free | awk '/Mem:/ {printf("%.2f"), $3/$2 * 100}')

echo "Total: ${MEM_TOTAL} MB"
echo "Used : ${MEM_USED} MB"
echo "Free : ${MEM_FREE} MB"
echo "Usage: ${MEM_PERCENT}%"
echo ""

# --------------------------------------
# Disk Usage
# --------------------------------------
echo "---- Disk Usage ----"
df -h --total | grep 'total' | awk '
{
    print "Total: " $2;
    print "Used : " $3;
    print "Free : " $4;
    print "Usage: " $5
}'
echo ""

# --------------------------------------
# Top 5 Processes by CPU
# --------------------------------------
echo "---- Top 5 Processes by CPU ----"
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head -n 6
echo ""

# --------------------------------------
# Top 5 Processes by Memory
# --------------------------------------
echo "---- Top 5 Processes by Memory ----"
ps -eo pid,ppid,cmd,%mem --sort=-%mem | head -n 6
echo ""

# --------------------------------------
# Additional Stats (Stretch Goal)
# --------------------------------------

echo "---- OS Version ----"
cat /etc/os-release | grep PRETTY_NAME | cut -d= -f2 | tr -d '"'
echo ""

echo "---- Uptime ----"
uptime -p
echo ""

echo "---- Load Average ----"
uptime | awk -F'load average:' '{ print $2 }'
echo ""

echo "---- Logged In Users ----"
who
echo ""

echo "---- Failed Login Attempts (last 5) ----"
if command -v lastb &> /dev/null; then
    lastb | head -n 5
else
    echo "lastb command not available"
fi
echo ""

echo "======================================"
echo "          END OF REPORT               "
echo "======================================"



To excute the file of the svript 


chmod +x server-stats.sh
./server-stats.sh

End of the result of the script 


======================================
        SERVER PERFORMANCE STATS
======================================
Timestamp: Thu May 20 14:30:01 IST 2026

---- CPU Usage ----
Total CPU Usage: 18%

---- Memory Usage ----
Total: 7977 MB
Used : 3250 MB
Free : 2100 MB
Usage: 40.75%

---- Disk Usage ----
Total: 100G
Used : 45G
Free : 50G
Usage: 48%

---- Top 5 Processes by CPU ----
  PID  PPID CMD                         %CPU
 2345     1 python app.py               25.3
 1987     1 java -jar service.jar       18.1
 1672     1 nginx                       10.2
 1455     1 mysqld                      7.5
 1123     1 node server.js              5.8

---- Top 5 Processes by Memory ----
  PID  PPID CMD                         %MEM
 1987     1 java -jar service.jar       22.4
 2345     1 python app.py               18.7
 1455     1 mysqld                      15.3
 1672     1 nginx                       8.9
 1123     1 node server.js              7.8

---- OS Version ----
Ubuntu 22.04.3 LTS

---- Uptime ----
up 3 days, 4 hours, 12 minutes

---- Load Average ----
0.42, 0.35, 0.30

---- Logged In Users ----
root     pts/0        2026-05-20 12:10 (192.168.1.5)

---- Failed Login Attempts (last 5) ----
root     ssh:notty    192.168.1.10    Wed May 20 11:12 - 11:12  (00:00)

======================================
          END OF REPORT
======================================


