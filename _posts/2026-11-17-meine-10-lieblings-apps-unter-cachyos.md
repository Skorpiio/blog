---
title: "Meine 10 Lieblings-Apps unter CachyOS"
date: 2026-11-17 15:00:00 +0100
categories: [Linux]
tags: [cachyos, linux, apps, open-source]
media_subpath: /assets/img/posts/lieblings-apps/
---

Angefangen hat alles mit Windows. Ich hatte zuerst noch Dualboot, hab Windows aber kaum benutzt. Und wenn doch, war es wirklich schrecklich: Jedes Mal kam irgendeine Werbung für OneDrive oder für die Windows-KI. Das hat mich richtig genervt, und genau das war überhaupt erst der Grund, warum ich zu einer Linux-Distribution wechseln wollte. Mit der Zeit kamen dann noch andere Gründe dazu: Performance, weil unter Windows ständig irgendwas im Hintergrund läuft, und Datenschutz, weil man bei Windows nie so genau weiß, was alles an Microsoft geht. Ich will einfach selbst entscheiden, was auf meinem PC passiert.

Ubuntu und Fedora haben mir ehrlich gesagt ein bisschen zu alt ausgesehen. Ich weiß, die sind stable und reliable und viele ältere Programme laufen da einfach, aber ich wollte lieber etwas auf Basis von Arch. Ein großer Grund dafür: SteamOS auf dem Steam Deck basiert auf Arch, und Valve unterstützt Arch seit 2024 sogar direkt mit Geld für die Infrastruktur. Wenn eine so große Firma Geld in Arch steckt und selbst darauf aufbaut, bekommt Arch gerade beim Gaming mehr Aufmerksamkeit als zum Beispiel Fedora oder Ubuntu, finde ich. Deshalb finde ich es einfach sicherer und entspannter, Arch zu benutzen.

Dazu kommt, dass Arch super customizable und sehr performant ist. Man kann sich das System genau so bauen, wie man es braucht. CachyOS ist für mich dabei der perfekte Mittelweg: Es basiert auf Arch, ist aber zusätzlich auf Performance optimiert und deutlich einfacher zu installieren. Ich benutze es jetzt seit ungefähr anderthalb Jahren.

Für jemanden, der Windows gewohnt ist, ist die Anfangsarbeit wahrscheinlich erstmal ziemlich hoch, bis alles so läuft, wie man es gerne hätte. Meiner Meinung nach lohnt sich das aber komplett.

Hier sind die 10 Programme, die ich unter CachyOS am meisten benutze und am nützlichsten finde. Dabei habe ich bewusst eher Sachen genommen, die nicht jeder kennt.

## 1. Vesktop

Vesktop ist im Prinzip Discord, nur mit ein paar Vorteilen: Es ist datenschutzfreundlicher, weil Discord darüber weniger Zugriff auf euer System hat, und es braucht weniger Ressourcen. Außerdem ist Vencord schon mit drin, und damit gibt es voll viele Plugins, mit denen man sich Discord so einstellen kann, wie man es haben will.

Wie ich darauf gestoßen bin, weiß ich gar nicht mehr genau. Ich glaube, es waren die ganzen Diskussionen rund um Discord und Datenschutz. Irgendwann bin ich jedenfalls bei Vesktop gelandet, und im Alltag benutze ich es genau so wie Discord.

Ein Plugin, das ich wirklich empfehlen kann, ist **CustomIdle**. Eigentlich soll man nur auf „Abwesend“ gehen, wenn man wirklich eine Weile nichts am PC macht. Vesktop hat mich aber, zumindest vor ein paar Monaten noch, nach 15 Minuten einfach auf abwesend gesetzt, obwohl ich die ganze Zeit am PC war, nur eben nicht im Vesktop-Fenster. Ob das mittlerweile gefixt ist, weiß ich nicht. Mit CustomIdle kann man jedenfalls selbst einstellen, wann man auf abwesend gesetzt wird.

Für wen lohnt es sich? Ehrlich gesagt wüsste ich nicht, für wen nicht. Es ist halt einfach Discord, nur ein bisschen besser. Es gibt auch Plugins, mit denen man ein paar Nitro-Funktionen freischalten kann. Das ist aber eine Grauzone, und ich weiß nicht, ob man dafür gebannt werden kann.

### Muten, auch wenn Vesktop nicht vorne ist

Was mich an Vesktop wirklich nervt: Es gibt keine globalen Tastenkürzel. Wenn man sich Mute auf eine Taste legt und aus dem Fenster rausgetabbt ist, zum Beispiel weil man gerade ein Game spielt, dann funktioniert die Taste einfach nicht.

Die Lösung dafür habe ich auf GitHub gefunden: [vesktopCustomCommands](https://github.com/NitramO-YT/vesktopCustomCommands) von NitramO, kurz VCC. Damit bekommt man zwei kleine Skripte, `mute.sh` und `deafen.sh`, die Vesktop stumm oder taub schalten, egal welches Fenster gerade vorne ist. Die kann man zum Beispiel in den KDE-Systemeinstellungen bei den Kurzbefehlen auf eine Tastenkombination legen. Als Befehl trägt man dort den vollen Pfad zum Skript ein, also `/home/euername/.vesktopCustomCommands/mute.sh`. Genauso kann man die Skripte auch auf eine Maustaste oder ein Stream Deck legen.

**VCC installieren:** Vesktop muss vorher mindestens einmal gelaufen sein. Dann Vesktop komplett beenden (Rechtsklick auf das Symbol in der Taskleiste und beenden), ein Terminal öffnen und diesen Befehl einfügen. Der funktioniert auch direkt in fish:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/NitramO-YT/vesktopCustomCommands/refs/heads/main/install.sh)"
```

Danach stellt der Installer ein paar Fragen auf Englisch. Bei den y/n-Fragen reicht ein Tastendruck. So habe ich geantwortet:

1. **Automatically install?** `y`
2. **Is the path of Vencord of Vesktop "~/.config/Vencord/dist/"?** `n`, weil Vesktop Vencord woanders ablegt.
3. **Please enter the path:** Den vorgegebenen Pfad löschen, `~/.config/vesktop/sessionData/vencordFiles/` eintippen und Enter drücken. Das gilt, wenn Vesktop als normales Paket installiert ist, so wie bei mir. Bei der Flatpak-Version liegt der Ordner woanders.
4. **Enable automatic repatch?** `n`. Das übernimmt mein Skript weiter unten, und zwar zuverlässiger.
5. **Enable automatic update?** `n`, aus demselben Grund.
6. **Start Vesktop now?** `y`

<details markdown="1">
<summary>VCC wieder entfernen</summary>

Vesktop vorher komplett beenden. Die Befehle funktionieren direkt in fish. Falls ihr meine Verbesserungen von unten eingerichtet habt, werden die dabei gleich mit entfernt.

```bash
systemctl --user disable --now vcc-lokal.path vcc-autorepatch.timer vcc-autoupdate.timer 2>/dev/null
rm -f ~/.config/systemd/user/vcc-lokal.path ~/.config/systemd/user/vcc-lokal.service ~/.config/systemd/user/vcc-autorepatch.service ~/.config/systemd/user/vcc-autorepatch.timer ~/.config/systemd/user/vcc-autoupdate.service ~/.config/systemd/user/vcc-autoupdate.timer
systemctl --user daemon-reload
rm -rf ~/.vesktopCustomCommands ~/.config/vesktop/sessionData/vencordFiles
```

Der letzte Befehl löscht auch die Vencord-Dateien von Vesktop, in die sich VCC eingetragen hat. Das ist Absicht: Beim nächsten Start lädt Vesktop Vencord einfach frisch herunter, ganz ohne VCC. Eure Vencord-Einstellungen und Plugins bleiben dabei erhalten. Zum Schluss noch die Tastenkürzel in den Systemeinstellungen wieder löschen.
</details>

### Meine Verbesserungen für VCC

VCC funktioniert, aber mit der Zeit sind mir drei Probleme aufgefallen, die ich gelöst habe:

1. **Die Mute-Taste hat manchmal einfach nichts gemacht.** VCC drückt intern auf den Mute-Knopf von Discord und erkennt den Knopf an einem Merkmal, das sich je nach Start ändert. Dann findet VCC den Knopf nicht. Meine Fassung schaltet stattdessen direkt über Discords eigene Funktion um. Nur wenn das ausnahmsweise nicht klappt, wird wie vorher der Knopf gedrückt.
2. **Vesktop ist immer stumm gestartet.** Bei mir hat sich Vesktop nicht gemerkt, dass ich entmutet war, und war nach jedem Start wieder stumm. Das merkt man oft erst, wenn einen im Call keiner hört. Deshalb schaut mein Skript bei jedem Start von Vesktop, also auch automatisch beim Hochfahren, ob man stumm ist, und hebt die Stummschaltung dann auf. War man nicht stumm, passiert gar nichts, und danach kann man ganz normal muten.
3. **Nach Updates gingen die Tasten nicht mehr.** Ein Vesktop-Update ersetzt die Datei, in die sich VCC einträgt, und ein VCC-Update würde meine Verbesserungen überschreiben. VCC hat dafür zwar eine eigene Automatik, die schaut aber nur alle 30 Sekunden nach. Und beim Neustarten von Vesktop hat sie sich bei mir selbst gleich mit beendet, Vesktop blieb danach einfach zu. Mein Skript wird sofort aktiv, sobald sich eine der beiden Dateien ändert, trägt alles wieder ein, startet Vesktop neu und meldet sich kurz mit einer Benachrichtigung.

Was genau passiert, steht in den Kommentaren im Block.

**So geht's:**

1. VCC wie oben installieren.
2. Terminal öffnen, `bash` eintippen und Enter drücken (fish versteht die folgenden Befehle nicht).
3. Den ganzen Block unten kopieren, ins Terminal einfügen und Enter drücken. Läuft Vesktop gerade, wird es dabei einmal neu gestartet.
4. `exit` eintippen. Fertig.

Zum Testen: In Vesktop stummschalten, Vesktop komplett beenden und wieder starten. Ein paar Sekunden nach dem Start sollte man wieder entstummt sein.

<details markdown="1">
<summary>Der Block zum Kopieren</summary>

```bash
# Ordner finden, in dem VCC seine Dateien abgelegt hat (steht in der Einstellungsdatei von VCC)
source ~/.vesktopCustomCommands/.config
VDIR="${vencord_path/#\~/$HOME}"; VDIR="${VDIR%/}"
ZIEL="$VDIR/vesktopCustomCommands"

if ! grep -qs '\[VesktopCustomCommands\]' "$VDIR/vencordDesktopMain.js"; then
echo "VCC ist noch nicht eingerichtet. Bitte zuerst den Installer von VCC ausführen."
else

# 1. Den Ladecode von VCC sichern, damit er nach Updates ohne Internet wieder eingefügt werden kann
python3 - "$VDIR/vencordDesktopMain.js" ~/.vesktopCustomCommands/injection.js <<'PY'
import re, sys
src = open(sys.argv[1], encoding="utf-8").read()
m = re.search(r"/\* === VesktopCustomCommands Injection === \*/.*?/\* === End VesktopCustomCommands === \*/", src, re.S)
if not m:
    sys.exit("VCC ist noch nicht eingerichtet. Bitte zuerst den Installer von VCC ausführen.")
open(sys.argv[2], "w", encoding="utf-8").write(m.group(0))
PY

# 2. tasten.js: Stumm und Taub über Discords eigene Funktion statt über den Knopf
cat > "$ZIEL/tasten.js" <<'ENDE'
// tasten.js: schaltet Stumm und Taub über Discords eigene Funktion um.
// Warum: VCC sucht den Mute-Knopf über ein Merkmal (__lottie_element_5), das sich je nachdem ändert,
// was Discord beim Start zuerst zeichnet. Dann findet VCC den Knopf nicht und die Taste macht nichts.
// Discords eigene Funktionen (toggleSelfMute und toggleSelfDeaf) gibt es dagegen immer.
// Klappt das ausnahmsweise nicht, wird wie vorher der Knopf geklickt.
const { BrowserWindow } = require('electron');

module.exports = async (funktion, ersatz) => {
  for (const win of BrowserWindow.getAllWindows()) {
    try {
      const ok = await win.webContents.executeJavaScript(`(function () {
        try {
          const W = window.Vencord && window.Vencord.Webpack;
          const a = W && W.findByProps && W.findByProps("${funktion}");
          if (a && typeof a["${funktion}"] === "function") { a["${funktion}"](); return true; }
        } catch (e) {}
        return false;
      })()`);
      if (ok) { console.log('[Tasten]', funktion, 'ausgeführt'); return; }
    } catch (e) {}
  }
  console.warn('[Tasten]', funktion, 'nicht verfügbar, klicke stattdessen den Knopf');
  ersatz();
};
ENDE

# 3. autoUnmute.js: beim Start von Vesktop entmuten
cat > "$ZIEL/autoUnmute.js" <<'ENDE'
// autoUnmute.js: hebt beim Start von Vesktop die Stummschaltung auf.
// Discord merkt sich auch über einen Neustart hinweg, ob man stumm war. Dieses Skript schaltet
// nicht einfach blind um, sondern fragt vorher nach, ob man wirklich stumm ist.
// Es greift nur einmal beim Start, danach kann man ganz normal muten.
// Geladen wird es von customCode.js aus vesktopCustomCommands, also bei jedem Start von Vesktop.
const { app, BrowserWindow } = require('electron');
const LOG = '[AutoUnmute]';

// Dieser Teil läuft im Discord-Fenster selbst:
// 1. Warten, bis Discord geladen und angemeldet ist. Vorher nimmt Discord das Umschalten nicht an.
// 2. Nachsehen, ob man stumm ist. Wenn nicht, ist nichts zu tun.
// 3. Stummschaltung aufheben und prüfen, ob es geklappt hat. Notfalls bis zu 10 Versuche.
const RENDERER_CODE = `(async () => {
  const warte = ms => new Promise(r => setTimeout(r, ms));
  for (let i = 0; i < 90; i++) {
    try {
      const W = window.Vencord && window.Vencord.Webpack;
      const store = W && W.findStore && W.findStore("MediaEngineStore");
      const actions = W && W.findByProps && W.findByProps("toggleSelfMute");
      const nutzer = W && W.findStore && W.findStore("UserStore");
      const angemeldet = nutzer && typeof nutzer.getCurrentUser === "function" && nutzer.getCurrentUser();
      if (store && actions && angemeldet && typeof store.isSelfMute === "function") {
        if (!store.isSelfMute()) return "war nicht stumm, nichts zu tun";
        for (let n = 1; n <= 10; n++) {
          actions.toggleSelfMute();
          await warte(1500);
          if (!store.isSelfMute()) return "Stummschaltung aufgehoben (Versuch " + n + ")";
        }
        return "Entstummen hat nicht geklappt";
      }
    } catch (e) {}
    await warte(1000);
  }
  return "Discord nicht rechtzeitig bereit (Timeout)";
})()`;

// Sobald ein Vesktop-Fenster fertig geladen ist, den Code oben darin ausführen
// und das Ergebnis in die Konsole schreiben (Strg+Umschalt+I in Vesktop).
const hook = (win) => {
  win.webContents.on('did-finish-load', () => {
    win.webContents.executeJavaScript(RENDERER_CODE)
      .then(r => { console.log(LOG, r); win.webContents.executeJavaScript(`console.log("${LOG}", ${JSON.stringify(r)})`).catch(() => {}); })
      .catch(e => console.error(LOG, e.message));
  });
};

app.on('browser-window-created', (_e, win) => hook(win));
BrowserWindow.getAllWindows().forEach(hook);
console.log(LOG, 'aktiv');
ENDE

# 4. Das Skript, das nach Updates alles wieder in Ordnung bringt
cat > ~/.vesktopCustomCommands/vcc-repatch-lokal.sh <<'ENDE'
#!/usr/bin/env bash
# Bringt VCC und meine Ergänzungen nach Updates sofort wieder in Ordnung.
# Wird von vcc-lokal.path gestartet, sobald sich Vencords Hauptdatei oder customCode.js von VCC ändert.
# Ein Vesktop-Update ersetzt die Hauptdatei und entfernt damit den Ladecode von VCC.
# Ein VCC-Update ersetzt customCode.js und entfernt damit meine Ergänzungen.
set -u
source ~/.vesktopCustomCommands/.config
VDIR="${vencord_path/#\~/$HOME}"; VDIR="${VDIR%/}"
MAIN="$VDIR/vencordDesktopMain.js"
CC="$VDIR/vesktopCustomCommands/customCode.js"
INJ="$HOME/.vesktopCustomCommands/injection.js"
geaendert=0

sleep 2   # Vesktop oder VCC schreibt eventuell noch

# Ladecode wieder einfügen, und zwar mit der lokalen Kopie statt mit Code aus dem Internet
if [ -f "$MAIN" ] && [ -f "$INJ" ] && ! grep -q '\[VesktopCustomCommands\]' "$MAIN"; then
  python3 - "$MAIN" "$INJ" <<'PY' && geaendert=1
import sys
main, inj = sys.argv[1], sys.argv[2]
code = open(inj, encoding="utf-8").read().strip()
src = open(main, encoding="utf-8").read()
if "//# sourceURL=" not in src:
    sys.exit(1)
open(main, "w", encoding="utf-8").write(src.replace("//# sourceURL=", code + "//# sourceURL=", 1))
PY
fi

# Meine Ergänzungen in customCode.js eintragen, falls sie fehlen
if [ -f "$CC" ]; then
  if ! grep -q "tasten.js" "$CC"; then
    sed -i -e "s|clickInRenderer(muteButtonSelector);|require('./tasten.js')('toggleSelfMute', () => clickInRenderer(muteButtonSelector));|" \
           -e "s|clickInRenderer(deafenButtonSelector);|require('./tasten.js')('toggleSelfDeaf', () => clickInRenderer(deafenButtonSelector));|" "$CC"
    grep -q "tasten.js" "$CC" && geaendert=1
  fi
  if ! grep -q "autoUnmute.js" "$CC"; then
    echo "try { require(path.join(__dirname, 'autoUnmute.js')); } catch (e) { console.error('[AutoUnmute]', e.message); }" >> "$CC"
    geaendert=1
  fi
fi
[ "$geaendert" = 1 ] || exit 0

# Liegengebliebene Tastendrücke verwerfen, sonst ist man nach dem Neustart sofort stumm
rm -f "$VDIR/vesktopCustomCommands/mute" "$VDIR/vesktopCustomCommands/deafen"

# Vesktop neu starten, damit die Änderung greift, aber nur, wenn es gerade läuft.
# Gesucht wird nur das echte Vesktop, nicht alles mit "vesktop" im Namen (sonst beendet sich das Skript selbst).
laeuft() { pgrep -f '/usr/lib/vesktop/app\.asar' >/dev/null || pgrep -x vesktop >/dev/null; }
if laeuft; then
  pkill -f '/usr/lib/vesktop/app\.asar'; pkill -x vesktop
  for _ in $(seq 1 20); do laeuft || break; sleep 0.5; done
  systemd-run --user --collect --quiet vesktop 2>/dev/null || setsid -f vesktop >/dev/null 2>&1
  notify-send -i discord "Vesktop" "Neu gepatcht und neu gestartet" 2>/dev/null
else
  notify-send -i discord "Vesktop" "Neu gepatcht" 2>/dev/null
fi
exit 0
ENDE
chmod +x ~/.vesktopCustomCommands/vcc-repatch-lokal.sh

# 5. systemd soll auf beide Dateien aufpassen und das Skript sofort bei einer Änderung starten
cat > ~/.config/systemd/user/vcc-lokal.path <<ENDE
[Unit]
Description=Vesktop: auf Updates von Vesktop und VCC achten

[Path]
PathChanged=$VDIR/vencordDesktopMain.js
PathChanged=$ZIEL/customCode.js
Unit=vcc-lokal.service

[Install]
WantedBy=default.target
ENDE

cat > ~/.config/systemd/user/vcc-lokal.service <<'ENDE'
[Unit]
Description=Vesktop: VCC nach Updates neu patchen

[Service]
Type=oneshot
ExecStart=%h/.vesktopCustomCommands/vcc-repatch-lokal.sh
ENDE

# 6. Die eigenen Timer von VCC abschalten (falls an) und meine Lösung einschalten
systemctl --user disable --now vcc-autorepatch.timer vcc-autoupdate.timer 2>/dev/null
systemctl --user daemon-reload
~/.vesktopCustomCommands/vcc-repatch-lokal.sh
systemctl --user enable --now vcc-lokal.path
fi
```
</details>

<details markdown="1">
<summary>Wieder entfernen</summary>

Auch hier vorher `bash` eintippen, dann:

```bash
systemctl --user disable --now vcc-lokal.path
source ~/.vesktopCustomCommands/.config
VDIR="${vencord_path/#\~/$HOME}"; VDIR="${VDIR%/}"
ZIEL="$VDIR/vesktopCustomCommands"
sed -i -e "s|require('./tasten.js')('toggleSelfMute', () => clickInRenderer(muteButtonSelector));|clickInRenderer(muteButtonSelector);|" \
       -e "s|require('./tasten.js')('toggleSelfDeaf', () => clickInRenderer(deafenButtonSelector));|clickInRenderer(deafenButtonSelector);|" \
       -e "/autoUnmute.js/d" "$ZIEL/customCode.js"
rm "$ZIEL/tasten.js" "$ZIEL/autoUnmute.js"
rm ~/.vesktopCustomCommands/vcc-repatch-lokal.sh ~/.vesktopCustomCommands/injection.js
rm ~/.config/systemd/user/vcc-lokal.path ~/.config/systemd/user/vcc-lokal.service
systemctl --user daemon-reload
```

Danach Vesktop neu starten. VCC selbst bleibt dabei installiert.
</details>

## 2. LibreWolf

LibreWolf ist ein Firefox-Fork, der nochmal deutlich mehr Wert auf Datenschutz legt. Bei Firefox gab es ja schon ein paar Kontroversen. Ende Februar 2025 hat Mozilla zum Beispiel zum ersten Mal Nutzungsbedingungen für Firefox eingeführt und gleichzeitig das Versprechen, die Daten der Nutzer niemals zu verkaufen, aus den FAQ gestrichen. Das kam bei vielen nicht so gut an. LibreWolf ist im Grunde einfach Firefox, aber so, wie man sich Firefox wünscht.

Draufgestoßen bin ich, als ich nach Browser-Alternativen gesucht habe. In Sachen Datenschutz geht zum Beispiel der Mullvad Browser noch weiter, aber der ist für den Alltag ein bisschen Overkill. LibreWolf ist da einfach der bessere Kompromiss, und ich benutze ihn für wirklich alles.

Was am Anfang genervt hat: LibreWolf löscht standardmäßig beim Schließen alle Cookies, und auf Seiten, auf denen ich oft bin, musste ich das erstmal von Hand ändern. Bei den meisten Seiten ist mir das egal, weil ich sie zu selten benutze, aber bei YouTube zum Beispiel will ich mich nicht jedes Mal neu anmelden. Das Gute ist, dass man das pro Seite einstellen kann.

Für wen lohnt er sich? Für alle, die sowieso Firefox benutzen und einfach Firefox, aber besser, haben wollen. Der einzige Nachteil, der mir einfällt: Neue Firefox-Versionen kommen bei LibreWolf etwas später an. Gemerkt habe ich davon aber kaum etwas.

## 3. espanso

espanso ist ein Text-Expander, der systemweit funktioniert: Man legt Kürzel fest, und wenn man die irgendwo eintippt, werden sie durch einen Text ersetzt. Wenn ich zum Beispiel `:mail` eintippe, steht da sofort `impressum@skorp-io.de`.

![Meine espanso-Datei mit den Kürzeln :mail und :username](espanso.webp){: w="1280" h="1388" }
_So sieht die Datei mit den Kürzeln aus. Meine eigenen stehen ganz unten._

<p><video class="embed-video file" style="aspect-ratio: auto;" src="/assets/img/posts/lieblings-apps/espanso-date.mp4" autoplay loop muted playsinline></video><em>Hier mit dem Kürzel :date. Vertippt (:dase) oder mit falschem Zeichen davor (#date) passiert nichts.</em></p>

Eine klassische Autovervollständigung ist das aber nicht. Bei einer Autovervollständigung tippt man den Anfang eines Wortes und bekommt den Rest vorgeschlagen. Bei espanso ist das Kürzel etwas völlig anderes als der Text, der rauskommt. Man kann damit auch ganze Vorlagen ablegen: Wenn man für viele E-Mails immer dieselbe Vorlage braucht, tippt man einfach `:vorlage1`, und die Vorlage steht sofort da.

Ein Tipp: Nehmt Kürzel, die man nicht aus Versehen eintippt. Ich würde zum Beispiel nicht einfach `ad` als Kürzel nehmen. Sobald man das am Anfang eines Wortes oder Satzes tippt, zum Beispiel im Chat in einem Game, kommt sofort die Ersetzung, und das wird schnell nervig. Deshalb fangen meine Kürzel alle mit einem Doppelpunkt an.

### Mein Problem mit espanso und wie ich es gelöst habe

Unter CachyOS (und generell unter Linux) wird eine Maus mit vielen Zusatztasten oft zusätzlich als eigene Tastatur erkannt. Man hat also die normale Tastatur, die „Tastatur“ der Maus und die Maus selbst als Geräte.

Wenn espanso beim Systemstart automatisch startet, klappt das meistens. CachyOS startet aber sehr schnell. Das klingt erstmal komisch, weil ein schneller Start ja eigentlich gut ist. Nur kann das System so schnell starten, dass zum Beispiel Eingabegeräte noch gar nicht richtig angemeldet sind und noch ein, zwei Sekunden brauchen. Das ist nicht viel, reicht aber schon, um espanso durcheinanderzubringen. Bei mir hat der Autostart deshalb mal funktioniert und mal nicht.

Gemerkt habe ich das, als ich testweise die zweite Tastatur, also die der Maus, deaktiviert habe. Dann hat espanso zuverlässig funktioniert. So wollte ich es aber natürlich nicht für immer lassen. Das Problem war: Die Maus-Tastatur wurde etwas später angemeldet als die richtige Tastatur, und espanso war dann manchmal verwirrt, auf welche Tastatur es hören soll. Dann hat es einfach gar nicht mehr reagiert.

Mein Fix: espanso startet erst, wenn die Desktop-Sitzung steht und sich die Liste der Eingabegeräte ein paar Sekunden lang nicht mehr ändert, frühestens aber 30 Sekunden nach dem Anmelden. Und wenn später ein Gerät dazukommt oder sich neu verbindet, wird espanso automatisch neu gestartet. Das Ganze ist nicht an eine bestimmte Maus oder Tastatur gebunden.

Wenn ihr dasselbe Problem habt, könnt ihr meine Lösung einfach übernehmen. Ihr müsst dafür nicht verstehen, wie sie funktioniert. Wer es genauer wissen will, findet in den Skripten Kommentare, die jeden Schritt erklären.

**Voraussetzungen:** KDE Plasma unter Wayland, so wie bei mir, und espanso läuft als Dienst. Falls ihr espanso noch nicht als Dienst eingerichtet habt, macht das vorher einmal mit `espanso service register`.

**So geht's:**

1. Terminal öffnen.
2. `bash` eintippen und Enter drücken. CachyOS benutzt standardmäßig die Shell fish, und die versteht die folgenden Befehle nicht.
3. Den ganzen Block unten kopieren, ins Terminal einfügen und Enter drücken.
4. `exit` eintippen. Fertig, ab dem nächsten Start wartet espanso, bis alles bereit ist.

<details markdown="1">
<summary>Der Block zum Kopieren</summary>

```bash
mkdir -p ~/.local/bin ~/.config/systemd/user/espanso.service.d

cat > ~/.local/bin/eingabegeraete-warten <<'EOF'
#!/bin/sh
# Wartet beim Anmelden, bis die Plasma-Sitzung steht und sich die Eingabegeräte (Maus, Tastatur …) nicht mehr ändern.
# Für Programme wie espanso, die Tastaturen direkt über /dev/input lesen: Taucht ein Gerät erst nach ihrem Start auf
# (oder verbindet sich neu), bekommen sie es nicht mit. Nicht an ein bestimmtes Gerät gebunden.
#   eingabegeraete-warten [ruhig_sekunden=5] [max_sekunden=90] [mindestens_sekunden_nach_anmeldung=0]
RUHE=${1:-5}; MAX=${2:-90}; MIN=${3:-0}; i=0
# 1. Eigene Plasma-Sitzung (nicht der Anmeldebildschirm)
until pgrep -u "$(id -u)" -x kwin_wayland >/dev/null && systemctl --user is-active --quiet plasma-plasmashell.service \
      && systemctl --user show-environment | grep -q "^WAYLAND_DISPLAY="; do
  sleep 1; i=$((i+1)); [ $i -ge $MAX ] && break
done
# 1b. Mindestens MIN Sekunden nach dem Start der Plasma-Sitzung abwarten
if [ "$MIN" -gt 0 ]; then
  seit=$(systemctl --user show plasma-plasmashell.service -p ActiveEnterTimestampMonotonic --value 2>/dev/null)
  jetzt=$(cut -d' ' -f1 /proc/uptime | cut -d. -f1)
  if [ -n "$seit" ] && [ "$seit" -gt 0 ] 2>/dev/null; then
    rest=$(( MIN - (jetzt - seit / 1000000) ))
    [ "$rest" -gt 0 ] && [ "$rest" -le "$MIN" ] && sleep "$rest"
  fi
fi
# 2. udev hat alle angesteckten Geräte fertig eingerichtet
udevadm settle --timeout=30 2>/dev/null
# 3. Die Liste der Eingabegeräte bleibt RUHE Sekunden lang gleich
alt=""; ruhig=0
while [ $i -lt $MAX ]; do
  neu=$(ls /dev/input/by-id /dev/input/by-path 2>/dev/null | sort | md5sum)
  if [ "$neu" = "$alt" ]; then ruhig=$((ruhig+1)); else ruhig=0; alt=$neu; fi
  [ $ruhig -ge "$RUHE" ] && break
  sleep 1; i=$((i+1))
done
# Stand merken, „eingabegeraete-neu“ vergleicht später damit
for d in /sys/class/input/event*; do
  case "$(readlink -f "$d")" in */devices/virtual/*) continue ;; esac
  echo "${d##*/} $(cat "$d/device/name" 2>/dev/null)"
done | sort > "${XDG_RUNTIME_DIR:-/tmp}/eingabegeraete.liste"
exit 0
EOF

cat > ~/.local/bin/eingabegeraete-neu <<'EOF'
#!/bin/sh
# Wird von „eingabegeraete-neu.path“ aufgerufen, sobald sich unter /dev/input etwas tut.
# Hat sich die Liste der ECHTEN Eingabegeräte geändert (keine virtuellen wie espansos eigenes),
# werden Programme neu gestartet, die Tastaturen direkt lesen. Nicht an ein bestimmtes Gerät gebunden.
DIENSTE="espanso"   # weitere Dienste mit Leerzeichen dahinter
STAND="${XDG_RUNTIME_DIR:-/tmp}/eingabegeraete.liste"
liste() {
  for d in /sys/class/input/event*; do
    case "$(readlink -f "$d")" in */devices/virtual/*) continue ;; esac
    echo "${d##*/} $(cat "$d/device/name" 2>/dev/null)"
  done | sort
}
sleep 1; udevadm settle --timeout=10 2>/dev/null
# warten, bis sich nichts mehr tut (Geräte melden sich oft in mehreren Teilen an)
alt=""; ruhig=0; i=0
while [ $i -lt 20 ]; do
  neu=$(liste)
  if [ "$neu" = "$alt" ]; then ruhig=$((ruhig+1)); else ruhig=0; alt=$neu; fi
  [ $ruhig -ge 3 ] && break
  sleep 1; i=$((i+1))
done
if [ -f "$STAND" ] && [ "$neu" = "$(cat "$STAND")" ]; then exit 0; fi
echo "$neu" > "$STAND"
for d in $DIENSTE; do
  if systemctl --user is-active --quiet "$d"; then
    systemctl --user restart --no-block "$d"
  fi
done
EOF

# espanso wartet beim Start (5 s Ruhe, höchstens 90 s, frühestens 30 s nach dem Anmelden)
cat > ~/.config/systemd/user/espanso.service.d/override.conf <<'EOF'
[Service]
TimeoutStartSec=0
ExecStartPre=%h/.local/bin/eingabegeraete-warten 5 90 30
EOF

# beobachtet /dev/input und startet espanso neu, wenn Geräte dazukommen oder wegfallen
cat > ~/.config/systemd/user/eingabegeraete-neu.path <<'EOF'
[Unit]
Description=Eingabegeräte beobachten (espanso bei neuen Geräten neu starten)

[Path]
PathChanged=/dev/input
Unit=eingabegeraete-neu.service

[Install]
WantedBy=default.target
EOF

cat > ~/.config/systemd/user/eingabegeraete-neu.service <<'EOF'
[Unit]
Description=Eingabegeräte geändert, betroffene Dienste neu starten
StartLimitIntervalSec=0

[Service]
Type=oneshot
ExecStart=%h/.local/bin/eingabegeraete-neu
EOF

chmod +x ~/.local/bin/eingabegeraete-warten ~/.local/bin/eingabegeraete-neu
systemctl --user daemon-reload
systemctl --user enable --now eingabegeraete-neu.path
systemctl --user restart espanso
```
</details>

<details markdown="1">
<summary>Wieder entfernen</summary>

Auch hier vorher `bash` eintippen, dann:

```bash
systemctl --user disable --now eingabegeraete-neu.path
rm ~/.local/bin/eingabegeraete-warten ~/.local/bin/eingabegeraete-neu
rm ~/.config/systemd/user/eingabegeraete-neu.path ~/.config/systemd/user/eingabegeraete-neu.service
rm -r ~/.config/systemd/user/espanso.service.d
systemctl --user daemon-reload
systemctl --user restart espanso
```
</details>

## 4. Snowhush

Wenn man Wallpaper Engine kennt, weiß man ungefähr, was Snowhush macht. Nur viel simpler und mit viel weniger Ressourcen. Es lässt Schnee über den Desktop fallen, und der Schnee bleibt auf Fenstern und auf der Taskleiste liegen. Wenn man mit der Maus durch den Schnee fährt, wirbelt er auf. Statt Schnee kann man auch Herbstlaub, Regen oder sogar Kirschblüten einstellen, und die Intensität lässt sich ebenfalls anpassen. Also eigentlich kein Hintergrund, sondern eher ein animierter Vordergrund, mit dem man interagieren kann.

Draufgestoßen bin ich über TikTok und hab dann gezielt danach gesucht. Zuerst hatte ich XSnow gefunden, aber das ist schon sehr alt und funktioniert leider nicht so gut wie Snowhush. Man sollte dazu wissen, dass bei der Entwicklung von Snowhush KI mit im Spiel war: Claude steht bei den Contributors. Nur, damit man das im Hinterkopf hat.

![Steam-Fenster mit Schnee obendrauf und verschneiter Taskleiste](snowhush.webp){: w="1770" h="1185" }
_Der Schnee liegt auf dem Steam-Fenster und auf der Taskleiste. Wie schön das in Bewegung aussieht, kann ein Screenshot leider nicht zeigen._

![Nahaufnahme: Schnee auf der oberen Kante des Steam-Fensters](snowhush-fenster.webp){: w="1020" h="170" }
_Nahaufnahme vom Fenster …_

![Nahaufnahme: Schnee auf der Taskleiste](snowhush-taskleiste.webp){: w="1080" h="110" }
_… und von der Taskleiste._

Was mich nervt oder was fehlt? Ehrlich gesagt gar nichts. Es macht alles, was es soll, und sieht gut aus. Lohnt sich für alle, die Wert auf einen schönen Bildschirm legen. Wenn es euch nicht juckt, braucht ihr es auch nicht, es ist halt eine kleine Spielerei. Aber vor allem jetzt im Winter sieht es einfach richtig cool aus.

## 5. Shelly

Shelly ist ein Paketmanager mit grafischer Oberfläche. Damit kann man normale Pakete (pacman), Pakete aus dem AUR, Flatpaks und sogar AppImages installieren. Unter Arch macht man ja normalerweise viel übers Terminal, aber mit Shelly kriegt das wirklich jeder hin. Es gibt auch ein paar praktische Extras, zum Beispiel kann man auf Knopfdruck die Mirrors prüfen lassen.

![Shelly mit den empfohlenen Paketen](shelly.webp){: w="1781" h="901" }

Draufgestoßen bin ich über CachyOS selbst, Shelly ist dort seit diesem Jahr der Standard-Paketmanager. Gerade hat CachyOS aber auch eine neue Version von seinem eigenen Package Installer angekündigt (2.0, mit eigenen Bereichen für Repos, AUR und Flatpaks und mehreren Farbthemen). Wie die beiden zusammenhängen, ist noch nicht ganz klar. Ich bleibe erstmal bei Shelly, aber vielleicht wechsle ich irgendwann.

Im Alltag installiere und deinstalliere ich damit Programme. Beim Deinstallieren nervt es manchmal, dass die Suche bei den installierten Paketen nicht so gut funktioniert und man oft den genauen Namen kennen muss. Was mich auch nervt: Man muss sein Passwort öfter eingeben. Einmal verstehe ich ja, aber wenn man gerade mehrere Sachen hintereinander macht, ist es lowkey nervig, jedes Mal dasselbe Passwort einzugeben.

Für wen lohnt es sich? Für Leute, die sich auskennen und lieber alles übers Terminal machen, natürlich nicht. Für legit alle anderen, vor allem für Neueinsteiger, auf jeden Fall.

## 6. EasyEffects

Am ehesten ist EasyEffects mit Voicemeeter Banana unter Windows zu vergleichen. Man kann damit sowohl sein Headset als auch sein Mikrofon komplett einstellen, mit Equalizer, Effekten und so weiter, damit sich alles einfach besser anhört.

Ob es schon mitgekommen ist oder ob ich danach gesucht habe, weiß ich nicht mehr genau. Auf jeden Fall ist es mit das bekannteste Programm für sowas, es sieht modern aus und hat moderne Plugins. Bei mir läuft es immer, mit meinem Mikro und meinem Headset eingestellt. Für Kopfhörer und Headsets gibt es mit [AutoEq](https://autoeq.app) übrigens eine Seite, auf der man die passenden Einstellungen für sein Modell findet und direkt als Preset für EasyEffects herunterladen kann.

Richtig gut sind Plugins wie DeepFilterNet. Das ist ein KI-Plugin, das die Stimme erkennt und Hintergrundgeräusche wie Tastatur oder Lüfter rausfiltert. Damit kann man sein Mikrofon wirklich krass verbessern.

![EasyEffects mit meiner Mikrofon-Kette und DeepFilterNet](easyeffects.webp){: w="1275" h="1384" }
_Meine Mikrofon-Kette: Filter, DeepFilterNet (Deep Noise Remover), Expander, Equalizer, Kompressor, Deesser und Limiter._

Eine Sache sollte man wissen: „Easy Effects Sink“ und „Easy Effects Source“ sollte man nicht als Standardgerät einstellen, davon rät auch der Entwickler selbst ab. Als Standard bleiben einfach euer Headset und euer Mikrofon eingestellt, EasyEffects leitet den Ton dann im Hintergrund selbst durch seine Effekte.

Für wen lohnt es sich? Ehrlich gesagt für alle. Außer man hat schon ein anderes Audio-Programm, ein Audio-Interface mit eigener Software oder ein Mischpult, das das übernimmt.

## 7. Variety

Variety kümmert sich um den Bildschirmhintergrund. Es kann Bilder aus verschiedenen Quellen holen, zum Beispiel jeden Tag das NASA-Bild des Tages oder Bilder von Flickr. Man kann aber auch einfach einen eigenen Ordner auswählen. Wie oft gewechselt wird, also alle fünf Minuten, bei jedem Start oder wann auch immer, kann man selbst festlegen.

Variety läuft ganz unauffällig im Hintergrund und wechselt halt ab und zu das Bild, viel mehr passiert da nicht. Gestört hat mich daran noch nie etwas. Es macht genau das, wofür es gedacht ist. Lohnt sich für alle, die einen coolen, abwechslungsreichen Hintergrund wollen, ohne dafür viel Leistung zu opfern.

## 8. ZapZap

ZapZap ist ein WhatsApp-Client, den ich unter CachyOS benutze. Viel gibt es dazu nicht zu erzählen: Wenn man den WhatsApp-Client unter Windows schon benutzt hat, benutzt man ZapZap genauso. Im Grunde baut es auf WhatsApp Web auf, kann also alles, was WhatsApp Web kann, und es funktioniert einfach.

## 9. Mission Center

Mission Center ist im Prinzip der Task-Manager von Windows, nur für Linux, und ich finde die GUI viel, viel schöner. Es braucht selbst kaum CPU und bei mir ungefähr 160 MB Arbeitsspeicher. Viel mehr gibt es dazu nicht zu sagen: Es macht das, was der Task-Manager macht, nur eben hübscher.

![Mission Center mit der Prozessor-Ansicht](mission-center.webp){: w="1584" h="977" }

## 10. Prism Launcher

Prism Launcher ist ein Minecraft-Launcher, mit dem man wirklich alles machen kann. Man kann Welten, Ressourcenpakete und so weiter importieren, und er ist mit allem kompatibel: Modrinth, CurseForge, Feed the Beast, Forge, Fabric, völlig egal. Sogar einen Proxy kann man für die Verbindung einstellen.

![Prism Launcher mit meinen Instanzen](prism-launcher.webp){: w="1236" h="825" }

Und selbst wenn mal Mods fehlen, was relativ selten passiert, bekommt man direkt eine Liste mit den fehlenden Mods und den Download-Links. Die öffnet man kurz, lädt die Mods in seinen Download-Ordner, und wenn Prism Zugriff auf den Ordner hat, importiert er sie komplett automatisch. Das ist einfach genial. Man kann außerdem Befehle einstellen, die vor dem Start oder nach dem Schließen von Minecraft laufen sollen.

## Fazit

Was ich mit dem Post eigentlich sagen will: Seid ein bisschen offener, bei neuen Programmen genauso wie bei einem neuen Betriebssystem. Klar, unter CachyOS dauert es am Anfang manchmal etwas länger, bis alles so läuft, wie man will, als unter Windows mal eben kurz was zu installieren. Dafür gibt es voll viele Programme kostenlos, Open Source und sehr customizable, und niemand drückt euch Werbung rein.

Und das Beste: Wenn man es einmal eingerichtet hat, hat man es meistens für immer. Die Einstellungen kann man exportieren, kurz in einen Backup-Ordner packen, und dann muss man sich darüber keine Gedanken mehr machen.

Und eine Sache noch: Macht euch bewusst, wie scheiße Windows mittlerweile ist. Und dann macht euch bewusst, dass ihr was dagegen machen könnt.
