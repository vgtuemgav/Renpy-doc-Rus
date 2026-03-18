.. _sprites:

Спрайты
=======

Для поддержки отображения большого количества изображений одновременно Ren'Py
поддерживает систему спрайтов. Эта система позволяет создавать спрайты, где каждый
спрайт содержит displayable-объект. Затем можно изменять положение спрайтов на
экране и их вертикальный порядок.

Если игнорировать производительность, система спрайтов концептуально схожа
с :func:`Fixed`, оборачивающим :func:`Transform`. Спрайты гораздо
быстрее, чем трансформации, но и менее гибкие. Большое улучшение
производительности спрайтов заключается в том, что каждый displayable-объект
отрисовывается только один раз за кадр, даже если этот displayable-объект используется
многими спрайтами. Ограничение заключается в том, что спрайты позволяют изменять
только их xoffset и yoffset, в отличие от множества свойств,
которые есть у Transform.

Чтобы использовать систему спрайтов, создайте объект SpriteManager, а затем
вызовите его метод create для создания новых частиц. По мере необходимости
обновляйте поля xoffset, yoffset и zorder каждого спрайта, чтобы перемещать
его по экрану. Предоставляя аргументы `update` и `event` для
SpriteManager, вы можете заставить спрайты меняться со временем и реагировать
на ввод пользователя.

Классы спрайтов
---------------

.. include:: inc/sprites
.. include:: inc/sprites_extra

Примеры спрайтов
----------------

Класс SnowBlossom — это простой в использовании способ размещения падающих
объектов на экране.

::

    image snow = SnowBlossom("snow.png", count=100)


Этот пример показывает, как SpriteManager можно использовать для создания сложного
поведения. В данном случае он показывает 400 частиц и заставляет их избегать
указателя мыши.

::

    init python:
        import math

        def repulsor_update(st):

            # If we don't know where the mouse is, give up.
            if repulsor_pos is None:
                return .01

            px, py = repulsor_pos

            # For each sprite...
            for i in repulsor_sprites:

                # Compute the vector between it and the mouse.
                vx = i.x - px
                vy = i.y - py

                # Get the vector length, normalize the vector.
                vl = math.hypot(vx, vy)
                if vl >= 150:
                    continue

                # Compute the distance to move.
                distance = 3.0 * (150 - vl) / 150

                # Move
                i.x += distance * vx / vl
                i.y += distance * vy / vl

                # Ensure we stay on the screen.
                if i.x < 2:
                    i.x = 2

                if i.x > repulsor.width - 2:
                    i.x = repulsor.width - 2

                if i.y < 2:
                    i.y = 2

                if i.y > repulsor.height - 2:
                    i.y = repulsor.height - 2

            return .01

        # On an event, record the mouse position.
        def repulsor_event(ev, x, y, st):
            store.repulsor_pos = (x, y)


    label repulsor_demo:

        python:
            # Create a sprite manager.
            repulsor = SpriteManager(update=repulsor_update, event=repulsor_event)
            repulsor_sprites = [ ]
            repulsor_pos = None

            # Ensure we only have one smile displayable.
            smile = Image("smile.png")

            # Add 400 sprites.
            for i in range(400):
                repulsor_sprites.append(repulsor.create(smile))

            # Position the 400 sprites.
            for i in repulsor_sprites:
                i.x = renpy.random.randint(2, 798)
                i.y = renpy.random.randint(2, 598)

            del smile
            del i

        # Add the repulsor to the screen.
        show expression repulsor as repulsor

        "..."

        hide repulsor

        # Clean up.
        python:
            del repulsor
            del repulsor_sprites
            del repulsor_pos