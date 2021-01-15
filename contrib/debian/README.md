
Debian
====================
This directory contains files used to package bitcoinlegendd/bitcoinlegend-qt
for Debian-based Linux systems. If you compile bitcoinlegendd/bitcoinlegend-qt yourself, there are some useful files here.

## bitcoinlegend: URI support ##


bitcoinlegend-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install bitcoinlegend-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your bitcoinlegendqt binary to `/usr/bin`
and the `../../share/pixmaps/bitcoinlegend128.png` to `/usr/share/pixmaps`

bitcoinlegend-qt.protocol (KDE)

