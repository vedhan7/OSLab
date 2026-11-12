Experiment 9: Access Control (DAC, RBAC, MAC)
9 Access Control - DAC, RBAC, MAC (Code)
#include <stdio.h>
#include <string.h>
#include <sys/stat.h>

/* ---------- DAC: check file permissions ---------- */
void check_dac(const char *filename) {
    struct stat st;
    if (stat(filename, &st) != 0) {
        printf("File not found: %s\n", filename);
        return;
    }
    printf("\n[DAC] File: %s\n", filename);
    printf("Owner : %c%c%c\n",
           (st.st_mode & S_IRUSR) ? 'r' : '-',
           (st.st_mode & S_IWUSR) ? 'w' : '-',
           (st.st_mode & S_IXUSR) ? 'x' : '-');
    printf("Group : %c%c%c\n",
           (st.st_mode & S_IRGRP) ? 'r' : '-',
           (st.st_mode & S_IWGRP) ? 'w' : '-',
           (st.st_mode & S_IXGRP) ? 'x' : '-');
    printf("Others: %c%c%c\n",
           (st.st_mode & S_IROTH) ? 'r' : '-',
           (st.st_mode & S_IWOTH) ? 'w' : '-',
           (st.st_mode & S_IXOTH) ? 'x' : '-');
}

/* ---------- RBAC: role-based permission check ---------- */
struct Role {
    char name[10];
    int can_read;
    int can_write;
    int can_execute;
};

void check_rbac(const char *role_name, const char *operation) {
    struct Role roles[] = {
        {"admin", 1, 1, 1},
        {"staff", 1, 1, 0},
        {"guest", 1, 0, 0}
    };
    int n = sizeof(roles) / sizeof(roles[0]);

    printf("\n[RBAC] Role: %s, Operation: %s\n", role_name, operation);
    for (int i = 0; i < n; i++) {
        if (strcmp(roles[i].name, role_name) == 0) {
            int allowed = 0;
            if (strcmp(operation, "read") == 0)    allowed = roles[i].can_read;
            if (strcmp(operation, "write") == 0)   allowed = roles[i].can_write;
            if (strcmp(operation, "execute") == 0) allowed = roles[i].can_execute;
            printf("Access %s\n", allowed ? "GRANTED" : "DENIED");
            return;
        }
    }
    printf("Unknown role\n");
}

/* ---------- MAC: label-based check (Bell-LaPadula style) ---------- */
/* Levels: 1 = public, 2 = confidential, 3 = secret */
void check_mac(int user_level, int file_level, const char *operation) {
    int allowed = 0;
    printf("\n[MAC] User level: %d, File level: %d, Operation: %s\n",
           user_level, file_level, operation);
    if (strcmp(operation, "read") == 0)
        allowed = (user_level >= file_level);   /* no read up */
    else if (strcmp(operation, "write") == 0)
        allowed = (user_level <= file_level);   /* no write down */
    printf("Access %s\n", allowed ? "GRANTED" : "DENIED");
}

int main() {
    /* DAC: create a test file and set its permissions first */
    FILE *fp = fopen("testfile.txt", "w");
    if (fp) { fprintf(fp, "sample data\n"); fclose(fp); }
    system("chmod 754 testfile.txt");   /* owner rwx, group r-x, others r-- */
    check_dac("testfile.txt");

    /* RBAC */
    check_rbac("admin", "write");
    check_rbac("guest", "write");
    check_rbac("staff", "execute");

    /* MAC */
    check_mac(3, 2, "read");    /* secret user reads confidential file: allowed */
    check_mac(1, 2, "read");    /* public user reads confidential file: denied */
    check_mac(3, 1, "write");   /* secret user writes to public file: denied */

    return 0;
}
Steps to Run
Save the code. Open a terminal and create the file:
nano access_control.c
Paste the program, then press Ctrl+O, Enter to save, and Ctrl+X to exit.
Compile:
gcc access_control.c -o access_control
If gcc is not installed, run sudo apt install gcc first (Ubuntu/Debian).
Run:
./access_control
The ./ tells the shell to run the program from the current folder.
Check the output. You should see the DAC permissions of testfile.txt, then the RBAC and MAC results. The DAC part shows:
Owner : rwx
Group : r-x
Others: r--
