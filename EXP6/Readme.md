Experiment 6: Page Replacement Algorithms
6(a) Page Replacement - LRU
#include <stdio.h>
 
int main()
{
    int frames[10], pages[50], counter[10];
    int n, f, i, j, k;
    int page_faults = 0, page_hits = 0;
    int time = 0, found, pos;
 
    printf("Enter the number of frames: ");
    scanf("%d", &f);
 
    printf("Enter the number of pages: ");
    scanf("%d", &n);
 
    printf("Enter the page reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &pages[i]);
 
    /* Initialize frames */
    for (i = 0; i < f; i++)
    {
        frames[i] = -1;
        counter[i] = 0;
    }
 
    printf("\nPage\tFrames\t\tStatus\n");
 
    for (i = 0; i < n; i++)
    {
        found = 0;
 
        /* Check whether page is already present */
        for (j = 0; j < f; j++)
        {
            if (frames[j] == pages[i])
            {
                found = 1;
                time++;
                counter[j] = time;
                page_hits++;
                break;
            }
        }
 
        /* Page fault */
        if (found == 0)
        {
            page_faults++;
            time++;
 
            /* Check for empty frame */
            pos = -1;
            for (j = 0; j < f; j++)
            {
                if (frames[j] == -1)
                {
                    pos = j;
                    break;
                }
            }
 
            /* If no empty frame, find LRU page */
            if (pos == -1)
            {
                pos = 0;
 
                for (j = 1; j < f; j++)
                {
                    if (counter[j] < counter[pos])
                        pos = j;
                }
            }
 
            frames[pos] = pages[i];
            counter[pos] = time;
        }
 
        /* Display frames */
        printf("%d\t", pages[i]);
 
        for (k = 0; k < f; k++)
        {
            if (frames[k] == -1)
                printf("- ");
            else
                printf("%d ", frames[k]);
        }
 
        if (found)
            printf("\tHit");
        else
            printf("\tPage Fault");
 
        printf("\n");
    }
 
    printf("\nTotal Page Faults = %d", page_faults);
    printf("\nTotal Page Hits   = %d\n", page_hits);
 
    return 0;
}
 
6(b) Page Replacement - Optimal
#include <stdio.h>
 
int isPresent(int cache[], int cacheSize, int page)
{
    int i;
 
    for (i = 0; i < cacheSize; i++)
    {
        if (cache[i] == page)
            return 1;
    }
 
    return 0;
}
 
/* Find how far a page is used in the future */
int findLength(int ref[], int n, int current, int page)
{
    int i;
 
    for (i = current + 1; i < n; i++)
    {
        if (ref[i] == page)
            return i - current;
    }
 
    /* Page is not used again */
    return 9999;
}
 
/* Find the page with maximum future distance */
int findMax(int length[], int cacheSize)
{
    int i, max = 0;
 
    for (i = 1; i < cacheSize; i++)
    {
        if (length[i] > length[max])
            max = i;
    }
 
    return max;
}
 
int main()
{
    int ref[50], cache[10], length[10];
    int n, cacheSize;
    int i, j, index;
    int pageFaults = 0;
 
    printf("Enter the number of pages: ");
    scanf("%d", &n);
 
    printf("Enter the reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &ref[i]);
 
    printf("Enter the cache size: ");
    scanf("%d", &cacheSize);
 
    /* Initialize cache */
    for (i = 0; i < cacheSize; i++)
        cache[i] = -1;
 
    printf("\nPage\tCache\t\tStatus\n");
 
    for (i = 0; i < n; i++)
    {
        /* Page is already present */
        if (isPresent(cache, cacheSize, ref[i]))
        {
            printf("%d\t", ref[i]);
 
            for (j = 0; j < cacheSize; j++)
                printf("%d ", cache[j]);
 
            printf("\tHit\n");
        }
        else
        {
            /* Fill empty cache first */
            index = -1;
 
            for (j = 0; j < cacheSize; j++)
            {
                if (cache[j] == -1)
                {
                    index = j;
                    break;
                }
            }
 
            /* If cache is full, find optimal replacement */
            if (index == -1)
            {
                for (j = 0; j < cacheSize; j++)
                {
                    length[j] = findLength(ref, n, i, cache[j]);
                }
 
                index = findMax(length, cacheSize);
            }
 
            cache[index] = ref[i];
            pageFaults++;
 
            printf("%d\t", ref[i]);
 
            for (j = 0; j < cacheSize; j++)
                printf("%d ", cache[j]);
 
            printf("\tPage Fault\n");
        }
    }
 
    printf("\nTotal Page Faults = %d\n", pageFaults);
 
    return 0;
}
 
6© Page Replacement - FIFO
#include <stdio.h>
 
int main()
{
    int pages[50], frames[10];
    int n, f;
    int i, j;
    int pointer = 0;
    int pageFaults = 0, pageHits = 0;
    int found;
 
    printf("Enter the number of pages: ");
    scanf("%d", &n);
 
    printf("Enter the page reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &pages[i]);
 
    printf("Enter the number of frames: ");
    scanf("%d", &f);
 
    /* Initialize frames */
    for (i = 0; i < f; i++)
        frames[i] = -1;
 
    printf("\nPage\tFrames\t\tStatus\n");
 
    for (i = 0; i < n; i++)
    {
        found = 0;
 
        /* Check whether page is already in memory */
        for (j = 0; j < f; j++)
        {
            if (frames[j] == pages[i])
            {
                found = 1;
                pageHits++;
                break;
            }
        }
 
        /* Page fault */
        if (found == 0)
        {
            frames[pointer] = pages[i];
 
            /* Move pointer in circular manner */
            pointer = (pointer + 1) % f;
 
            pageFaults++;
        }
 
        /* Display current page and frames */
        printf("%d\t", pages[i]);
 
        for (j = 0; j < f; j++)
        {
            if (frames[j] == -1)
                printf("- ");
            else
                printf("%d ", frames[j]);
        }
 
        if (found)
            printf("\tHit");
        else
            printf("\tPage Fault");
 
        printf("\n");
    }
 
    printf("\nTotal Page Faults = %d", pageFaults);
    printf("\nTotal Page Hits   = %d\n", pageHits);
 
    return 0;
}
 
