Experiment 4: Shared Memory (IPC)
4(a) Shared Memory - Writer Process
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
 
int main()
{
    // ftok to generate unique key
    key_t key = ftok("shmfile",65);
 
    // shmget returns an identifier in shmid
    int shmid = shmget(key,1024,0666|IPC_CREAT);
 
    // shmat to attach to shared memory
    char *str = (char*) shmat(shmid,(void*)0,0);
 
    printf("Write Data : ");
    gets(str);
 
    printf("Data written in memory: %s\n",str);
 
    //detach from shared memory
    shmdt(str);
 
    return 0;
}
 
4(b) Shared Memory - Reader Process
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
 
int main()
{
    // ftok to generate unique key
    key_t key = ftok("shmfile",65);
 
    // shmget returns an identifier in shmid
    int shmid = shmget(key,1024,0666|IPC_CREAT);
 
    // shmat to attach to shared memory
    char *str = (char*) shmat(shmid,(void*)0,0);
 
    printf("Data read from memory: %s\n",str);
 
    //detach from shared memory
    shmdt(str);
 
    // destroy the shared memory
    shmctl(shmid,IPC_RMID,NULL);
 
    return 0;
}
 
