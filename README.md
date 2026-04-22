ps -eo user,pcpu | awk '
NR>1 {cpu[$1]+=$2}
END {
  for (u in cpu)
    printf "%-15s %.2f%%\n", u, cpu[u]
}' | sort -k2 -nr


ps -eo user,pid,pcpu,args --sort=-pcpu | head -20


ps -eo user,pcpu,args | grep oracle | grep -v grep | \
awk '{sum+=$2} END {print "Oracle CPU:", sum "%"}'

uick Answer (what to run now)

Run this to get CPU usage by user (live evidence):

ps -eo user,pcpu | awk '
NR>1 {cpu[$1]+=$2}
END {
  for (u in cpu)
    printf "%-15s %.2f%%\n", u, cpu[u]
}' | sort -k2 -nr

👉 This gives user-wise CPU consumption (your main proof)

🔥 Step 1 — Identify top consumers (process level)
ps -eo user,pid,pcpu,args --sort=-pcpu | head -20

👉 Look for:

oracle → expdp / DB activity
bods / dsuser / service account → BODS
Others → OS / unknown load
🔥 Step 2 — Separate Oracle vs BODS clearly

This is the key for your situation 👇

👉 Oracle CPU (expdp / DB workload)
ps -eo user,pcpu,args | grep oracle | grep -v grep | \
awk '{sum+=$2} END {print "Oracle CPU:", sum "%"}'
👉 BODS CPU (replace user if needed)
ps -eo user,pcpu,args | grep -i bods | grep -v grep | \
awk '{sum+=$2} END {print "BODS CPU:", sum "%"}'

👉 If BODS runs with a service account, use that username instead:

ps -eo user,pcpu | grep <bods_user>
🔥 Step 3 — Real-time monitoring (best proof during call)

Run:

topas

Press:

P   (process view)

👉 You will see:

CPU per process
USER column

✔ Take screenshot → strong evidence for Sunny / management

🔥 Step 4 — Correlate with system CPU

Run:

vmstat 1 5




ps aux | awk '
{cpu[$1]+=$3}
END {
  for (u in cpu)
    printf "%-15s %.2f%%\n", u, cpu[u]
}' | sort -k2 -nr


ps aux | sort -k3 -nr | head -20

ps aux | grep oracle | grep -v grep | awk '{sum+=$3} END {print "Oracle CPU:", sum "%"}'

ps aux | grep -i bods | grep -v grep | awk '{sum+=$3} END {print "BODS CPU:", sum "%"}'

ps aux | awk '$1=="dsuser" {sum+=$3} END {print "BODS CPU:", sum "%"}'
