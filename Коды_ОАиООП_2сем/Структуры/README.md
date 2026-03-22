## Задание бригада №11
Описать структуру с именем STUDENT, содержащую следующие поля:
– фамилия и инициалы;
– номер группы;
– успеваемость (массив из пяти элементов).
Написать программу, выполняющую следующие действия:
– ввод с клавиатуры данных в массив, состоящий из десяти
структур типа STUDENT;
– вывод таблицы на экран;
– записи упорядочить по возрастанию номера группы;
– вывод отсортированной таблицы на экран;
– вывод на дисплей фамилий и номеров групп для всех студентов,
включенных в массив, если средний балл студента больше 4.0;
– если таких студентов нет, вывести соответствующее сообщение.

```
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>
#include <stdio.h>
#include <string.h>
#include <iostream>
#include <iomanip>
using namespace std;

struct student {
    char name[100];
    int group_number;
    int grades[5];
};
struct student students[10];

int main()
{
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);

    //ввод данных
    for (int i = 0; i < 10; i++) {
        cout << "Студент #" << i + 1 << endl;
        cout << "Фамилия и инициалы: ";
        cin.getline(students[i].name, 100);
        cout << "Номер группы: ";
        cin >> students[i].group_number;
        cout << "Оценки: ";
        for (int j = 0; j < 5; j++) {
            cin >> students[i].grades[j];
        }
        cin.ignore();
    }

    cout << "\n\n";

    //таблица без сортировки
    cout << "+------------------------------+--------+-----------------------+\n";
    cout << "|      Фамилия и инициалы      | Группа | Успеваемость          |\n";
    cout << "+------------------------------+--------+-----------------------+\n";

    for (int i = 0; i < 10; i++) {
        cout << "| " << left << setw(28) << students[i].name << " | ";
        cout << right << setw(6) << students[i].group_number << " | ";
        for (int j = 0; j < 5; j++) {
            cout << right << setw(4) << students[i].grades[j];
        }
        cout << "  |\n";
    }
    cout << "+------------------------------+--------+-----------------------+\n";

    //сортировка пузырьком по возрастанию номера группы
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 10 - i - 1; j++) {
            if (students[j].group_number > students[j + 1].group_number) {
                student temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }

    cout << "\n Сортировка по номеру группы:\n\n";
    cout << "+------------------------------+--------+-----------------------+\n";
    cout << "|     Фамилия и инициалы       | Группа | Успеваемость          |\n";
    cout << "+------------------------------+--------+-----------------------+\n";

    for (int i = 0; i < 10; i++) {
        cout << "| " << left << setw(28) << students[i].name << " | ";
        cout << right << setw(6) << students[i].group_number << " | ";


        for (int j = 0; j < 5; j++) {
            cout << right << setw(4) << students[i].grades[j];
        }
        cout << "  |\n";
    }
    cout << "+------------------------------+--------+-----------------------+\n";
    cout << "\n Студенты со средним баллом выше 4.0: \n\n";

    int count = 0;

    //вывод среднего балла
    for (int i = 0; i < 10; i++) {

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

    //поиск и вывод отличников заданной группы
    int group;
    cout << "\nВведите номер группы: ";
    cin >> group;

    bool found = false;
    for (int i = 0; i < 10; i++) {
        if (students[i].group_number == group) {
            double sum = 0;
            for (int j = 0; j < 5; j++) {
                sum += students[i].grades[j];
            }
            double average = sum / 5.0;

            if (average == 5.0) {
                if (!found) {
                    cout << "\nОтличники данной группы:\n\n";
                    found = true;
                }
                cout << students[i].name << ", группа " << students[i].group_number
                    << " (средний балл: " << average << ")\n";
            }
        }
    }

    if (!found) {
        cout << "\nВ данной группе нет отличников" << endl;
    }
   
    return 0;
}
```
