=========================
Специальные имена экранов
=========================

В Ren'Py есть два вида специальных имён экранов. Первый вид — это
экраны, которые будут автоматически отображаться при выполнении команд языка
сценариев Ren'Py (или их программных эквивалентов).
Второй вид — это экраны меню. У них есть общепринятые имена для
общепринятой функциональности, но экраны можно опускать или изменять
по мере необходимости.

На этой странице мы приведём примеры экранов. Важно понимать,
что, хотя некоторые экраны должны обладать минимальной функциональностью, система
экранов позволяет добавлять к ним дополнительные возможности.
Например, хотя стандартный экран say отображает только
текст, система экранов позволяет легко добавить такие функции, как пропуск,
режим автопрокрутки или отключение звука.

Некоторые специальные экраны принимают параметры. К этим параметрам можно
обращаться как к переменным в области видимости экрана.

У некоторых экранов также есть связанные с ними специальные id.
Специальный id должен быть присвоен отображаемому объекту (displayable)
определённого типа. Это может привести к присвоению свойств этому
отображаемому объекту и сделать его доступным для той части Ren'Py,
которая отображает экран.

.. note::

    Примеры экранов на этой странице могут не совпадать с экранами, включёнными в состав новой созданной игры.

Внутриигровые экраны
====================

Эти экраны автоматически отображаются при выполнении определённых операторов Ren'Py.

.. _say-screen:

Say
---

Экран ``say`` вызывается оператором say при отображении диалога в режиме ADV.
Он отображается со следующими параметрами:

`who`
    Текст имени говорящего персонажа.
`what`
    Диалог, произносимый говорящим персонажем.

Ожидается, что будут объявлены отображаемые объекты (displayables) со следующими id:

"who"
    Текстовый отображаемый объект, показывающий имя говорящего
    персонажа. Объекту персонажа можно передать аргументы, которые стилизуют
    этот displayable.

"what"
    Текстовый отображаемый объект, показывающий реплику говорящего
    персонажа. Объекту персонажа можно передать аргументы, которые стилизуют
    этот displayable. **Отображаемый объект (displayable) с этим id должен быть определён**,
    так как Ren'Py использует его для расчёта времени режима автопрокрутки,
    ожидания клика для продолжения (click-to-continue) и других вещей.

"window"
    Окно или фрейм. Обычно он содержит текст who и what.
    Объекту персонажа можно передать аргументы, которые стилизуют
    этот displayable.

::

    screen say(who, what):

        window id "window":
            has vbox

            if who:
                text who id "who"

            text what id "what"


.. _choice-screen:

Choice
------

Экран ``choice`` используется для отображения внутриигровых выборов, созданных с помощью
оператора menu. Ему передаётся следующий параметр:

`items`
    Это список объектов записей меню, представляющих каждый из
    выборов в меню. У каждого объекта есть следующие
    поля:

    .. attribute:: caption

        Строка с заголовком пункта меню.

    .. attribute:: action

        Действие, которое должно быть вызвано при выборе пункта
        меню. Может быть None, если это заголовок меню, а
        :var:`config.narrator_menu` имеет значение False.

    .. attribute:: chosen

        Имеет значение True, если этот выбор был сделан хотя бы один раз
        за любое прохождение игры.

    .. attribute:: args

        Это кортеж, содержащий любые позиционные аргументы,
        переданные пункту меню.

    .. attribute:: kwargs

        Это словарь, содержащий любые именованные аргументы,
        переданные пункту меню.

    Эти элементы и действия в них становятся недействительными после
    завершения оператора menu.

Кроме того, любые аргументы, переданные оператору menu, передаются во время
вызова экрана.

::

    screen choice(items):

        window:
            style "menu_window"

            vbox:
                style "menu"

                for i in items:

                    if i.action:

                        button:
                            action i.action
                            style "menu_choice_button"

                            text i.caption style "menu_choice"

                    else:
                        text i.caption style "menu_caption"


.. _input-screen:

Input
-----

Экран ``input`` используется для отображения :func:`renpy.input`. Ему передаётся
один параметр:

`prompt`
    Текст-подсказка, предоставленный renpy.input.

Ожидается, что будет объявлен отображаемый объект (displayable) со следующим id:

"input"
    Отображаемый объект для ввода, который должен существовать. Ему
    передаются все параметры, предоставленные renpy.input,
    поэтому он должен существовать.

::

    screen input(prompt):

        window:
            has vbox

            text prompt
            input id "input"


.. _nvl-screen:

NVL
---

Экран ``nvl`` используется для отображения диалога в режиме NVL. Ему
передаётся следующий параметр:

`dialogue`
    Список объектов NVL Entry, каждый из которых соответствует
    строке диалога для отображения. Каждая запись имеет следующие
    поля:

    .. attribute:: current

        Истинно (True), если это текущая строка диалога. Текст `what`
        текущей строки диалога должен отображаться с
        id `what`.

    .. attribute:: who

        Имя говорящего персонажа, или None, если такого
        имени нет.

    .. attribute:: what

        Произносимый текст.

    .. attribute:: who_id, what_id, window_id

        Предпочтительные id для говорящего, диалога и окна, связанных с
        записью.

    .. attribute:: who_args, what_args, window_args

        Свойства, связанные с говорящим, диалогом и окном. Они
        применяются автоматически, если id установлен, как указано выше, но также
        доступны и отдельно.

    .. attribute:: multiple

        Если используется :doc:`диалог с несколькими персонажами <multiple>`, это
        кортеж из двух компонентов. Первый компонент — это номер
        блока диалога (начиная с 1), а второй — общее количество
        блоков диалога в операторе `multiple`.

`items`
    Это тот же список элементов, который был бы предоставлен экрану
    :ref:`choice <choice-screen>`. Если он пуст,
    меню не должно отображаться.

Когда `items` отсутствует, ожидается, что экран NVL всегда будет
присваивать текстовому виджету id `what`. Ren'Py использует его для
расчёта времени режима автопрокрутки, ожидания клика для продолжения
(click-to-continue) и других вещей. (Это
выполняется автоматически, если используется `what_id` по умолчанию.)

Ren'Py также поддерживает экран ``nvl_choice``, который принимает те же
параметры, что и ``nvl``, и используется вместо ``nvl``, когда
пользователю предоставляется внутриигровой выбор, если такой экран существует.

::

    screen nvl(dialogue, items=None):

        window:
            style "nvl_window"

            has vbox:
                style "nvl_vbox"

            # Display dialogue.
            for d in dialogue:
                window:
                    id d.window_id

                    has hbox:
                        spacing 10

                    if d.who is not None:
                        text d.who id d.who_id

                    text d.what id d.what_id

            # Display a menu, if given.
            if items:

                vbox:
                    id "menu"

                    for i in items:

                        if action:

                            button:
                                style "nvl_menu_choice_button"
                                action i.action

                                text i.caption style "nvl_menu_choice"

                        else:

                            text i.caption style "nvl_dialogue"


.. _notify-screen:

Notify
------

Экран ``notify`` используется функцией :func:`renpy.notify` для отображения
уведомлений пользователю. Обычно он используется в сочетании с
трансформацией (transform) для выполнения всей задачи уведомления.
Ему передаётся один параметр:

`message`
    Сообщение для отображения.

Пример экрана notify::

    screen notify(message):
        zorder 100

        text "[message!tq]" at _notify_transform

        # This controls how long it takes between when the screen is
        # first shown, and when it begins hiding.
        timer 3.25 action Hide('notify')

    transform _notify_transform:
        # These control the position.
        xalign .02 yalign .015

        # These control the actions on show and hide.
        on show:
            alpha 0
            linear .25 alpha 1.0
        on hide:
            linear .5 alpha 0.0


.. _skip-indicator:

Индикатор пропуска
------------------

Если он существует, экран ``skip_indicator`` отображается во время пропуска
и скрывается, когда пропуск завершается. Он не принимает параметров.

Вот очень простой пример экрана индикатора пропуска::


    screen skip_indicator():

        zorder 100

        text _("Skipping")


.. _ctc-screen:

CTC (Нажмите-для-продолжения)
-----------------------------

Если он существует, экран ``ctc`` отображается после завершения показа
диалога, чтобы побудить игрока нажать для отображения дальнейшего текста.
Ему может быть передан один позиционный параметр и несколько именованных аргументов.

`arg`
    Отображаемый объект (displayable) ctc, выбранный :func:`Character`. Это один из
    аргументов `ctc`, `ctc_pause` или `ctc_timedpause` для Character,
    в зависимости от ситуации. Если CTC не задан для Character, этот аргумент
    вообще не передаётся.

Кроме того, есть несколько параметров, которые передаются, только если экран
их запрашивает.

`ctc_kind`
    Тип отображаемого CTC. Один из: "last" (для последнего CTC в строке),
    "pause" или "timedpause".

`ctc_last`
    Аргумент `ctc` для :func:`Character`.

`ctc_pause`
    Аргумент `ctc_pause` для :func:`Character`.

`ctc_timedpause`
    Аргумент `ctc_timedpause` для :func:`Character`.


Вот очень простой пример экрана ctc::

    screen ctc(arg=None):

        zorder 100

        hbox:
            xalign 0.98
            yalign 0.98

            add arg

            text _("Click to Continue"):
                size 12



Внеигровые экраны меню
======================

Это экраны меню. Экраны ``main_menu`` и ``yesno_prompt``
вызываются неявно. Когда пользователь вызывает игровое меню,
будет отображён экран, имя которого указано в :data:`_game_menu_screen`.
(По умолчанию это ``save``.)

Помните, что экраны меню можно довольно свободно комбинировать и изменять.

.. _main-menu-screen:

Главное меню
------------

Экран ``main_menu`` — это первый экран, который отображается при
запуске игры.

::

    screen main_menu():

        # This ensures that any other menu screen is replaced.
        tag menu

        # The background of the main menu.
        window:
            style "mm_root"

        # The main menu buttons.
        frame:
            style_prefix "mm"
            xalign .98
            yalign .98

            has vbox

            textbutton _("Start Game") action Start()
            textbutton _("Load Game") action ShowMenu("load")
            textbutton _("Preferences") action ShowMenu("preferences")
            textbutton _("Help") action Help()
            textbutton _("Quit") action Quit(confirm=False)

    style mm_button:
        size_group "mm"

.. _navigation-screen:

Навигация
---------

Экран ``navigation`` не является специальным для Ren'Py. Но
по соглашению, мы размещаем навигацию игрового меню в экране
с именем ``navigation``, а затем используем этот экран из экранов
сохранения, загрузки и настроек.

::

    screen navigation():

        # The background of the game menu.
        window:
            style "gm_root"

        # The various buttons.
        frame:
            style_prefix "gm_nav"
            xalign .98
            yalign .98

            has vbox

            textbutton _("Return") action Return()
            textbutton _("Preferences") action ShowMenu("preferences")
            textbutton _("Save Game") action ShowMenu("save")
            textbutton _("Load Game") action ShowMenu("load")
            textbutton _("Main Menu") action MainMenu()
            textbutton _("Help") action Help()
            textbutton _("Quit") action Quit()

    style gm_nav_button:
        size_group "gm_nav"

.. _save-screen:

Сохранение
----------

Экран ``save`` используется для выбора файла, в который будет
сохранена игра.

::

    screen save():

        # This ensures that any other menu screen is replaced.
        tag menu

        use navigation

        frame:
            has vbox

            # The buttons at the top allow the user to pick a
            # page of files.
            hbox:
                textbutton _("Previous") action FilePagePrevious()
                textbutton _("Auto") action FilePage("auto")

                for i in range(1, 9):
                    textbutton str(i) action FilePage(i)

                textbutton _("Next") action FilePageNext()

            # Display a grid of file slots.
            grid 2 5:
                transpose True
                xfill True

                # Display ten file slots, numbered 1 - 10.
                for i in range(1, 11):

                    # Each file slot is a button.
                    button:
                        action FileAction(i)
                        xfill True
                        style "large_button"

                        has hbox

                        # Add the screenshot and the description to the
                        # button.
                        add FileScreenshot(i)
                        text ( " %2d. " % i
                               + FileTime(i, empty=_("Empty Slot."))
                               + "\n"
                               + FileSaveName(i)) style "large_button_text"

.. _load-screen:

Загрузка
--------

Экран ``load`` используется для выбора файла, из которого будет
загружена игра.

::

    screen load():

        # This ensures that any other menu screen is replaced.
        tag menu

        use navigation

        frame:
            has vbox

            # The buttons at the top allow the user to pick a
            # page of files.
            hbox:
                textbutton _("Previous") action FilePagePrevious()
                textbutton _("Auto") action FilePage("auto")

                for i in range(1, 9):
                    textbutton str(i) action FilePage(i)

                textbutton _("Next") action FilePageNext()

            # Display a grid of file slots.
            grid 2 5:
                transpose True
                xfill True

                # Display ten file slots, numbered 1 - 10.
                for i in range(1, 11):

                    # Each file slot is a button.
                    button:
                        action FileAction(i)
                        xfill True
                        style "large_button"

                        has hbox

                        # Add the screenshot and the description to the
                        # button.
                        add FileScreenshot(i)
                        text ( " %2d. " % i
                               + FileTime(i, empty=_("Empty Slot."))
                               + "\n"
                               + FileSaveName(i)) style "large_button_text"

.. _preferences-screen:

Настройки
---------

Экран ``preferences`` используется для выбора опций, которые управляют
отображением игры.

В целом, настройки — это либо действия (actions), либо значения для
ползунка (bar values), возвращаемые функцией :func:`Preference`.

::

    screen preferences():

        tag menu

        # Include the navigation.
        use navigation

        # Put the navigation columns in a three-wide grid.
        grid 3 1:
            style_prefix "prefs"
            xfill True

            # The left column.
            vbox:
                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Display")
                    textbutton _("Window") action Preference("display", "window")
                    textbutton _("Fullscreen") action Preference("display", "fullscreen")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Transitions")
                    textbutton _("All") action Preference("transitions", "all")
                    textbutton _("None") action Preference("transitions", "none")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Text Speed")
                    bar value Preference("text speed")

                frame:
                    style_prefix "pref"
                    has vbox

                    textbutton _("Joystick...") action ShowMenu("joystick_preferences")

            vbox:

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Skip")
                    textbutton _("Seen Messages") action Preference("skip", "seen")
                    textbutton _("All Messages") action Preference("skip", "all")

                frame:
                    style_prefix "pref"
                    has vbox

                    textbutton _("Begin Skipping") action Skip()

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("After Choices")
                    textbutton _("Stop Skipping") action Preference("after choices", "stop")
                    textbutton _("Keep Skipping") action Preference("after choices", "skip")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Auto-Forward Time")
                    bar value Preference("auto-forward time")

            vbox:

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Music Volume")
                    bar value Preference("music volume")

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Sound Volume")
                    bar value Preference("sound volume")
                    textbutton "Test" action Play("sound", "sound_test.ogg") style "soundtest_button"

                frame:
                    style_prefix "pref"
                    has vbox

                    label _("Voice Volume")
                    bar value Preference("voice volume")
                    textbutton "Test" action Play("voice", "voice_test.ogg") style "soundtest_button"

    style pref_frame:
        xfill True
        xmargin 5
        top_margin 5

    style pref_vbox:
        xfill True

    style pref_button:
        size_group "pref"
        xalign 1.0

    style pref_slider:
        xmaximum 192
        xalign 1.0

    style soundtest_button:
        xalign 1.0

.. _yesno-prompt-screen:
.. _confirm-screen:

Подтверждение
-------------

Экран ``confirm`` используется, чтобы запрашивать у пользователя выбор
«да/нет». Он принимает следующие параметры:

`message`
    Сообщение для отображения пользователю. Ren'Py использует как минимум следующие сообщения:

    * gui.ARE_YOU_SURE - "Are you sure?" Это должно быть значением по умолчанию, если сообщение неизвестно.
    * gui.DELETE_SAVE - "Are you sure you want to delete this save?"
    * gui.OVERWRITE_SAVE - "Are you sure you want to overwrite your save?"
    * gui.LOADING - "Loading will lose unsaved progress.\nAre you sure you want to do this?"
    * gui.QUIT - "Are you sure you want to quit?"
    * gui.MAIN_MENU - "Are you sure you want to return to the main\nmenu? This will lose unsaved progress."
    * gui.CONTINUE - "Are you sure you want to continue where you left off?"
    * gui.END_REPLAY - "Are you sure you want to end the replay?"
    * gui.SLOW_SKIP - "Are you sure you want to begin skipping?"
    * gui.FAST_SKIP_SEEN - "Are you sure you want to skip to the next choice?"
    * gui.FAST_SKIP_UNSEEN - "Are you sure you want to skip unseen dialogue to the next choice?"
    * UNKNOWN_TOKEN - This save was created on a different device. Maliciously constructed save files can harm your computer. Do you trust this save's
      creator and everyone who could have changed the file?
    * TRUST_TOKEN - Do you trust the device the save was created on? You should only choose yes if you are the device's sole user.

    Значения переменных — это строки, что означает, что их можно
    отобразить с помощью текстового displayable.

`yes_action`
    Действие, которое выполняется, когда пользователь выбирает «Да» (Yes).

`no_action`
    Действие, которое выполняется, когда пользователь выбирает «Нет» (No).

До версии Ren'Py 6.99.10 этот экран был известен как экран ``yesno_prompt``.
Если экран ``confirm`` отсутствует, вместо него используется ``yesno_prompt``.

Этот экран также будет вызываться функцией :func:`renpy.confirm` и действием :func:`Confirm`.

::

    screen confirm(message, yes_action, no_action):

        modal True

        window:
            style "gm_root"

        frame:
            style_prefix "confirm"

            xfill True
            xmargin 50
            ypadding 25
            yalign .25

            vbox:
                xfill True
                spacing 25

                text _(message):
                    textalign 0.5
                    xalign 0.5

                hbox:
                    spacing 100
                    xalign .5
                    textbutton _("Yes") action yes_action
                    textbutton _("No") action no_action