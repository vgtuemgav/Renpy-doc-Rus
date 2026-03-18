.. _keymap:

Настройка раскладки клавиатуры
==============================

Переменная :var:`config.keymap` содержит сопоставление имён событий со списками
кодов клавиш (keysyms), которые вызывают эти события.

.. note::

    Многие игроки привыкли к стандартному набору клавиш Ren'Py и
    ожидают, что он будет одинаковым во всех играх.

В Ren'Py коды клавиш (keysyms) — это строки, представляющие кнопки мыши, джойстика
или клавиатуры.

Кнопки мыши используют коды вида 'mouseup_#' или 'mousedown_#',
где # — это номер кнопки. Ren'Py предполагает наличие пяти кнопочной мыши,
где кнопки 1, 2 и 3 — это левая, средняя и правая кнопки, а
кнопки 4 и 5 генерируются при прокрутке колеса вверх и вниз.
Например, "mousedown_1" — это, как правило, нажатие левой кнопки мыши,
"mouseup_1" — отпускание этой кнопки, а "mousedown_4" — прокрутка колеса мыши
вверх.

Существует два вида кодов клавиш клавиатуры. Первый — это строка, содержащая
символ, который генерируется при нажатии клавиши. Это полезно для
привязки буквенных и цифровых клавиш. Примеры таких кодов: "a", "A" и "7".
Обратите внимание, что они чувствительны к регистру, "a" не совпадает с "A". Этот
тип кода клавиши полезен только тогда, когда событие генерирует текст — например,
отпускание клавиши 'a' не будет соответствовать ``keyup_a``, так как текст не
генерируется.

Коды клавиш клавиатуры также могут быть символическими именами клавиш. Это может быть любая из
констант K\_, взятых из `pygame.constants`. Этот тип кода клавиши выглядит как
"K_BACKSPACE", "K_RETURN" и "K_TAB"; полный список таких кодов
можно найти `здесь <http://www.pygame.org/docs/ref/key.html>`_.

Кодам клавиш клавиатуры и мыши могут предшествовать следующие префиксы,
разделённые подчёркиваниями:

alt
    Срабатывает, если нажата клавиша Alt. Коды клавиш без этого префикса срабатывают,
    когда клавиша Alt не нажата.
meta
    Срабатывает, если нажата клавиша Meta, Command или Windows. Коды клавиш без
    этого префикса срабатывают, когда клавиша meta не нажата.
ctrl
    Срабатывает, если нажата клавиша Ctrl. Коды клавиш без этого префикса срабатывают,
    когда клавиша Ctrl не нажата. (Ctrl не очень полезен, так как
    обычно запускает пропуск.)
osctrl
    Это Alt на Macintosh и Ctrl на других платформах.
anymod
    Срабатывает независимо от состояния клавиш Alt, Meta или Ctrl.
shift
    Срабатывает, когда нажата клавиша Shift.
noshift
    Срабатывает, когда клавиша Shift не нажата.
caps
    Срабатывает, когда включён Caps Lock.
nocaps
    Срабатывает, когда Caps Lock выключен.
num
    Срабатывает, когда включён Num Lock.
nonum
    Срабатывает, когда Num Lock выключен.
repeat
    Срабатывает, когда нажатие является повторным из-за удержания клавиши.
    Коды клавиш без префиксов repeat или anyrepeat не срабатывают на повторы. (Это
    не работает с кнопками мыши.)
anyrepeat
    Срабатывает как при первоначальном нажатии, так и при повторах.
keydown
    Срабатывает при нажатии клавиши (по умолчанию).
keyup
    Срабатывает при отпускании клавиши.

Например, код клавиши "shift_alt_K_F5" сработает при нажатии клавиши F5
с зажатыми Shift и Alt. Код "shift_mouse_1" сработает при
нажатии левой кнопки мыши с зажатой клавишей Shift.

Чтобы изменить привязку, обновите соответствующий список в :var:`config.keymap`.
Следующий код добавляет клавишу 't' в список клавиш, которые закрывают
реплику, и удаляет из этого списка клавишу пробела. ::

    init python:
        config.keymap['dismiss'].append('K_t')
        config.keymap['dismiss'].remove('K_SPACE')

Раскладка по умолчанию содержится в файле renpy/common/00keymap.rpy, и
начиная с версии 8.4.0, выглядит следующим образом::

    config.keymap = dict(

        # Привязки, действующие почти везде, если не отключены явно.
        rollback = [ 'anyrepeat_K_PAGEUP', 'anyrepeat_KP_PAGEUP', 'K_AC_BACK', 'mousedown_4' ],
        screenshot = [ 'alt_K_s', 'alt_shift_K_s', 'noshift_K_s' ],
        toggle_afm = [ ],
        toggle_fullscreen = [ 'alt_K_RETURN', 'alt_K_KP_ENTER', 'K_F11', 'noshift_K_f' ],
        game_menu = [ 'K_ESCAPE', 'K_MENU', 'K_PAUSE', 'mouseup_3' ],
        hide_windows = [ 'mouseup_2', 'noshift_K_h' ],
        launch_editor = [ 'shift_K_e' ],
        dump_styles = [ ],
        reload_game = [ 'alt_K_r', 'shift_K_r' ],
        inspector = [ 'alt_noshift_K_i', 'shift_K_i' ],
        full_inspector = [ 'alt_shift_K_i' ],
        developer = [ 'alt_K_d', 'shift_K_d', ],
        quit = [ ],
        iconify = [ ],
        help = [ 'K_F1', 'meta_shift_/' ],
        choose_renderer = ['alt_K_g', 'shift_K_g' ],
        progress_screen = [ 'alt_shift_K_p', 'meta_shift_K_p', 'K_F2' ],
        bubble_editor = [ 'alt_K_b', 'shift_K_b' ],

        # Специальные возможности.
        accessibility = [ 'shift_K_a' ],
        self_voicing = [ 'alt_K_v', 'K_v' ],
        clipboard_voicing = [ 'alt_shift_K_c', 'shift_K_c' ],
        debug_voicing = [ 'alt_shift_K_v', 'meta_shift_K_v' ],
        extra_voicing = [ '?' ],

        # Реплики.
        rollforward = [ 'anyrepeat_K_PAGEDOWN', 'anyrepeat_KP_PAGEDOWN', 'mousedown_5', ],
        dismiss = [ 'K_RETURN', 'K_SPACE', 'K_KP_ENTER', 'K_SELECT', 'mouseup_1' ],
        dismiss_unfocused = [ ],

        # Пауза.
        dismiss_hard_pause = [ ],

        # Фокус.
        focus_left = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        focus_right = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        focus_up = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        focus_down = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],

        # Кнопка.
        button_ignore = [ 'mousedown_1' ],
        button_select = [ 'K_RETURN', 'K_KP_ENTER', 'K_SELECT', 'mouseup_1',  ],
        button_alternate = [ 'mouseup_3' ],
        button_alternate_ignore = [ 'mousedown_3' ],

        # Ввод.
        input_backspace = [ 'anyrepeat_K_BACKSPACE' ],
        input_enter = [ 'K_RETURN', 'K_KP_ENTER' ],
        input_next_line = [ 'shift_K_RETURN', 'shift_K_KP_ENTER' ],
        input_left = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        input_right = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        input_up = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        input_down = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],
        input_delete = [ 'anyrepeat_K_DELETE', 'anyrepeat_KP_DELETE' ],
        input_home = [ 'K_HOME', 'KP_HOME', 'meta_K_LEFT' ],
        input_end = [ 'K_END', 'KP_END', 'meta_K_RIGHT' ],
        input_copy = [ 'ctrl_noshift_K_INSERT', 'ctrl_noshift_K_c', 'meta_noshift_K_c' ],
        input_paste = [ 'shift_K_INSERT', 'ctrl_noshift_K_v', 'meta_noshift_K_v' ],
        input_jump_word_left = [ 'osctrl_K_LEFT', 'osctrl_KP_LEFT' ],
        input_jump_word_right = [ 'osctrl_K_RIGHT', 'osctrl_KP_RIGHT' ],
        input_delete_word = [ 'osctrl_K_BACKSPACE' ],
        input_delete_full = [ 'meta_K_BACKSPACE' ],

        # Viewport.
        viewport_leftarrow = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        viewport_rightarrow = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        viewport_uparrow = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        viewport_downarrow = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],
        viewport_wheelup = [ 'mousedown_4' ],
        viewport_wheeldown = [ 'mousedown_5' ],
        viewport_drag_start = [ 'mousedown_1' ],
        viewport_drag_end = [ 'mouseup_1' ],
        viewport_pageup = [ 'anyrepeat_K_PAGEUP', 'anyrepeat_KP_PAGEUP'],
        viewport_pagedown = [ 'anyrepeat_K_PAGEDOWN', 'anyrepeat_KP_PAGEDOWN' ],

        # Эти клавиши управляют пропуском.
        skip = [ 'anymod_K_LCTRL', 'anymod_K_RCTRL' ],
        stop_skipping = [ ],
        toggle_skip = [ 'K_TAB' ],
        fast_skip = [ '>', 'shift_K_PERIOD' ],

        # Полоса прокрутки (Bar).
        bar_activate = [ 'mousedown_1', 'K_RETURN', 'K_KP_ENTER', 'K_SELECT' ],
        bar_deactivate = [ 'mouseup_1', 'K_RETURN', 'K_KP_ENTER', 'K_SELECT' ],
        bar_left = [ 'anyrepeat_K_LEFT', 'anyrepeat_KP_LEFT' ],
        bar_right = [ 'anyrepeat_K_RIGHT', 'anyrepeat_KP_RIGHT' ],
        bar_up = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        bar_down = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],

        # Удалить сохранение.
        save_delete = [ 'K_DELETE', 'KP_DELETE' ],

        # Постраничная навигация на экране сохранения/загрузки.
        save_page_prev = ['mousedown_4'],
        save_page_next = ['mousedown_5'],

        # Перетаскиваемый объект (Draggable).
        drag_activate = [ 'mousedown_1' ],
        drag_deactivate = [ 'mouseup_1' ],

        # Отладочная консоль.
        console = [ 'shift_K_o', 'alt_shift_K_o' ],
        console_exit = [ 'K_ESCAPE', 'K_MENU', 'K_PAUSE', 'mouseup_3' ],
        console_older = [ 'anyrepeat_K_UP', 'anyrepeat_KP_UP' ],
        console_newer = [ 'anyrepeat_K_DOWN', 'anyrepeat_KP_DOWN' ],

        # Режиссёр (Director)
        director = [ 'noshift_K_d' ],

        # Игнорируется (сохранено для обратной совместимости).
        toggle_music = [ ],
        viewport_up = [ ],
        viewport_down = [ ],

        # Команды профилирования.
        performance = [ 'K_F3' ],
        image_load_log = [ 'K_F4' ],
        profile_once = [ 'K_F8' ],
        memory_profile = [ 'K_F7' ],
    )

Привязки геймпада работают немного иначе. Привязки геймпада работают
путём сопоставления события геймпада с одним или несколькими именами событий Ren'Py.
Стандартный набор привязок геймпада приведён ниже::

    config.pad_bindings = {
        "pad_leftshoulder_press" : [ "rollback", ],
        "pad_lefttrigger_pos" : [ "rollback", ],
        "pad_back_press" : [ "rollback", ],

        "repeat_pad_leftshoulder_press" : [ "rollback", ],
        "repeat_pad_lefttrigger_pos" : [ "rollback", ],
        "repeat_pad_back_press" : [ "rollback", ],

        "pad_guide_press" : [ "game_menu", ],
        "pad_start_press" : [ "game_menu", ],

        "pad_y_press" : [ "hide_windows", ],
        "pad_x_press" : [ "button_alternate" ],

        "pad_rightshoulder_press" : [ "rollforward", ],
        "repeat_pad_rightshoulder_press" : [ "rollforward", ],

        "pad_righttrigger_pos" : [ "dismiss", "button_select", "bar_activate", "bar_deactivate" ],
        "pad_a_press" : [ "dismiss", "button_select", "bar_activate", "bar_deactivate"],
        "pad_b_press" : [ "game_menu" ],

        "pad_dpleft_press" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "pad_leftx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "pad_rightx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],

        "pad_dpright_press" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "pad_leftx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "pad_rightx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],

        "pad_dpup_press" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "pad_lefty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "pad_righty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],

        "pad_dpdown_press" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "pad_lefty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "pad_righty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],

        "repeat_pad_dpleft_press" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "repeat_pad_leftx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],
        "repeat_pad_rightx_neg" : [ "focus_left", "bar_left", "viewport_leftarrow" ],

        "repeat_pad_dpright_press" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "repeat_pad_leftx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],
        "repeat_pad_rightx_pos" : [ "focus_right", "bar_right", "viewport_rightarrow" ],

        "repeat_pad_dpup_press" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "repeat_pad_lefty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],
        "repeat_pad_righty_neg" : [ "focus_up", "bar_up", "viewport_uparrow" ],

        "repeat_pad_dpdown_press" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "repeat_pad_lefty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],
        "repeat_pad_righty_pos" : [ "focus_down", "bar_down", "viewport_downarrow" ],
    }

Кнопки геймпада имеют имя события в форме "pad_*button*_press" или
"pad_*button*_release". События аналоговых осей имеют форму "pad_*axis*_pos",
"pad_*axis*_neg" или "pad_*axis*_zero". При удержании генерируется
вторая привязка геймпада с префиксом "repeat\_".

Геймпады, которые не работают без специальной инициализации, по умолчанию отключены.
К ним относится контроллер Nintendo Switch Pro, который требует специальной
инициализации для работы на ПК. Это занесение в чёрный список контролируется
переменной :var:`config.controller_blocklist`.

.. include:: inc/keymap