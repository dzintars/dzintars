---
title: "How to Integrate Flyway Into Linux Desktop Environment"
date: "2024-08-05T18:33:04+03:00"
draft: false
tags: ["Flyway", "Linux"]
categories: ["How To's"]
---

{{< badge label="Flyway" icon="flyway" color="CC0200" alt="Flyway Badge" >}}

Today I decided to ditch Flyway Desktop execution from CLI and integrate into my
Sway + Wofi desktop environment.
Basically, there is what you need:

1. Get an [Flyway icon](https://simpleicons.org/?q=flayway) and paste it into `flyway.svg` file

2. Copy that SVG file into
   `$HOME/.local/share/icons/hicolor/scalable/apps/flyway.svg`

3. Change the color for the icon by adding `fill="#CC0200"` to the path

4. Create an `$HOME/.local/share/applications/flyway.desktop` file with the
   content like this:

   ```toml
    [Desktop Entry]
    Version=1.0
    Type=Application
    Name=Flyway
    GenericName=Database migration tool
    Comment=Database Migrations Made Easy
    Categories=IDE;Development
    Keywords=Database;SQL;IDE;JDBC;ODBC;MySQL;PostgreSQL;Oracle;DB2;MariaDB
    Exec=$HOME/.local/share/flyway/flyway-desktop/flyway-desktop --enable-features=UseOzonePlatform --ozone-platform=wayland --no-sandbox %U
    Icon=flyway
    Terminal=false
    MimeType=application/sql
   ```

   Because Flyway Desktop is Electron app, we are passing few flags to run it on
   Wayland system. If you still use Xorg, then just remove those flags.

5. Copy entire Flyway directory which you downloaded from their
   [archive](https://download.red-gate.com/installers/Flyway_Desktop_LinuxX64/2024-07-05/Flyway_Desktop_LinuxX64.tar.gz)
   into the `$HOME/.local/share/flyway` directory

6. Rename the `Flyway Desktop` executable

   ```bash
   mv $HOME/.local/share/flyway/flyway-desktop/Flyway\ Desktop
   $HOME/.local/share/flyway/flyway-desktop/flyway-desktop
   ```

   This is required so that your desktop entry actually works.

That's basially it. You might be required to logout from your session. Or just
run `update-desktop-database`. This will update the cache and your desktop entry
should show up.

The catch might be with the `flyway.desktop` file itself. Deppending which
application launcher you use, it migth be picky about the properties used in the
desktop file. If so... wipe everything. Start just with `Exec` and `Icon`
fields. Icon might be an issue. If you use absolute path, then file extension is
required. If you use just icon name, then don't include the file extension. Make
sure icon lives in the right path.
