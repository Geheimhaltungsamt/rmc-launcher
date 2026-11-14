# rmc – schlanker Minecraft-Launcher in Rust

Ein ~3,5 MB großer Kommandozeilen-Launcher. Er installiert Minecraft + Fabric + Performance-Mods,
stellt alles passend zur Hardware ein und beendet sich nach dem Start (verbraucht beim Spielen 0 MB).

## Warum 200 Chunks funktionieren
Vanilla erlaubt maximal 32 echte Chunks. Die 200 Chunks kommen von **Distant Horizons** (LODs):
In der Nähe werden echte Chunks gerendert (8–16, je nach PC), dahinter vereinfachte Landschaft
bis 200 Chunks. Das kostet nur einen Bruchteil der Leistung.

Automatisch installiert (Modrinth): Fabric API, Sodium, Lithium, FerriteCore, ModernFix,
ImmediatelyFast, Distant Horizons.

## Befehle
```
rmc login      # Microsoft-Anmeldung (Device-Code)
rmc            # Minecraft starten
rmc install    # nur herunterladen
rmc info       # Hardware-Erkennung + Einstellungen
rmc tune       # Grafik-Preset erneut erzwingen
rmc config     # Pfad + Inhalt der config.json
rmc launch --wait --version 1.21.1 --instance survival
```

## Konfiguration
`%APPDATA%\rmc-launcher\config.json` (Ordner per `RMC_HOME` änderbar):

| Feld | Bedeutung |
|---|---|
| `version` | Minecraft-Version (Standard 1.21.1) |
| `lod_radius` | Distant-Horizons-Sichtweite (Standard 200) |
| `ram_mb` | Heap in MB, `null` = automatisch |
| `render_distance` / `simulation_distance` | echte Chunks, `null` = automatisch |
| `ms_client_id` | Azure-Client-ID für den Login |
| `java_path` | eigenes Java statt Mojang-Runtime |
| `high_priority` | Prozesspriorität „Höher als normal“ |

## Microsoft-Login einrichten
Microsoft verlangt für jeden Launcher eine eigene App-Registrierung:
1. Im Azure-Portal eine App registrieren (Kontotyp: *persönliche Microsoft-Konten*),
   unter „Authentifizierung“ **Öffentliche Clientflows zulassen** aktivieren.
2. Die Client-ID über Mojangs Formular für die Minecraft-API freischalten lassen
   (kann einige Tage dauern).
3. Die Client-ID in `ms_client_id` eintragen, dann `rmc login`.

## Bauen
```
cargo build --release   # -> target/release/rmc.exe
```
