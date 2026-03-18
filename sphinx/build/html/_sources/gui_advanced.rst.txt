.. _gui-advanced:

===============
Продвинутый GUI
===============

Этот раздел содержит разрозненную информацию о продвинутом использовании GUI.


Функции Python
==============

Существуют некоторые функции Python, которые поддерживают GUI.

.. include:: inc/gui

.. function:: gui.button_text_properties(kind=None, accent=False):

    Устаревший псевдоним для :func:`gui.text_properties`.

Подробнее о gui.rebuild
-----------------------

Функция `gui.rebuild` — это довольно медленная функция, которая обновляет GUI
в соответствии с текущим состоянием Ren'Py. Она делает следующее:

* Перезапускает все операторы ``define``, которые определяют переменные в пространстве
  имён `gui`.
* Перезапускает все блоки ``translate python`` для текущего языка.
* Перезапускает все операторы ``style``.
* Пересоздаёт все стили в системе.

Обратите внимание, что блоки ``init python`` не перезапускаются при вызове ``gui.rebuild``.
В этом смысле, ::

    define gui.text_size = persistent.text_size

и::

    init python:
        gui.text_size = persistent.text_size

отличаются.


.. _gui-default:

Оператор default, пространство имён gui и gui.rebuild
-----------------------------------------------------

Оператор ``default`` имеет изменённую семантику применительно к пространству имён ``gui``.
Когда он применяется к переменной в пространстве имён ``gui``, оператор `default`
выполняется вперемежку с операторами `define`, и операторы `default`
не перезапускаются при вызове :func:`gui.rebuild`.

Это означает, что если у нас есть::

    default gui.accent_color = "#c04040"
    define gui.hover_color = gui.accent_color

При первом запуске игры будет установлен `accent_color`, а затем `hover_color` будет
присвоен цвет `accent_color`. (Оба цвета затем используются для установки
различных цветов стиля.)

Однако если в рамках игрового скрипта у нас есть::

    $ gui.accent_color = "#4040c0"
    $ gui.rebuild()

Ren'Py перезапустит только `define`, поэтому он установит `hover_color` равным
`accent_color`, а затем обновит стили. Это позволяет создавать
части GUI, которые меняются по ходу игры.

При установке переменных `gui` во время выполнения может потребоваться вызывать `gui.rebuild` в
метке ``after_load``::

    label after_load:
        $ gui.rebuild()
        return

Это относится только к переменным, установленным с помощью ``default``. Все переменные в
пространстве имён `gui` должны устанавливаться с помощью ``default`` или ``define``.


.. _gui-preferences:

Настройки GUI
=============

Ren'Py также поддерживает систему настроек GUI, состоящую из одной функции
и пары действий.

.. include:: inc/gui_preference

Пример
------

Система настроек GUI используется путём вызова :func:`gui.preference` при определении
переменных, с указанием имени настройки и значения по умолчанию.
Например, можно использовать настройки GUI для определения шрифта и
размера текста. ::

    define gui.text_font = gui.preference("font", "DejaVuSans.ttf")
    define gui.text_size = gui.preference("size", 22)

Затем можно использовать действия :class:`gui.SetPreference` и :class:`gui.TogglePreference`
для изменения значений настроек. Вот несколько примеров,
которые можно добавить на экран настроек. ::

    vbox:
        style_prefix "check"
        label _("Options")
        textbutton _("OpenDyslexic") action gui.TogglePreference("font", "OpenDyslexic-Regular.otf", "DejaVuSans.ttf")

    vbox:
        style_prefix "radio"
        label _("Text Size")
        textbutton _("Small") action gui.SetPreference("size", 20)
        textbutton _("Medium") action gui.SetPreference("size", 22)
        textbutton _("Big") action gui.SetPreference("size", 24)