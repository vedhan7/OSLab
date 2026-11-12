Experiment 10: File I/O (Text and Binary)
10 File I/O - Text and Binary Files
#include <stdio.h>
 
#define TEXT_FILE "student.txt"
#define BIN_FILE  "numbers.bin"
 
int main() {
    FILE *fp;
    char line[100], name[30], ch;
    int roll, nums[5] = {10, 20, 30, 40, 50}, back[5], i;
 
    /* ---------- WRITE TO A TEXT FILE ---------- */
    fp = fopen(TEXT_FILE, "w");
    if (fp == NULL) {
        printf("Error: cannot create %s\n", TEXT_FILE);
        return 1;
    }
    fputs("Student Record File\n", fp);     /* fputs() */
    fprintf(fp, "%d %s\n", 101, "Arun");    /* fprintf() */
    fputs("Grade: ", fp);
    fputc('A', fp);                         /* fputc() */
    fputc('\n', fp);
    fclose(fp);
    printf("Text file written using fputs(), fprintf(), fputc()\n\n");
 
    /* ---------- READ FROM A TEXT FILE ---------- */
    fp = fopen(TEXT_FILE, "r");
    if (fp == NULL) {
        printf("Error: cannot open %s\n", TEXT_FILE);
        return 1;
    }
    fgets(line, sizeof(line), fp);          /* fgets() reads a whole line */
    printf("Header line (fgets): %s", line);
    fscanf(fp, "%d %s", &roll, name);       /* fscanf() reads formatted data */
    printf("Roll number (fscanf): %d\n", roll);
    printf("Name        (fscanf): %s\n", name);
    fgetc(fp);                              /* skip the newline after the name */
    printf("Remaining text (fgetc):\n");
    while ((ch = fgetc(fp)) != EOF)         /* fgetc() reads one character */
        putchar(ch);
    fclose(fp);
 
    /* ---------- WRITE TO A BINARY FILE ---------- */
    fp = fopen(BIN_FILE, "wb");
    if (fp == NULL) {
        printf("Error: cannot create %s\n", BIN_FILE);
        return 1;
    }
    fwrite(nums, sizeof(int), 5, fp);       /* fwrite() */
    fclose(fp);
    printf("\nBinary file written using fwrite()\n");
 
    /* ---------- READ FROM A BINARY FILE ---------- */
    fp = fopen(BIN_FILE, "rb");
    if (fp == NULL) {
        printf("Error: cannot open %s\n", BIN_FILE);
        return 1;
    }
    fread(back, sizeof(int), 5, fp);        /* fread() */
    fclose(fp);
    printf("Numbers read back using fread(): ");
    for (i = 0; i < 5; i++)
        printf("%d ", back[i]);
    printf("\n");
 
    return 0;
}
 
