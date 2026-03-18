Боковые изображения
===================

Многие визуальные новеллы включают в свой интерфейс изображение говорящего
персонажа. Ren'Py называет это изображение боковым (side image) и имеет
поддержку для автоматического выбора и отображения бокового изображения
в рамках диалога.

Поддержка боковых изображений предполагает, что :func:`Character` объявлен
со связанным тегом изображения::

    define e = Character("Eileen", image="eileen")

Когда персонаж со связанным тегом изображения говорит, Ren'Py создает пул
атрибутов изображения. Связанный тег изображения добавляется в этот пул, так же как и
текущие атрибуты изображения, связанные с этим тегом.

В дополнение к тегу, в пуле должен быть хотя бы один атрибут.
Если нет, боковое изображение не показывается.

Чтобы определить боковое изображение, связанное с тегом, Ren'Py пытается найти
изображение с тегом "side" и наибольшим количеством атрибутов из
пула. Если изображение не может быть найдено или несколько изображений имеют
одинаковое количество атрибутов, вместо него отображается :class:`Null`.

Например, допустим, у нас есть следующий скрипт::

    define e = Character("Eileen", image="eileen")

    image eileen happy = "eileen_happy.png"
    image eileen concerned = "eileen_concerned.png"

    image side eileen happy = "side_eileen_happy.png"
    image side eileen = "side_eileen.png"

    label start:

        show eileen happy

        e "Let's call this line Point A."

        e concerned "And this one is point B."

В точке A говорит персонаж ``e``, который связан с тегом изображения
"eileen". Отображается изображение "eileen happy", поэтому пул атрибутов
составляет "eileen" и "happy". Мы ищем изображение с тегом "side" и как
можно большим количеством этих атрибутов — и находим совпадение с "side eileen happy",
что и является боковым изображением, которое Ren'Py отобразит.

В точке B отображается изображение "eileen concerned". Пул атрибутов
теперь составляет "eileen" и "concerned". Единственное совпадающее изображение — "side eileen",
поэтому Ren'Py выбирает его. Если бы существовало изображение "side concerned",
возникла бы неоднозначность, и Ren'Py не стал бы отображать изображение.


Невидимые персонажи
-------------------

Другое применение бокового изображения — показывать изображение персонажа игрока,
когда у этого персонажа есть реплика. Для этого нужно связать изображение с
персонажем, а затем использовать конструкцию say with attributes, чтобы выбрать
боковое изображение для показа.

Например::

    define p = Character("Player", image="player")

    image side player happy = "side_player_happy.png"
    image side player concerned = "side_player_concerned.png"

    label start:

        p happy "This is shown with the 'side player happy' image."

        p "This is also shown with 'side player happy'."

        p concerned "This is shown with 'side player concerned'."

Переменные Config и Store
-------------------------

Существует ряд атрибутов боковых изображений, которыми можно управлять
с помощью переменных конфигурации.

.. var:: _side_image_tag = None
.. var:: config.side_image_tag = None

    Если _side_image_tag не равно None, оно имеет приоритет над config.side_image_tag.

    Если это указано, то боковое изображение будет отслеживать данный тег изображения,
    а не изображение, связанное с говорящим в данный момент персонажем. Например,

    ::

        define e = Character("Eileen", image="eileen")
        define config.side_image_tag = "eileen"

    Заставит боковое изображение отслеживать тег "eileen", который связан
    с персонажем ``e``.

.. var:: config.side_image_only_not_showing = False

    Если установлено в true, боковое изображение будет отображаться только в том случае, если изображение с этим тегом
    еще не отображается на экране.

.. var:: _side_image_prefix_tag = None
.. var:: config.side_image_prefix_tag = 'side'

    Если _side_image_prefix_tag не равно None, оно имеет приоритет над
    config.side_image_prefix_tag.

    Префикс, который используется при поиске бокового изображения.

.. var:: config.side_image_null = Null()

    Null displayable-объект, который используется, когда боковое изображение не отображается. Его можно
    изменить, но только на другие Null-объекты. Одной из причин для этого
    может быть установка размера Null (например, ``Null(width=200, height=150)``)
    для предотвращения обрезки dissolve-эффектов.

.. var:: config.side_image_same_transform = None

    Если не None, это трансформация, которая используется, когда новое боковое изображение
    имеет тот же тег, что и предыдущее боковое изображение.

.. var:: config.side_image_change_transform = None

    Если не None, это трансформация, которая используется, когда новое боковое изображение
    не имеет того же тега изображения (или когда одно из старых или новых
    боковых изображений не существует).


Трансформации и переходы
------------------------

Трансформации :var:`config.side_image_same_transform` и
:var:`config.side_image_change_transform` вызываются с двумя
аргументами — старым и новым displayable-объектами боковых изображений — каждый раз,
когда отображается боковое изображение. Их можно использовать для перемещения
боковых изображений или для применения перехода между ними.

Этот код заставляет боковое изображение выезжать и заезжать при смене
персонажа, связанного с этим изображением::

    transform change_transform(old, new):
        contains:
            old
            yalign 1.0
            xpos 0.0 xanchor 0.0
            linear 0.2 xanchor 1.0
        contains:
            new
            yalign 1.0
            xpos 0.0 xanchor 1.0
            linear 0.2 xanchor 0.0

    define config.side_image_change_transform = change_transform

Это используется для dissolve-перехода между старым и новым боковыми изображениями,
когда персонаж остается прежним (например, при смене эмоции).
Чтобы :class:`Dissolve` работал корректно, оба боковых изображения должны
быть одинакового размера. ::

    transform same_transform(old, new):
        old
        new with Dissolve(0.2, alpha=True)

    define config.side_image_same_transform = same_transform


Когда :func:`SideImage` уменьшается, может иметь смысл включить
мипмаппинг в :func:`Dissolve`::

    transform same_transform(old, new):
        old
        new with Dissolve(0.2, alpha=True, mipmap=True)

    define config.side_image_same_transform = same_transform

Функции
-------

.. include:: inc/side