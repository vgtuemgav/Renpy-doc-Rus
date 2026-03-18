.. _achievement:

Достижения
==========

Модуль достижений позволяет разработчику присваивать достижения игроку,
очищать достижения и определять, было ли присвоено достижение. Он также позволяет
записывать прогресс в достижении.

По умолчанию достижения хранят информацию в persistent-файле. Если
поддержка Steam доступна и включена, информация о достижениях
автоматически синхронизируется со Steam.

Поддержка Steam должна быть добавлена в Ren'Py, чтобы гарантировать, что она распространяется
только создателями, которые были приняты в партнерскую программу Steam. Чтобы установить
ее, выберите "preferences", "Install libraries", "Install Steam Support".


.. include:: inc/achievement


Переменные, которые управляют достижениями:

.. var:: achievement.steam_position = None

    Если не None, устанавливает положение всплывающего уведомления Steam.
    Это должна быть строка, одна из: "top left", "top right", "bottom left"
    или "bottom right".

.. var:: config.steam_appid = None

    Если не None, это должен быть appid Steam. Ren'Py автоматически установит
    этот appid при запуске. Это нужно установить с помощью оператора define::

        define config.steam_appid = 12345

.. var:: config.automatic_steam_timeline = True

    Если true, то при запуске через Steam игра будет автоматически обновлять Steam Timeline.

    В настоящее время это включает:

    * Обновление фазы игры в соответствии с :var:`save_name`, если эта переменная установлена.
    * Обновление режима игры, чтобы отразить, когда игрок находится в меню.


API Steamworks
--------------

Когда Steam доступен, привязка к API Steamworks на основе ctypes доступна как
``achievement.steamapi``. Привязка является экземпляром модуля
steamapi, который можно найти `здесь <https://github.com/renpy/renpy-build/blob/master/steamapi/steamapi.py>`_,
и представляет собой машинный перевод API C++ Steamworks на Python.

Кроме того, большое количество функций доступно в объекте achievement.steam, если и только
если API Steamworks доступен.

.. var:: achievement.steam

    Если Steam инициализировался успешно, это пространство имен с высокоуровневыми методами Steam. Если Steam не
    инициализировался, это None. Всегда проверяйте, что это не None, перед вызовом метода.

Приложения Steam
^^^^^^^^^^^^^^^^

.. include:: inc/steam_apps

Оверлей Steam
^^^^^^^^^^^^^

.. include:: inc/steam_overlay

Статистика Steam
^^^^^^^^^^^^^^^^

.. include:: inc/steam_stats

Хронология Steam
^^^^^^^^^^^^^^^^

.. include:: inc/steam_timeline

Пользователь Steam
^^^^^^^^^^^^^^^^^^

.. include:: inc/steam_user

Мастерская Steam
^^^^^^^^^^^^^^^^

.. include:: inc/steam_ugc