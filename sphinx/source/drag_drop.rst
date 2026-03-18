.. _drag-and-drop:

Перетаскивание (Drag and Drop)
==============================

Ren'Py включает в себя displayable-объекты для перетаскивания, которые позволяют
перемещать элементы по экрану с помощью мыши. Некоторые из применений перетаскивания:

*   Возможность пользователям изменять положение окон с сохранением их позиций.
*   Карточные игры, в которых требуется перетаскивать карты по экрану (например, пасьянс).
*   Системы инвентаря.
*   Системы изменения порядка перетаскиванием.

Displayable-объекты для перетаскивания позволяют реализовать эти и другие
сценарии использования. Здесь задействованы два класса. Класс
Drag представляет собой нечто, что можно перетаскивать по экрану, нечто, на
что можно сбросить перетаскиваемый объект, или нечто, что может выполнять
обе эти функции. Класс DragGroup представляет собой группу Drag-объектов. Чтобы
перетаскивание произошло, оба Drag-объекта должны быть частью одной и той же
drag-группы.

Систему перетаскивания можно использовать либо через :doc:`язык экранов <screens>`,
либо непосредственно как displayable-объекты. Имеет смысл использовать язык экранов,
когда вам не нужно обращаться к созданным Drag-объектам после их создания.
Это может быть случай, когда перетаскиваемый объект представляет собой окно,
которое пользователь размещает на экране. Если вам нужно обращаться к
drag-объектам после их создания, то часто лучше создавать Drag-объекты
напрямую и добавлять их в DragGroup.

Сбрасывание
-----------

Ren'Py может обрабатывать сбрасывание объекта двумя способами:

*   Если `mouse_drop` имеет значение true, перетаскиваемый объект будет сброшен
    на тот droppable-объект, который находится непосредственно под курсором мыши.
*   Если `mouse_drop` имеет значение false (по умолчанию), сбрасывание произойдет на
    тот droppable-объект, с которым у перетаскиваемого объекта наибольшее перекрытие.

В отличие от начала перетаскивания, где используется `focus_mask`, при сбрасывании
учитываются полные прямоугольные области перетаскиваемого и принимающего
объектов, включая любые прозрачные пиксели. Вам может потребоваться
спроектировать ваши displayable-объекты для перетаскивания с учётом этого,
делая их в основном прямоугольной формы.

Displayable-объекты
-------------------

.. include:: inc/drag_drop

Примеры
-------

Пример экрана say, который позволяет пользователю выбирать расположение
окна, перетаскивая его по экрану.::

    screen say(who, what):

        drag:
            drag_name "say"
            yalign 1.0
            drag_handle (0, 0, 1.0, 30)

            xalign 0.5

            window id "window":
                # Ensure that the window is smaller than the screen.
                xmaximum 600

                has vbox

                if who:
                    text who id "who"

                text what id "what"

Вот более сложный пример, который показывает, как перетаскивание может
использоваться для влияния на игровой процесс. Он демонстрирует, как с
помощью перетаскивания можно отправить персонажа в определённое место.::

    init python:

        def detective_dragged(drags, drop):

            if not drop:
                return

            store.detective = drags[0].drag_name
            store.city = drop.drag_name

            return True

    screen send_detective_screen:

        # A map as background.
        add "europe.jpg"

        # A drag group ensures that the detectives and the cities can be
        # dragged to each other.
        draggroup:

            # Our detectives.
            drag:
                drag_name "Ivy"
                droppable False
                dragged detective_dragged
                xpos 100 ypos 100

                add "ivy.png"
            drag:
                drag_name "Zack"
                droppable False
                dragged detective_dragged
                xpos 150 ypos 100

                add "zack.png"

            # The cities they can go to.
            drag:
                drag_name "London"
                draggable False
                xpos 450 ypos 140

                add "london.png"

            drag:
                drag_name "Paris"
                draggable False
                xpos 500 ypos 280

                add "paris.png"

    label send_detective:
        "We need to investigate! Who should we send, and where should they go?"

        call screen send_detective_screen

        "Okay, we'll send [detective] to [city]."


Для правильной реализации более сложных систем требуются значительные
навыки программирования.

..
    The `Ren'Py cardgame framework <http://www.renpy.org/wiki/renpy/Frameworks#Cardgame>`_
    является как примером использования перетаскивания в сложной
    системе, так и сам по себе полезен для создания карточных игр.

.. _as-example:

Выражение ``as`` можно использовать для привязки drag-объекта к переменной, которую
затем можно использовать для вызова методов этого объекта. ::

    screen snap():

        drag:
            as carmen
            draggable True
            xpos 100 ypos 100
            frame:
                style "empty"
                background "carmen.png"
                xysize (100, 100)

                vbox:
                    textbutton "London" action Function(carmen.snap, 450, 140, 1.0)
                    textbutton "Paris" action Function(carmen.snap, 500, 280, 1.0)