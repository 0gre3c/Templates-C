## Задание 1. бригада №11. 
Задана строка «a / b = ?», где a и b – целые числа. 
Вычислить результат и подставить его в строку вместо знака вопроса.

```
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdlib.h>
#include <string.h>
using namespace std;

int main()
{
    setlocale(LC_ALL, "rus");
    char str[] = "25 / 5 = ?"; //тест 1
    //char str[] = "-10 / 2 = ?"; //тест 2
    //char str[] = "6 / 0 = ?"; //тест 3
    cout << "Исходная строка: " << str << endl;

    char* p = strchr(str, '/'); //поиск символа / в строке str

    int a = atoi(str); //поиск первого целого числа в str
    int b = atoi(p + 1); //поиск второго целого числа после /
   
    int res = 0;
    if (b == 0)
    {
        cout << "Деление на нуль невозможно\n";
    }
    else
    {
        res = a / b;
        char result[100];
        strcpy(result, str);
        char* p1 = strchr(result, '?'); //"обрезаем" исходную строку
        if (p1 != NULL)
        {
            *p1 = '\0';
        }
        char num[100]; //копируем результат и переводим из числа в строки
        _itoa(res, num, 10);
        strcat(result, num);
        cout << "Результат: " << result << endl;
    }
}
```

## Задание 2. 
Пользователь вводит текст. Вывести исходный текст, заменив в нем слово «три» на «удовлетворительно». Вычислить количество слов начинающихся на «к».

```
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <Windows.h>
using namespace std;

int main()
{
 SetConsoleCP(1251);
 SetConsoleOutputCP(1251);
 const int MAXLEN = 1024;
 char text[MAXLEN];
 puts("Введите текст: ");
 gets_s(text, MAXLEN);

 char* three;
 while ((three = strstr(text, "три")) != NULL) //замена
 {
  char begin[MAXLEN];
  strncpy(begin, text, three - text);
  begin[three - text] = '\0';

  strcat(begin, "удовлетворительно");
  strcat(begin, three + 3);
  strcpy(text, begin);
 }

 char text_copy[MAXLEN]; 
 strcpy(text_copy, text);
 char* word = strtok(text_copy, " ");
 int count = 0;
 while (word != NULL) //подсчет
 {
  if (word[0] == 'к') 
  {
   count++;
  }
  word = strtok(NULL, " ");
 }
 cout << "Текст после замены: " << text << endl;
 cout << "Количество слов, начинающихся на букву 'к': " << count << endl;
}
```

## Задание 3.
Исходный текст набран с ошибками. после слова может находиться один или более пробелов перед запятой (или нет). Вывести исходный текст, убрав в нем пробелы перед запятой (между словом и запятой). В исходном тексте может быть много предложений и запятых. А также удалить символы «–».

```
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <Windows.h>
using namespace std;

int main()
{
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);
    const int MAXLEN = 2048;
    char text[MAXLEN];
    puts("Введите текст: ");
    gets_s(text, MAXLEN);

    char* ptr;
    char bez[MAXLEN];
    while ((ptr = strstr(text, "-")) != NULL)
    {
        char* start = ptr;
        while (*ptr == '-')
        {
            ptr++;
        }
        bez[0] = '\0';
        strncat(bez, text, start - text);
        strcat(bez, " ");
        if (*ptr != '\0')
        {
            strcat(bez, ptr);
        }
        strcpy(text, bez);
    }

    while ((ptr = strstr(text, " ,")) != NULL)
    {
        bez[0] = '\0';
        strncat(bez, text, ptr - text);
        strcat(bez, ptr + 1);
        strcpy(text, bez);
    }
    cout << "Исправленный текст: " << text << endl;
}
```

## Задание 4.
Дана строка текста. Найти определить позицию первого символа «+». Упорядочить по убыванию элементы массива расположенные за этим символом.

```
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <Windows.h>
using namespace std;

int main()
{
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);
    const int MAXLEN = 2048;
    char text[MAXLEN];
    puts("Введите строку: ");
    gets_s(text, MAXLEN);

    char* first = strchr(text, '+'); //поиск позиции первого "+"
    int poz = first - text;
    int len = strlen(text); 
    int aft = len - poz - 1;

    char after[MAXLEN]; //работа только с частью после первого "+"
    strcpy(after, first + 1); 

    for (int i = 0; i < aft - 1; i++) //сортировка пузырьком
    {
        for (int j = 0; j < aft - i - 1; j++)
        {
            if (after[j] < after[j + 1])
            {
                char temp = after[j];
                after[j] = after[j + 1];
                after[j + 1] = temp;
            }
        }
    }

    char res[MAXLEN];
    strncpy(res, text, poz);
    res[poz] = '\0';
    strcat(res, "+");
    strcat(res, after);

    cout << "Позиция первого символа '+': " << poz << endl;
    cout << "Текст после сортировки: " << res << endl;
}
```

## Задание 5.
Даны две строки текста. Определить сколько раз встречается каждый символ первой строки во второй строке. Например, Strl = "xyz"; Str2 = "х a d с х у х w". Тогда "х" - встречается 3 раза, "у"- встречается 1 раз, "z". – встречается 0 раз . Удалить все слова «кризис».

```
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <Windows.h>
#include <cstring>
using namespace std;

int main()
{
    SetConsoleCP(1251);
    SetConsoleOutputCP(1251);

    const int MAXLEN = 512;
    char str1[MAXLEN];
    char str2[MAXLEN];
    
    puts("Введите первую строку: ");
    gets_s(str1, MAXLEN);

    puts("Введите вторую строку: ");
    gets_s(str2, MAXLEN);
   
 
    for (int i = 0; i < strlen(str1); i++) //поиск одинаковых элементов с счетчиком
    {
        char sym = str1[i];
        if (sym != ' ')
        {
            int count = 0;
            for (int j = 0; j < strlen(str2); j++)
            {
                if (str2[j] == sym)
                {
                    count++;
                }
            }
            cout << "Количетсво " << sym << " во второй строке: " << count << endl;
        }
    }

    char* ptr;
    while ((ptr = strstr(str1, "кризис")) != NULL)
    {
        char bez[MAXLEN];
        strncpy(bez, str1, ptr - str1);
        bez[ptr - str1] = '\0';
        strcat(bez, ptr + 6);
        strcpy(str1, bez);
    }

    while ((ptr = strstr(str2, "кризис")) != NULL)
    {
        char bez[MAXLEN];
        strncpy(bez, str2, ptr - str2);
        bez[ptr - str2] = '\0';
        strcat(bez, ptr + 6);
        strcpy(str2, bez);
    }
    cout << "Первая строка: " << str1 << endl;
    cout << "Вторая строка: " << str2 << endl;
}
```
