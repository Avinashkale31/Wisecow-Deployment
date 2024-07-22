**Problem Statement 2**

1.Here's a basic bash script that monitors the health of a Linux system by checking CPU usage, memory usage, disk space, and running processes. It will send alerts to the console if any of the metrics exceed predefined thresholds.

**Scripts **

```bash
#!/bin/bash

# Define thresholds
CPU_THRESHOLD=80
MEMORY_THRESHOLD=80
DISK_THRESHOLD=90

# Log file
LOG_FILE="/var/log/system_health.log"

# Check CPU usage
cpu_usage=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}')
if (( $(echo "$cpu_usage > $CPU_THRESHOLD" |bc -l) )); then
    echo "CPU usage is high: ${cpu_usage}%" | tee -a $LOG_FILE
fi

# Check memory usage
memory_usage=$(free | grep Mem | awk '{print $3/$2 * 100.0}')
if (( $(echo "$memory_usage > $MEMORY_THRESHOLD" |bc -l) )); then
    echo "Memory usage is high: ${memory_usage}%" | tee -a $LOG_FILE
fi

# Check disk space usage
disk_usage=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')
if [ "$disk_usage" -gt "$DISK_THRESHOLD" ]; then
    echo "Disk space usage is high: ${disk_usage}%" | tee -a $LOG_FILE
fi

# Check for running processes (e.g., check if the number of processes exceeds a threshold)
process_count=$(ps aux | wc -l)
PROCESS_THRESHOLD=200
if [ "$process_count" -gt "$PROCESS_THRESHOLD" ]; then
    echo "Number of running processes is high: ${process_count}" | tee -a $LOG_FILE
fi

```
Usage :
1.Save the script to a file, e.g., system_health_monitor.sh.
2. Make the script executable with chmod +x system_health_monitor.sh.
3. Run the script with ./system_health_monitor.sh.

