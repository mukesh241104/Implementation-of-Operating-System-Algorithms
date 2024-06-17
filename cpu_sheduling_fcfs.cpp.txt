#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Process {
public:
    int id;
    int arrival_time;
    int burst_time;
    int start_time;
    int finish_time;
    int waiting_time;
    int turnaround_time;

    Process(int i, int a, int b) : id(i), arrival_time(a), burst_time(b), start_time(0), finish_time(0), waiting_time(0), turnaround_time(0) {}
};

bool compareArrivalTime(Process a, Process b) {
    return a.arrival_time < b.arrival_time;
}

void fcfsScheduling(vector<Process>& processes) {
    sort(processes.begin(), processes.end(), compareArrivalTime);
    int current_time = 0;
    double total_waiting_time = 0;
    double total_turnaround_time = 0;
    for (auto& process : processes) {
        if (current_time < process.arrival_time) {
            current_time = process.arrival_time;
        }
        process.start_time = current_time;
        process.finish_time = current_time + process.burst_time;
        current_time += process.burst_time;
        process.waiting_time = process.start_time - process.arrival_time;
        process.turnaround_time = process.finish_time - process.arrival_time;
        total_waiting_time += process.waiting_time;
        total_turnaround_time += process.turnaround_time;
    }
    double avg_waiting_time = total_waiting_time / processes.size();
    double avg_turnaround_time = total_turnaround_time / processes.size();

    cout << "Scheduled Processes (FCFS):" << endl;
    for (const auto& process : processes) {
        cout << "Process ID: " << process.id
             << ", Arrival Time: " << process.arrival_time
             << ", Burst Time: " << process.burst_time
             << ", Waiting Time: " << process.waiting_time
             << ", Turnaround Time: " << process.turnaround_time << endl;
    }
    cout << "Average Waiting Time: " << avg_waiting_time << endl;
    cout << "Average Turnaround Time: " << avg_turnaround_time << endl;
}

int main() {
    vector<Process> processes = {
        Process(1, 0, 4),
        Process(2, 1, 3),
        Process(3, 2, 1),
        Process(4, 3, 2)
    };

    fcfsScheduling(processes);

    return 0;
}
