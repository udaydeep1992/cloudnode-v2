
Debian
====================
This directory contains files used to package cloudenoded/cloudenode-qt
for Debian-based Linux systems. If you compile cloudenoded/cloudenode-qt yourself, there are some useful files here.

## cloudenode: URI support ##


cloudenode-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install cloudenode-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your cloudenodeqt binary to `/usr/bin`
and the `../../share/pixmaps/cloudenode128.png` to `/usr/share/pixmaps`

cloudenode-qt.protocol (KDE)

