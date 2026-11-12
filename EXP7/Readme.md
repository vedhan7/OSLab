Experiment 7: Memory Management (Contiguous Allocation)
7(a) Memory Management - Best Fit
/*
 * Memory Management Scheme using Best Fit
 * -----------------------------------------
 * Allocates each process to the smallest block that is large enough
 * to hold it (i.e., the block that leaves the least fragmentation).
 */
 
#include <stdio.h>
 
int main() {
    int nBlocks, nProcesses;
    int blockSize[50], processSize[50];
    int allocation[50];      // allocation[i] = block index allocated to process i (-1 if none)
    int blockFree[50];       // 1 = block still free, 0 = block used
    int i, j;
 
    printf("Enter number of memory blocks: ");
    scanf("%d", &nBlocks);
 
    printf("Enter sizes of the %d blocks:\n", nBlocks);
    for (i = 0; i < nBlocks; i++) {
        printf("Block %d size: ", i + 1);
        scanf("%d", &blockSize[i]);
        blockFree[i] = 1;
    }
 
    printf("Enter number of processes: ");
    scanf("%d", &nProcesses);
 
    printf("Enter sizes of the %d processes:\n", nProcesses);
    for (i = 0; i < nProcesses; i++) {
        printf("Process %d size: ", i + 1);
        scanf("%d", &processSize[i]);
        allocation[i] = -1;
    }
 
    /* Best Fit allocation logic */
    for (i = 0; i < nProcesses; i++) {
        int bestIdx = -1;
 
        for (j = 0; j < nBlocks; j++) {
            if (blockFree[j] && blockSize[j] >= processSize[i]) {
                if (bestIdx == -1 || blockSize[j] < blockSize[bestIdx]) {
                    bestIdx = j;
                }
            }
        }
 
        if (bestIdx != -1) {
            allocation[i] = bestIdx;
            blockFree[bestIdx] = 0;   // mark block as used
        }
    }
 
    /* Display results */
    printf("\nProcess No.\tProcess Size\tBlock No.\tFragmentation\n");
    for (i = 0; i < nProcesses; i++) {
        printf("P%d\t\t%d\t\t", i + 1, processSize[i]);
        if (allocation[i] != -1) {
            printf("B%d\t\t%d\n", allocation[i] + 1,
                   blockSize[allocation[i]] - processSize[i]);
        } else {
            printf("Not Allocated\t-\n");
        }
    }
 
    return 0;
}
 
7(b) Memory Management - First Fit
/*
 * Memory Management Scheme using First Fit
 * -----------------------------------------
 * Allocates each process to the first free block that is large enough
 * to hold it, scanning blocks in order.
 */
 
#include <stdio.h>
 
int main() {
    int nBlocks, nProcesses;
    int blockSize[50], processSize[50];
    int allocation[50];      // allocation[i] = block index allocated to process i (-1 if none)
    int blockFree[50];       // 1 = block still free, 0 = block used
    int i, j;
 
    printf("Enter number of memory blocks: ");
    scanf("%d", &nBlocks);
 
    printf("Enter sizes of the %d blocks:\n", nBlocks);
    for (i = 0; i < nBlocks; i++) {
        printf("Block %d size: ", i + 1);
        scanf("%d", &blockSize[i]);
        blockFree[i] = 1;
    }
 
    printf("Enter number of processes: ");
    scanf("%d", &nProcesses);
 
    printf("Enter sizes of the %d processes:\n", nProcesses);
    for (i = 0; i < nProcesses; i++) {
        printf("Process %d size: ", i + 1);
        scanf("%d", &processSize[i]);
        allocation[i] = -1;
    }
 
    /* First Fit allocation logic */
    for (i = 0; i < nProcesses; i++) {
        for (j = 0; j < nBlocks; j++) {
            if (blockFree[j] && blockSize[j] >= processSize[i]) {
                allocation[i] = j;
                blockFree[j] = 0;   // mark block as used
                break;              // stop at the FIRST block that fits
            }
        }
    }
 
    /* Display results */
    printf("\nProcess No.\tProcess Size\tBlock No.\tFragmentation\n");
    for (i = 0; i < nProcesses; i++) {
        printf("P%d\t\t%d\t\t", i + 1, processSize[i]);
        if (allocation[i] != -1) {
            printf("B%d\t\t%d\n", allocation[i] + 1,
                   blockSize[allocation[i]] - processSize[i]);
        } else {
            printf("Not Allocated\t-\n");
        }
    }
 
    return 0;
}
 
7© Memory Management - Worst Fit
/*
 * Memory Management Scheme using Worst Fit
 * -----------------------------------------
 * Allocates each process to the LARGEST free block that is big enough
 * to hold it, leaving the biggest possible leftover space.
 */
 
#include <stdio.h>
 
int main() {
    int nBlocks, nProcesses;
    int blockSize[50], processSize[50];
    int allocation[50];      // allocation[i] = block index allocated to process i (-1 if none)
    int blockFree[50];       // 1 = block still free, 0 = block used
    int i, j;
 
    printf("Enter number of memory blocks: ");
    scanf("%d", &nBlocks);
 
    printf("Enter sizes of the %d blocks:\n", nBlocks);
    for (i = 0; i < nBlocks; i++) {
        printf("Block %d size: ", i + 1);
        scanf("%d", &blockSize[i]);
        blockFree[i] = 1;
    }
 
    printf("Enter number of processes: ");
    scanf("%d", &nProcesses);
 
    printf("Enter sizes of the %d processes:\n", nProcesses);
    for (i = 0; i < nProcesses; i++) {
        printf("Process %d size: ", i + 1);
        scanf("%d", &processSize[i]);
        allocation[i] = -1;
    }
 
    /* Worst Fit allocation logic */
    for (i = 0; i < nProcesses; i++) {
        int worstIdx = -1;
 
        for (j = 0; j < nBlocks; j++) {
            if (blockFree[j] && blockSize[j] >= processSize[i]) {
                if (worstIdx == -1 || blockSize[j] > blockSize[worstIdx]) {
                    worstIdx = j;
                }
            }
        }
 
        if (worstIdx != -1) {
            allocation[i] = worstIdx;
            blockFree[worstIdx] = 0;   // mark block as used
        }
    }
 
    /* Display results */
    printf("\nProcess No.\tProcess Size\tBlock No.\tFragmentation\n");
    for (i = 0; i < nProcesses; i++) {
        printf("P%d\t\t%d\t\t", i + 1, processSize[i]);
        if (allocation[i] != -1) {
            printf("B%d\t\t%d\n", allocation[i] + 1,
                   blockSize[allocation[i]] - processSize[i]);
        } else {
            printf("Not Allocated\t-\n");
        }
    }
 
    return 0;
}
 
