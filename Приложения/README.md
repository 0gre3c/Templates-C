Для создания приложения при создании проекта типа нужно выбрать:
<img width="517" height="96" alt="image" src="https://github.com/user-attachments/assets/c754ac59-2bc1-4428-937c-104b70190f47" />

Далее тыкаем "Проект" - "Добавить новый элемент" и добавляем форму и файл с++. В свойствах  проекта меняем консоль на винду, точка входа - название созданного файла с++.
В созданном файле с++ прописываем:
```
#include "pch.h"
#include "MyForm.h"
using namespace System;
using namespace System::Windows::Forms;
[STAThread]
void Main(array<String^>^ args)
{
Application::EnableVisualStyles();
Application::SetCompatibleTextRenderingDefault(false);
Название_проекта::MyForm form;
Application::Run(%form);
}
```

Далее редактируем форму и ее компоненты, готово!
