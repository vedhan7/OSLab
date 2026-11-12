Experiment 2: Process Management and File System Calls
2(a) fork() - Parent and Child Process
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdlib.h>
 
int main()
{
    pid_t pid;
 
    pid = fork();
 
    if(pid < 0)
    {
        printf("Fork Failed\n");
        return 1;
    }
    else if(pid == 0)
    {
        printf("\nChild Process");
        printf("\nChild PID = %d", getpid());
        printf("\nParent PID = %d\n", getppid());
        exit(0);
    }
    else
    {
        wait(NULL);
        printf("\nParent Process");
        printf("\nParent PID = %d", getpid());
        printf("\nChild PID = %d\n", pid);
    }
 
    return 0;
}
 
2(b) exec - Display Current Date and Time
#include <stdio.h>
#include <unistd.h>
 
int main()
{
    printf("Displaying Current Date and Time\n");
 
    execlp("date", "date", NULL);
 
    printf("This line will not execute.\n");
 
    return 0;
}
 
2© stat() - File Information
#include <stdio.h>
#include <sys/stat.h>
#include <unistd.h>
#include <time.h>
 
int main()
{
    struct stat s;
    char filename[100];
 
    printf("Enter the file name: ");
    scanf("%s", filename);
 
    if (stat(filename, &s) == -1)
    {
        perror("stat");
        return 1;
    }
 
    printf("\nFile Information\n");
    printf("----------------------------\n");
    printf("File Size          : %ld bytes\n", s.st_size);
    printf("Inode Number       : %ld\n", s.st_ino);
    printf("File Mode          : %o\n", s.st_mode);
    printf("Number of Links    : %ld\n", s.st_nlink);
    printf("User ID            : %d\n", s.st_uid);
    printf("Group ID           : %d\n", s.st_gid);
    printf("Last Access Time   : %s", ctime(&s.st_atime));
    printf("Last Modified Time : %s", ctime(&s.st_mtime));
 
    return 0;
}
 
2(d) fork(), wait() - Process Creation
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdlib.h>
 
int main()
{
    pid_t pid;
 
    printf("Before Fork\n");
 
    pid = fork();
 
    if (pid < 0)
    {
        printf("Fork Failed\n");
        return 1;
    }
    else if (pid == 0)
    {
        printf("\nChild Process Executing...\n");
        printf("Child PID : %d\n", getpid());
        printf("Parent PID: %d\n", getppid());
        exit(0);
    }
    else
    {
        wait(NULL);
 
        printf("\nParent Process Resumed...\n");
        printf("Parent PID: %d\n", getpid());
        printf("Child PID : %d\n", pid);
    }
 
    return 0;
}
 
2(e) File System Calls - creat, open, write, read, close
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
 
int main()
{
    int fd;
    char write_buf[] = "Operating Systems Lab";
    char read_buf[50];
 
    /* Create and open file */
    fd = creat("sample.txt", 0666);
 
    if (fd < 0)
    {
        printf("File creation failed\n");
        return 1;
    }
 
    close(fd);
 
    /* Open file in write mode */
    fd = open("sample.txt", O_WRONLY);
 
    write(fd, write_buf, strlen(write_buf));
 
    close(fd);
 
    /* Open file in read mode */
    fd = open("sample.txt", O_RDONLY);
 
    read(fd, read_buf, sizeof(read_buf));
 
    printf("Contents of the file: %s\n", read_buf);
 
    close(fd);
 
    return 0;
}
 
