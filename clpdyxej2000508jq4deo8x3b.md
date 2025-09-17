---
title: "Способ организации документации проекта C++ в стиле Doc As Code"
seoTitle: "Способ организации документации проекта C++ в стиле Doc As Code"
seoDescription: "Doc As Code в связке с Doxygen / Markdown - это отличный способ организовать документацию проекта на С++"
datePublished: Sat Nov 25 2023 11:28:50 GMT+0000 (Coordinated Universal Time)
cuid: clpdyxej2000508jq4deo8x3b
slug: sposob-organizacii-dokumentacii-proekta-c-v-stile-doc-as-code
canonical: https://mediocre-developer.ru/cpp-doc-as-code
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700911613086/e3ac4144-a6e9-4323-9e33-1c33a69a41da.jpeg
tags: markdown, documentation, doxygen, documentationbestpractices, documentation-tool, docascode

---

# Что такое Doc As Code

Как нетрудно догадаться, Doc As Code призывает оформлять документацию проекта в привычном для программиста виде - как код. В этом случае мы можем использовать систему контроля версий для хранения и управления документацией со всеми вытекающими. Подробнее о Doc As Code [можно почитать здесь](https://habr.com/ru/companies/plesk/articles/555110/), а мы перейдем непосредственно к практике и рассмотрим один из способов организации документации проекта на C++.

# Состав документации

Глобально выделим две составляющих документации проекта:

* техническая документация (technical overview): включает проектную документацию, руководства разработчика, руководства пользователя и т.п.
    
* описание сущностей (reference manual): включает документацию API, классов, структур, функций и т.д.
    

# Инструменты

*Описание сущностей* уже само по себе располагается прямо в исходном коде. Классы и методы описываются либо в заголовочных файлах .h, либо в их реализации .cpp. Для работы с этим типом документации будем использовать [Doxygen](https://www.doxygen.nl/manual/docblocks.html). Doxygen позволяет оформлять документацию исходного кода в разных стилях (Javadoc, Qt-style и др.) и предоставляет инструменты для генерации ее пользовательского представления. Например, HTML-документ.

*Техническую документацию* оформим в формате [Markdown](https://gist.github.com/Jekins/2bf2d0638163f1294637). Это простой текстовый формат разметки документов. Markdown используется, например, в [Вики](https://docs.github.com/ru/communities/documenting-your-project-with-wikis/about-wikis) на платформах GitHub/GitLab. Если ваш проект размещается на платформе с поддержкой Вики, ее и используйте. Мы же рассмотрим ведение всей документации проекта в пределах одного репозитория.

Современные среды разработки как правило поддерживают работу с Markdown из коробки. Для работы с документацией даже не обязательно выходить из любимой IDE. Например, Visual Code поддерживает реал-тайм предпросмотр для Markdown-файлов.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700909101128/09ff89e1-dcef-4e8a-b7d8-f558a14ae492.png align="center")

Используя [возможности Doxygen по встраиванию Markdown-файлов](https://www.doxygen.nl/manual/markdown.html), мы создадим единую HTML-документацию с технической частью и описанием сущностей.

# Структура файлов проекта

Рассмотрим типовой проект библиотеки C++ с набором тестов. Файлы технической документации разместим в папке Doc в корне репозитория. На блоке ниже показана типовая структура файлов проекта, содержащая файл конфигурации Doxygen и папку Doc с Markdown-файлами:

```plaintext
Library
|---Readme.md
|---Doxygen                <----- Конфигурационный файл Doxygen
|---CMakeLists.txt
|---Lib
|   |---CMakeLists.txt
|   |---Lib.h
|   |---sources...
|---Tests
|   |---CMakeLists.txt
|   |---test sources...
|---Doc                    <----- Папка с Markdown-файлами
    |---Main.md
    |---Getting_Started.md
    |---other md-files...
```

В папке Doc разместим обязательный файл Main.md, определяющий главную страницу HTML-документации.

# Конфигурационный файл Doxygen

Doxygen генерирует документацию на основе параметров, определенных в конфигурационном файле. Среди прочих параметров конфигурации Doxygen, рассмотрим наиболее интересные в нашем контексте.

[Для начала генерируем](https://www.doxygen.nl/manual/starting.html) шаблонный файл конфигурации:

```bash
doxygen -g Doxygen
```

Полученный файл содержит [все возможные параметры](https://www.doxygen.nl/manual/config.html). У некоторых даже проставлены значения. Каждый параметр сопровождается подробной справкой. Удобно. Мы же обратим внимание на эти:

| Параметр | Описание |
| --- | --- |
| PROJECT\_NAME | Название проекта. Будет отображаться в HTML-документе. |
| OUTPUT\_DIRECTORY | Путь к сгенерированным файлам. Желательно указать какое-то осмысленное имя конечной папки. |
| MARKDOWN\_SUPPORT | Поддержка Markdown. По умолчанию включена, делать ничего не надо. |
| INPUT | Содержит папки и файлы, для которых будет генерироваться документация. Можно оставить пустым или указать ".", что будет значить генерацию для всех папок проекта. |
| FILE\_PATTERNS | Шаблоны имен, по которым нужно искать файлы для обработки. В нашем случае нужны заголовочные файлы .h и Markdown-файлы .md. |
| RECURSIVE | Включаем, чтобы Doxygen заходил во вложенные папки. |
| EXCLUDE\_PATTERNS | Шаблоны имен файлов или папок, которые Doxygen должен пропустить. |
| USE\_MDFILE\_AS\_MAINPAGE | Указываем Markdown-файл, который будет использоваться как главная страница документации. У нас это Main.md. |

Пример заполнения рассмотренных полей:

```plaintext
...
PROJECT_NAME           = "Library"
OUTPUT_DIRECTORY       = LibraryReferenceDocumentation
MARKDOWN_SUPPORT       = YES
INPUT                  = .
FILE_PATTERNS          = *.h \
                         *.md
RECURSIVE              = YES
USE_MDFILE_AS_MAINPAGE = Doc/Main.md
...
```

# Документация исходного кода

Правила оформления документации исходного кода хорошо отражены в [документации](https://www.doxygen.nl/manual/docblocks.html) Doxygen, а также в многочисленных [материалах](https://habr.com/ru/articles/252101/) в Сети. Поэтому рассмотрим лишь небольшой пример для класса нашей импровизированной библиотеки Lib:

```cpp
/**
 * @brief The Lib class defines a super library object.
 */
class Lib
{
public:
  /**
   * @brief Constructor.
   * 
   * @param arg the super argument
   */
  Lib(int arg);
};
```

Теперь запустим Doxygen, подав ему на вход наш конфигурационный файл:

```bash
doxygen Doxygen
```

Полученная документация класса Lib будет выглядеть так:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700895079708/daf8007e-c6bf-4908-99b6-28a2b7d129f6.png align="center")

# Особенности оформления Markdown

Как указано выше, главную страницу HTML-документации будет определять файл Main.md. Doxygen [поддерживает Markdown](https://www.doxygen.nl/manual/markdown.html), поэтому значимых проблем с разметкой быть не должно.

Файл Main.md обычно содержит приветственную информацию и ссылки на другие Markdown-файлы документации. Ссылки нужно указывать прямо на .md файлы относительно папки файла Main.md.

Рассмотрим небольшой пример Main.md:

```markdown
# Library

Welcome to Library documentation!

[Getting Started](Getting_Started.md)
```

Тогда главная страница HTML-документации будет выглядеть так:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700895881860/1f9a4551-497a-426b-8b3c-25dfa3d4457b.png align="center")

В файле Getting\_Started.md размещаем руководство по быстрому старту работы с нашей библиотекой:

```markdown
# Getting started

Just build the Lib!
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700896010392/9be83e11-db9e-4632-a434-682579edd190.png align="center")

# Вывод

Doc As Code в связке с Doxygen / Markdown - это отличный способ организовать документацию проекта на С++. Вся документация размещается в репозитории рядом с исходным кодом. Для небольших проектов этого более чем достаточно.

---

Телеграм: [**Так себе программист**](https://t.me/mediocre_developer).