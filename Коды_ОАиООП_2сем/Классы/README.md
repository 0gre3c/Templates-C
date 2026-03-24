## Задание бригада №11
В текстовом файле хранится информация об абонентах АТС. Каждая строка файла содержит запись об одном абоненте. Формат записи: номер
телефона, ФИО, тариф, сумма к оплате.
Описать класс «SUBSCRIBER». Предусмотреть возможность работы с произвольным числом абонентов, поиска абонента по какому-либо признаку
(по ФИО, по номеру телефона, по тарифу), добавления абонентов, удаления абонентов, сортировки абонентов по разным полям.
Написать программу, демонстрирующую работу с этим классом.
Программа должна содержать меню, позволяющее осуществить проверку всех методов класса.

```
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <windows.h>
#include <cstring>
#include <iomanip>
#include <cstdio>
using namespace std;

#define max_abonents 10000
#define max_len 100

class SUBSCRIBER {
private:
	char phone[max_len];
	char name[max_len];
	char tarif[max_len];
	float payment;

public:
	SUBSCRIBER() { //конструктор по умолчанию для самостоятельного добавления абонента
		strcpy(phone, "");
		strcpy(name, "");
		strcpy(tarif, "");
		payment = 0.0f;
	}

	SUBSCRIBER(const char* p, const char* n, const char* t, float pay) { //констр с параметрами для чтения из файла
		strncpy(phone, p, max_len - 1);
		phone[max_len - 1] = '\0';
		strncpy(name, n, max_len - 1);
		name[max_len - 1] = '\0';
		strncpy(tarif, t, max_len - 1);
		tarif[max_len - 1] = '\0';
		payment = pay;
	}

	//методы для поиска и сортировки:
	const char* getPhone() {
		return phone;
	}
	const char* getName() {
		return name;
	}
	const char* getTarif() {
		return tarif;
	}
	float getPay() {
		return payment;
	}

	//метод вывода 1 абонента
	void print() {
		cout << "| " << left << setw(14) << phone << " | ";
		cout << left << setw(25) << name << " | ";
		cout << left << setw(20) << tarif << " | ";
		cout << right << setw(13) << fixed << setprecision(2) << payment << " |" << endl;
	}
};

SUBSCRIBER sub[max_abonents]; //объект класса
int countSub = 0;

void head() {
	cout << "+----------------+---------------------------+----------------------+---------------+" << endl;
	cout << "|    Телефон     |           ФИО             |        Тариф         | Оплата (руб.) |" << endl;
	cout << "+----------------+---------------------------+----------------------+---------------+" << endl;
}

void bottom() {
	cout << "+----------------+---------------------------+----------------------+---------------+" << endl;
}

void printAll() {
	if (countSub == 0) {
		cout << "Список абонентов пуст.\n" << endl;
		return;
	}
	head();

	for (int i = 0; i < countSub; i++) {
		sub[i].print();
	}
	bottom();
	cout << "Всего " << countSub << " абонентов." << endl << endl;
}

int addSub(const char* p, const char* n, const char* t, float pay) { //добавление абонента в класс
	if (countSub >= max_abonents) {
		printf("Недостаточно места для записи.\n");
		return 0;
	}
	sub[countSub] = SUBSCRIBER(p, n, t, pay);
	countSub++;
	return 1;
}

void inputSub() { //добавление абонента с консоли
	char p[max_len], n[max_len], t[max_len];
	float pay;

	printf("Введите номер телефона: ");
	scanf("%s", p);
	scanf("%*c"); //с помощью форматирования убираем \n, введенную с клавиатуры

	printf("Введите ФИО: ");
	scanf("%[^\n]", n);
	scanf("%*c");

	printf("Введите тариф: ");
	scanf("%[^\n]", t);
	scanf("%*c");

	printf("Введите сумму оплаты: ");
	scanf("%f", &pay);
	scanf("%*c");

	if (addSub(p, n, t, pay)) {
		printf("\nАбонент успешно добавлен.\n");
	}
}

void deleteSub() {
	if (countSub == 0) {
		cout << "Недостаточно данных.\n" << endl;
		return;
	}
	printAll();
	int num;
	cout << "Введите номер абонента для удаления: ";
	scanf("%d", &num);
	scanf("%*c");

	if (num >= 1 && num <= countSub) {
		for (int i = num - 1; i < countSub - 1; i++) {
			sub[i] = sub[i + 1];
		}
		countSub--;
		cout << "Абонент удален.\n" << endl;
	}
	else {
		cout << "Неверный номер.\n" << endl;
	}
}

void searchSub() { //поиск абонентов по критериям
    if (countSub == 0) {
        printf("Недостаточно данных для поиска.\n");
        return;
    }

	int choice;
	cout << "Поиск по:" << endl;
	cout << "1. ФИО" << endl;
	cout << "2. Номеру телефона" << endl;
	cout << "3. Тарифу" << endl;
	cout << "\nВыберите критерий: ";
	cin >> choice;
	cin.ignore();

	char searchStr[max_len];
    printf("\nВведите значение для поиска: ");
    scanf("%[^\n]", searchStr);
    scanf("%*c");

	int found = 0; //счетчик для найденных абонентов
	int i = 0;

	while (i < countSub) {
		int flag = 0;

		if (choice == 1 && strstr(sub[i].getName(), searchStr) != NULL) flag = 1;
		else if (choice == 2 && strcmp(sub[i].getPhone(), searchStr) == 0) flag = 1;
		else if (choice == 3 && strstr(sub[i].getTarif(), searchStr) != NULL) flag = 1;
		else if (choice < 1 || choice > 3) {
			printf("Неверный номер.\n");
			return;
		}

		if (flag) {
			if (found == 0) {
				cout << "\nРезультат поиска:\n";
				head();
			}
			sub[i].print();
			found++;
		}
		i++;
	}

	if (found > 0) {
		bottom();
		printf("Найдено абонентов: %d\n\n", found);
	}
	else {
		printf("\nНичего не найдено.\n\n");
	}
}

void sortSub() { //любимая пузырьковая сортировка
	if (countSub < 2) {
		cout << "Недостаточно данных для сортировки.\n" << endl;
		return;
	}

	int choice;
	cout << "Сортировка по:" << endl;
	cout << "1. ФИО" << endl;
	cout << "2. Номеру телефона" << endl;
	cout << "3. Тарифу" << endl;
	cout << "4. Сумме к оплате" << endl;
	cout << "\nВыберите критерий: ";
	cin >> choice;
	cin.ignore();

	for (int i = 0; i < countSub - 1; i++) {
		for (int j = 0; j < countSub - i - 1; j++) {
			bool flag = false; //флаг для обозначения необходимости поменять местами 2 абонентов

			if (choice == 1 && strcmp(sub[j].getName(), sub[j + 1].getName()) > 0) {
				flag = true;
			}
			else if (choice == 2 && strcmp(sub[j].getPhone(), sub[j + 1].getPhone()) > 0) {
				flag = true;
			}
			else if (choice == 3 && strcmp(sub[j].getTarif(), sub[j + 1].getTarif()) > 0) {
				flag = true;
			}
			else if (choice == 4 && sub[j].getPay() > sub[j + 1].getPay()) {
				flag = true;
			}
			if (flag) {
				SUBSCRIBER temp = sub[j];
				sub[j] = sub[j + 1];
				sub[j + 1] = temp;
			}
		}
	}
	cout << "\nСортировка выполнена.\n" << endl;
	printAll();
}

void inputFile() {
	const char* filename = "subs.txt";
	FILE* f;
	if ((f = fopen(filename, "w")) == NULL) {
		perror(filename);
		return;
	}
	for (int i = 0; i < countSub; i++) {
		fprintf(f, "%s\t%s\t%s\t%.2f\n", sub[i].getPhone(), sub[i].getName(), sub[i].getTarif(),sub[i].getPay());
	}
	fclose(f);
	cout << "Данные успешно сохранены в " << filename << ".\n" << endl;
}

void outputFile() {
	const char* filename = "subs.txt";
	FILE* f;
	if ((f = fopen(filename, "r")) == NULL) {
		perror(filename);
		return;
	}
	char str[1000];
	countSub = 0;

	while (fgets(str, 100, f) != NULL) {
		str[strcspn(str, "\n")] = '\0';

		char Phone[max_len];
		Phone[0] = '\0'; //делаем нулевым, чтобы случайно не наткнуться на ошибку или мусор
		char Name[max_len];
		Name[0] = '\0';
		char Tarif[max_len];
		Tarif[0] = '\0';
		float Pay = 0.0f;

		char* pstr = strtok(str, "\t");
		if (pstr) {
			strcpy(Phone, pstr);
		}
		pstr = strtok(NULL, "\t"); //чтобы не совершился переход на новую строку переобозначим указатель
		if (pstr) {
			strcpy(Name, pstr);
		}
		pstr = strtok(NULL, "\t");
		if (pstr) {
			strcpy(Tarif, pstr);
		}
		pstr = strtok(NULL, "\t");
		if (pstr) {
			Pay = atof(pstr);
		}

		if (countSub < max_abonents) {
			sub[countSub] = SUBSCRIBER(Phone, Name, Tarif, Pay);
			countSub++;
		}
	}

	fclose(f);
	cout << "Загружено " << countSub << " абонентов.\n\n";
}

void menu() {
	int choice;
	do {
		cout << "1. Показать всех абонентов" << endl;
		cout << "2. Добавить абонента" << endl;
		cout << "3. Удалить абонента" << endl;
		cout << "4. Поиск абонента" << endl;
		cout << "5. Сортировка абонентов" << endl;
		cout << "6. Сохранить в файл" << endl;
		cout << "7. Загрузить из файла" << endl;
		cout << "\nВаш выбор: ";
		cin >> choice;
		cin.ignore();
		cout << endl;

		switch (choice) {
		case 1: printAll(); break;
		case 2: inputSub(); break;
		case 3: deleteSub(); break;
		case 4: searchSub(); break;
		case 5: sortSub(); break;
		case 6: inputFile(); break;
		case 7: outputFile(); break;
		default: cout << "Неверный выбор.\n" << endl;
		}
	} while (choice != 0);
}

int main() {
	SetConsoleCP(1251);
	SetConsoleOutputCP(1251);
	menu();
}


```
