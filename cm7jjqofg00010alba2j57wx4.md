---
title: "Как создать оконное приложение на Qt в среде QtCreator"
seoTitle: "Как создать оконное приложение на Qt в среде QtCreator"
seoDescription: "Учимся создавать свое первое оконное приложение на Qt с использованием QMainWindow в среде QtCreator"
datePublished: Mon Feb 24 2025 21:05:01 GMT+0000 (Coordinated Universal Time)
cuid: cm7jjqofg00010alba2j57wx4
slug: kak-sozdat-okonnoe-prilozhenie-na-qt-v-srede-qtcreator
canonical: https://mediocre-developer.ru/qt-window
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1740347214725/00253f42-120f-478b-9da1-f43ca84d4f10.png
tags: cpp, layout, qt, qt-creator

---

В этой статье мы рассмотрим процесс создания простого оконного приложения с помощью [библиотеки Qt](https://www.qt.io/product/framework) в среде разработки QtCreator. Мы поработаем с [лэйаутами](https://doc.qt.io/qt-6/layout.html), то есть с разметкой элементов окна, добавим в окно таблицу, несколько полей ввода данных и кнопку.

[Библиотека Qt](https://www.qt.io/product/framework) - это набор классов C++ для создания кросс-платформенных приложений с графическим интерфейсом. С помощью этой библиотеки созданы, например, среда разработки [QtCreator](https://www.qt.io/product/development-tools) и инструмент для записи видео [OBS Studio](https://obsproject.com/).

Для работы потребуется: компилятор C++; [библиотека Qt](https://www.qt.io/product/framework), соответствующая компилятору (неплохая [инструкция по установке](https://metanit.com/cpp/qt/1.2.php)); среда разработки QtCreator (или любая другая).

*Статья доступна в видео-формате:* [*YouTube*](https://youtu.be/UzNY4rsSFEc) *|* [*ВКонтакте*](https://vk.com/video-228420545_456239021)

*Читайте больше о разработке в моем Телеграм:* [*t.me/mediocre\_developer*](http://t.me/mediocre_developer)

# Макет приложения

На макете ниже показано как будет выглядеть наше приложение. С левой стороны разместим таблицу со списком людей, определяемых именем, фамилией и возрастом. А справа - панель добавления данных в таблицу. Тут будут те же самые поля, что и в таблице, а также кнопка для добавления.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740322592520/46ce3820-dc40-4657-9cc0-c95be88a25e4.png align="center")

# Исходный код

Для начала создадим новый проект Qt Widgets в QtCreator на базе [**CMake**](https://cmake.org/).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740322697235/b95e5bf5-3033-4226-b54e-6027e45a5e34.png align="center")

Обращаю внимание, что нам не нужно создавать файл `MainWindow.ui`, так как весь интерфейс окна мы будем задавать в коде, избегая инструментов QtDesigner. Поэтому снимаем соответствующую галочку.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740323078160/0d92e8ff-f627-48db-98c2-3c67743054da.png align="center")

В результате работы мастера будет создан проект из 4-х файлов:

* Файл проекта `CMakeLists.txt`;
    
* Главный файл программы с точкой входа `main.cpp`;
    
* Класс окна MainWindow с заголовочным файлом `MainWindow.h` и реализацией `MainWindow.cpp`.
    

## MainWindow.h

```cpp
#pragma once

#include <QMainWindow>

class QTableWidget;
class QLineEdit;
class QSpinBox;

class MainWindow : public QMainWindow
{
  Q_OBJECT

public:
  MainWindow(QWidget *parent = nullptr);
  ~MainWindow();

private slots:
  void onAddClicked();

private:
  QTableWidget* table;
  QLineEdit* nameInput;
  QLineEdit* secondNameInput;
  QSpinBox* ageInput;
};
```

В заголовочном файле мы объявляем слот `void onAddClicked();`, который будет содержать реализацию обработчика нажатия на кнопку. Слот в Qt - это специальный метод класса, участвующий в концепции [сигнал/слот](https://doc.qt.io/qt-6/signalsandslots.html). Слоты всегда объявляются в заголовочных файлах с указанием ключевого слова `slots`. Например, наш слот определен в закрытой секции класса `private slots:`.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Если коротко, то <a target="_self" rel="noopener noreferrer nofollow" href="https://doc.qt.io/qt-6/signalsandslots.html" style="pointer-events: none">концепция сигналов и слотов в Qt</a> - это что-то вроде событийной системы. Сигналы - это методы классов, определяющие некоторое событие. У этих методов нет реализации, мы используем от них только имена и аргументы. Слоты же - это методы классов, отвечающие за обработку событий. Вызов конкретного сигнала всегда будет инициировать вызов присоединенного к нему слота. Об их соединении поговорим ниже.</div>
</div>

Также в заголовочном файле мы объявляем указатели на те виджеты, к которым нам понадобится доступ внутри слота.

```cpp
private:
  QTableWidget* table;
  QLineEdit* nameInput;
  QLineEdit* secondNameInput;
  QSpinBox* ageInput;
```

## MainWindow.cpp

Весь исходный код реализации класса `MainWindow` приведен ниже. Создание всего интерфейса окна происходит в конструкторе. Рассмотрим некоторые его моменты подробнее.

```cpp
#include "MainWindow.h"
#include <QHBoxLayout>
#include <QTableWidget>
#include <QLabel>
#include <QLineEdit>
#include <QSpinBox>
#include <QPushButton>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
  QWidget* centralWidget = new QWidget;
  setCentralWidget(centralWidget);
  QHBoxLayout* mainLayout = new QHBoxLayout;
  centralWidget->setLayout(mainLayout);

  table = new QTableWidget;
  table->setColumnCount(3);
  table->setHorizontalHeaderLabels(QStringList() << "Name" << "Second name" << "Age");
  mainLayout->addWidget(table);

  QVBoxLayout* verticalLayout = new QVBoxLayout;
  mainLayout->addLayout(verticalLayout);

  verticalLayout->addWidget(new QLabel("Name:"));
  nameInput = new QLineEdit;
  verticalLayout->addWidget(nameInput);

  verticalLayout->addWidget(new QLabel("Second name:"));
  secondNameInput = new QLineEdit;
  verticalLayout->addWidget(secondNameInput);

  verticalLayout->addWidget(new QLabel("Age:"));
  ageInput = new QSpinBox;
  verticalLayout->addWidget(ageInput);

  QPushButton* addButton = new QPushButton("Add");
  connect(addButton, &QPushButton::clicked, this, &MainWindow::onAddClicked);
  verticalLayout->addWidget(addButton);

  verticalLayout->addStretch();
}

MainWindow::~MainWindow()
{
}

void MainWindow::onAddClicked()
{
  const QString name = nameInput->text();
  const QString secondName = secondNameInput->text();
  const int age = ageInput->value();

  const int rowCount = table->rowCount();
  table->setRowCount(rowCount + 1);

  table->setItem(rowCount, 0, new QTableWidgetItem(name));
  table->setItem(rowCount, 1, new QTableWidgetItem(secondName));
  table->setItem(rowCount, 2, new QTableWidgetItem(QString::number(age)));
}
```

### Конструктор

В первую очередь мы создаем главный [горизонтальный лэйаут](https://doc.qt.io/qt-6/layout.html#horizontal-vertical-grid-and-form-layouts) `mainLayout` ([QHBoxLayout](https://doc.qt.io/qt-6/qhboxlayout.html)) нашего окна.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><a target="_self" rel="noopener noreferrer nofollow" href="https://doc.qt.io/qt-6/layout.html" style="pointer-events: none">Лэйауты в Qt</a> предназначены для управления размещением виджетов в окне. Он задают не только место и порядок размещения виджетов, но и их размер, который вычисляется динамически при изменении размеров окна. <a target="_self" rel="noopener noreferrer nofollow" href="https://doc.qt.io/qt-6/qtwidgets-index.html" style="pointer-events: none">Виджетами в Qt</a> называют все графические элементы, которые могут быть размещены в окне (поля ввода, кнопки, таблицы, списки и т.д.)</div>
</div>

Горизонтальный лэйаут подразумевает, что все виджеты, добавленные на него, будут размещены по горизонтали друг за другом слева направо в порядке добавления.

```cpp
QWidget* centralWidget = new QWidget;
setCentralWidget(centralWidget);
QHBoxLayout* mainLayout = new QHBoxLayout;
centralWidget->setLayout(mainLayout);
```

Дальше мы добавляем в лэйаут пустую таблицу [QTableWidget](https://doc.qt.io/qt-6/qtablewidget.html) с тремя колонками под именами “Name” (имя человека), “Second name” (его фамилия) и “Age” (возраст).

```cpp
table = new QTableWidget;
table->setColumnCount(3);
table->setHorizontalHeaderLabels(QStringList() << "Name" << "Second name" << "Age");
mainLayout->addWidget(table);
```

Теперь дело за правой панелью. Как видно на макете выше виджеты правой панели размещены по вертикали. Чтобы этого добиться в Qt, мы должны использовать вертикальный лэйаут [QVBoxLayout](https://doc.qt.io/qt-6/qvboxlayout.html), который добавляем внутрь горизонтального. Да, мы можем вкладывать лэйауты друг в друга.

```cpp
QVBoxLayout* verticalLayout = new QVBoxLayout;
mainLayout->addLayout(verticalLayout);
```

Как только вертикальный лэйаут создан, мы можем его заполнять виджетами сверху вниз.

В верхней части добавляем статический текст `“Name:”` с помощью виджета [QLabel](https://doc.qt.io/qt-6/qlabel.html). Сразу после него размещаем поле ввода однострочного текста [QLineEdit](https://doc.qt.io/qt-6/qlineedit.html), которое будет обрабатывать ввод имени.

```cpp
verticalLayout->addWidget(new QLabel("Name:"));
nameInput = new QLineEdit;
verticalLayout->addWidget(nameInput);
```

Дальше идет похожий блок кода, только для ввода фамилии.

```cpp
verticalLayout->addWidget(new QLabel("Second name:"));
secondNameInput = new QLineEdit;
verticalLayout->addWidget(secondNameInput);
```

Третий элемент ввода отличается от остальных, так как он отвечает за ввод целочисленного значения возраста человека. Ввод целочисленных значений выполним с помощью виджета [QSpinBox](https://doc.qt.io/qt-6/qspinbox.html).

```cpp
verticalLayout->addWidget(new QLabel("Age:"));
ageInput = new QSpinBox;
verticalLayout->addWidget(ageInput);
```

И последний элемент окна - кнопка [QPushButton](https://doc.qt.io/qt-6/qpushbutton.html).

Здесь как раз появляется вызов [connect()](https://doc.qt.io/qt-6/signalsandslots.html), отвечающий за соединение сигналов и слотов. Этот вызов можно прочитать так: “Присоединить вызов сигнала `&QPushButton::clicked` объекта `addButton` к слоту `&MainWindow::onAddClicked` объекта `this` (то есть самого класса `MainWindow`)”. Таким образом клик по кнопке испускает сигнал `clicked()`, который инициирует вызов метода `onAddClicked()`.

```cpp
QPushButton* addButton = new QPushButton("Add");
connect(addButton, &QPushButton::clicked, this, &MainWindow::onAddClicked);
verticalLayout->addWidget(addButton);
```

В завершение мы добавляем в вертикальный лэйаут “пружинку”, которая как бы подожмет все элементы лэйаута к верхней его части. Если не добавить “пружинку”, то все элементы лэйаута равномерно распределятся по всей его площади. При больших размерах окна это будет смотреться несобранно, а это нам не подходит.

```cpp
verticalLayout->addStretch();
```

### Обработчик нажатия кнопки

Слот `onAddClicked()`, реализующий обработку нажатия кнопки, включает три условных этапа. Сначала мы получаем текущие данные из полей ввода.

```cpp
const QString name = nameInput->text();
const QString secondName = secondNameInput->text();
const int age = ageInput->value();
```

Затем увеличиваем количество строк в таблице на 1. Тем самым создаем новую пустую строку в конце таблицы.

```cpp
const int rowCount = table->rowCount();
table->setRowCount(rowCount + 1);
```

И, наконец, заполняем эту строку данными путем добавления текстовых элементов [QTableWidgetItem](https://doc.qt.io/qt-6/qtablewidgetitem.html) в каждую ячейку. При этом целочисленную переменную `age` предварительно конвертируем в строку `QString` с помощью вызова статического метода `QString::number()`.

```cpp
table->setItem(rowCount, 0, new QTableWidgetItem(name));
table->setItem(rowCount, 1, new QTableWidgetItem(secondName));
table->setItem(rowCount, 2, new QTableWidgetItem(QString::number(age)));
```

## main.cpp

Главный файл программы остается практически без изменений. Добавляем только свое название окна вызовом `setWindowTitle(“People”);` и изменяем изначальный размер окна - делаем его немного больше вызовом `resize(800, 600);`.

```cpp
#include "MainWindow.h"
#include <QApplication>

int main(int argc, char *argv[])
{
  QApplication a(argc, argv);
  MainWindow w;
  w.setWindowTitle("People");
  w.resize(800, 600);
  w.show();
  return a.exec();
}
```

## CMakeLists.txt

Файл проекта `CMakeLists.txt` изменений не потребует и останется точно таким каким его создал мастер во время создания проекта. Например, его содержимое может выглядеть так:

```cpp
cmake_minimum_required(VERSION 3.5)

project(qt_example VERSION 0.1 LANGUAGES CXX)

# Включаем генераторы Qt
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Находим библиотеку Qt
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

# Перечисляем исходные файлы проекта
set(PROJECT_SOURCES
        main.cpp
        MainWindow.cpp
        MainWindow.h
)

# Создаем исполняемый файл проекта
add_executable(qt_example ${PROJECT_SOURCES})

# Прилинковываем библиотеку QtWidgets к исполняемому файлу проекта
target_link_libraries(qt_example PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)
```

# Результат

После сборки и запуска проекта мы увидим наше окно.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740340235370/c9f113a7-590a-4e0e-bd87-e7d04d76ac5d.png align="center")

Вводим данные и нажимаем кнопку “Add”. Новая запись появляется в таблице.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740340326053/2803374a-6453-49f0-b312-3d83338d5a24.png align="center")

По умолчанию данные таблицы можно редактировать. Делаем двойной клик на интересующей ячейке и вводим новые данные. Например, меняем имя с “Ivan” на “Petr”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740340415671/5597caf2-f5ca-4954-942f-9e9e136ac6b3.png align="center")

На этом все.

В статье мы рассмотрели самую верхушку айсберга Qt: горизонтальные и вертикальные лэйауты и несколько виджетов. Но принципы построения окна во всех остальных случаях будут точно такие же. Изучайте и пробуйте.

---

*Статья доступна в видео-формате:* [*YouTube*](https://youtu.be/UzNY4rsSFEc) *|* [*ВКонтакте*](https://vk.com/video-228420545_456239021)

*Читайте больше о разработке в моем Телеграм:* [*t.me/mediocre\_developer*](http://t.me/mediocre_developer)