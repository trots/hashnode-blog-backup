---
title: "Как создать юнит-тест QtTest в среде разработки QtCreator"
seoTitle: "Как создать юнит-тест QtTest в среде разработки QtCreator"
seoDescription: "В рамках статьи рассмотрим как создавать юнит-тесты с помощью библиотеки QtTest в QtCreator."
datePublished: Mon Nov 06 2023 12:44:54 GMT+0000 (Coordinated Universal Time)
cuid: clomwa1tz000408l4bh1d0kh6
slug: kak-sozdat-yunit-test-qttest-v-srede-razrabotki-qtcreator
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1699273508666/ca920591-f53b-4171-98d4-94d451ac27bb.png
tags: unit-testing, qt, qt-creator, qttest

---

Среда разработки QtCreator включает в себя инструменты для работы с [юнит-тестами](https://ru.wikipedia.org/wiki/%D0%9C%D0%BE%D0%B4%D1%83%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5_%D1%82%D0%B5%D1%81%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5) проекта. В рамках статьи рассмотрим как создавать юнит-тесты с помощью библиотеки [QtTest](https://doc.qt.io/qt-6/qtest-overview.html) в QtCreator.

# Создание основного приложения

Начнем с создания нового проекта (CTRL+N) как проекта с поддиректориями (Subdirs Project). Этот проект по умолчанию не содержит ничего кроме файла проекта.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699218342328/492c7798-191e-4785-82d9-faee8bcbb902.png align="center")

Поэтому следующим шагом среда запрашивает создание первого подпроекта. Выбираем простое приложение на языке C++.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699218618194/65bc86ae-7a66-45b2-bbbc-4f5822ce814f.png align="center")

Мастер автоматически создаст шаблонное приложение.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699219092992/b4a3348b-403c-4f59-a3a1-9a20d3a4ba48.png align="center")

Добавим в проект импровизированный класс калькулятора `Calculator`, который умеет складывать (`sum()`), вычитать (`dif()`), умножать (`mul()`) и делить (`div()`) указанные значения.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699219248189/b1e39adf-30e3-49f1-b55b-4ae1f67074e0.png align="center")

Основное приложение-калькулятор готово. Теперь покроем юнит-тестами разработанный класс.

# Создание подпроекта с юнит-тестом

Через контекстное меню корневого элемента проекта (`untitled`) вызываем мастер создания нового подпроекта (New Subproject...) и выбираем подпроект авто-теста (Auto Test Project). В мастере вводим название подпроекта `test` и название класса юнит-теста `tst_Calculator`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699219564012/b2883f30-190f-44a9-9892-b324a56c83c8.png align="center")

В результате мастер создаст подпроект `test` с шаблонным модулем `tst_tst_calculator.cpp`. Модуль содержит следующие элементы:

* класс теста `tst_Calculator`, который будет реализовывать основную функциональность;
    
* точку входа `main()`, определенную макросом [`QTEST_APPLESS_MAIN()`](https://doc.qt.io/qt-6/qtest.html#QTEST_APPLESS_MAIN), который разворачивает всю необходимую рутину теста Qt;
    
* включение [moc-файла](https://doc.qt.io/qt-6/moc.html) (`#include "*.moc"`), который содержит мета-функциональность `QObject` для класса `tst_Calculator`.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699270519648/7f680189-d4c6-42be-9b14-49e10b3aa441.png align="center")

Класс теста в QtTest состоит из тест-методов, определенных как закрытые слоты (private slots). По умолчанию мастер создал один тест-метод `test_case1()`. Помимо тест-методов в QtTest существуют [специальные методы](https://doc.qt.io/qt-6/qtest-overview.html#creating-a-test), которые вызываются автоматически в ключевых точках выполнения теста:

* `initTestCase()` вызывается перед самым первым тест-методом;
    
* `cleanupTestCase()` вызывается после самого последнего тест-метода;
    
* `init()` вызывается перед каждым тест-методом;
    
* `cleanup()` вызывается после каждого тест-метода.
    

Эти методы можно использовать для инициализации или очистки каких-либо сущностей. Но в нашем примере мы их опустим.

Создадим 4 тест-метода для проверки соответствующих методов класса `Calculator`: `testSum()`, `testDif()`, `testMul()` и `testDiv()`. Для проверки возвращаемых методами значений воспользуемся макросом [`QCOMPARE(a, b)`](https://doc.qt.io/qt-6/qtest.html#QCOMPARE), который сравнивает текущее левое значение с эталонным правым и в случае несовпадения завершает тест с поясняющим сообщением.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699271582253/4bf16192-5a07-4d4e-8fa0-dd8cf2c603bc.png align="center")

> Еще один полезный макрос - [`QVERIFY(condition)`](https://doc.qt.io/qt-6/qtest.html#QVERIFY), прекращающий выполнение теста, если входящее условие не истино (`false`).

Для корректной компиляции проекта необходимо прописать исходные файлы класса `Calculator` в проект `test`. Иначе получим ошибки компоновки (link).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699272338244/50f01bab-7bed-4ca6-92f9-9427875a5e6e.png align="center")

Для запуска тестов используем панель Tests в QtCreator, где отображаются все юнит-тесты, которые среда разработки распознала в нашем проекте.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699272556792/04cbfd18-55cc-489c-b9c4-c421d9bd336b.png align="center")

Через контекстное меню можно запустить как все тесты разом, так и тесты отдельного класса, либо отдельный тест-метод. Есть возможность запуска под отладчиком.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699272788460/f296d41c-aa36-4a16-a372-7c213e7a20a4.png align="center")

Результат выполнения теста появится на соответствующей вкладке нижней панели (Test Results). В этот раз все юнит-тесты "зеленые", то есть прошли успешно.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699272940036/d6c961c7-ebb8-434a-84b1-e36933069f7c.png align="center")

В случае обнаружения несовпадений, QtCreator выведет описание проблемы и укажет, в какой строке она произошла.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699273026791/e007f27f-5f09-401a-881f-f79f19faf34b.png align="center")

Простейшие юнит-тесты на QtTest готовы. Полный код рассмотренного примера [размещен на GitHub](https://github.com/trots/qttest-example).

---

Телеграм: [Так себе программист](https://t.me/mediocre_developer).