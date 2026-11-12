Experiment 8: Disk Scheduling Algorithms
8(a) Disk Scheduling - FCFS
#include <stdio.h>
#include <stdlib.h>
 
void fcfs(int requests[], int n, int head) {
    int total = 0, current = head;
 
    printf("\n--- FCFS Disk Scheduling ---\n");
    printf("Seek Sequence: %d", head);
 
    for (int i = 0; i < n; i++) {
        total += abs(requests[i] - current);
        current = requests[i];
        printf(" -> %d", current);
    }
 
    printf("\nTotal Seek Time (FCFS): %d cylinders\n", total);
}
 
int main() {
    int n, head;
    int requests[100];
 
    printf("Enter the number of requests: ");
    scanf("%d", &n);
 
    printf("Enter the request queue: ");
    for (int i = 0; i < n; i++)
        scanf("%d", &requests[i]);
 
    printf("Enter the initial head position: ");
    scanf("%d", &head);
 
    fcfs(requests, n, head);
 
    return 0;
}
 
8(b) Disk Scheduling - SSTF
#include <stdio.h>
#include <stdlib.h>
 
void sstf(int requests[], int n, int head) {
    int visited[100] = {0};
    int total = 0, current = head;
 
    printf("\n--- SSTF Disk Scheduling ---\n");
    printf("Seek Sequence: %d", head);
 
    for (int count = 0; count < n; count++) {
        int minDist = 100000, index = -1;
 
        for (int i = 0; i < n; i++) {
            if (!visited[i] && abs(requests[i] - current) < minDist) {
                minDist = abs(requests[i] - current);
                index = i;
            }
        }
 
        visited[index] = 1;
        total += minDist;
        current = requests[index];
        printf(" -> %d", current);
    }
 
    printf("\nTotal Seek Time (SSTF): %d cylinders\n", total);
}
 
int main() {
    int n, head;
    int requests[100];
 
    printf("Enter the number of requests: ");
    scanf("%d", &n);
 
    printf("Enter the request queue: ");
    for (int i = 0; i < n; i++)
        scanf("%d", &requests[i]);
 
    printf("Enter the initial head position: ");
    scanf("%d", &head);
 
    sstf(requests, n, head);
 
    return 0;
}
 
8© Disk Scheduling - SCAN
#include <stdio.h>
#include <stdlib.h>
 
int compare(const void *a, const void *b) {
    return (*(int *)a - *(int *)b);
}
 
void scan(int requests[], int n, int head, int disk_size, int direction) {
    int total = 0, current = head;
    int sorted[100];
 
    for (int i = 0; i < n; i++)
        sorted[i] = requests[i];
    sorted[n] = head;
    qsort(sorted, n + 1, sizeof(int), compare);
 
    int pos = 0;
    for (int i = 0; i <= n; i++)
        if (sorted[i] == head) { pos = i; break; }
 
    printf("\n--- SCAN Disk Scheduling ---\n");
    printf("Direction: %s\n", direction == 1 ? "Towards higher cylinders" : "Towards lower cylinders");
    printf("Seek Sequence: %d", head);
 
    if (direction == 1) {
        /* Move right to the end of disk, then reverse */
        for (int i = pos + 1; i <= n; i++) {
            total += abs(sorted[i] - current);
            current = sorted[i];
            printf(" -> %d", current);
        }
        if (current != disk_size - 1) {
            total += abs((disk_size - 1) - current);
            current = disk_size - 1;
            printf(" -> %d", current);
        }
        for (int i = pos - 1; i >= 0; i--) {
            total += abs(sorted[i] - current);
            current = sorted[i];
            printf(" -> %d", current);
        }
    } else {
        /* Move left to the start of disk, then reverse */
        for (int i = pos - 1; i >= 0; i--) {
            total += abs(sorted[i] - current);
            current = sorted[i];
            printf(" -> %d", current);
        }
        if (current != 0) {
            total += abs(current - 0);
            current = 0;
            printf(" -> %d", current);
        }
        for (int i = pos + 1; i <= n; i++) {
            total += abs(sorted[i] - current);
            current = sorted[i];
            printf(" -> %d", current);
        }
    }
 
    printf("\nTotal Seek Time (SCAN): %d cylinders\n", total);
}
 
int main() {
    int n, head, disk_size, direction;
    int requests[100];
 
    printf("Enter the maximum number of cylinders: ");
    scanf("%d", &disk_size);
 
    printf("Enter the number of requests: ");
    scanf("%d", &n);
 
    printf("Enter the request queue: ");
    for (int i = 0; i < n; i++)
        scanf("%d", &requests[i]);
 
    printf("Enter the initial head position: ");
    scanf("%d", &head);
 
    printf("Enter direction (1 = towards higher, 0 = towards lower): ");
    scanf("%d", &direction);
 
    scan(requests, n, head, disk_size, direction);
 
    return 0;
}
 
