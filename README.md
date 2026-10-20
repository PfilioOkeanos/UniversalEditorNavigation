# Universal Editor Navigation

C++-Editor-Plugin für **Unreal Engine 5.8.3**. Freie lokale Kamera-Rotation im normalen Perspective-Level-Viewport, ohne Kamera-Actor, Piloting oder Play-Modus.

Stand: **0.3.0 – Local Rotation**, Beta.

**[Ausführliche Installationsanleitung für Unreal unter Windows](INSTALL-DE.md)** – mit Blueprint-Projekt-Vorbereitung, Ordnerstruktur, Visual-Studio-Build und Fehlerhilfe.

## Bedienung

| Eingabe | Funktion |
| --- | --- |
| RMB halten + Maus | Lokales Yaw/Pitch ohne Pitch-Limit oder Welt-Z-Aufrichtung |
| RMB + Q halten | Links um die lokale Blickachse rollen |
| RMB + E halten | Rechts um die lokale Blickachse rollen |
| Q und E gleichzeitig | Rollbewegungen heben sich auf |

Standard-Rolltempo: **120 Grad/Sekunde**. Loslassen stoppt die jeweilige Eingabe, ohne die Orientierung zurückzusetzen.

Bei aktivierter Rollfunktion sind Q/E im fokussierten Perspective-Level-Viewport reserviert, auch ohne RMB. Ihre normalen Viewport-Funktionen sind dort deaktiviert. Ohne RMB wird nicht gerollt. Textfelder, andere Editoren und Ctrl/Alt/Cmd-Kombinationen werden nicht abgefangen.

## Installation

1. C++-Entwicklungsumgebung für UE 5.8.3 einrichten. Bei reinen Blueprint-Projekten zuerst eine C++-Klasse ergänzen.
2. Unreal schließen und die Repository-Dateien nach `Plugins/UniversalEditorNavigation` im eigenen Projekt kopieren. Die `.uplugin`-Datei muss direkt darin liegen.
3. Visual-Studio-Projektdateien erzeugen und das Editor-Ziel des Projekts als **Development Editor / Win64** bauen.
4. Projekt öffnen und das Plugin unter **Edit → Plugins** prüfen.

Die einzelnen Klicks und Beispiele stehen in [INSTALL-DE.md](INSTALL-DE.md).

Dieses Repository enthält Quellcode. Fertige Editor-DLLs und Unreal Engine selbst sind nicht enthalten. Die geprüfte Engine-Version ist 5.8.3; andere Versionen sind nicht verifiziert.

## Einstellungen

**Edit → Project Settings → Plugins → Universal Editor Navigation**:

- `Enable Navigation (Local Rotation)`: gesamte Erweiterung ein-/ausschalten.
- `Enable Local Mouse Look`: lokale Quaternion-Maussteuerung.
- `Mouse Sensitivity Multiplier`: Faktor auf Unreals native Maus-Empfindlichkeit; invert-Y wird beachtet.
- `Enable Roll`: Rollfunktion und Reservierung der Rolltasten.
- `Roll Left Key` / `Roll Right Key`: Rolltasten, standardmäßig Q/E.
- `Roll Degrees Per Second`: Rolltempo von 1 bis 720 Grad/Sekunde.

Nach Änderungen die beteiligten Tasten kurz loslassen. Vollständig deaktivieren: Plugin unter **Edit → Plugins** ausschalten und den Editor neu starten.

## Umfang und Grenzen

Maus-Yaw, Maus-Pitch und Roll werden lokal mit Quaternions integriert. Eine gerollte Kamera behält dadurch unabhängige Mausachsen. Es wird keine Kamerapose dauerhaft gespeichert oder nachträglich erzwungen. Das Plugin verwendet öffentliche Editor-/Slate-APIs und benötigt keine Engine-Quelländerungen.

WASD, Mausrad und Translation bleiben nativ. Ctrl/Space sind noch nicht entlang der lokalen Kamera-Up-Achse angebunden. **Vollständige lokale 6DOF-Translation ist nicht implementiert.**

Der Adapter greift nicht in PIE/Simulation, Piloting, Actor-/Camera-Locks oder Gizmo-Drag ein. Native Rotationsphysik (`bUsePhysicsBasedRotation=true`) bleibt ausgeschlossen. Trackpad-/Touch-Maussteuerung bleibt nativ. Orbit, Bookmarks und View-Snapping dürfen die Ansicht weiterhin verändern.

Das Modul ist Editor-only; es implementiert keine Runtime-Spielerkamera und verändert keine Projektassets.

## Verifikation

- Editor-Build: Win64 Development, UE 5.8.3.
- Fünf erfolgreiche Automation-Tests unter dem Präfix `UniversalEditorNavigation`.
- Regressionstest für unabhängige Mausachsen nach +/-90 Grad Roll und mehrfache lokale Pitch-/Yaw-Umdrehungen durch die Welt-Pole.
- Tests für Rollgeschwindigkeit, Tastenfreigabe, Fokus-/Capture-Grenzen, Einstellungen und native Pitch-Lebensdauer.
- Separater vollständiger normaler Editor-Shutdown erfolgreich geprüft.

Die Eingabetests verwenden deterministische Viewport-Zustände; eine vollständige interaktive Prüfung aller Terrain-/Editor-Werkzeuge ist nicht erfolgt.

## Lizenz

**MIT-Lizenz**, Copyright (c) 2026 Pfilio_okeanos. Der vollständige Lizenztext steht in [LICENSE](LICENSE).

Unreal Engine und seine Abhängigkeiten werden nicht mitgeliefert; ihre Bedingungen gelten separat. Das Veröffentlichungs-Paket verwendet ein eigenes Navigationssymbol anstelle des Unreal-Vorlagenicons.
