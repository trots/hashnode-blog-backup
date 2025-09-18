---
title: "Как загрузить код проекта на GitHub"
seoTitle: "Как загрузить код проекта на GitHub"
seoDescription: "Рассмотрим несколько способов загрузки исходного кода проекта на GitHub"
datePublished: Fri Jan 07 2022 16:36:53 GMT+0000 (Coordinated Universal Time)
cuid: cky4mfcdk0b4vqfs18attbilj
slug: kak-zagruzit-kod-proekta-na-github
canonical: https://mediocre-developer.ru/kak-zagruzit-kod-proekta-na-github
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1641285856478/8xDUNxLvQ.png
tags: github, git

---

Уровень: новичок.

База: [системы контроля версий](https://www.google.com/search?q=%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D1%8B+%D0%BA%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D1%8F+%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D0%B9), [распределенная система контроля версий git](https://www.google.com/search?q=%D1%80%D0%B0%D1%81%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%BD%D0%B0%D1%8F+%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0+%D0%BA%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D1%8F+%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D0%B9+git).

## Создание пустого репозитория GitHub

Пусть есть проект с исходным кодом, состоящим из одного файла `main.py`. Загрузим его в репозиторий GitHub.

![project_file_in_explorer](https://cdn.hashnode.com/res/hashnode/image/upload/v1641285856478/8xDUNxLvQ.png)

Переходим на сайт [https://github.com](https://github.com). Регистрируемся, если не зарегистрированы, и входим в свой аккаунт. На главной странице GitHub нажимаем кнопку "New" для создания нового репозитория.

![new_repo](https://cdn.hashnode.com/res/hashnode/image/upload/v1641287190990/8B0_tAHfS.png)

На открывшейся странице вводим название репозитория. Название должно отражать суть проекта.

![new_repo_params](https://cdn.hashnode.com/res/hashnode/image/upload/v1641296163267/0kmfl6Ejo.png)

Нажимаем кнопку "Create repository" для завершения создания нового репозитория.

![new_repo_create](https://cdn.hashnode.com/res/hashnode/image/upload/v1641296448625/kAwJ4ZcwGs.png)

Пустой репозиторий GitHub создан. Теперь загрузим сюда код нашего проекта. Сделать это можно разными способами. Рассмотрим несколько из них.

> Не нужно загружать в репозиторий производные файлы проекта. Это те файлы, которые генерируются тем или иным образом из исходного кода. Например, это файлы сборки (вроде Makefile), файлы редактора кода (IDE) и т.п.


## Загрузка проекта через веб-интерфейс GitHub

Загрузить проект средствами GitHub проще и быстрее всего. Этот вариант хорошо подходит в случаях, когда нужно просто сохранить проект, чтобы не потерять или поделиться им. Открываем страницу проекта и нажимаем ссылку "uploading an existing file".

![upload_files_link](https://cdn.hashnode.com/res/hashnode/image/upload/v1641297038810/jL_uQ82uU.png)

Далее в открывшееся окно можно либо перетащить файлы, либо указать их через диалоговое окно по ссылке "choose your files".

![upload_files_choose](https://cdn.hashnode.com/res/hashnode/image/upload/v1641297130472/8IRw7C5T2.png)

Выбираем файлы (или папки, если есть) проекта и жмем "Открыть". 

![upload_files_open](https://cdn.hashnode.com/res/hashnode/image/upload/v1641297399257/thwqiMV-e.png)

Выбранные файлы отобразятся в интерфейсе.

![upload_files_preview](https://cdn.hashnode.com/res/hashnode/image/upload/v1641297538503/cwJVv_1q9m.png)

После загрузки всех файлов проекта, изменения нужно зафиксировать в репозитории. Иначе говоря, сделать коммит (от англ. commit). Нажимаем на кнопку "Commit changes" внизу страницы. При желании можно изменить автоматическое сообщение коммита "Add files via upload" на что-то свое. Например, "Первая версия проекта".

![upload_files_commit](https://cdn.hashnode.com/res/hashnode/image/upload/v1641297813080/fC7Occe_G.png)

После некоторого ожидания загрузки, мы увидим страницу нашего репозитория с файлом проекта.

![upload_files_result](https://cdn.hashnode.com/res/hashnode/image/upload/v1641297991813/tDWkrsCSO.png)


## Загрузка проекта через интерфейс командной строки git

Этот способ немного сложнее предыдущего и потребует навыков работы с командной строкой. Для начала работы нам нужно установить Git на свой компьютер. Переходим на сайт [git-scm.com](https://git-scm.com/) и скачиваем нужный установщик. Во время установки можно следовать рекомендуемым параметрам мастера установки.

![git_download](https://cdn.hashnode.com/res/hashnode/image/upload/v1641299711477/smCKS0KG6.png)

На странице проекта переключаемся на ссылку "HTTPS" и далее будем следовать инструкциям из первого блока (команды можно копировать прямо из него ).

![https_switch](https://cdn.hashnode.com/res/hashnode/image/upload/v1641299939334/Q97CNXuKR.png)

Запускаем консоль командной строки. В Windows ее можно найти в меню Пуск через поиск. И переходим в папку с проектом командой `cd`.

![cmd_cd](https://cdn.hashnode.com/res/hashnode/image/upload/v1641300279984/VU2BWzk0d.png)

Инициализируем пустой локальный репозиторий командой `git init`.

![git_init](https://cdn.hashnode.com/res/hashnode/image/upload/v1641300347986/jx2nAlLVn.png)

Добавляем в локальный репозиторий файлы проекта командой `git add`.

![git_add](https://cdn.hashnode.com/res/hashnode/image/upload/v1641300414301/P59Tmr3wU.png)

Фиксируем добавленные в репозиторий файлы командой `git commit`.

![git_commit](https://cdn.hashnode.com/res/hashnode/image/upload/v1641300606284/yGPz994Nc.png)

Переименуем главную ветку проекта в `main`.

![git_rename](https://cdn.hashnode.com/res/hashnode/image/upload/v1641301225044/O9VoomtKw.png)

Добавим созданный ранее репозиторий GitHub как удаленный репозиторий для нашего проекта. Не забудьте подставить свою ссылку в команде.

![git_remote](https://cdn.hashnode.com/res/hashnode/image/upload/v1641301436462/Lu5KlWkvgX.png)

Отправляем наш локальный репозиторий в GitHub командой `git push -u origin main`. Здесь git может запросить учетные данные пользователя GitHub - вводим свои логин и пароль.

![git_push](https://cdn.hashnode.com/res/hashnode/image/upload/v1641301653797/lAYeiG-jc.png)

Теперь заходим на страницу проекта в GitHub и видим наш репозиторий с файлом проекта. Если страница уже была открыта, ее нужно обновить.

![git_result](https://cdn.hashnode.com/res/hashnode/image/upload/v1641301799130/Ss7Jfgm_K.png)


## Загрузка проекта с помощью TortoiseGit

TortoiseGit - это графическая оболочка git для Windows с множеством приложений, облегчающих работу с системой контроля версий. Для начала работы с TortoiseGit, нужно установить сам git как это указано в начале предыдущего раздела. После этого [скачиваем установщик TortoiseGit](https://tortoisegit.org/) с официального сайта и устанавливаем программу, следуя указаниям мастера установки.

![tortoise_download](https://cdn.hashnode.com/res/hashnode/image/upload/v1641463124960/A6ySyow7W.png)

Переходим в Проводнике в папку с проектом и вызываем контекстное меню на свободном месте. В меню выбираем пункт "Git Create repository here..." для начала инициализации репозитория.

![tortoise_init](https://cdn.hashnode.com/res/hashnode/image/upload/v1641463362464/XBKszZS0t.png)

В появившемся окне просто жмем "Ок". Создавать чистый репозиторий нам не нужно.

![tortoise_skip_bare](https://cdn.hashnode.com/res/hashnode/image/upload/v1641463703994/_G_-kmRxa.png)

Получаем сообщение, что пустой репозиторий инициализирован. Жмем "Ок".

![tortoise_empty_initialized](https://cdn.hashnode.com/res/hashnode/image/upload/v1641463777792/FeOegL1or.png)

Теперь добавим в репозиторий файлы нашего проекта. В папке репозитория переходим в контекстное меню "TortoiseGit -> Add...".

![tortoise_add_menu](https://cdn.hashnode.com/res/hashnode/image/upload/v1641463925618/WY6SRV66R.png)

В открывшемся окне ставим галочки напротив файлов, которые мы хотим добавить в репозиторий. Жмем "Ок".

![tortoise_add_dialog](https://cdn.hashnode.com/res/hashnode/image/upload/v1641464038614/vjToVvuDv.png)

Следующим окном TortoiseGit информирует нас, что все выбранные файлы добавлены в репозиторий. Нажимаем кнопку "Commit...", чтобы зафиксировать внесенные файлы в репозитории или, другими словами, сделать коммит.

![tortoise_add_result](https://cdn.hashnode.com/res/hashnode/image/upload/v1641464269862/6zPxDRKgY.png)

Обязательно набираем сообщение коммита, например, "Добавление первой версии проекта" и жмем кнопку "Commit".

![tortoise_commit](https://cdn.hashnode.com/res/hashnode/image/upload/v1641464462339/jEuRgq140.png)

Дожидаемся успешного завершения коммита и закрываем окно.

![tortoise_commit_result](https://cdn.hashnode.com/res/hashnode/image/upload/v1641464597136/cNLl2eLVu.png)

Переименуем основную ветку репозитория в "main". Переходим в контекстное меню "TortoiseGit -> Browse References" в папке проекта.

![tortoise_browse_references_menu](https://cdn.hashnode.com/res/hashnode/image/upload/v1641464857349/gOK8JZzJd.png)

В открывшемся окне вызываем контекстное меню для основной ветки, жмем "Rename" и вводим новое имя ветки "main". Закрываем диалог по кнопке "Ок".

![tortoise_rename_branch](https://cdn.hashnode.com/res/hashnode/image/upload/v1641465084578/NUof3ftsE.png)

Укажем созданный репозиторий на GitHub как удаленный репозиторий для нашего проекта. Предварительно на странице проекта на GitHub переключаемся на ссылку "HTTPS" и копируем ее.

![tortoise_copy_link](https://cdn.hashnode.com/res/hashnode/image/upload/v1641463278383/PBJ6v6bNw.png)

Переходим в настройки репозитория через контекстное меню "TortoiseGit -> Settings..." в папке проекта.

![tortoise_settings_menu](https://cdn.hashnode.com/res/hashnode/image/upload/v1641572990529/JL1zZQfAa.png)

В разделе настроек "Remote" вводим имя удаленного репозитория как "origin" и в поле "URL:" вставляем ссылку репозитория на GitHub. Нажимаем кнопку "Add New/Save" для добавления репозитория в список.

![tortoise_add_origin](https://cdn.hashnode.com/res/hashnode/image/upload/v1641465508290/1b7QWgsik.png)

В появившемся запросе можем смело нажать "Нет". Все равно никаких веток в нашем репозитории на GitHub еще не существует.

![tortoise_discard_fetch](https://cdn.hashnode.com/res/hashnode/image/upload/v1641465870027/7lsUeSo9o.png)

Удаленный репозиторий origin добавлен. Закрываем окно настроек по кнопке "Ок".

![tortoise_origin_result](https://cdn.hashnode.com/res/hashnode/image/upload/v1641465967664/hU2LLhb8vA.png)

И, наконец, отправим наш проект в репозиторий GitHub. Вызываем окно "Push" через контекстное меню "TortoiseGit -> Push..." в папке проекта.

![tortoise_push_menu](https://cdn.hashnode.com/res/hashnode/image/upload/v1641466093179/MMXyTbKFa.png)

В окне "Push" обязательно ставим галочку "Set upstream/track remote branch", чтобы наша локальная ветка на компьютере привязалась к своей удаленной копии на GitHub. Нажимаем "Ок" и ждем окончания процесса отправки проекта.

![tortoise_push](https://cdn.hashnode.com/res/hashnode/image/upload/v1641466269683/EulEnbXQP.png)

Проект успешно отправлен на GitHub. Закрываем окно.

![tortoise_push_result](https://cdn.hashnode.com/res/hashnode/image/upload/v1641466353842/An2DecSIL.png)

Теперь заходим на страницу проекта в GitHub и видим наш репозиторий с файлом проекта. Если страница уже была открыта, ее нужно обновить.

![tortoise_result](https://cdn.hashnode.com/res/hashnode/image/upload/v1641466448859/yFt2k4cdCH.png)
