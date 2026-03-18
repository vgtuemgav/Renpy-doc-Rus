===========================================================
Галерея изображений, музыкальная комната и действия повтора
===========================================================

.. _image-gallery:

Галерея изображений
-------------------

Галерея изображений — это экран, который позволяет игроку разблокировать
изображения, а затем просматривать их. С экраном связана одна или несколько
кнопок, и с каждой кнопкой связано одно или несколько изображений. Кнопки
и изображения также имеют условия, определяющие, разблокированы ли они.

Галереи изображений управляются экземплярами класса Gallery. Один и тот
же экземпляр класса Gallery может использоваться несколькими экранами
галерей изображений.

С галереей связана одна или несколько кнопок, с кнопкой связано одно
или несколько изображений, и с каждым изображением связано одно или несколько
отображаемых объектов (displayables). К кнопкам и изображениям можно
привязать условия. Кнопка разблокируется, когда все связанные с ней условия
выполнены и хотя бы одно изображение, связанное с этой кнопкой, разблокировано.
Изображение разблокируется, когда все связанные с ним условия выполнены.

Создание галереи изображений состоит из следующих четырех шагов.

1. Создайте экземпляр Gallery.

2. Добавьте в эту галерею кнопки и изображения, а также условия,
   которые определяют, разблокированы ли кнопки и изображения, к
   которым они относятся. Это также многошаговый процесс.

   1. Объявите новую кнопку, вызвав :meth:`Gallery.button`.

   2. По желанию, добавьте к кнопке одно или несколько условий
      разблокировки, вызвав :meth:`Gallery.unlock` или :meth:`Gallery.condition`.

   3. Объявите изображение, вызвав :meth:`Gallery.image` с одним или
      несколькими отображаемыми объектами в качестве аргументов. Или
      вместо этого вызовите удобный метод :meth:`Gallery.unlock_image`.

   4. По желанию, вызовите :meth:`Gallery.transform` для привязки
      трансформаций к отображаемым объектам.

   5. По желанию, добавьте одно или несколько условий разблокировки
      к изображению, вызвав :meth:`Gallery.unlock`, :meth:`Gallery.condition`
      или :meth:`Gallery.allprior`.

   Дополнительные изображения можно добавить к кнопке, повторив шаги 3–5,
   а дополнительные кнопки можно добавить в галерею, повторив все пять
   шагов.

3. Создайте экран галереи изображений. Экран должен отображать фон
   и содержать навигацию, позволяющую пользователю переключаться на
   другие галереи изображений или возвращаться в главное или
   дополнительное меню.

4. Добавьте способ отображения экрана галереи изображений в главное
   или дополнительное меню.

Вот пример::

    init python:

        # Step 1. Create the gallery object.
        g = Gallery()

        # Step 2. Add buttons and images to the gallery.

        # A button with an image that is always unlocked.
        g.button("title")
        g.image("title")

        # A button that contains an image that automatically unlocks.
        g.button("dawn")
        g.image("dawn1")
        g.unlock("dawn1")

        # This button has multiple images associated with it. We use unlock_image
        # so we don't have to call both .image and .unlock. We also apply a
        # transform to the first image.
        g.button("dark")
        g.unlock_image("bigbeach1")
        g.transform(slowpan)
        g.unlock_image("beach1 mary")
        g.unlock_image("beach2")
        g.unlock_image("beach3")

        # This button has a condition associated with it, allowing the game
        # to choose which images unlock.
        g.button("end1")
        g.condition("persistent.unlock_1")
        g.image("transfer")
        g.image("moonpic")
        g.image("girlpic")
        g.image("nogirlpic")
        g.image("bad_ending")

        g.button("end2")
        g.condition("persistent.unlock_2")
        g.image("library")
        g.image("beach1 nomoon")
        g.image("bad_ending")

        # The last image in this button has an condition associated with it,
        # so it will only unlock if the user gets both endings.
        g.button("end3")
        g.condition("persistent.unlock_3")
        g.image("littlemary2")
        g.image("littlemary")
        g.image("good_ending")
        g.condition("persistent.unlock_3 and persistent.unlock_4")

        g.button("end4")
        g.condition("persistent.unlock_4")
        g.image("hospital1")
        g.image("hospital2")
        g.image("hospital3")
        g.image("heaven")
        g.image("white")
        g.image("good_ending")
        g.condition("persistent.unlock_3 and persistent.unlock_4")

        # The final two buttons contain images that show multiple pictures
        # at the same time. This can be used to compose character art onto
        # a background.
        g.button("dawn mary")
        g.unlock_image("dawn1", "mary dawn wistful")
        g.unlock_image("dawn1", "mary dawn smiling")
        g.unlock_image("dawn1", "mary dawn vhappy")

        g.button("dark mary")
        g.unlock_image("beach2", "mary dark wistful")
        g.unlock_image("beach2", "mary dark smiling")
        g.unlock_image("beach2", "mary dark vhappy")

        # The transition used when switching images.
        g.transition = dissolve

    # Step 3. The gallery screen we use.
    screen gallery:

        # Ensure this replaces the main menu.
        tag menu

        # The background.
        add "beach2"

        # A grid of buttons.
        grid 3 3:

            xfill True
            yfill True

            # Call make_button to show a particular button.
            add g.make_button("dark", "gal-dark.png", xalign=0.5, yalign=0.5)
            add g.make_button("dawn", "gal-dawn.png", xalign=0.5, yalign=0.5)
            add g.make_button("end1", "gal-end1.png", xalign=0.5, yalign=0.5)

            add g.make_button("end2", "gal-end2.png", xalign=0.5, yalign=0.5)
            add g.make_button("end3", "gal-end3.png", xalign=0.5, yalign=0.5)
            add g.make_button("end4", "gal-end4.png", xalign=0.5, yalign=0.5)

            add g.make_button("dark mary", "gal-dark_mary.png", xalign=0.5, yalign=0.5)
            add g.make_button("dawn mary", "gal-dawn_mary.png", xalign=0.5, yalign=0.5)
            add g.make_button("title", "title.png", xalign=0.5, yalign=0.5)


        # The screen is responsible for returning to the main menu. It could also
        # navigate to other gallery screens.
        textbutton "Return" action Return() xalign 0.5 yalign 0.5

Шаг 4 будет варьироваться в зависимости от структуры вашей игры,
но один из способов сделать это — добавить следующую строку::

        textbutton "Gallery" action ShowMenu("gallery")

на экран главного меню.

.. include:: inc/gallery


.. _music-room:

Музыкальная комната
-------------------

Музыкальная комната — это экран, который позволяет пользователю
выбирать и воспроизводить музыкальные треки из игры. Эти треки могут
быть заблокированы, когда пользователь только начинает играть, и будут
разблокированы по мере того, как пользователь слушает музыку во время
прохождения игры.

Музыкальная комната управляется экземпляром класса MusicRoom. В одной
игре может быть несколько экземпляров MusicRoom, что позволяет иметь
несколько музыкальных комнат. Создание музыкальной комнаты состоит из
следующих четырех шагов:

1. Создайте экземпляр MusicRoom. Конструктор MusicRoom принимает
   параметры для управления каналом, на котором воспроизводится музыка,
   и временем затухания и возобновления музыки.

2. Добавьте музыкальные файлы в этот экземпляр.

3. Создайте экран, который использует экземпляр MusicRoom для создания
   действий для кнопок, кнопок-изображений или активных областей.
   Эти действия могут выбирать трек, следующий или предыдущий трек,
   а также останавливать и запускать музыку.

   Обратите внимание, что используемые действия являются членами
   экземпляра MusicRoom, поэтому если экземпляр MusicRoom называется
   ``mr``, то ``mr.Play("track1.ogg")`` — это способ использовать
   действие воспроизведения.

4. Добавьте экран музыкальной комнаты в главное или дополнительное
   меню.

Вот пример::

    init python:

        # Step 1. Create a MusicRoom instance.
        mr = MusicRoom(fadeout=1.0)

        # Step 2. Add music files.
        mr.add("track1.ogg", always_unlocked=True)
        mr.add("track2.ogg")
        mr.add("track3.ogg")


    # Step 3. Create the music room screen.
    screen music_room:

        tag menu

        frame:
            has vbox

            # The buttons that play each track.
            textbutton "Track 1" action mr.Play("track1.ogg")
            textbutton "Track 2" action mr.Play("track2.ogg")
            textbutton "Track 3" action mr.Play("track3.ogg")

            null height 20

            # Buttons that let us advance tracks.
            textbutton "Next" action mr.Next()
            textbutton "Previous" action mr.Previous()

            null height 20

            # The button that lets the user exit the music room.
            textbutton "Main Menu" action ShowMenu("main_menu")

        # Start the music playing on entry to the music room.
        on "replace" action mr.Play()

        # Restore the main menu music upon leaving.
        on "replaced" action Play("music", "track1.ogg")

Шаг 4 будет варьироваться в зависимости от структуры вашей игры, но один
из способов сделать это — добавить следующую строку::

        textbutton "Music Room" action ShowMenu("music_room")

на экран главного меню.

Используя функцию :func:`Preferences`, особенно
``Preferences("music volume")``, можно добавить на музыкальный
экран ползунок громкости.

.. include:: inc/music_room


.. _replay:

Повтор
------

Ren'Py также включает возможность повторного воспроизведения сцены из
главного или игрового меню. Это можно использовать для создания
"галереи сцен" или галереи воспоминаний, которая позволяет игроку
повторять важные сцены. После завершения сцены Ren'Py возвращается
туда, откуда был запущен повтор.

Повтор сцен также возможен с помощью действия :func:`Start`. Различия
между двумя режимами:

* Повтор можно запустить с любого экрана, в то время как Start
  можно использовать только в главном меню или на экранах, показанных
  из главного меню.

* Когда повтор заканчивается, управление возвращается в точку, откуда
  был вызван повтор. Эта точка может находиться, например, внутри
  главного или игрового меню. Если игра находится в процессе выполнения
  при вызове повтора, состояние игры сохраняется.

* Сохранение отключено в режиме повтора. Перезагрузка, которая
  требует сохранения, также отключена.

* В режиме повтора вызов :func:`renpy.end_replay` завершит повтор.
  В обычном режиме end_replay ничего не делает.

Чтобы воспользоваться режимом повтора, сцена должна начинаться с
метки (label) и заканчиваться вызовом :func:`renpy.end_replay`.
Сцена не должна делать никаких предположений о состоянии слоев
или переменных, которые могут сильно отличаться в обычном режиме
и режиме повтора (за исключением тех, что установлены через
параметр `scope` при входе в режим повтора). Когда начинается
повтор, метка вызывается с черного экрана.

Например::

        "And finally, I met the wizard himself."

    label meaning_of_life:

        scene revelation

        "Mage" "What is the meaning of life, you say?"

        "Mage" "I've thought about it long and hard. A long time, I've
                spent pondering that very thing."

        "Mage" "And I'll say - the answer - the meaning of life
                itself..."

        "Mage" "Is forty-three."

        $ renpy.end_replay()

        "Mage" "Something like that, anyway."

С такой определенной сценой повтор можно вызвать с помощью
действия Replay::

    textbutton "The meaning of life" action Replay("meaning_of_life")

В режиме повтора используется одна переменная хранилища (store):

.. var:: _in_replay

    В режиме повтора эта переменная отправляется в метку, с которой
    начался режим повтора — в ту метку, которая была вызвана, а не ту,
    из которой произошел вызов. Вне режима повтора эта переменная
    равна None.

Кроме того, :var:`config.enter_replay_transition` и
:var:`config.exit_replay_transition` используются при входе и выходе из
режима повтора соответственно. :var:`config.replay_scope` добавляет
переменные в очищенное хранилище (store) при входе в режим повтора
и по умолчанию устанавливает :var:`_game_menu_screen`, чтобы правый
клик мыши в режиме повтора по умолчанию показывал экран настроек.

В режиме повтора используются следующие переменные и действия:

.. include:: inc/replay