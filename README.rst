Introduction
============


.. image:: https://readthedocs.org/projects/adafruit-circuitpython-usb-host-mouse/badge/?version=latest
    :target: https://docs.circuitpython.org/projects/usb_host_mouse/en/latest/
    :alt: Documentation Status


.. image:: https://raw.githubusercontent.com/adafruit/Adafruit_CircuitPython_Bundle/main/badges/adafruit_discord.svg
    :target: https://adafru.it/discord
    :alt: Discord


.. image:: https://github.com/adafruit/Adafruit_CircuitPython_USB_Host_Mouse/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_USB_Host_Mouse/actions
    :alt: Build Status


.. image:: https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json
    :target: https://github.com/astral-sh/ruff
    :alt: Code Style: Ruff

Helper class that encapsulates the objects needed for user code to interact with a USB mouse, draw a visible cursor, and determine when buttons are pressed.


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://circuitpython.org/libraries>`_
or individual libraries can be installed using
`circup <https://github.com/adafruit/circup>`_.


`USB Wired Mouse - Two Buttons plus Wheel <https://www.adafruit.com/product/2025>`_

Installing from PyPI
=====================

On supported GNU/Linux systems like the Raspberry Pi, you can install the driver locally `from
PyPI <https://pypi.org/project/adafruit-circuitpython-usb-host-mouse/>`_.
To install for current user:

.. code-block:: shell

    pip3 install adafruit-circuitpython-usb-host-mouse

To install system-wide (this may be required in some cases):

.. code-block:: shell

    sudo pip3 install adafruit-circuitpython-usb-host-mouse

To install in a virtual environment in your current project:

.. code-block:: shell

    mkdir project-name && cd project-name
    python3 -m venv .venv
    source .env/bin/activate
    pip3 install adafruit-circuitpython-usb-host-mouse

Installing to a Connected CircuitPython Device with Circup
==========================================================

Make sure that you have ``circup`` installed in your Python environment.
Install it with the following command if necessary:

.. code-block:: shell

    pip3 install circup

With ``circup`` installed and your CircuitPython device connected use the
following command to install:

.. code-block:: shell

    circup install adafruit_usb_host_mouse

Or the following command to update an existing version:

.. code-block:: shell

    circup update

Usage Example
=============

.. code-block:: python

    import supervisor
    import terminalio
    from adafruit_display_text.bitmap_label import Label
    from displayio import Group

    from adafruit_usb_host_mouse import find_and_init_boot_mouse

    display = supervisor.runtime.display

    # group to hold visual elements
    main_group = Group()

    # make the group visible on the display
    display.root_group = main_group

    mouse = find_and_init_boot_mouse()
    if mouse is None:
        raise RuntimeError("No mouse found connected to USB Host")

    # text label to show the x, y coordinates on the screen
    output_lbl = Label(terminalio.FONT, text=f"{mouse.x},{mouse.y}", color=0xFFFFFF, scale=1)

    # move it to the upper left corner
    output_lbl.anchor_point = (0, 0)
    output_lbl.anchored_position = (1, 1)

    # add it to the main group
    main_group.append(output_lbl)

    # add the mouse tile grid to the main group
    main_group.append(mouse.tilegrid)

    # main loop
    while True:
        # update mouse
        pressed_btns = mouse.update()

        # string with updated coordinates for the text label
        out_str = f"{mouse.x},{mouse.y}"

        # add pressed buttons to out str
        if pressed_btns is not None and len(pressed_btns) > 0:
            out_str += f" {" ".join(pressed_btns)}"

        # update the text label with the new coordinates
        # and buttons being pressed
        output_lbl.text = out_str


Documentation
=============
API documentation for this library can be found on `Read the Docs <https://docs.circuitpython.org/projects/usb_host_mouse/en/latest/>`_.

For information on building library documentation, please check out
`this guide <https://learn.adafruit.com/creating-and-sharing-a-circuitpython-library/sharing-our-docs-on-readthedocs#sphinx-5-1>`_.

Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_USB_Host_Mouse/blob/HEAD/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.
