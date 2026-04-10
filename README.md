 CPU Scheduling Simulator

This project implements CPU Scheduling Algorithms:

1. First Come First Serve (FCFS)
2. Shortest Job First (SJF)
3. Round Robin (RR)
4. Shortest Remaining Time First (SRTF)

 Input
- Process ID
- Arrival Time
- Burst Time

 Output
- Waiting Time
- Turnaround Time
- Gantt Chart

First Come First Serve (FCFS)
Information:
Processes executed in arrival order
Non-preemptive
Simple but large waiting time
Algorithm:
Sort by arrival time
Execute first process
Compute completion time
Compute waiting time
Compute turnaround time
FCFS Code (Python):
# FCFS Scheduling

n = int(input("Enter number of processes: "))

pid = []
arrival = []
burst = []

for i in range(n):
    pid.append(input("Process ID: "))
    arrival.append(int(input("Arrival Time: ")))
    burst.append(int(input("Burst Time: ")))

waiting = [0]*n
turnaround = [0]*n

for i in range(1,n):
    waiting[i] = waiting[i-1] + burst[i-1]

for i in range(n):
    turnaround[i] = waiting[i] + burst[i]

print("\nProcess  Waiting  Turnaround")
for i in range(n):
    print(pid[i],"\t",waiting[i],"\t",turnaround[i])
    
Shortest Job First (SJF)
Information:
Select smallest burst time
Non-preemptive
Better than FCFS
Algorithm:
Sort by burst time
Execute shortest job first
Compute waiting time
Compute turnaround time
SJF Code
# SJF Scheduling

n = int(input("Enter number of processes: "))

pid=[]
burst=[]

for i in range(n):
    pid.append(input("Process ID: "))
    burst.append(int(input("Burst Time: ")))

# sorting
for i in range(n):
    for j in range(i+1,n):
        if burst[i] > burst[j]:
            burst[i],burst[j] = burst[j],burst[i]
            pid[i],pid[j] = pid[j],pid[i]

waiting=[0]*n
turnaround=[0]*n

for i in range(1,n):
    waiting[i]=waiting[i-1]+burst[i-1]

for i in range(n):
    turnaround[i]=waiting[i]+burst[i]

print("\nProcess Waiting Turnaround")
for i in range(n):
    print(pid[i],"\t",waiting[i],"\t",turnaround[i])
Round Robin (RR)
Information:
Each process gets time quantum
Preemptive scheduling
Fair CPU allocation
Algorithm:
Set time quantum
Execute process for quantum
Remaining time → queue
Repeat until complete
Round Robin Code
# Round Robin Scheduling

n = int(input("Enter number of processes: "))
quantum = int(input("Enter Time Quantum: "))

burst = list(map(int,input("Enter burst times: ").split()))

remaining = burst[:]
waiting = [0]*n
time = 0

while True:
    done = True
    for i in range(n):
        if remaining[i] > 0:
            done = False
            if remaining[i] > quantum:
                time += quantum
                remaining[i] -= quantum
            else:
                time += remaining[i]
                waiting[i] = time - burst[i]
                remaining[i] = 0
    if done:
        break

turnaround = [burst[i] + waiting[i] for i in range(n)]

print("\nProcess Waiting Turnaround")
for i in range(n):
    print("P",i+1,"\t",waiting[i],"\t",turnaround[i])

 Shortest Remaining Time First (SRTF) — Preemptive SJF
 Information
Preemptive version of SJF
Choose shortest remaining time
If new process arrives → preempt
Algorithm
Select shortest remaining time
Execute for 1 unit
Check new arrival
Preempt if needed
Repeat until done
 SRTF Code 
 # SRTF Scheduling

n = int(input("Enter number of processes: "))

arrival = list(map(int,input("Arrival time: ").split()))
burst = list(map(int,input("Burst time: ").split()))

remaining = burst[:]
complete = 0
time = 0
waiting = [0]*n
turnaround = [0]*n

while complete != n:
    min_bt = 999
    shortest = 0
    check = False

    for i in range(n):
        if arrival[i] <= time and remaining[i] < min_bt and remaining[i] > 0:
            min_bt = remaining[i]
            shortest = i
            check = True

    if not check:
        time += 1
        continue

    remaining[shortest] -= 1

    if remaining[shortest] == 0:
        complete += 1
        finish = time + 1
        waiting[shortest] = finish - burst[shortest] - arrival[shortest]

        if waiting[shortest] < 0:
            waiting[shortest] = 0

    time += 1

for i in range(n):
    turnaround[i] = burst[i] + waiting[i]

print("\nProcess Waiting Turnaround")
for i in range(n):
    print("P",i+1,"\t",waiting[i],"\t",turnaround[i])
    
Author
B.Nehasri
24MIP10182
