#include <windows.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <stdlib.h>

struct Student {
    char studentID[10];
    char studentName[20];
    char studentEmail[30];
    char studentPhone[20];
    int  courseCount;
};

struct Course {
    char courseStudentID[10];
    char courseCode[10];
    char courseName[20];
};

struct Student students[100];
struct Course courses[500];

// Global variables
int totalStudents = 0;
int totalCourses = 0;
char searchID[10];
FILE *studentFile;
FILE *courseFile;
bool isRunning = true;

void showMenu();
void addStudent();
void displayStudents();
int  findStudent(char studentID[10]);
void updateStudent(int studentIndex);
void removeStudent(int studentIndex);
void removeAllStudents();
int  checkExisting(char data[30], char infoType, char studentID[10]);
void removeCourseByIndex(int courseIndex);
void removeStudentByIndex(int studentIndex);
void displayGuidelines();
void displayAbout();
void returnOrExit();
void terminateProgram();
void seedData();

int main() {
    seedData(); // Seed dummy data

    while(isRunning) {
        showMenu();
        int choice;
        scanf("%d", &choice);
        switch(choice) {
        case 0:
            isRunning = false;
            terminateProgram();
            break;
        case 1:
            system("cls");
            printf("\n\t\t **** Add A New Student ****\n\n");
            addStudent();
            returnOrExit();
            break;
        case 2:
            system("cls");
            printf("\n\t\t **** All Students ****\n\n");
            displayStudents();
            returnOrExit();
            break;
        case 3:
            system("cls");
            printf("\n\t\t **** Search Students ****\n\n");
            printf(" Enter The Student ID: ");
            scanf("%s", searchID);
            int studentIndex = findStudent(searchID);
            if(studentIndex < 0) {
                printf(" No Student Found\n\n");
            }
            printf("\n");
            returnOrExit();
            break;
        case 4:
            system("cls");
            printf("\n\t\t **** Edit a Student ****\n\n");
            printf(" Enter The Student ID: ");
            scanf("%s", searchID);
            studentIndex = findStudent(searchID);
            if(studentIndex >= 0) {
                updateStudent(studentIndex);
            } else {
                printf(" No Student Found\n\n");
            }
            returnOrExit();
            break;
        case 5:
            system("cls");
            printf("\n\t\t **** Delete a Student ****\n\n");
            printf(" Enter The Student ID: ");
            scanf("%s", searchID);
            int deleteIndex = findStudent(searchID);
            if(deleteIndex >= 0) {
                char confirm = 'N';
                getchar();
                printf("\n\n");
                printf(" Are you sure you want to delete this student? (Y/N): ");
                scanf("%c", &confirm);
                if(confirm == 'Y' || confirm == 'y') {
                    removeStudent(deleteIndex);
                } else {
                    printf(" Your data is safe.\n\n");
                    returnOrExit();
                }
            } else {
                printf(" No Student Found\n\n");
                returnOrExit();
            }
            break;
        case 6:
            system("cls");
            printf("\n\t\t **** Delete ALL Students ****\n\n");
            char confirmAll = 'N';
            getchar();
            printf(" Are you sure you want to delete all the students? (Y/N): ");
            scanf("%c", &confirmAll);
            if(confirmAll == 'Y' || confirmAll == 'y') {
                removeAllStudents();
            } else {
                printf(" Your data is safe.\n\n");
                returnOrExit();
            }
            break;
        case 7:
            system("cls");
            break;
        case 8:
            system("cls");
            displayGuidelines();
            returnOrExit();
            break;
        case 9:
            system("cls");
            displayAbout();
            returnOrExit();
            break;
        default:
            terminateProgram();
            break;
        }
    }

    return 0;
}

void showMenu() {
    printf("\n\n\t*** Student Management System ***\n\n");
    printf("\t\t\tMAIN MENU\n");
    printf("\t\t=======================\n");
    printf("\t\t[1] Add A New Student\n");
    printf("\t\t[2] Show All Students\n");
    printf("\t\t[3] Search A Student\n");
    printf("\t\t[4] Edit A Student\n");
    printf("\t\t[5] Delete A Student\n");
    printf("\t\t[6] Delete All Students\n");
    printf("\t\t[7] Clear The Window\n");
    printf("\t\t[8] User Guidelines\n");
    printf("\t\t[9] About Us\n");
    printf("\t\t[0] Exit the Program\n");
    printf("\t\t=======================\n");
    printf("\t\tEnter Your Choice: ");
}

void addStudent() {
    char newStudentID[300];
    char newName[300];
    char newPhone[300];
    char newEmail[300];
    int newCourseCount;
    char newCourseCode[300];
    char newCourseName[300];

    int validID = 0;
    while(!validID) {
        printf(" Enter The ID: ");
        scanf("%s", &newStudentID);
        if(checkExisting(newStudentID, 'i', newStudentID) > 0) {
            printf(" Error: This ID already exists.\n\n");
            validID = 0;
        } else if(strlen(newStudentID) > 10) {
            printf(" Error: ID cannot be more than 10 characters.\n\n");
            validID = 0;
        } else if(strlen(newStudentID) <= 0) {
            printf(" Error: ID cannot be empty.\n\n");
            validID = 0;
        } else {
            validID = 1;
        }
    }

    int validName = 0;
    while(!validName) {
        printf(" Enter The Name: ");
        scanf(" %[^\n]s", &newName);
        if(strlen(newName) > 20) {
            printf(" Error: Name cannot be more than 20 characters.\n\n");
            validName = 0;
        }
        if(strlen(newName) <= 0) {
            printf(" Error: Name cannot be empty.\n\n");
            validName = 0;
        } else {
            validName = 1;
        }
    }

    int validEmail = 0;
    while(!validEmail) {
        printf(" Enter The Email: ");
        scanf("%s", &newEmail);
        if(checkExisting(newEmail, 'e', newStudentID) > 0) {
            printf(" This Email already exists.\n");
            validEmail = 0;
        } else if(strlen(newEmail) > 30) {
            printf(" Error: Email cannot be more than 30 characters.\n\n");
            validEmail = 0;
        } else if(strlen(newEmail) <= 0) {
            printf(" Error: Email cannot be empty.\n\n");
            validEmail = 0;
        } else {
            validEmail = 1;
        }
    }

    int validPhone = 0;
    while(!validPhone) {
        printf(" Enter The Phone: ");
        scanf("%s", &newPhone);
        if(checkExisting(newPhone, 'p', newStudentID) > 0) {
            printf(" This Phone Number already exists.\n");
            validPhone = 0;
        } else if(strlen(newPhone) > 20) {
            printf(" Error: Phone cannot be more than 20 characters.\n\n");
            validPhone = 0;
        } else if(strlen(newPhone) <= 0) {
            printf(" Error: Phone cannot be empty.\n\n");
            validPhone = 0;
        } else {
            validPhone = 1;
        }
    }

    int validCourseCount = 0;
    while(!validCourseCount) {
        printf(" Number of courses: ");
        scanf("%d", &newCourseCount);
        if(newCourseCount <= 0 || newCourseCount > 4) {
            printf(" Error: Number of courses cannot be more than 4 and less than 1.\n\n");
            validCourseCount = 0;
        } else {
            validCourseCount = 1;
        }
    }

    strcpy(students[totalStudents].studentID, newStudentID);
    strcpy(students[totalStudents].studentName, newName);
    strcpy(students[totalStudents].studentPhone, newPhone);
    strcpy(students[totalStudents].studentEmail, newEmail);
    students[totalStudents].courseCount = newCourseCount;
    totalStudents++;

    for(int i = 0; i < newCourseCount; i++) {
        printf(" Enter Course %d Code: ", i + 1);
        scanf("%s", &newCourseCode);

        printf(" Enter Course %d Name: ", i + 1);
        scanf(" %[^\n]s", &newCourseName);

        strcpy(courses[totalCourses].courseStudentID, newStudentID);
        strcpy(courses[totalCourses].courseCode, newCourseCode);
        strcpy(courses[totalCourses].courseName, newCourseName);
        totalCourses++;
    }

    printf("\n Student Added Successfully.\n\n");
}

void displayStudents() {
    printf("|==========|====================|==============================|====================|=============|\n");
    printf("|    ID    |        Name        |            Email             |       Phone        |  NO.Course  |\n");
    printf("|==========|====================|==============================|====================|=============|\n");

    for(int i = 0; i < totalStudents; i++) {
        printf("|");
        printf("%s", students[i].studentID);
        for(int j = 0; j < (10 - strlen(students[i].studentID)); j++) {
            printf(" ");
        }
        printf("|");
        printf("%s", students[i].studentName);
        for(int j = 0; j < (20 - strlen(students[i].studentName)); j++) {
            printf(" ");
        }
        printf("|");
        printf("%s", students[i].studentEmail);
        for(int j = 0; j < (30 - strlen(students[i].studentEmail)); j++) {
            printf(" ");
        }
        printf("|");
        printf("%s", students[i].studentPhone);
        for(int j = 0; j < (20 - strlen(students[i].studentPhone)); j++) {
            printf(" ");
        }
        printf("|");
        printf("%d", students[i].courseCount);
        for(int j = 0; j < 12; j++) {
            printf(" ");
        }
        printf("|\n");
        printf("|----------|--------------------|------------------------------|--------------------|-------------|\n");
    }
    printf("\n");
}

int findStudent(char studentID[10]) {
    system("cls");
    int studentIndex = -1;

    for(int i = 0; i < totalStudents; i++) {
        if(strcmp(studentID, students[i].studentID) == 0) {
            studentIndex = i;
            printf("\n One Student Found for ID: %s\n\n", studentID);
            printf(" Student Information\n");
            printf("-------------------------\n");

            printf(" ID:    %s\n", students[i].studentID);
            printf(" Name:  %s\n", students[i].studentName);
            printf(" Email: %s\n", students[i].studentEmail);
            printf(" Phone: %s\n", students[i].studentPhone);
            printf("\n Total Number of Courses: %d\n", students[i].courseCount);
        }
    }
    int courseCount = 0;
    for(int j = 0; j < totalCourses; j++) {
        if(strcmp(studentID, courses[j].courseStudentID) == 0) {
            courseCount++;
            printf(" Course %d Code: %s\n", courseCount, courses[j].courseCode);
            printf(" Course %d Name: %s\n", courseCount, courses[j].courseName);
        }
    }

    return studentIndex;
}

void updateStudent(int studentIndex) {
    printf("\n\t\t **** Update The Student ****\n\n");

    char updatedName[300];
    char updatedPhone[300];
    char updatedEmail[300];
    int updatedCourseCount;
    char studentID[300];
    strcpy(studentID, students[studentIndex].studentID);
    int oldCourseCount = students[studentIndex].courseCount;

    int validName = 0;
    while(!validName) {
        printf(" Enter The New Name (0 for skip): ");
        scanf(" %[^\n]s", &updatedName);
        if(strlen(updatedName) > 20) {
            printf(" Error: Name cannot be more than 20 characters.\n\n");
            validName = 0;
        } else if(strlen(updatedName) <= 0) {
            printf(" Error: Name cannot be empty.\n\n");
            validName = 0;
        } else {
            validName = 1;
        }
    }

    int validEmail = 0;
    while(!validEmail) {
        printf(" Enter The New Email (0 for skip): ");
        scanf("%s", &updatedEmail);
        if(strlen(updatedEmail) > 30) {
            printf(" Error: Email cannot be more than 30 characters.\n\n");
            validEmail = 0;
        } else if(strlen(updatedEmail) <= 0) {
            printf(" Error: Email cannot be empty.\n\n");
            validEmail = 0;
        } else if(checkExisting(updatedEmail, 'e', studentID) > 0) {
            printf(" Error: This Email Already Exists.\n\n");
            validEmail = 0;
        } else {
            validEmail = 1;
        }
    }

    int validPhone = 0;
    while(!validPhone) {
        printf(" Enter The New Phone (0 for skip): ");
        scanf("%s", &updatedPhone);
        if(strlen(updatedPhone) > 20) {
            printf(" Error: Phone cannot be more than 20 characters.\n\n");
            validPhone = 0;
        } else if(strlen(updatedPhone) <= 0) {
            printf(" Error: Phone cannot be empty.\n\n");
            validPhone = 0;
        } else if(checkExisting(updatedPhone, 'p', studentID) > 0) {
            printf(" Error: This Phone Number is Already Exists.\n\n");
            validPhone = 0;
        } else {
            validPhone = 1;
        }
    }

    int validCourseCount = 0;
    while(!validCourseCount) {
        printf(" Number of New Courses (0 for skip): ");
        scanf("%d", &updatedCourseCount);

        if(updatedCourseCount > 4 || updatedCourseCount < 0) {
            printf(" Error: A Student can have a maximum of 4 and a minimum of 0 courses.\n\n");
            validCourseCount = 0;
        } else {
            validCourseCount = 1;
        }
    }

    if(strcmp(updatedName, "0") != 0) {
        strcpy(students[studentIndex].studentName, updatedName);
    }

    if(strcmp(updatedEmail, "0") != 0) {
        strcpy(students[studentIndex].studentEmail, updatedEmail);
    }

    if(strcmp(updatedPhone, "0") != 0) {
        strcpy(students[studentIndex].studentPhone, updatedPhone);
    }

    if(updatedCourseCount != 0) {
        int firstCourseIndex;
        for(int dc = 0; dc < totalCourses; dc++) {
            if(strcmp(studentID, courses[dc].courseStudentID) == 0) {
                firstCourseIndex = dc; // store the index for delete
                break;
            }
        }

        // Delete all courses for the student
        for(int dc = 1; dc <= oldCourseCount; dc++) {
            removeCourseByIndex(firstCourseIndex);
        }

        char courseCode[300];
        char courseName[300];
        for(int i = 1; i <= updatedCourseCount; i++) {
            printf(" Enter New Course %d Code: ", i);
            scanf("%s", &courseCode);

            printf(" Enter New Course %d Name: ", i);
            scanf(" %[^\n]s", &courseName);

            strcpy(courses[totalCourses].courseStudentID, studentID);
            strcpy(courses[totalCourses].courseCode, courseCode);
            strcpy(courses[totalCourses].courseName, courseName);
            totalCourses++;
        }
    }

    printf(" Student Updated Successfully.\n\n");
}

void removeStudent(int studentIndex) {
    int firstCourseIndex;
    for(int d = 0; d < totalCourses; d++) {
        if(strcmp(students[studentIndex].studentID, courses[d].courseStudentID) == 0) {
            firstCourseIndex = d;
            break;
        }
    }

    for(int d = 1; d <= students[studentIndex].courseCount; d++) {
        removeCourseByIndex(firstCourseIndex);
    }
    removeStudentByIndex(studentIndex);
    printf(" Student Deleted Successfully.\n\n");
    returnOrExit();
}

void removeAllStudents() {
    totalStudents = 0;
    totalCourses = 0;
    printf(" All Students Deleted Successfully.\n\n");
    returnOrExit();
}

void removeCourseByIndex(int courseIndex) {
    for(int c = 0; c < totalCourses - 1; c++) {
        if(c >= courseIndex) {
            courses[c] = courses[c + 1];
        }
    }
    totalCourses--;
}

void removeStudentByIndex(int studentIndex) {
    for(int s = 0; s < totalStudents - 1; s++) {
        if(s >= studentIndex) {
            students[s] = students[s + 1];
        }
    }
    totalStudents--;
}

int checkExisting(char data[300], char infoType, char studentID[300]) {
    int emailExists = 0;
    int phoneExists = 0;
    int idExists = 0;

    for(int i = 0; i < totalStudents; i++) {
        if(strcmp(data, students[i].studentID) == 0) {
            idExists++;
        }
        if(strcmp(data, students[i].studentEmail) == 0 && strcmp(studentID, students[i].studentID) != 0 ) {
            emailExists++;
        }
        if(strcmp(data, students[i].studentPhone) == 0 && strcmp(studentID, students[i].studentID) != 0) {
            phoneExists++;
        }
    }

    if(infoType == 'i') {
        return idExists;
    } else if(infoType == 'e') {
        return emailExists;
    } else if(infoType == 'p') {
        return phoneExists;
    } else {
        return 0;
    }
}

void displayGuidelines() {
    printf("\n\t\t **** How it Works? ****\n\n");
    printf(" -> You will be able to store the Student's ID, Name, Email, Phone, Number of Courses.\n");
    printf(" -> A student can have a maximum of 4 courses and a minimum of 1 course.\n");
    printf(" -> Student ID can be a maximum of 10 characters long and unique for every student.\n");
    printf(" -> Student Name can be a maximum of 20 characters long.\n");
    printf(" -> Student Email can be a maximum of 30 characters long and unique for every student.\n");
    printf(" -> Student Phone can be a maximum of 20 characters long and unique for every student.\n");
    printf(" -> Course code can be a maximum of 10 characters long.\n");
    printf(" -> Course Name can be a maximum of 20 characters long.\n\n");
    printf(" ->> visit www.insideTheDiv.com for more projects like this. <<-\n\n");
}

void displayAbout() {
    printf("\n\t\t **** About Us ****\n\n");
    printf(" Some important notes to remember:\n");
    printf(" -> This is a simple student management system project.\n");
    printf(" -> You can modify the source code.\n");
    printf(" -> You can use this project only for personal purposes, not for business.\n\n");
    printf(" ->> visit www.insideTheDiv.com for more projects like this. <<-\n\n");
}

void returnOrExit() {
    getchar();
    char option;
    printf(" Go back (b)? or Exit (0)?: ");
    scanf("%c", &option);
    if(option == '0') {
        terminateProgram();
    } else {
        system("cls");
    }
}

void terminateProgram() {
    system("cls");
    int i;
    char thankYou[100]     = " ========= Thank You =========\n";
    char seeYouSoon[100]   = " ========= See You Soon ======\n";
    for(i = 0; i < strlen(thankYou); i++) {
        printf("%c", thankYou[i]);
        Sleep(40);
    }
    for(i = 0; i < strlen(seeYouSoon); i++) {
        printf("%c", seeYouSoon[i]);
        Sleep(40);
    }
    exit(0);
}

void seedData() {
    // Store some dummy data
    // student 1
    strcpy(students[0].studentID, "S-1");
    strcpy(students[0].studentName, "Student 1");
    strcpy(students[0].studentPhone, "016111111111");
    strcpy(students[0].studentEmail, "student-1@gmail.com");
    students[0].courseCount = 1;

    strcpy(courses[0].courseStudentID, "S-1");
    strcpy(courses[0].courseCode, "CSE-1");
    strcpy(courses[0].courseName, "Course - 1");

    // student 2
    strcpy(students[1].studentID, "S-2");
    strcpy(students[1].studentName, "Student 2");
    strcpy(students[1].studentPhone, "016111111112");
    strcpy(students[1].studentEmail, "student-2@gmail.com");
    students[1].courseCount = 2;

    strcpy(courses[1].courseStudentID, "S-2");
    strcpy(courses[1].courseCode, "CSE-1");
    strcpy(courses[1].courseName, "Course - 1");

    strcpy(courses[2].courseStudentID, "S-2");
    strcpy(courses[2].courseCode, "CSE-2");
    strcpy(courses[2].courseName, "Course - 2");

    // student 3
    strcpy(students[2].studentID, "S-3");
    strcpy(students[2].studentName, "Student 3");
    strcpy(students[2].studentPhone, "016111111113");
    strcpy(students[2].studentEmail, "student-3@gmail.com");
    students[2].courseCount = 3;

    strcpy(courses[3].courseStudentID, "S-3");
    strcpy(courses[3].courseCode, "CSE-1");
    strcpy(courses[3].courseName, "Course - 1");

    strcpy(courses[4].courseStudentID, "S-3");
    strcpy(courses[4].courseCode, "CSE-2");
    strcpy(courses[4].courseName, "Course - 2");

    strcpy(courses[5].courseStudentID, "S-3");
    strcpy(courses[5].courseCode, "CSE-3");
    strcpy(courses[5].courseName, "Course - 3");

    totalStudents = 3;
    totalCourses = 6;
}
