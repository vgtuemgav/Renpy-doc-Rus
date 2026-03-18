Chrome OS/Chromebook
====================

Существует два способа запустить Ren'Py в Chrome OS.

Android в Chrome OS
-------------------

Самый простой способ сделать игру доступной для Chrome OS — это упаковать её для
Android, как описано в :doc:`документации Android <android>`. Поддержка Android
в Ren'Py также была разработана с учётом Chrome OS.

Этот режим поддерживает только запуск игр, а не разработку новых.

Linux в Chrome OS
-----------------

Игры Ren'Py и Ren'Py SDK также можно установить на Chromebook. Это позволяет
вам разрабатывать игры Ren'Py на вашем Chromebook.

Для установки:

1. Установите Linux для Chromebook, как описано по ссылке https://support.google.com/chromebook/answer/9145439?hl=en .

2. Измените параметр "Crosstini GPU Support" на "enabled", введя chrome://flags/#crostini-gpu-support и выбрав "enable".

3. Перезагрузите ваш Chromebook.

4. Обновите Linux, запустив терминал и выполнив::

    sudo apt update
    sudo apt dist-upgrade

5. Установите некоторые требуемые зависимости, выполнив::

    sudo apt install libnss3 python3-tk

Чтобы установить версию Ren'Py, откройте терминал и выполните (изменив версию на соответствующую)::

    wget https://www.renpy.org/dl/8.2.0/renpy-8.2.0-sdkarm.tar.bz2
    tar xaf renpy-8.2.0-sdkarm.tar.bz2

Чтобы запустить эту версию Ren'Py, откройте терминал и выполните::

    cd ~/ab/renpy-8.2.0-sdkarm
    ./renpy.sh

Обратите внимание, что это работает и с другими версиями Ren'Py, если вы замените 8.2.0
на более позднюю версию.

SDK, установленный таким образом, можно использовать для запуска игр, которые изначально
не поддерживают ARM Chromebook, на ARM Chromebook. Просто распакуйте игру
в директорию проектов и нажмите "Обновить", затем запустите её через
лаунчер Ren'Py. (Директорию проектов можно задать в настройках.)