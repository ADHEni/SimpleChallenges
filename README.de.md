# Simple Challenges

[English](README.md) | [Deutsch](README.de.md)

Ein Minecraft-Challenge-Plugin für **Paper 26.3**, um gemeinsam mit Freunden zu spielen.

Inspiriert von den Videos von **BastiGHG** wollte ich Minecraft-Challenges selbst mit meinen Freunden spielen. Dafür habe ich dieses Plugin entwickelt und stelle es auch anderen zur Verfügung.

## Challenges

| Modus | So funktioniert es                                                                                                                                                                |
| --- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Random Items · Solo** | Sammle zufällig zugewiesene Items, bevor die Zeit abläuft. Erfüllte Ziele geben Punkte. Wer die meisten Punkte hat, gewinnt.                                                      |
| **Random Items · Duo** | Dieselbe Challenge in Teams mit bis zu zwei Spielern. Teammitglieder teilen Ziele, Punkte, Skip-Tokens und einen Rucksack(/bp).                                                   |
| **Achievement Challenge** | Erreiche innerhalb des Zeitlimits möglichst viele Achievements.                                                                                                                   |
| **Duo Bingo** | Teams teilen eine Karte mit 20 Aufgaben. Wer zuerst die konfigurierte Anzahl erfüllt, gewinnt. Ein Team kann auch allein gespielt werden.                                         |
| **Skyblock Bingo** | Jeder Spieler startet auf seiner eigenen Insel in einer leeren Welt. Alle erhalten dieselbe Bingo-Karte. Eine zusammenhängende Reihe, Spalte oder Diagonale entscheidet den Sieg. |

Random Items und Achievements laufen standardmäßig **zwei Stunden**. Duo Bingo und Skyblock haben kein Zeitlimit; der Timer zählt bis einer Gewinnt.

Challenges verwenden separate temporäre Welten. Runden können auch über Serverneustarts hinweg pausiert und fortgesetzt werden. Bei der Rückkehr werden Lobby-Inventar und Spielerzustand wiederhergestellt. Zum Fortsetzen müssen alle ursprünglichen Teilnehmer anwesend sein.

## Installation und Einstieg

1. Verwende **Java 25** und einen **Paper-26.3-Server**.
2. Pack `SimpleChallenges.jar` in `plugins/` und starte den Server neu.
3. Öffne mit `/challenge` die Modusauswahl. Zum Starten/Verwalten der Runden brauchst du OP oder `simplechallenges.challenge.admin`.

Ersetze beim Upgrade die alte JAR und behalte ihren Datenordner für den automatischen Import. Eigene Berechtigungen verwenden jetzt `simplechallenges.*`.

| Befehl | Funktion                                                                                                                                                            |
| --- |---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `/challenge` | Modusauswahl öffnen.                                                                                                                                                |
| `/challenge start <mode>` | `randomitems`, `randomitemsduo`, `achievements`, `duobingo` oder `skyblock` starten.                                                                                |
| `/challenge pause` / `/challenge resume` | Die aktuelle Runde speichern oder fortsetzen.                                                                                                                       |
| `/challenge stop` | Die aktive Runde beenden und zur Lobby zurückkehren.                                                                                                                |
| `/challenge discard` | Eine pausierte Runde verwerfen.                                                                                                                                     |
| `/bp` | Den Rucksack in Random Items oder Duo Bingo öffnen.                                                                                                                 |
| `/bingo` | Die Duo-Bingo- oder Skyblock-Karte öffnen.                                                                                                                          |
| `/result` | Am Ende einer Challenge wird damit das Ergebnis aufdecken.                                                                                                          |
| `/result overview [player]` | Ein einzelnes Ergebnis ansehen; bei Duo Bingo wird stattdessen die Teamfarbe verwendet.                                                                             |
| `/skyblock reset` | Den zentralen Chunk der eigenen Insel samt Starttruhe wiederherstellen, einmal pro drei Minuten aktiver Spielzeit. Inventar und Bingo-Fortschritt bleiben erhalten. |

Bei Bingo und Skyblock beendet ein weiteres `/result` nach der letzten Karte die Ergebnisphase und bringt alle zurück zur Lobby.

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
| `modes.duobingo` | Countdown vor der Bingo-Runde. |
| `duo_bingo` | Aufgabenliste, benötigte Aufgabenanzahl zum Sieg, Kartenmischung, PvP und Abstand zwischen Team-Spawns. |
| `skyblock` | Inselabstand, Inventarleerung, Inhalt der Starttruhe, Angelbeute und Bingo-Einstellungen. |

Zeitangaben sind in **Sekunden**: `7200` entspricht zwei Stunden.

### Bingo-Aufgaben hinzufügen oder bearbeiten

Du kannst Einträge in den Aufgabenlisten hinzufügen, entfernen oder bearbeiten. Passe Anforderungen, `title` und `icon` an. Verwende Material- und Kreaturnamen in Großbuchstaben sowie Fortschrittskennungen wie `minecraft:story/mine_diamond`.

**Duo Bingo:** Füge diese Einträge unter `duo_bingo.tasks` ein:

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

Es sollten aber genügend Aufgaben für eine **Karte mit 20 Aufgaben** vorhanden sein, mit höchstens vier Biomen. `win_count` bestimmt die benötigte Anzahl zum Sieg.

**Skyblock:** Füge Einträge unter `skyblock.bingo.tasks` ein:

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

Mit `board-size` und `in-row-to-win` legst du Kartengröße und Länge der Gewinnreihe fest. Bei `strict-no-duplicates: true` braucht eine 5×5-Karte **25 geeignete Aufgaben**. Verwende `min-players: 2` für Aufgaben, die mehrere Spieler erfordern.

Unter `skyblock.generator.starter-chest.items` verwendest du Einträge wie `LAVA_BUCKET:1` oder `ICE:2`. Angelbeute verwendet `item`, `min`, `max` und eine relative Gewichtung `weight`; `keep-vanilla-percent` bestimmt, wie oft der normale Fang erhalten bleibt. Die Datei `skyblock_template.json` definiert die Insel für neue Runden und Insel-Resets.

### Random-Items-Filter — `blacklist-config.yml`

Der Server liefert den **aktuellen Item-Katalog** automatisch. Beide Random-Items-Modi wenden diese Filter an:

| Abschnitt | Filterwirkung |
| --- | --- |
| `material-blacklist.items` | Schließt exakte Materialnamen wie `NETHER_STAR` aus. |
| `name-filters.patterns` | Schließt Namen aus, die eine Zeichenfolge wie `_SPAWN_EGG` enthalten. Das sind keine Platzhaltermuster. |
| `colored-variants.categories` | Schließt mithilfe von `color-prefixes` farbige Varianten aufgelisteter Kategorien wie `WOOL` aus. |
| `options.debug-export` | Erstellt CSV-Referenzlisten, wenn die Random-Items-Auswahl geladen wird. |
| `options.warn-unknown-materials` | Protokolliert Ausschluss-Einträge, die in der laufenden Version nicht existieren. |

Jede Filtergruppe besitzt einen `enabled`-Schalter. Verwende Namen und Muster in Großbuchstaben. Um ein Item zuzulassen, entferne **alle** passenden Ausschlüsse. Muster treffen auch Teilzeichenfolgen: `LIGHT` schließt beispielsweise `LIGHT_BLUE_WOOL` aus.

Bei aktiviertem CSV-Export schreibt das Plugin:

- **`randomitem_pool.csv`** - erlaubte Ziele mit den Spalten `name`, `isBlock` und `isEdible`.
- **`randomitem_all_items.csv`** - den aktuellen Item-Katalog mit Ausschlussgrund; bei erlaubten Items bleibt dieser leer.

Bearbeite **`blacklist-config.yml`**, nicht die CSVs. Die Exporte werden beim Laden eines Random-Items-Modus neu erstellt. Das Material der Skip-Tokens ist immer von den Zielen ausgeschlossen.
