# AI-Hub — Releases

Hier liegen die Installer von **AI-Hub**, einer Tauri-2-Desktop-App für Windows 11. Sie zeigt Claude-Code-Sessions über alle Projekte hinweg an, startet sie wieder und sichert die Transkripte, bevor Claude Code sie nach 30 Tagen löscht.

**Der Quellcode ist nicht öffentlich.** Dieses Repository trägt ausschließlich die Release-Dateien: den NSIS-Installer, seine Signatur und die `latest.json`, gegen die der Updater der App prüft.

## Installation

Den aktuellen Installer gibt es unter [Releases](../../releases/latest): `AI-Hub_<version>_x64-setup.exe`. Er installiert in das Benutzerprofil und braucht keine Administratorrechte.

Beim ersten Start zeigt Windows eine SmartScreen-Warnung — „Der Computer wurde durch Windows geschützt“ —, weil der Installer nicht mit einem gekauften Code-Signing-Zertifikat versehen ist. Über *Weitere Informationen* → *Trotzdem ausführen* geht es weiter.

Ab der Installation aktualisiert sich die App selbst: Sie sucht beim Start still nach einer neueren Version und weist unter „Einstellungen“ darauf hin.

## Signatur

Jeder Installer ist mit [minisign](https://jedisct1.github.io/minisign/) signiert, Fingerabdruck `7F9B71B1BEE8804C`. Die App prüft diese Signatur bei jedem Update und weist eine Datei ab, die nicht dazu passt. Die `.sig` neben dem Installer enthält sie.

Das ist unabhängig von der SmartScreen-Warnung oben: Diese Signatur schützt den Update-Weg, ein Code-Signing-Zertifikat würde Windows gegenüber den Herausgeber ausweisen.

## Warum ein eigenes Repository

Der Tauri-Updater kommt an die Release-Dateien eines privaten Repositories nicht sauber heran. Die `latest.json` verweist auf `browser_download_url`s, und die lassen sich nicht mit einem Token authentifizieren; der Weg über die Asset-API verlangt eine Asset-ID, die sich mit jedem Release ändert und deshalb nicht in eine statische Konfiguration passt. Die verbreitete Abhilfe bettet einen Token zur Bauzeit in die Binärdatei ein.

Stattdessen trägt dieses öffentliche Repository nur die fertigen Dateien: Der Quellcode bleibt geschlossen, der Updater arbeitet ohne Token, und die Signaturprüfung bleibt erhalten.
