# Linux-Helfer „Lumi“ 

Ein kleiner Avatar auf dem Desktop, der Linux-Einsteigern hilft: Lumi öffnet
die richtigen Einstellungen, startet Programme, findet Dateien, erklärt Begriffe
und Terminal-Befehle, prüft bei Problemen nach und erinnert dich an Dinge.

**Grundregel:** Lumi schaut nur nach, erklärt und öffnet Fenster. Sie führt nie
selbst Befehle mit Administratorrechten aus und ändert keine Einstellungen.
Ändern tust du selbst.
Alles läuft offline auf deinem Rechner.

## Starten

### Als AppImage (für Nutzer)

Auf der [Download-Seite](https://github.com/chincia84/Linuxhelper-Lumi/releases/latest)
gibt es zwei Dateien:

| Datei | Für wen |
| --- | --- |
| `Lumi-x86_64.AppImage` | klein, ohne eingebaute KI (die KI lässt sich in Lumi nachladen) |
| `Lumi-Komplett-x86_64.AppImage` | mit eingebauter KI (Qwen3 1.7B), etwa 1,4 GB, danach kein Download mehr nötig |

1. Datei herunterladen und am besten in einen festen Ordner legen, z. B. `~/Programme`.
2. Rechtsklick → *Eigenschaften* → *Berechtigungen* → **„Ausführbar“** (KDE) bzw.
   **„Als Programm ausführen“** (GNOME) anhaken.
3. Doppelklick. Beim ersten Start bietet Lumi an, sich ins **Startmenü** einzutragen.

Es wird nichts installiert und kein Administrator gebraucht. Einstellungen liegen in
`~/.config/linux-helfer/`. Löschen der AppImage-Datei entfernt Lumi; Startmenü-Eintrag
entfernen: Rechtsklick auf Lumi → *Im Startmenü anzeigen* abhaken. Verschiebst du die
Datei, trägt Lumi beim nächsten Start den neuen Ort ins Startmenü und den Autostart ein.
Updates holt sich Lumi selbst (signiert, nur nach Nachfrage) und ersetzt dabei die Datei;
„Lumi Komplett“ bleibt dabei „Komplett“.

Startet das AppImage nicht, fehlt meist FUSE: `sudo apt install libfuse2` (Ubuntu/Mint)
bzw. `sudo dnf install fuse` (Fedora). Ohne FUSE geht es auch so:
`./Lumi-x86_64.AppImage --appimage-extract-and-run`.

### Aus dem Ordner (für Entwickler)

```bash
chmod +x start.sh
./start.sh
```

Beim ersten Start legt das Skript im Unterordner `.umgebung` eine eigene
Python-Umgebung an und lädt dafür einmalig PySide6 und PyYAML
herunter. Es wird nichts systemweit installiert. Danach startet der Helfer
ohne Internet.

Beim Start öffnet sich ein **Willkommensfenster**, in dem sich Lumi vorstellt: was sie
ist, was sie kann und dass sie nichts heimlich ändert. Es kommt bei jedem Start, bis
du unten rechts den Haken **„Beim Start nicht mehr anzeigen“** setzt. Wieder öffnen:
Rechtsklick auf Lumi → *Lumi stellt sich vor* (dort lässt sich der Haken auch wieder
entfernen). Zum Testen ohne Fenster: `HELFER_WILLKOMMEN=0 ./start.sh`.

Beim **ersten Start** folgt auf das Willkommensfenster der **Einrichtungs-Assistent**.
In fünf kurzen Schritten erklärt Lumi alle Einstellungen in einfachen Worten, jede mit
einem Haken zum Einschalten:

| Schritt | Einstellungen |
| --- | --- |
| Wann ich da bin | Beim Anmelden starten · Im Startmenü anzeigen |
| Hilfe im Alltag | Kopierte Terminal-Befehle erklären · Getippte Befehle prüfen · Warnungen (Speicher, Akku, Updates) |
| Erinnerungen | Trinkpause und eigene Erinnerungen |
| Auf dem neuesten Stand | Wöchentlich nach Neuem suchen · Unbeantwortete Fragen ans Lumi-Team schicken |
| KI und Aussehen | Lokale KI benutzen bzw. einrichten · Figur aussuchen |

Eingeschaltet wird nur, was du anhakst; Empfehlungen sind markiert, und *Empfohlene
Auswahl* hakt sie alle auf einmal an. Erst **Fertig** übernimmt die Wahl, eine Übersicht
zeigt vorher alles noch einmal. *Später einrichten* ändert nichts. Wieder öffnen:
Rechtsklick auf Lumi → *Lumi einrichten …* Zum Testen ohne Assistent: `HELFER_EINRICHTUNG=0`.

- **Linksklick** auf Lumi: Frage stellen
- **Ziehen**: Lumi verschieben
- **Sprechblase größer oder kleiner**: Sie startet in einer festen Größe, damit sie nicht bei
  jeder Antwort springt; längere Texte lassen sich scrollen. Größer oder kleiner ziehst du sie am
  Anfasser oben links (drei schräge Striche) oder am linken bzw. oberen Rand. Lumi merkt sich die Größe. Doppelklick auf den Anfasser oder
  *Aussehen → Sprechblase auf Standardgröße* stellt sie zurück.
- **Rechtsklick**: Menü mit Aussehen, Infos und „Beenden“

### Symbol neben der Uhr

Lumi erscheint nicht in der Taskleiste, sondern als kleines Symbol im
Systembereich neben der Uhr. Ein Klick darauf blendet Lumi ein oder aus,
ein Rechtsklick öffnet dasselbe Menü wie auf Lumi selbst.

- **Beim Anmelden starten:** Haken im Menü setzen. Lumi legt dann
  `~/.config/autostart/linux-helfer.desktop` an; Haken entfernen löscht die Datei wieder.
- **GNOME:** Ohne die Erweiterung „AppIndicator and KStatusNotifierItem Support“
  gibt es dort keinen Systembereich. Lumi bleibt dann wie bisher in der Taskleiste.
  Ubuntu bringt diese Erweiterung schon mit.
- **Von selbst ausblenden:** Passiert 5 Minuten lang nichts, verabschiedet sich Lumi
  und wartet neben der Uhr, bis du auf das Symbol klickst. Mitten in einer Anleitung
  oder beim Tippen blendet sie sich nie aus. Andere Wartezeit: `HELFER_AUSBLENDEN_MIN=10 ./start.sh`,
  nie ausblenden: `HELFER_AUSBLENDEN_MIN=0 ./start.sh`. Ohne Symbol neben der Uhr bleibt Lumi immer sichtbar.
- Ohne Symbol starten: `HELFER_SYMBOL=0 ./start.sh`

Probier zum Beispiel: „Wie mache ich den Bildschirm abends gelber?“,
„dunkles Design“, „ich höre keinen Ton“, „alles ist so klein“.

## Was Lumi kann

| Werkzeug | So fragst du zum Beispiel |
| --- | --- |
| Einstellungen finden | „Wie mache ich den Bildschirm abends gelber?“, „dunkles Design“ |
| Programme starten | „Öffne den Taschenrechner“, „Ich will einen Brief schreiben“ |
| Systeminfo | „Wie viel Speicher ist frei?“, „Welche Linux-Version habe ich?“, „Wie voll ist der Akku?“ |
| Erste Hilfe | „Ich höre nichts“, „Mein Internet geht nicht“, „Bluetooth geht nicht“ |
| Programme installieren | „Wie installiere ich VLC?“ (öffnet Discover bzw. GNOME Software mit der Suche) |
| Browser | „Ich möchte einen anderen Browser“, „Wie installiere ich Brave?“, „Ich will im Internet surfen“ (schlägt Firefox, Chromium, Chrome, Brave, Vivaldi, LibreWolf, Edge, Falkon/GNOME Web und Tor Browser vor und öffnet die Flatpak-Seite im Software-Shop; ist Flatpak noch nicht eingerichtet, zeigt Lumi, wie) |
| Updates | „Gibt es Updates?“ |
| Dateien finden | „Wo ist meine Bewerbung?“, „Wo habe ich die Urlaub-Fotos?“ |
| Lexikon | „Was ist ein Terminal?“, „Was bedeutet Root?“ |
| Erinnerungen | „Erinnere mich in 20 Minuten an den Kuchen“, „… um 15:30 an den Zahnarzt“, „… alle 60 Minuten ans Trinken“, „Pomodoro starten“. Regelmäßige Erinnerungen stellst du unter Rechtsklick → *Erinnerungen einstellen* frei ein (Text, Abstand, an/aus), neue kommen mit „+“ dazu; als Beispiel ist „Trinkpause, alle 60 Minuten“ schon angelegt |
| Befehle erklären | „Was macht sudo apt update?“ oder einfach den Befehl eintippen |

**Von selbst meldet sich Lumi, wenn …**

- **du einen Terminal-Befehl kopierst**, während ein Terminal offen ist. Sie erklärt jeden Teil
  und stuft ihn ein: *Harmlos*, *Vorsicht* oder *Achtung, gefährlich!* Sie warnt besonders vor
  Befehlen, die alles löschen, Laufwerke überschreiben, Skripte ungesehen aus dem Internet
  ausführen oder mit unsichtbaren bzw. vertauschten Zeichen getarnt sind. Läuft eine lokale KI,
  erklärt sie zusätzlich Befehle, die Lumi nicht kennt.
  Passt der Befehl nicht zu deinem System (z. B. `sudo apt install vlc` aus einer
  Ubuntu-Anleitung, du nutzt aber Fedora), sagt Lumi **„Halt! Dieser Befehl ist nicht für dein
  System“**, nennt den passenden Befehl (`sudo dnf install vlc`) und kann ihn dir in die
  Zwischenablage legen. Ausführen musst du ihn selbst. Lumi kennt apt, dnf, rpm-ostree
  (Silverblue/Kinoite/Bazzite), pacman/yay/paru, zypper, emerge, xbps, apk und eopkg und
  erkennt das System an `/etc/os-release`.
- **du einen Befehl von Hand ins Terminal tippst**, der nicht zu deinem System passt oder
  gefährlich ist. Das ist ausgeschaltet, bis du es unter *Werkzeuge → Im Terminal getippte
  Befehle prüfen* einschaltest. Lumi legt dafür mit deiner Zustimmung eine kleine Datei an
  (`~/.config/linux-helfer/terminal-hook.sh`) und trägt in `~/.bashrc` (und `~/.zshrc`, falls
  du zsh nutzt; bei fish eine Datei in `conf.d`) eine markierte Zeile ein. Ausschalten entfernt
  alles wieder. Die Shell meldet jeden Befehl kurz vor dem Ausführen in einen privaten Ordner im
  Arbeitsspeicher (`$XDG_RUNTIME_DIR/linux-helfer`), den es nur gibt, solange Lumi läuft. Lumi
  liest ihn jede Sekunde, leert ihn sofort und speichert nichts. Harmlose Befehle kommentiert
  sie nicht. Wichtig: Lumi kann einen Befehl nicht aufhalten. Bei `sudo`-Befehlen bleibt Zeit,
  solange das Terminal nach dem Passwort fragt; ohne `sudo` läuft der Befehl schon, und Lumi
  rät, ihn mit Strg+C abzubrechen.
- **der Speicherplatz knapp wird**, **der Akku schwach ist** oder **Updates bereitstehen**.
- **eine Erinnerung fällig ist**, auch regelmäßige wie „Trinkpause alle 60 Minuten“ (frei einstellbar unter Rechtsklick → Erinnerungen).

Alles davon lässt sich im Menü unter „Werkzeuge“ und „Erinnerungen“ ein- und ausschalten.

**Wunschliste:** Fragen, auf die Lumi keine Antwort weiß, notiert sie in
`~/.config/linux-helfer/wunschliste.txt` (Menü → Werkzeuge → Wunschliste öffnen).
Ist in `daten/rueckmeldung.json` eine Adresse eingetragen, fragt Lumi höchstens
einmal pro Woche und nur bei neuen Einträgen, ob sie die Liste dorthin schicken
darf. Bei „Ja“ öffnet sie nur das eigene E-Mail-Programm mit fertiger Mail
(Fragen, Lumi-Version, System); abschicken tut man selbst. Ohne E-Mail-Programm
liegt die Mail in der Zwischenablage, z. B. für Webmail. Abschalten: Menü →
Werkzeuge. Ist keine Adresse eingetragen, fragt Lumi nie.
Daraus lassen sich neue Anleitungen schreiben. Die Liste verlässt deinen Rechner nie.

## Aussehen nach Jahreszeit und Anlass

Lumi zieht sich passend zum Datum um. Anlässe haben Vorrang vor den Jahreszeiten.

![Alle Aussehen von Lumi](assets/vorschau-aussehen.png)

| Aussehen | Wann |
| --- | --- |
| Frühling, Sommer, Herbst, Winter | März–Mai, Juni–August, September–November, Dezember–Februar |
| Karneval | Weiberfastnacht bis Faschingsdienstag |
| Valentinstag | 14. Februar |
| Ostern | Palmsonntag bis Ostermontag (wird jedes Jahr berechnet) |
| Halloween | 24. bis 31. Oktober |
| Weihnachten | 1. bis 26. Dezember |
| Silvester | 31. Dezember und 1. Januar |

- **Selbst wählen:** Rechtsklick auf Lumi → Aussehen → „Figur und Verkleidung einstellen …“.
  Unter „Verkleidung“ lässt sich ein Aussehen fest einstellen; „Automatisch“ schaltet zurück.
- **Ausprobieren:** `./start.sh --thema weihnachten` oder `--thema halloween` usw.
- **Termine ansehen:** `./start.sh --themen`

**Halloween:** Lumi verwandelt sich in einen kleinen Geist mit Kürbislaterne vor dem
Nachthimmel (eigene Zeichnung, CC BY-SA 4.0). Das Bild liegt unter
`assets/avatare/halloween.png`, die bearbeitbare Vorlage unter `assets/avatare/quellen/halloween.svg`. Wer lieber die gezeichnete Figur mit
Hexenhut möchte, benennt die Datei einfach um oder löscht sie.

### Figuren (Grund-Avatare)

Lumi muss nicht die runde Figur sein. Im Fenster *Rechtsklick → Aussehen → Figur und
Verkleidung einstellen …* stehen mehrere Figuren zur Wahl. Jede verkleidet sich wie
Lumi nach Jahreszeit und Anlass, blinzelt, schaut zur Maus und bewegt beim Sprechen den Mund.
Über „Vorschau für“ lässt sich ansehen, wie jede Figur z. B. an Weihnachten aussieht.

![Die Figuren in verschiedenen Verkleidungen](assets/vorschau-figuren.png)

| Figur | Besonderheit |
| --- | --- |
| Lumi | die klassische runde Figur mit Leuchtantenne (vom Programm gezeichnet) |
| Birni | Glühbirne; an Halloween wird sie zum Kürbis |
| Flocke | flauschiger weißer Hundewelpe (Zwergspitz) |
| Mika | Katze |
| Robo | Roboter mit Antenne; wechselt mit der Jahreszeit die Farbe, eigene Sommerbrille |

Ausprobieren: `./start.sh --figur flocke --thema winter`, alle Figuren: `./start.sh --figuren`.

**Eigene Figuren** fügt man im selben Fenster mit „+ Eigene Figur hinzufügen …“ hinzu
(SVG-Zeichnung oder PNG-Bild). Sie bleiben auf dem eigenen Rechner unter
`~/.config/linux-helfer/avatare/`. Damit eine Zeichnung blinzelt, spricht und Mützen
trägt, braucht sie ein paar benannte Ebenen. Die Schaltfläche „Vorlage zum Selbermalen …“
speichert eine Inkscape-Vorlage, in der alles erklärt ist (auch: `assets/avatare/vorlage.svg`).
Kurz: `augen-offen`, `augen-zu`, `pupillen`, `mund-zu`, `mund-offen` sowie die unsichtbaren
Rechtecke `kopf` (Mützen, Hüte), `brille` (Sonnenbrille) und `hals` (Schal). Ebenen wie
`thema-halloween` erscheinen nur zu diesem Anlass, `zubehoer-weihnachtsmuetze` usw.
ersetzen das gezeichnete Zubehör.

**Neue Figuren fürs Projekt** kommen als Ordner nach `assets/avatare/<name>/` mit
`figur.svg` und `info.json` (Name, Beschreibung, Autor, Lizenz; `"einfaerben": true` färbt
die Lumi-Farben zur Jahreszeit um, `"antenne": true` gibt der Figur Lumis Antenne).
Ohne Lizenzangabe wird eine mitgelieferte Figur nicht angezeigt. Figuren reisen mit dem
signierten Wissenspaket mit, d. h. neue Figuren erreichen alle Nutzer über das
GitHub-Update, ohne dass eine neue Lumi-Version nötig ist. Lumi prüft jede Zeichnung:
keine Skripte, keine Verweise ins Internet oder auf andere Dateien.

### Eigene Bilder für die klassische Lumi

Statt der gezeichneten runden Lumi kann für jedes Aussehen ein eigenes Bild verwendet
werden. Lege dazu eine PNG-Datei mit durchsichtigem Hintergrund (am besten etwa
200 × 200 Pixel) unter dem Namen des Aussehens ab:

```
~/.config/linux-helfer/avatare/weihnachten.png   # nur für dich
assets/avatare/weihnachten.png                   # für alle, die das Projekt nutzen
```

Am einfachsten geht das mit:

```bash
./start.sh --avatar halloween ~/Bilder/geist.png      # Bild übernehmen
./start.sh --thema halloween                          # sofort ansehen
./start.sh --avatar halloween entfernen               # wieder gezeichnete Figur
```

Mögliche Namen: `standard`, `fruehling`, `sommer`, `herbst`, `winter`,
`weihnachten`, `silvester`, `halloween`, `ostern`, `karneval`, `valentinstag`.
Bei eigenen Bildern bewegen sich Augen und Mund nicht, Schnee, Konfetti und
Co. fallen aber weiter.

## Ohne Fenster testen

```bash
./start.sh --pruefen                          # alle Anleitungen prüfen
./start.sh --frage "Blaulichtfilter"          # welche Anleitung würde passen?
./start.sh --desktop kde --frage "dark mode"  # als KDE testen
python3 -m unittest discover -s tests -v      # automatische Tests (nur PyYAML nötig)
```

## Anleitungen prüfen – bitte mithelfen

Die meisten Anleitungen sind noch **ungeprüft**: Texte und Hinweise stammen
nicht von echten Systemen, sondern sind begründete Vermutungen. Wenn du eine
Anleitung ausprobierst und ein Hinweis (z. B. „Umschaltzeiten“) nicht zu dem
passt, was im Fenster steht, trage das richtige Wort in der YAML-Datei unter
`hinweis:` ein und setze `geprueft: true`. Bisher geprüft: KDE-Nachtlicht.

Es gibt 20 Themen, jeweils für GNOME und KDE (40 Anleitungen):

| Bildschirm und Aussehen | Ton und Geräte | System und Konto |
| --- | --- | --- |
| Auflösung und Skalierung | Ton-Ausgabe | WLAN (anderes Netz, neues Passwort) |
| Schriftgröße | Mikrofon | Bluetooth |
| Mehrere Bildschirme | Drucker | Passwort ändern |
| Dunkles Design | Maus und Touchpad | Bildschirmsperre |
| Nachtlicht | Tastatur-Layout | Datum, Uhrzeit, Zeitzone |
| Hintergrundbild | | Standardprogramme |
| Bildschirm ausschalten (Energie) | | Autostart |
| | | Benachrichtigungen / Nicht stören |

GNOME hat ab Version 46 einige Seiten verschoben (z. B. Benutzer und Datum unter
„System“). Lumi probiert dann die passende Seite für deine Version. Unter GNOME
gibt es für den Autostart keine eigene Einstellungsseite; Lumi öffnet dafür
„Optimierungen“ (GNOME Tweaks), wenn es installiert ist.

## Lumi aktuell halten

Desktops ändern sich mit Updates: Einstellungsseiten ziehen um oder heißen anders.
Damit Lumi trotzdem weiß, wo alles ist, gibt es drei Bausteine.

**1. Lumi merkt System-Updates.** Beim Start liest Lumi die Version von Plasma bzw.
GNOME und der Distribution. Hat sie sich seit dem letzten Start geändert, prüft
Lumi alle Anleitungen: Gibt es ihre Einstellungsseite auf diesem System noch? Das
Ergebnis meldet sie dir („Dein System wurde aktualisiert: KDE Plasma 6.1 → 6.2 …
2 Anleitungen passen nicht mehr ganz“) und notiert es in der Wunschliste. Prüfen
kannst du jederzeit unter *Lumi aktualisieren → Anleitungen auf diesem System
prüfen* oder mit `./start.sh --pruefen`. Öffnest du eine Anleitung, deren Seite
fehlt, warnt Lumi, dass die Schritte vielleicht nicht mehr ganz stimmen.

**2. Anleitungen für bestimmte Versionen.** Eine Anleitung kann angeben, für
welche Desktop-Versionen sie gilt, z. B. `versionen: ">=46"` oder
`versionen: ">=6.0, <6.3"`. So können alte und neue Fassung nebeneinander liegen,
und Lumi nimmt die passende.

**3. Updates über GitHub.** Unter *Lumi aktualisieren → Nach Updates suchen* schaut
Lumi im GitHub-Projekt nach:
- **Neue Lumi-Version** (GitHub Releases): Lumi zeigt, was neu ist, lädt das Paket,
  prüft die Signatur, tauscht ihren Programmordner aus und startet neu. Einstellungen,
  eigene Avatare und die Python-Umgebung bleiben erhalten. Die bisherige Version
  bleibt als `linux-helfer.vorher` liegen: *Vorherige Lumi-Version zurückholen*.
  Braucht die neue Version weitere Python-Pakete, ergänzt `start.sh` sie beim Start.
  Als AppImage wird einfach die AppImage-Datei ersetzt (die alte bleibt als `.vorher`).
- **Neues Wissen** (Veröffentlichung „wissen-<stand>“ mit `wissen.json`, `wissen.json.sig` und
  `wissen.tar.gz`; ältere Projekte: Ordner `wissenspaket/`): neue und überarbeitete
  Anleitungen, Lexikon und Befehlsliste, ohne neue Programmversion.

Sicherheit:
- Lumi sucht nur, wenn du es sagst, oder einmal pro Woche, wenn du das einschaltest.
  Sie fragt immer, bevor sie etwas holt, und sendet nichts über dich.
- Alles ist mit Ed25519 signiert; Lumi prüft mit dem mitgelieferten öffentlichen
  Schlüssel. Ohne gültige Signatur wird nichts installiert. Nur `https://`.
- Programm-Archive werden sicher entpackt (keine Pfade nach außen, keine Verknüpfungen),
  und die Version im Paket muss zur Veröffentlichung passen.
- Anleitungen dürfen nur Einstellungsprogramme öffnen, auch aus einem Update.
- Vorabversionen bietet Lumi nur an, wenn du *Auch Vorabversionen anbieten* einschaltest.
- Läuft Lumi aus einem Git-Ordner, aktualisiert sie sich nicht selbst (dann `git pull`).

### Veröffentlichen auf GitHub (für das Projekt)

**Am einfachsten mit der Lumi-Werkstatt** – einem eigenen Fenster statt Terminal:

```bash
./werkstatt.sh
```

| Reiter | Was du dort machst |
| --- | --- |
| 1 · Schlüssel | Schlüssel anlegen, Sicherungskopie speichern (z. B. USB-Stick), aus einer Sicherung einspielen; Fingerabdruck ansehen |
| 2 · GitHub | Anmeldestatus, „Bei GitHub anmelden …“ (öffnet ein Terminal mit `gh auth login`), Projekt zum ersten Mal hochladen |
| 3 · Anleitungen | sieht, welche Anleitungen/Figuren seit dem letzten Wissenspaket neu oder geändert sind; prüfen, signieren, veröffentlichen |
| 4 · Neue Version | neue Versionsnummer wählen, „Was ist neu?“ schreiben, veröffentlichen; danach AppImages signieren |
| Prüfen | alle Signaturen oder eine einzelne Datei so prüfen, wie Lumi es tut; Tests laufen lassen |

**Selbst im Browser hochladen (Standard):** Oben in der Werkstatt steht „Hochladen: Selbst im Browser“.
Dann braucht es weder git noch gh. Die Werkstatt prüft und signiert alles und legt es in
`Schreibtisch/Lumi-hochladen/<Datum>_<Art>/` ab, aufgeteilt in Pakete zu höchstens 90 Dateien
(GitHub nimmt im Browser höchstens 100 auf einmal). Sie öffnet den Ordner und zeigt Knöpfe für die
passenden GitHub-Seiten – der Browser öffnet sich erst, wenn du einen davon klickst (die Links stehen
auch im Protokoll). In jedem Ordner erklärt `SO GEHT ES.txt` jeden Schritt. Nach dem Hochladen
„Ich habe alles hochgeladen“ klicken – beim nächsten Mal kommen nur die Änderungen. Für eine neue
Version legt sie zusätzlich `lumi-<version>.tar.gz` mit Signatur bereit und bietet die Seite
„Draft a new release“ mit schon eingetragener Version und Text an; die AppImage-Signaturen erstellt
sie später aus GitHubs Prüfsummen, ohne die AppImages herunterzuladen.

Im Modus „Automatisch“ nutzt die Werkstatt dieselben Befehle wie unten (`veroeffentlichen.sh`), alles steht im Protokoll
unten im Fenster. Sie fragt vor jedem Hochladen nach und prüft vorher, ob der Schlüssel zu
`daten/update.json` passt und ob du bei GitHub angemeldet bist. Den geheimen Schlüssel speichert
sie nie im Projektordner.

**Oder im Terminal:**

Das Projekt liegt unter https://github.com/chincia84/Linuxhelper-Lumi (eingetragen in
`daten/update.json`). Das Skript `veroeffentlichen.sh` erledigt alles; gebraucht werden
`git` und `gh` ([GitHub CLI](https://cli.github.com/), einmal `gh auth login`). Auf Bazzite und
anderen Atomic-Systemen (Bluefin, Aurora, Silverblue, Kinoite) geht `dnf install` nicht, dort:
`brew install gh`. Fehlt PyYAML, legt das Skript beim ersten Mal selbst eine kleine Umgebung
`.werkzeug/` an; für den Schlüssel (`einrichten`) reicht Python allein.

```bash
./veroeffentlichen.sh einrichten              # einmalig: Schlüssel anlegen und eintragen
./veroeffentlichen.sh erstmals                # einmalig: Projekt zum ersten Mal hochladen
./veroeffentlichen.sh wissen "Neu: …"          # neue oder geänderte Anleitungen
./veroeffentlichen.sh version "- Was neu ist"  # neue Lumi-Version (vorher Version in helfer/__init__.py erhöhen)
./veroeffentlichen.sh appimages               # etwa 20 Minuten später: AppImages signieren
```

**AppImages:** Sobald eine Version veröffentlicht ist, baut GitHub beide AppImages
(`.github/workflows/appimage.yml`, auf Ubuntu 22.04, damit sie auch auf älteren Systemen
laufen) und hängt sie samt Prüfsumme an die Version. Vorher startet ein Rauchtest Lumi mit
echtem Qt ohne Bildschirm und zeichnet alle Figuren. Signiert wird nicht auf GitHub,
denn der geheime Schlüssel bleibt bei dir: `./veroeffentlichen.sh appimages` holt nur die
Prüfsummen (kein Download von 1,4 GB), gleicht sie mit GitHubs eigener Prüfsumme ab,
signiert sie und lädt die `.sig`-Dateien hoch. Erst dann bieten bestehende Lumis das
Update an.

**Selbst bauen** (wenn GitHub nicht bauen soll, z. B. weil nur das Nötigste hochgeladen wird):
`./appimage/bauen-im-container.sh leicht|komplett|beide` oder in der Werkstatt „AppImages selbst
bauen“. Gebaut wird in einem Ubuntu-22.04-Container mit podman (bei Bazzite schon dabei) oder
docker, genau wie auf GitHub – so laufen die AppImages auch auf älteren Systemen, und auf dem
eigenen System wird nichts installiert. Braucht Internet (beim ersten Mal etwa 300 MB, für
„Komplett“ zusätzlich 1,3 GB, danach zwischengespeichert) und etwa 4 GB Platz; Ergebnis in
`ausgabe/`. Ohne Container geht `./appimage/bauen.sh …` direkt, die AppImages laufen dann aber
nur auf Systemen, die mindestens so neu sind wie das eigene.

**Was muss auf GitHub?** Für Lumis Funktion nur die Veröffentlichungen (Releases): neue
Versionen mit den signierten Paketen und AppImages und neues Wissen als „wissen-<stand>“ (als
Vorabversion markiert, damit „Latest“ die Lumi-Version bleibt). Dazu gehören README und Lizenzen
auf die Projektseite. In der Werkstatt heißt das
„Auf GitHub: Nur was Lumi braucht“ (Standard beim Hochladen im Browser). Programm, Tests und
Werkzeuge bleiben dann auf dem eigenen Rechner; die AppImages baut man selbst. Mit „Ganzes
Projekt mit Quelltext“ kommt alles hoch, und GitHub baut die AppImages. Den Bau kann man auf GitHub unter *Actions →
AppImages → Run workflow* auch für eine bestehende Version neu anstoßen.

Der geheime Schlüssel liegt in `~/.config/linux-helfer-projekt/lumi-geheim.key`,
außerhalb des Projekts. Gut sichern und nie hochladen: Wer ihn hat, kann Updates
für alle Lumi-Nutzer signieren; geht er verloren, nehmen bestehende Lumis keine
Updates mehr an. Vor jedem Hochladen prüft das Skript Anleitungen und Tests.

## Lokale KI (optional)

Ohne KI nutzt Lumi eine Stichwortsuche. Mit KI versteht sie Fragen besser und
beantwortet auch Fragen, für die es noch keine Anleitung gibt. Die KI läuft ganz
auf deinem Rechner; nichts verlässt ihn.

**Einrichten mit einem Klick:** Rechtsklick auf Lumi → *Lokale KI → KI einrichten*.
Lumi schaut, wie viel Arbeitsspeicher dein Rechner hat, und schlägt ein Modell vor:

| Modell | Größe | Gedacht für | Lizenz |
| --- | --- | --- | --- |
| Qwen3 1.7B (Q4_K_M, [ggml-org](https://huggingface.co/ggml-org/Qwen3-1.7B-GGUF)) | etwa 1,3 GB | eingebaut in „Lumi Komplett“; ab 4 GB Arbeitsspeicher | Apache 2.0 |
| Qwen3 4B (Q4_K_M, [Qwen](https://huggingface.co/Qwen/Qwen3-4B-GGUF)) | etwa 2,5 GB | zum Nachladen; ab 8 GB Arbeitsspeicher, versteht schwierigere Fragen besser | Apache 2.0 |

Später gibt es zwei AppImages: **„Lumi Komplett“** mit eingebautem Qwen3 1.7B (läuft
sofort ohne Internet, etwa 1,5 GB) und **„Lumi“** ohne KI (klein, die KI per Klick
nachladbar). In beiden lässt sich das größere Qwen3 4B unter *Lokale KI* nachladen;
ein nachgeladenes Modell hat Vorrang vor dem eingebauten.

Dazu kommt `llama-server` aus [llama.cpp](https://github.com/ggml-org/llama.cpp)
(etwa 20 MB, MIT), außer es ist schon über die Paketverwaltung installiert. Beides
kommt von den offiziellen Quellen (GitHub, Hugging Face) und wird mit der vom
Anbieter veröffentlichten SHA-256-Prüfsumme geprüft. Ein abgebrochener Download wird
beim nächsten Mal fortgesetzt. Alles liegt in `~/.local/share/linux-helfer/ki/`.

**Mitgeliefert statt heruntergeladen:** Liegen `llama-server` und ein Modell schon im
Lumi-Ordner unter `ki/`, nutzt Lumi diese und lädt nichts herunter. So kommt die KI
später fertig ins AppImage („Lumi Komplett“). Für das Projekt holt
`python3 -m helfer.ki_dienst mitliefern` beides dorthin (das kleine Modell). Der
Ordner `ki/` kommt nicht ins Git-Projekt (zu groß); bei einem Selbst-Update der
Ordner-Version wird er übernommen.

Danach startet Lumi die KI bei jedem Start selbst im Hintergrund, nur auf diesem
Rechner erreichbar, und beendet sie mit sich. Im selben Menü lässt sie sich aus- und
einschalten oder ganz entfernen.

**So setzt Lumi die KI ein:**
- Ist die Stichwortsuche sicher, fragt Lumi die KI gar nicht (das spart Zeit).
- Sonst wählt die KI unter den vorhandenen Anleitungen; sie darf keine erfinden.
- Gibt es keine Anleitung, antwortet die KI frei. Lumi kennzeichnet das deutlich
  („Das hat meine lokale KI geantwortet. Sie kann sich irren“), prüft jeden genannten
  Terminal-Befehl wie einen kopierten und notiert die Frage in der Wunschliste.
- Unbekannte Terminal-Befehle erklärt die KI zusätzlich.

**Eigener KI-Server:** Wer schon Ollama oder einen eigenen llama-server nutzt:

```bash
HELFER_KI_URL=http://127.0.0.1:11434/v1 HELFER_KI_MODELL=qwen3:4b ./start.sh   # Ollama
HELFER_KI=0 ./start.sh                                                         # KI ganz aus
```

Auf einem normalen Prozessor ohne Grafikkarte braucht eine Antwort einige Sekunden.

## X11 und Wayland

Lumi läuft unter X11 und Wayland. Unter Wayland startet Lumi selbst über
XWayland, weil sie nur so unten rechts verankert bleiben und immer im
Vordergrund liegen kann. Abschalten lässt sich das mit `HELFER_XWAYLAND=0 ./start.sh`.

## Bekannte Grenzen dieser Testversion

- Die meisten Anleitungen sind noch ungeprüft (siehe oben).
- Kopierte Befehle: Unter Wayland erfahren Programme im Hintergrund nicht, was kopiert wurde.
  Unter KDE fragt Lumi deshalb jede Sekunde die Plasma-Zwischenablage (Klipper) ab, auf anderen
  Wayland-Desktops `wl-paste`, wenn es installiert ist. Lumi meldet sich nur, wenn ein Terminal
  offen ist. Wird ein Befehl ohne offenes Terminal kopiert, wartet Lumi bis zu 10 Minuten und
  erklärt ihn, sobald ein Terminal aufgeht. Über *Werkzeuge → Kopierten Befehl erklären* geht es
  jederzeit von Hand. Im Terminal selbst kopiert man mit **Strg+Umschalt+C**; nur markierter Text
  landet nicht in der Zwischenablage.
- Die Internet-Prüfung („Mein Internet geht nicht“) baut auf deinen Wunsch hin eine kurze
  Testverbindung auf. Sonst nimmt Lumi keinen Kontakt nach außen auf (außer zu einer lokalen KI).
- Noch keine Sprachein- und -ausgabe, keine Embedding-Suche.
- Noch kein AppImage: Der erste Start braucht einmal Internet.

## Fehlerbehebung

- **„Could not load the Qt platform plugin "xcb"“**: Qt braucht die Bibliothek
  `libxcb-cursor0`. Unter Debian/Ubuntu: `sudo apt install libxcb-cursor0`
  (das AppImage wird sie später mitbringen).
- **„python3-venv fehlt“**: `sudo apt install python3-venv` (nur Debian/Ubuntu/Mint).

## Aufbau

```
helfer/
  __main__.py    Startpunkt und Kommandozeile
  oberflaeche.py Avatar, Sprechblase, Ablauf einer Anleitung
  verstehen.py   Frage → Anleitung (Stichwörter, optional lokale KI)
  wissen.py      Anleitungen laden und prüfen
  aktionen.py    Einstellungsseiten öffnen
  desktop.py     GNOME/KDE und X11/Wayland erkennen
  themen.py      Aussehen nach Jahreszeit und Anlass (inkl. Osterformel)
  avatare.py     Figuren (Grund-Avatare): finden, prüfen, in Ebenen zerlegen
  autostart.py   Autostart und Startmenü-Eintrag (auch für das AppImage)
  einrichtung.py Einrichtungs-Assistent: alle Einstellungen mit Erklärung
  werkstatt.py   Lumi-Werkstatt (für das Projekt): Schlüssel, Sicherung, Version, Änderungen
  werkstatt_fenster.py  ihr Fenster (./werkstatt.sh)
  einstellungen.py  gespeicherte Wahl in ~/.config/linux-helfer
  fensterhilfe.py   Lumi aus der Taskleiste nehmen (X11, ctypes)
  autostart.py      „Beim Anmelden starten“
  werkzeuge/
    absicht.py      welches Werkzeug passt zu einer Frage
    programme.py    Programme aus den .desktop-Dateien finden und starten
    systeminfo.py   Speicher, Akku, Version, Temperatur …
    diagnose.py     Erste Hilfe bei Ton, Internet, Bluetooth
    software.py     Software-Center und Updates öffnen
    dateien.py      Dateien im persönlichen Ordner finden
    lexikon.py      Begriffe erklären (Inhalte in daten/lexikon.yaml)
    befehle.py      Terminal-Befehle erklären und einstufen (daten/befehle.yaml)
    system.py       erkennt die Distribution und ob ein Befehl dazu passt
    browser.py      Browser vorschlagen und als Flatpak im Software-Shop öffnen
    terminal.py     Terminal-Begleitung: eingetippte Befehle über einen Shell-Hook sehen
    erinnerungen.py Erinnerungen in Alltagssprache verstehen
    waechter.py     Warnungen bei Speicher, Akku, Updates
    wunschliste.py  unbeantwortete Fragen sammeln
  protokoll.py   Protokoll zur Fehlersuche
  systemstand.py Desktop-Version merken, Änderungen nach Updates erkennen
  update.py      Wissens-Updates prüfen, installieren, zurücknehmen
  ki_dienst.py   lokale KI: llama.cpp und Sprachmodell holen, starten, beenden
  selbstupdate.py neue Lumi-Version von GitHub holen, austauschen, zurückholen
  wissenspaket.py Updates für GitHub bauen und signieren (für das Projekt)
  ed25519.py     Signaturprüfung ohne Zusatzpakete
anleitungen/     YAML-Anleitungen für GNOME und KDE
daten/           Lexikon und Befehls-Erklärungen (YAML, leicht zu erweitern)
assets/avatare/  Figuren (birni/, mika/, robo/), Vorlage zum Selbermalen, Halloween-Bild
tests/           Automatische Tests
```

## Eine eigene Anleitung schreiben

```yaml
id: gnome-beispiel          # eindeutig
titel: Kurzer Titel
beschreibung: Ein bis zwei Sätze, was die Einstellung bewirkt.
beispielfragen:             # verschiedene Formulierungen
  - Wie mache ich …?
stichwoerter: [wort1, wort2]
desktop: gnome              # gnome, kde oder alle
geprueft: false
oeffnen: [gnome-control-center, display]
schritte:
  - text: Was der Mensch tun soll.
    hinweis: Beschriftung im Fenster   # optional, wonach man schauen soll
```

Lizenz: noch offen (siehe Projektplan).

## Lizenz

Lumi ist freie Software: der Code unter der **GPL-3.0-or-later** ([LICENSE](LICENSE)),
Anleitungen, Lexikon, Befehlsliste und Bilder unter **CC BY-SA 4.0**. Einzelheiten und
fremde Bestandteile stehen in [LIZENZEN.md](LIZENZEN.md).

