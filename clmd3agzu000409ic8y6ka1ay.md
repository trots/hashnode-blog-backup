---
title: "Как определить собственные JS-функции при работе с QJSEngine от PySide6"
seoTitle: "Как определить собственные JS-функции при работе с QJSEngine от PySide"
seoDescription: "В инструкции рассмотрим как определять пользовательские JavaScript-функции для QJSEngine от PySide6 в программах на Python"
datePublished: Sun Sep 10 2023 06:44:05 GMT+0000 (Coordinated Universal Time)
cuid: clmd3agzu000409ic8y6ka1ay
slug: kak-opredelit-sobstvennye-js-funkcii-pri-rabote-s-qjsengine-ot-pyside6
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1694301235033/ac57edf1-d9a3-4555-b7d0-5e499ea29bb0.png
tags: javascript, python, pyside6

---

Наличие в Qt движка языка JavaScript открывает широкие возможности по расширению програмных систем. В инструкции ниже рассмотрим как определять пользовательские JavaScript-функции для [`QJSEngine` от PySide6](https://doc.qt.io/qt-6/qjsengine.html) в программах на Python.

Основная идея [заключается](https://doc.qt.io/qt-6/qjsengine.html#qobject-integration) в создании класса-наследника от `QObject` и определении в этом классе необходимых функций. Далее экземпляр класса и его функции регистрируются в `QJSEngine`. После этого функции могут быть вызваны из JavaScript-скриптов, подаваемых в метод `QJSEngine.evaluate()`.

На самом деле в сети масса примеров решения этой задачи на Qt C++. При этом для Qt Python (PySide) информации очень мало. Главная проблема, с которой пришлось столкнуться, это чем в Python заменить макрос `Q_INVOKABLE` из C++. [Выяснилось](https://srinikom.github.io/pyside-docs/PySide/QtCore/Slot.html), что достаточно аннотировать методы как слоты (`@Slot()`). Остальное - дело техники: все вызовы практически полностью соответствуют примерам на С++.

Полный скрипт `qjsengine_in_pyside_example.py` на Python:

```python
from PySide6.QtCore import QCoreApplication, QObject, Slot
from PySide6.QtQml import QJSEngine


# Define class with invokable methods
class MyObject(QObject):
    def __init__(self, parent = None):
        super().__init__(parent)

    @Slot()
    def print_start(self):
        print("Start")

    @Slot(result=int)
    def get_integer(self):
        return 33
    
    @Slot(int)
    def print_integer(self, value):
        print("Integer is", value)

    @Slot()
    def print_finish(self):
        print("Finish")


# Create application and JS engine
app = QCoreApplication()
engine = QJSEngine()

# Create MyObject instance and declare it in the JS engine
my_object = MyObject()
my_object_js = engine.newQObject(my_object)

# Define JS functions based on the MyObject methods
engine.globalObject().setProperty("printStart", my_object_js.property("print_start"))
engine.globalObject().setProperty("getInteger", my_object_js.property("get_integer"))
engine.globalObject().setProperty("printInteger", my_object_js.property("print_integer"))
engine.globalObject().setProperty("printFinish", my_object_js.property("print_finish"))

# Read JS script
script = open("test.js", "r").read()

# Run JS script
res = engine.evaluate(script)
```

Файл `test.js` с JS-скриптом для теста программы:

```javascript
printStart();
var i = getInteger();
printInteger(i);
printFinish();
```

Запуск программы (файлы `qjsengine_in_pyside_example.py` и `test.js` должны находиться в одной директории):

```bash
> python qjsengine_in_pyside_example.py
Start
Integer is 33
Finish
```

Файлы с исходным кодом можно найти [на GitHub Gists](https://gist.github.com/trots/b8985b61242fcc3426883054c28e752d).