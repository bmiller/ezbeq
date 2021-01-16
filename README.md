# ezbeq

A simple web browser for [beqcatalogue](https://beqcatalogue.readthedocs.io/en/latest/) which integrates with [minidsp-rs](https://github.com/mrene/minidsp-rs)
for local remote control of a minidsp.

# Setup

## Installation

ssh into your rpi and

    $ ssh pi@myrpi
    $ sudo apt install python3 python3-venv python3-pip libyaml-dev git
    $ mkdir python
    $ cd python
    $ python3 -m venv ezbeq
    $ cd ezbeq
    $ . bin/activate
    $ pip install git+https://github.com/3ll3d00d/ezbeq

 ## Starting ezbeq on bootup
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is optional but recommended, it ensures the app starts automatically whenever the rpi boots up and makes
sure it restarts automatically if it ever crashes.

We will achieve this by creating and enabling a `systemd`_ service.

1) Create a file ezbeq.service in the appropriate location for your distro (e.g. ``/etc/systemd/system/`` for debian)::

   [Unit]
   Description=ezbeq
   After=network.target

   [Service]
   Type=simple
   User=myuser
   WorkingDirectory=/home/pi
   ExecStart=/home/pi/python/ezbeq/bin/ezbeq
   Restart=always
   RestartSec=1

   [Install]
   WantedBy=multi-user.target

2) enable the service and start it up::

   $ sudo systemctl enable ezbeq.service
   $ sudo service ezbeq start
   $ sudo journalctl -u ezbeq.service
   -- Logs begin at Sat 2019-08-17 12:17:02 BST, end at Sun 2019-08-18 21:58:43 BST. --
   Aug 18 21:58:36 swoop systemd[1]: Started ezbeq.

3) reboot and repeat step 2 to verify the recorder has automatically started
