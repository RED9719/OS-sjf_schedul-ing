# OS-sjf_schedul-ing
import pandas as pd

class Process:
    def __init__(self, pid, arrival_time, burst_time):
        self.pid = pid
        self.arrival_time = arrival_time
        self.burst_time = burst_time
        self.start_time = 0
        self.completion_time = 0
        self.turnaround_time = 0
        self.waiting_time = 0

def sjf_scheduling(processes):
    n = len(processes)
    processes.sort(key=lambda x: (x.arrival_time, x.burst_time))

    current_time = 0
    completed = 0
    while completed != n:
        ready_queue = [p for p in processes if p.arrival_time <= current_time and p.completion_time == 0]
        if ready_queue:
            ready_queue.sort(key=lambda x: x.burst_time)
            process = ready_queue[0]
            process.start_time = current_time
            process.completion_time = process.start_time + process.burst_time
            process.turnaround_time = process.completion_time - process.arrival_time
            process.waiting_time = process.turnaround_time - process.burst_time
            current_time = process.completion_time
            completed += 1
        else:
            current_time += 1

def print_process_info(processes):
    process_info = []
    for process in processes:
        process_info.append([process.pid, process.arrival_time, process.burst_time,
                             process.start_time, process.completion_time,
                             process.turnaround_time, process.waiting_time])

    df = pd.DataFrame(process_info, columns=["Process ID", "Arrival Time", "Burst Time",
                                             "Start Time", "Completion Time",
                                             "Turnaround Time", "Waiting Time"])
    print(df)

def main():
    processes = []
    num_processes = input("Enter the number of processes: ")
    while not num_processes.isdigit() or int(num_processes) <= 0:
        print("Invalid input. Please enter a positive integer.")
        num_processes = input("Enter the number of processes: ")

    num_processes = int(num_processes)

    for i in range(num_processes):
        pid = input(f"\nEnter Process ID for process {i + 1}: ")
        while not pid.isdigit() or int(pid) <= 0:
            print("Invalid input. Please enter a positive integer.")
            pid = input(f"\nEnter Process ID for process {i + 1}: ")
        pid = int(pid)

        arrival_time = input(f"Enter Arrival Time for process {pid}: ")
        while not arrival_time.isdigit() or int(arrival_time) < 0:
            print("Invalid input. Please enter a non-negative integer.")
            arrival_time = input(f"Enter Arrival Time for process {pid}: ")
        arrival_time = int(arrival_time)

        burst_time = input(f"Enter Burst Time for process {pid}: ")
        while not burst_time.isdigit() or int(burst_time) <= 0:
            print("Invalid input. Please enter a positive integer.")
            burst_time = input(f"Enter Burst Time for process {pid}: ")
        burst_time = int(burst_time)

        processes.append(Process(pid, arrival_time, burst_time))

    sjf_scheduling(processes)
    print("\nProcess Information after SJF Scheduling:")
    print_process_info(processes)

if __name__ == "__main__":
    main()
python sjf_scheduling.py
