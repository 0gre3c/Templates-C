## Задание бригада №11
Варианты заданий те же, что и по теме «Структуры».
Доработать программу:
1. добавить следующие запросы:
- сохранить таблицу в файл;
- прочитать таблицу из файла.
2. вызовы запросов оформить в виде меню;
3. запросы оформить в виде подпрограмм;
  
При работе с файлами использовать:
1. форматированный ввод-вывод;
2. символьный ввод-вывод;
3. строковый ввод-вывод;
4. блочный ввод-вывод;
5. организовать произвольный доступ к файлу

```
#define _CRT_SECURE_NO_WARNINGS
#include <Windows.h>
#include <string.h>
#include <stdio.h>
#include <iostream>
#include <iomanip>
#define N 1000
using namespace std;

#pragma pack(push, 1) 
struct student {
    char name[100];
    int group_number;
    int grades[5];
};
#pragma pack(pop)

struct student students[100];

void menu() { //вывод меню
    cout << "Меню:\n";
    cout << "1.  Ввод данных о студентах\n";
    cout << "2.  Очистка списка студентов\n";
    cout << "3.  Вывод таблицы со всеми студентами\n";
    cout << "4.  Вывод таблицы, отсортированной по номеру группы\n";
    cout << "5.  Вывод студентов со средним баллом выше 4.0\n";
    cout << "6.  Форматированный ввод в файл\n";
    cout << "7.  Форматированный вывод из файла\n";
    cout << "8.  Символьный ввод в файл\n";
    cout << "9.  Символьный вывод из файла\n";
    cout << "10. Строковый ввод в файл\n";
    cout << "11. Строковый вывод из файла\n";
    cout << "12. Блочный ввод в файл\n";
    cout << "13. Блочный вывод из файла\n";
    cout << "14. Произвольный доступ к файлу\n";
}

void clear(student students[], int& n) { //очистка
    n = 0;
    cout << "Список студентов очищен.\n\n";
}

void addStu(student students[], int& n) { //ввод студентов
    cout << "Введите количество студентов: ";
    cin >> n;
    cin.ignore();
    cout << "Введите информацию о студентах:\n";
    for (int i = 0; i < n; i++) {
        cout << "Студент #" << i + 1 << endl;
        cout << "Фамилия и инициалы: ";
        cin.getline(students[i].name, 100);
        cout << "Номер группы: ";
        cin >> students[i].group_number;
        cout << "Оценки (5 шт.): ";
        for (int j = 0; j < 5; j++) {
            cin >> students[i].grades[j];
        }
        cin.ignore();
    }
}

void table(student students[], int n) { //неотсортированная таблица
    if (n == 0) {
        cout << "Список студентов пуст!\n\n";
        return;
    }
    cout << "+------------------------------+--------+-----------------------+\n";
    cout << "|      Фамилия и инициалы      | Группа | Успеваемость          |\n";
    cout << "+------------------------------+--------+-----------------------+\n";
    for (int i = 0; i < n; i++) {
        cout << "| " << left << setw(28) << students[i].name << " | ";
        cout << right << setw(6) << students[i].group_number << " | ";
        for (int j = 0; j < 5; j++) {
            cout << right << setw(4) << students[i].grades[j];
        }
        cout << "  |\n";
    }
    cout << "+------------------------------+--------+-----------------------+\n\n";
}

void tableSort(student students[], int n) { //отсортированная таблица
    if (n == 0) {
        cout << "Список студентов пуст!\n\n";
        return;
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (students[j].group_number > students[j + 1].group_number) {
                student temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }
    cout << "\n Сортировка по номеру группы:\n\n";
    table(students, n);
}

void good(student students[], int n) { //вывод студентов с баллом выше 4.0
    if (n == 0) {
        cout << "Список студентов пуст!\n\n";
        return;
    }
    cout << "\n Студенты со средним баллом выше 4.0: \n\n";
    int count = 0;
    for (int i = 0; i < n; i++) {
        double sum = 0;
        for (int j = 0; j < 5; j++) {
            sum += students[i].grades[j];
        }
        double average = sum / 5.0;
        if (average > 4.0) {
            cout << students[i].name << ", группа " << students[i].group_number
                << " (средний балл: " << average << ")\n";
            count++;
        }
    }
    if (count == 0) {
        cout << "Нет студентов со средним баллом выше 4.0\n";
    }
    cout << "\n";
}
void inputForm(student students[], int n) { //форматированный ввод
    FILE* f;
    const char* filename = "StudForm.txt";

    if ((f = fopen(filename, "w")) == NULL) {
        perror(filename);
        return;
    }

    for (int i = 0; i < n; i++) {
        fprintf(f, "%s\n", students[i].name);
        fprintf(f, "%d\n", students[i].group_number);
        for (int j = 0; j < 5; j++) {
            fprintf(f, "%d", students[i].grades[j]);
            if (j < 4) fprintf(f, " ");
        }
        fprintf(f, "\n");
    }
    fclose(f);
    cout << "Данные форматированно сохранены в файл " << filename << "!\n\n";
}

void outputForm(student students[], int& n) { //форм вывод
    FILE* f;
    const char* filename = "StudForm.txt";

    if ((f = fopen(filename, "r")) == NULL) {
        perror(filename);
        return;
    }

    n = 0;
    char buffer[1000];

    while (n < N && fgets(buffer, sizeof(buffer), f) != NULL) {
        buffer[strcspn(buffer, "\n")] = '\0';
        strcpy(students[n].name, buffer);

        if (fgets(buffer, sizeof(buffer), f) == NULL) break;
        students[n].group_number = atoi(buffer);

        if (fgets(buffer, sizeof(buffer), f) == NULL) break;
        char* token = strtok(buffer, " ");
        for (int j = 0; j < 5 && token != NULL; j++) {
            students[n].grades[j] = atoi(token);
            token = strtok(NULL, " ");
        }
        n++;
    }
    fclose(f);
    cout << "Данные загружены из файла " << filename << " (загружено " << n << " записей).\n\n";
}

void inputChar(student students[], int n) { //символьный ввод
    FILE* f;
    const char* filename = "StudChar.txt";

    if ((f = fopen(filename, "w")) == NULL) {
        perror(filename);
        return;
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; students[i].name[j] != '\0'; j++) {
            fputc(students[i].name[j], f);
        }
        fputc('\n', f);

        char groupStr[10];
        sprintf(groupStr, "%d", students[i].group_number);
        for (int j = 0; groupStr[j] != '\0'; j++) {
            fputc(groupStr[j], f);
        }
        fputc('\n', f);

        for (int j = 0; j < 5; j++) {
            char gradeStr[10];
            sprintf(gradeStr, "%d", students[i].grades[j]);
            for (int k = 0; gradeStr[k] != '\0'; k++) {
                fputc(gradeStr[k], f);
            }
            if (j < 4) fputc(' ', f);
        }
        fputc('\n', f);
    }
    fclose(f);
    cout << "Данные символьно сохранены в файл " << filename << "!\n\n";
}

void outputChar(student students[], int& n) { //симв вывод
    FILE* f;
    const char* filename = "StudChar.txt";

    if ((f = fopen(filename, "r")) == NULL) {
        perror(filename);
        return;
    }

    n = 0;
    char buffer[100];
    int c;
    int pos = 0;

    while (n < N && (c = fgetc(f)) != EOF) {
        pos = 0;
        while (c != EOF && c != '\n') {
            buffer[pos++] = (char)c;
            c = fgetc(f);
        }
        buffer[pos] = '\0';
        if (pos > 0) {
            strcpy(students[n].name, buffer);
        }
        else {
            break;
        }

        c = fgetc(f);
        pos = 0;
        while (c != EOF && c != '\n') {
            buffer[pos++] = (char)c;
            c = fgetc(f);
        }
        buffer[pos] = '\0';
        students[n].group_number = atoi(buffer);

        c = fgetc(f);
        pos = 0;
        while (c != EOF && c != '\n') {
            buffer[pos++] = (char)c;
            c = fgetc(f);
        }
        buffer[pos] = '\0';
        char* token = strtok(buffer, " ");
        for (int j = 0; j < 5 && token != NULL; j++) {
            students[n].grades[j] = atoi(token);
            token = strtok(NULL, " ");
        }
        n++;
    }
    fclose(f);
    cout << "Данные загружены из файла " << filename << " (загружено " << n << " записей).\n\n";
}

void inputString(student students[], int n) { //строковый ввод
    FILE* f;
    const char* filename = "StudString.txt";

    if ((f = fopen(filename, "w")) == NULL) {
        perror(filename);
        return;
    }

    for (int i = 0; i < n; i++) {
        fprintf(f, "%s\n", students[i].name);
        fprintf(f, "%d\n", students[i].group_number);
        for (int j = 0; j < 5; j++) {
            fprintf(f, "%d", students[i].grades[j]);
            if (j < 4) fprintf(f, " ");
        }
        fprintf(f, "\n");
    }

    fclose(f);
    cout << "Данные строково сохранены в файл " << filename << "!\n\n";
}

void outputString(student students[], int& n) { //симв вывод
    FILE* f;
    const char* filename = "StudString.txt";

    if ((f = fopen(filename, "r")) == NULL) {
        perror(filename);
        return;
    }

    n = 0;
    char buffer[1000];

    while (n < N && fgets(buffer, sizeof(buffer), f) != NULL) {
        buffer[strcspn(buffer, "\n")] = '\0';

        strcpy(students[n].name, buffer);

        if (fgets(buffer, sizeof(buffer), f) == NULL) break;
        students[n].group_number = atoi(buffer);

        if (fgets(buffer, sizeof(buffer), f) == NULL) break;
        char* token = strtok(buffer, " ");
        for (int j = 0; j < 5 && token != NULL; j++) {
            students[n].grades[j] = atoi(token);
            token = strtok(NULL, " ");
        }
        n++;
    }
    fclose(f);
    cout << "Данные загружены из файла " << filename << " (загружено " << n << " записей).\n\n";
}

void inputBlock(student students[], int n) { //блочный ввод
    FILE* f;
    const char* filename = "StudBlock.dat";

    if ((f = fopen(filename, "wb")) == NULL) {
        perror(filename);
        return;
    }

    fwrite(students, sizeof(student), n, f);

    fclose(f);
    cout << "Данные блочно сохранены в файл " << filename << "!\n\n";
}

void outputBlock(student students[], int& n) { //блочный вывод
    FILE* f;
    const char* filename = "StudBlock.dat";

    if ((f = fopen(filename, "rb")) == NULL) {
        perror(filename);
        return;
    }

    n = fread(students, sizeof(student), 100, f);

    fclose(f);
    if (n > 0) {
        cout << "Данные загружены из файла " << filename << " (загружено " << n << " записей).\n\n";
    }
    else {
        cout << "Файл " << filename << " пуст или поврежден.\n\n";
    }
}

void randAccess(student students[], int& n) { //произвольный доступ
    FILE* f;
    const char* filename = "StudBlock.dat";
    if (n > 0) {
        inputBlock(students, n);
    }

    if ((f = fopen(filename, "rb")) == NULL) {
        perror(filename);
        return;
    }

    fseek(f, 0, SEEK_END);
    long size = ftell(f);
    rewind(f);

    int quan = size / sizeof(struct student);
    if (quan == 0) {
        cout << "Файл пуст." << endl;
        fclose(f);
        return;
    }

    cout << "В файле содержится " << quan << " записей.\n";
    cout << "Введите номер студента для просмотра (от 1 до " << quan << "): ";
    int pos;
    cin >> pos;

    if (pos >= 1 && pos <= quan) {
        student temp;
        long byte = (long)(pos - 1) * sizeof(struct student);
        fseek(f, byte, SEEK_SET);
        if (fread(&temp, sizeof(struct student), 1, f) == 1) {
            cout << "\n+------------------------------+--------+-----------------------+\n";
            cout << "|      Фамилия и инициалы      | Группа | Успеваемость          |\n";
            cout << "+------------------------------+--------+-----------------------+\n";
            cout << "| " << left << setw(28) << temp.name << " | ";
            cout << right << setw(6) << temp.group_number << " | ";
            for (int j = 0; j < 5; j++) {
                cout << right << setw(4) << temp.grades[j];
            }
            cout << "  |\n";
            cout << "+------------------------------+--------+-----------------------+\n\n";
        }
        else {
            cout << "Ошибка чтения записи.\n";
        }
    }
    else {
        cout << "Некорректный номер. Введите число от 1 до " << quan << ".\n";
    }

    fclose(f);
}

int main()
{
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);
    int n = 0;
    int choice = 0;

    while (true) {
        menu();

        cout << "\nВыберите действие: ";
        cin >> choice;
        cout << "\n";

        switch (choice) {
        case 1:
            addStu(students, n);
            break;
        case 2:
            clear(students, n);
            break;
        case 3:
            table(students, n);
            break;
        case 4:
            tableSort(students, n);
            break;
        case 5:
            good(students, n);
            break;
        case 6:
            inputForm(students, n);
            break;
        case 7:
            outputForm(students, n);
            break;
        case 8:
            inputChar(students, n);
            break;
        case 9:
            outputChar(students, n);
            break;
        case 10:
            inputString(students, n);
            break;
        case 11:
            outputString(students, n);
            break;
        case 12:
            inputBlock(students, n);
            break;
        case 13:
            outputBlock(students, n);
            break;
        case 14:
            randAccess(students, n);
            break;
        default:
            cout << "Неверное действие.\n\n";
        }
    }
}
```
