---
title: "Как расширить FreeCAD для импорта нового формата данных"
seoTitle: "Как расширить FreeCAD для импорта нового формата данных"
seoDescription: "Создаем простейший плагин для импорта пользовательских данных в FreeCAD"
datePublished: Sat Sep 09 2023 11:35:02 GMT+0000 (Coordinated Universal Time)
cuid: clmby8sqo001009l2f8jr2j19
slug: kak-rasshirit-freecad-dlya-importa-novogo-formata-dannyh
canonical: https://mediocre-developer.ru/freecad-import-extension
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1694258884753/b81206fd-30a0-46b3-bc2d-6b4811f02289.png
tags: 3d, cad, freecad, sapr

---

# Определение формата данных

Для примера определим импровизированный формат данных `.box`, описывающий параметры модели параллелепипеда. За основу формата возьмем JSON и объявим три параметра параллелепипеда: ширину, высоту и толщину. Тогда файл модели `model.box` будет содержать:

```json
{
  "width": 100,
  "height": 200,
  "depth": 300
}
```

# Структура расширения FreeCad

[Расширение FreeCad](https://wiki.freecad.org/Workbench_creation) (или аддон) реализуется на Python и в простоте своей имеет структуру как показано на схеме ниже. Файлы `Init.py` и `InitGui.py` являются обязательными. Файл `Init.py` используется FreeCad в консольном режиме, но вызывается и в режиме GUI. Файл `InitGui.py` содержит функциональность для расширения пользовательского интерфейса.

```plaintext
/Mod/
 +-- FreecadMod/
     +-- Init.py
     +-- InitGui.py
```

Для загрузки расширения в FreeCad достаточно разместить папку с расширением в Mod-директорию. В Windows эта директория обычно находится здесь: `C:\Users\<username>\Appdata\Roaming\FreeCAD\Mod`.

# Реализация расширения

Создадим модуль `FreecadImportMode.py` в папке расширения и разместим в нем реализацию импорта файлов формата `.box`. Определим две функции: `addExtension()` и `insert()`.

Функция `addExtension()` добавляет новый импортируемый формат данных в систему FreeCad.

```python
def addExtension():
    FreeCAD.addImportType("Box file type (*.box)", "FreecadImportMod")
```

Функция `insert()` будет вызываться системой FreeCad автоматически при открытии файла формата `.box`. Первый аргумент функции - имя импортируемого файла, а второй - название документа, в который происходит импорт.

Внутри функции необходимо открыть входящий файл и прочитать параметры модели параллелепипеда:

```python
def insert(file_name, document_name):
    with open(file_name) as box_file:
        data = json.load(box_file)
        width = data["width"]
        height = data["height"]
        depth = data["depth"]
```

Далее получаем объект документа и создаем эскиз с прямоугольником:

```python
doc = FreeCAD.getDocument(document_name)

doc.addObject('Sketcher::SketchObject', 'Sketch')
doc.Sketch.Placement = FreeCAD.Placement(FreeCAD.Vector(0.0, 0.0, 0.0), 
                                         FreeCAD.Rotation(0.0, 0.0, 0.0, 1.0))
line1 = Part.LineSegment()
line1.StartPoint = (0.0, 0.0, 0.0)
line1.EndPoint = (width, 0.0, 0.0)
doc.Sketch.addGeometry(line1, False)

line2 = Part.LineSegment()
line2.StartPoint = (width, 0.0, 0.0)
line2.EndPoint = (width, height, 0.0)
doc.Sketch.addGeometry(line2, False)

line3 = Part.LineSegment()
line3.StartPoint = (width, height, 0.0)
line3.EndPoint = (0.0, height, 0.0)
doc.Sketch.addGeometry(line3, False)

line4 = Part.LineSegment()
line4.StartPoint = (0.0, height, 0.0)
line4.EndPoint = (0.0, 0.0, 0.0)
doc.Sketch.addGeometry(line4, False)
```

После создания эскиза выполняем операцию "выдавливания" на заданную величину "depth" и обновляем документ:

```python
doc.addObject('Part::Extrusion','Extrude')
doc.Extrude.Base = doc.Sketch
doc.Extrude.DirMode = "Normal"
doc.Extrude.DirLink = None
doc.Extrude.LengthFwd = depth
doc.Extrude.LengthRev = 0.0
doc.Extrude.Solid = True
doc.Extrude.Reversed = False
doc.Extrude.Symmetric = False
doc.Extrude.TaperAngle = 0.0
doc.Extrude.TaperAngleRev = 0.0

doc.recompute()
```

В случае такого расширения изменений в интерфейсе FreeCad не требуется, поэтому файл `InitGui.py` оставляем пустым. При этом в файле `Init.py` необходимо вызвать функцию добавления нового формата в FreeCad:

```python
import FreecadImportMod
FreecadImportMod.addExtension()
```

Получается такая структура файлов расширения:

```plaintext
/Mod/
 +-- FreecadImportMod/
     +-- Init.py
     +-- InitGui.py
     +-- FreecadImportMod.py
```

Размещаем папку с расширением в директории Mod (`C:\Users\<username>\Appdata\Roaming\FreeCAD\Mod`).

# Результат

Запускаем FreeCad, создаем новый документ и переходим в меню "Файл -&gt; Импортировать...", где выбираем файл нашей модели `model.box`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694258379525/100353dc-b49a-4a8b-9765-240995c0bfc4.png align="center")

По нажатию кнопки "Открыть" выполняется сценарий функции `insert()` расширения `FreecadImportMode` и в результате строится модель параллелепипеда с заданными в файле параметрами.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694258728953/fe848840-89a9-4b19-b815-0cf986370380.png align="center")

Исходный код рассмотренного примера [доступен на GitHub](https://github.com/trots/FreecadImportMod).