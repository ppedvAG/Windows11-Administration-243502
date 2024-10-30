Voraussetzungen:
Windows ADK (Assessment and Deployment Kit) und Windows PE Add-On für ADK müssen auf deinem Computer installiert sein.
Ein Windows 11-Image (ISO-Datei) oder ein erstelltes WIM-Image, das du bereitstellen möchtest.
Hyper-V muss auf deinem Computer aktiviert sein

Schritt 1: Arbeitsumgebung vorbereiten
Starte die Windows ADK Umgebung
Öffne die „Deployment and Imaging Tools Environment“ (Administratorrechte erforderlich).

Erstelle ein Verzeichnis für das Windows PE-Image:
Erstelle einen neuen Ordner für das Windows PE-Image, z. B. C:\WinPE_11\.

Windows PE-Image erstellen:

Verwende das copype-Tool, um eine Windows PE-Umgebung für die Architektur zu erstellen, die du benötigst (meist amd64):

copype amd64 C:\WinPE_11\

Schritt 2: Windows PE-Image anpassen
Füge das Windows 11-Image in die PE-Umgebung ein:

Kopiere das Windows 11 WIM- oder ISO-Image (z. B. install.wim) in das Verzeichnis C:\WinPE_11\media\.
Erstelle eine Start.cmd für die automatische Installation:

Öffne den Ordner C:\WinPE_11\media und erstelle eine Datei Start.cmd mit folgendem Inhalt, um das Windows 11-Image auf der VM bereitzustellen:
dism /Apply-Image /ImageFile:"C:\WinPE_11\media\install.wim" /Index:1 /ApplyDir:C:\ /ScratchDir:C:\Scratch
bcdboot C:\Windows /s C:

Windows PE-Image als ISO erstellen:

Generiere ein bootfähiges ISO-Image für die Installation mit folgendem Befehl:
MakeWinPEMedia /ISO C:\WinPE_11 C:\WinPE_11\WinPE_11.iso

Schritt 3: Hyper-V-VM erstellen
Erstelle eine neue VM in Hyper-V:
Öffne den Hyper-V-Manager.
Klicke auf „Neue“ > „Virtuelle Maschine...“ und folge dem Assistenten.
Gib der VM einen Namen (z. B. Win11Deployment).
Wähle die Generation 2 für die VM.
Weise der VM mindestens 4 GB RAM zu.
Verbinde die VM mit einem virtuellen Switch (für Netzwerkzugriff, falls erforderlich).

Festplatte erstellen:
Erstelle eine virtuelle Festplatte mit mindestens 64 GB Speicher.

Boot-Optionen festlegen:
Wähle die Option, um von einem ISO-Image zu starten.
Wähle das WinPE_11.iso, das du zuvor erstellt hast.

Schritt 4: VM starten und Installation durchführen
Starte die VM:

Wähle im Hyper-V-Manager die erstellte VM aus und klicke auf „Verbinden“.
Starte die VM. Die VM sollte nun vom WinPE-ISO booten und die Start.cmd automatisch ausführen.
Beobachte die Installation:

Der Befehl in Start.cmd stellt das Windows 11-Image automatisch bereit und richtet den Bootloader ein.
Nach Abschluss der Installation starte die VM neu, um in die Windows 11-Umgebung zu booten.