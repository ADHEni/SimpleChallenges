# Simple Challenges

[English](README.md) | [Deutsch](README.de.md)

Ein Minecraft-Challenge-Plugin für **Paper 26.3**, um gemeinsam mit Freunden zu spielen.

Inspiriert von den Videos von **BastiGHG** wollte ich Minecraft-Challenges selbst mit meinen Freunden spielen. Dafür habe ich dieses Plugin entwickelt und stelle es auch anderen zur Verfügung.

## Challenges

| Modus | So funktioniert es |
| --- | --- |
| **Random Items · Solo** | Sammle zufällige Items vor Ablauf der Zeit. Die meisten Punkte gewinnen. |
| **Random Items · Duo** | Teams mit bis zu zwei Spielern teilen Ziele, Punkte, Skips und einen Rucksack. |
| **Achievement Challenge** | Erreiche innerhalb des Zeitlimits möglichst viele Minecraft-Fortschritte. Rezeptfreischaltungen zählen nicht. |
| **Duo Bingo** | Teams teilen eine Karte mit 20 Aufgaben. Wer die eingestellte Anzahl zuerst erfüllt, gewinnt. Eine Reihe ist nicht nötig; ein Team kann auch allein gespielt werden. |
| **Skyblock Bingo** | Starte auf deiner eigenen Insel und vervollständige eine Reihe, Spalte oder Diagonale auf der gemeinsamen Bingo-Karte. |
| **Mob Battle · Solo — Neu in 1.1.0** | Jage zufällig zugewiesene Mobs. Kills und Skips geben Punkte; die meisten Punkte gewinnen. |
| **Mob Battle · Duo — Neu in 1.1.0** | Teams teilen Ziele, Punkte, Skips und einen Rucksack. Friendly Fire ist aktiviert. |
| **XP Battle — Neu in 1.1.0** | Erreiche das höchste XP-Level vor Ablauf der Zeit. Tod und ausgegebene Level verringern deinen Rekord nicht. |
| **Chunkbound · Co-op — Neu in 1.1.0** | Schaltet Nachbarchunks für 1, 2, 3 … Level frei und besiegt gemeinsam den Enderdrachen. Auch der Nether hat freikaufbare Chunks; Portal-Ankunftschunks sind kostenlos und das End ist normal. |

Random Items, Mob Battle und Achievements laufen standardmäßig **zwei Stunden**. XP Battle hat ein einstellbares Zeitlimit. Bingo und Chunkbound haben kein Zeitlimit.

Alle Modi verwenden separate Welten und unterstützen Pause/Resume über Serverneustarts hinweg. Bei der Rückkehr werden Lobby-Inventar und Spielerzustand wiederhergestellt. Zum Fortsetzen müssen alle ursprünglichen Teilnehmer online sein.

Während der Vorbereitung zeigt eine Bossbar den aktuellen Schritt, die Wartezeit und den Fortschritt, soweit messbar. Random Items und Mob Battle zeigen außerdem Spieler-Richtungsmarkierungen über der Hotbar. Mit `locator-bar: false` unter dem jeweiligen Modus schaltest du sie aus.

## Installation und Einstieg

1. Verwende **Java 25** und einen **Paper-26.3-Server**.
2. Pack `SimpleChallenges-1.1.0.jar` in `plugins/` und starte den Server neu.
3. Öffne mit `/challenge` die Modusauswahl. Zum Starten/Verwalten der Runden brauchst du OP oder `simplechallenges.challenge.admin`.

Das Menü beginnt mit **Competitive** (gegeneinander) und **Cooperative** (zusammen). Unter Competitive findest du **Challenges** mit Random Items, Mob Battle, Achievements und XP Battle sowie **Bingos** mit Duo Bingo und Skyblock Bingo. Unter Cooperative steht **Chunkbound**. Solo und Duo stehen jeweils zusammen; **Back** führt zum vorherigen Menü.

Ersetze beim Upgrade die alte JAR und behalte ihren Datenordner für den automatischen Import. Eigene Berechtigungen verwenden jetzt `simplechallenges.*`.

| Befehl | Funktion                                                                                                                                                            |
| --- |---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `/challenge` | Modusauswahl öffnen.                                                                                                                                                |
| `/challenge start <mode> [seed]` | Starte `randomitems`, `randomitemsduo`, `mobbattle`, `mobbattleduo`, `achievements`, `duobingo`, `skyblock`, `xpbattle` oder `chunkbound`, optional mit Seed. |
| `/challenge pause` / `/challenge resume` | Die aktuelle Runde speichern oder fortsetzen.                                                                                                                       |
| `/challenge stop` | Die aktive Runde beenden und zur Lobby zurückkehren.                                                                                                                |
| `/challenge discard` | Eine pausierte Runde verwerfen.                                                                                                                                     |
| `/bp` | Den Rucksack in Random Items, Mob Battle oder Duo Bingo öffnen.                                                                                                                 |
| `/bingo` | Die Duo-Bingo- oder Skyblock-Karte öffnen.                                                                                                                          |
| `/result` | Am Ende einer Challenge wird damit das Ergebnis aufdecken.                                                                                                          |
| `/result overview [player]` | Ein einzelnes Ergebnis ansehen; bei Duo Bingo wird stattdessen die Teamfarbe verwendet.                                                                             |
| `/skyblock reset` | Den zentralen Chunk der eigenen Insel samt Starttruhe wiederherstellen, einmal pro drei Minuten aktiver Spielzeit. Inventar und Bingo-Fortschritt bleiben erhalten. |

Bei Bingo und Skyblock beendet ein weiteres `/result` nach der letzten Karte die Ergebnisphase und bringt alle zurück zur Lobby.

## Welt-Seeds · Neu in 1.1.0

Gib hinter dem Modusnamen eine Zahl oder Text an, um dieselbe Welt erneut zu spielen oder mit anderen zu teilen:

```text
/challenge start randomitems 123456789
/challenge start chunkbound Unsere gemeinsame Welt
```

Text wird immer in denselben Zahlen-Seed umgewandelt; Großschreibung und Leerzeichen zählen. Ohne Seed bleibt die Welt zufällig. Ziele und Bingo-Aufgaben bleiben random; Skyblock behält seinen Inselaufbau. Für gleiche Landschaften müssen Minecraft-Version und Welteinstellungen übereinstimmen. Das Menü startet zufällige Welten; fortgesetzte Runden behalten ihre bestehende Welt.

## Konfiguration

Die Einstellungen liegen in **`plugins/SimpleChallenges/`**. Bearbeite die YAML-Dateien, starte den Server neu und beginne eine neue Runde, um aktualisierte Aufgabenlisten zu verwenden.

### Allgemeine Einstellungen — `config.yml`

| Abschnitt | Einstellmöglichkeiten |
| --- | --- |
| `challenge` | Standard-Countdown vor Spielbeginn. |
| `world` | Lobby-Welt und Radius zum Vorladen von Chunks. |
| `players` | Inventare zu Beginn leeren; die Lobby-Inventare werden gespeichert. |
| `modes.achievements` | Countdown und Rundendauer. |
| `modes.randomitems` / `modes.randomitemsduo` | Countdown, Dauer, Anzahl der Skips sowie Material und Name der Tokens. |
| `modes.mobbattle` / `modes.mobbattleduo` | Countdown, Dauer und Skip-Tokens. |
| `modes.xpbattle` | Countdown und Rundendauer. |
| `modes.chunkbound` | PvP, Startabstand zum Stronghold und Geschwindigkeit beim Freischalten von Chunks. |
| `modes.duobingo` | Countdown, PvP, Team-Spawn-Abstand sowie Aufgaben, Siegbedingung und Karten-Item unter `bingo`. |
| `modes.skyblock` | Countdown, PvP, Inselabstand, Starttruhe, Angelbeute sowie Karte, Aufgaben und Karten-Item unter `bingo`. |

Beide Bingos beginnen immer mit frischem Inventar. Gemeinsame Optionen und die automatische Übernahme alter Configs: [Bingo-Konfiguration](../docs/BINGO_CONFIG.md).

Zeitangaben sind in **Sekunden**: `7200` entspricht zwei Stunden.

### Bingo-Aufgaben hinzufügen oder bearbeiten

Du kannst Einträge in den Aufgabenlisten hinzufügen, entfernen oder bearbeiten. Passe Anforderungen, `title` und `icon` an. Verwende Material- und Kreaturnamen in Großbuchstaben sowie Fortschrittskennungen wie `minecraft:story/mine_diamond`.

**Duo Bingo:** Füge diese Einträge unter `modes.duobingo.bingo.tasks` ein:

```yaml
- type: OBTAIN
  item: DIAMOND
  amount: 8
  title: "Finde acht Diamanten"
  icon: DIAMOND

- type: KILL
  mob: ZOMBIE
  count: 10
  icon: IRON_SWORD
```

Aufgabentypen: `OBTAIN`, `CRAFT`, `KILL`, `BIOME`, `STRUCTURE_FIND`, `ADVANCEMENT`, `VILLAGER_TRADE`, `BLOCK_PLACE`, `EXPERIENCE`, `DISTANCE_TRAVEL`, `FOOD_EAT`, `ENCHANT_ITEM`, `POTION_BREW`, `ANIMAL_BREED`, `SLEEP_BED`.

Es sollten aber genügend Aufgaben für eine **Karte mit 20 Aufgaben** vorhanden sein, mit höchstens vier Biomen. `bingo.win-count` bestimmt die benötigte Anzahl zum Sieg.

**Skyblock:** Füge Einträge unter `modes.skyblock.bingo.tasks` ein:

```yaml
- type: INVENTORY_HAS
  item: COBBLESTONE
  amount: 64
  title: "Sammle einen Stack Bruchstein"
  icon: COBBLESTONE

- type: FISH
  amount: 5
  icon: FISHING_ROD
```

Aufgabentypen: `INVENTORY_HAS`, `CRAFT_ITEM`, `SMELT_ITEM`, `PLACE_BLOCK`, `KILL_ENTITY`, `KILL_MOBS`, `ADVANCEMENT`, `COBBLE_GEN`, `REACH_HEIGHT`, `BREED_ANIMAL`, `FISH`, `BUILD_GOLEM`, `HARVEST_CROPS`, `GROW_TREES`, `TRADE_COUNT`, `GRASS_SPREAD`, `SHEAR`, `EAT_ITEM`, `SIGN_NAME`, `FALL_SURVIVE`, `ZOMBIE_VILLAGER_CATCH`, `COMPOST_PRODUCE`, `HIT_PLAYER_ARROW`.

Mit `bingo.board-size` legst du die Kartengröße fest. Die Gewinnreihe umfasst immer die gesamte Seitenlänge. Bei `strict-no-duplicates: true` braucht eine 5×5-Karte **25 geeignete Aufgaben**. Verwende `min-players: 2` für Aufgaben, die mehrere Spieler erfordern.

Unter `modes.skyblock.generator.starter-chest.items` verwendest du Einträge wie `LAVA_BUCKET:1` oder `ICE:2`. Angelbeute verwendet `item`, `min`, `max` und eine relative Gewichtung `weight`; `keep-vanilla-percent` bestimmt, wie oft der normale Fang erhalten bleibt. Die Datei `skyblock_template.json` definiert die Insel für neue Runden und Insel-Resets.

### Random-Items-Filter — `blacklist-config.yml`

Der Server liefert den **aktuellen Item-Katalog** automatisch. Beide Random-Items-Modi wenden diese Filter an:

| Abschnitt | Filterwirkung |
| --- | --- |
| `material-blacklist.items` | Schließt exakte Materialnamen wie `NETHER_STAR` aus. |
| `name-filters.patterns` | Schließt Namen aus, die eine Zeichenfolge wie `_SPAWN_EGG` enthalten. Das sind keine Platzhaltermuster. |
| `colored-variants.categories` | Schließt mithilfe von `color-prefixes` farbige Varianten aufgelisteter Kategorien wie `WOOL` aus. |
| `options.debug-export` | Erstellt CSV-Referenzlisten, wenn die Random-Items-Auswahl geladen wird. |
| `options.warn-unknown-materials` | Protokolliert Ausschluss-Einträge, die in der laufenden Version nicht existieren. |

Jede Filtergruppe besitzt einen `enabled`-Schalter. Groß- und Kleinschreibung spielt bei Namen und Mustern keine Rolle. Um ein Item zuzulassen, entferne **alle** passenden Ausschlüsse. Muster treffen auch Teilzeichenfolgen: `LIGHT` schließt beispielsweise `LIGHT_BLUE_WOOL` aus.

Bei aktiviertem CSV-Export schreibt das Plugin:

- **`randomitem_pool.csv`** - erlaubte Ziele mit den Spalten `name`, `isBlock` und `isEdible`.
- **`randomitem_all_items.csv`** - den aktuellen Item-Katalog mit Ausschlussgrund; bei erlaubten Items bleibt dieser leer.

Bearbeite **`blacklist-config.yml`**, nicht die CSVs. Die Exporte werden beim Laden eines Random-Items-Modus neu erstellt. Das Material der Skip-Tokens ist immer von den Zielen ausgeschlossen.

### Mob-Battle-Filter — `mob-blacklist-config.yml`

Schließe Ziele mit `entity-blacklist.entities` oder `name-filters.patterns` aus. `PLAYER` kann auch beim Alleinspielen erscheinen; Mob Battle Duo erlaubt Friendly Fire. Zum Ausschließen trage `PLAYER` in die Blacklist ein.

Mit `options.debug-export: true` erhältst du `mobbattle_pool.csv` und `mobbattle_all_entities.csv` als Referenzlisten. Bearbeite die YAML, nicht die CSVs. Änderungen gelten für neue Runden.
