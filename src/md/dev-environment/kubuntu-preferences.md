# Random settings

Dark theme

```sh
lookandfeeltool -a org.kde.breeze.dark
```

No lock screen

```sh
kwriteconfig5 --file ~/.config/kscreenlockerrc --group Daemon --key Autolock false
```

TODO: Super key opens the application menu (not working in xrdp)

```sh
kwriteconfig5 --file ~/.config/kwinrc --group ModifierOnlyShortcuts --key Meta "org.kde.plasmashell,/PlasmaShell,org.kde.PlasmaShell,activateLauncherMenu"
```
