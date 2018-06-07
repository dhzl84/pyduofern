pyduofern
=========
.. image:: https://travis-ci.org/gluap/pyduofern.svg?branch=master
    :target: https://travis-ci.org/gluap/pyduofern
.. image:: https://coveralls.io/repos/github/gluap/pyduofern/badge.svg?branch=master
    :target: https://coveralls.io/github/gluap/pyduofern?branch=master

These are my efforts in porting the `FHEM <http://fhem.de/fhem.html>`_
`Duofern USB-Stick <https://wiki.fhem.de/wiki/DUOFERN>`_ based module to
`Homeassistant <https://home-assistant.io/>`_. As of now the port is rather ugly, but it is usable enough to control
my Duofern blinds. I did not port the Weather-Station related features of the original module -- Mainly because I
do not own the corresponding hardware and have no means to test if it works. I only tested it with the model
*RolloTron Standard DuoFern 14233011 Funk-Gurtwickler Aufputz*.

I do not provide any guarantees for the usability of this software. Use at your own risk.

License::

   python interface for dufoern usb stick
   Copyright (C) 2017 Paul Görgen
   Rough python python translation of the FHEM duofern modules by telekatz
              (also licensed under GPLv2)
   This re-write does not literally contain contain any verbatim lines
   of the original code (given it was translated to another language)
   apart from some comments to facilitate translation of the not-yet
   translated parts of the original software. Modification dates are
   documented as submits to the git repository of this code, currently
   maintained at `https://github.com/gluap/pyduofern.git <https://github.com/gluap/pyduofern.git>`_

   This program is free software; you can redistribute it and/or modify
   it under the terms of the GNU General Public License as published by
   the Free Software Foundation; either version 2 of the License, or
   (at your option) any later version.

   This program is distributed in the hope that it will be useful,
   but WITHOUT ANY WARRANTY; without even the implied warranty of
   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   GNU General Public License for more details.

   You should have received a copy of the GNU General Public License
   along with this program; if not, write to the Free Software Foundation,
   Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301  USA

Setup custom component
=======================

Put the cover component file to your custom_components folder::

    config\custom_components\cover\duofern.py

After restarting HA the necessary python module will be installed automatically from GitHub

Put the duofern addon to your addons folder::

    addons\duofern\Dockerfile
    addons\duofern\config.json
    addons\duofern\run.sh

Setup addon
===============

To access the DuoFern Stick inside the addon, it must be mapped in the config.json file::

    addons\duofern\config.json

In my installation the DuoFern Stick is "/dev/ttyUSB2" and I just map it with the same name into the addon::

    "devices": ["/dev/ttyUSB2:/dev/ttyUSB2:rwm"],

For minimum changes to other files I suggest to just change the name of the duoFern Stick in the host system.


Pairing Devices
===============

Pairing is done via starting the addon, basically it just runs::

    python3 /share/pyduofern/scripts/duofern_cli.py --configfile /config/duofern.json --device /dev/ttyUSB2 --pair --pairtime 60

Renaming is currently not supported via addon, I just change the names manually in::

    config/duofern.json
    ...
    "devices": [
        {
            "id": "123456",
            "name": "YourName"
        }
    ],


Indexing paired blinds is not supported yet

Usage with Homeassistant
========================

Just add the roller shutters to your configuration::
    
    cover.yourname
    
Known issues
============

During initial setup I had issues pairing via addon while the duofern custom component was already configured.
Maybe the addon couldn't use the DuoFern Stick since it was already in use by the custom component.
If you experience this issue just remove the DuoFern component from the HA config and restart.

Changelog
=========

**0.22**
- Added recording of actions for replay in integration tests
- Improved unit tests
- Enable travis
- Enable coveralls

**0.21.1**
- fixed bug where device IDs containing `cc` would be be messed up when inserting channel number.

