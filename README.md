# Spritesheets-Mae-Mochi
Sprite für das Mochi-Widget von Maephira für StreamElements.


## Format
Sprite ist in 400x400 px abschnitten spaced, ohne paddings.
Es sind total 17 Columns und 19 Rows.

## Frames
| Key            | Start | Ende           | loop? | Notizen                                  | Empty Frames   |
|----------------|-------|----------------|-------|------------------------------------------|----------------|
| idle           | 001   | 016            | true  | Basis-Idle                               | 017            |
| follower       | 018   | 033            | false | Follow-Alert                             | 034            |
| subscriber     | 035   | 077            | false | Sub/Resub/Gift                           | 078 - 085      |
| raid           | 086   | 112            | false | Raid                                     | 113 - 119      |
| tip            | 120   | 149            | false | Tip                                      | 150 - 153      |
| cheer          | 154   | 183            | false | Bits/Cheer                               | 184 - 187      |
| cry_start      | 190   | 190            | false | Start der "weinen" Sequenz               |                |
| cry_idle       | 191   | 212            | true  | Weinen-Loop                              |                |
| cry_end        | 213   | 221            | false | Ende der Weinen-Sequenz                  |                |
| pat            | 222   | 238            | false | Pat/Headpat                              |                |
| sleep_enter    | 239   | 242            | false | Einschlafen-Transition                   |                |
| sleep_idle     | 243   | 274            | true  | Schlaf-Loop (immer zwei gleiche frames)  |                |
| sleep_wake     | 275   | 279            | false | Aufwachen-Transition                     | 280            |
| sleep_cry      | 281   | 285            | false | Aufwachen into weinen                    | 286 - 289      |
| dance          | 290   | 314            | false | Dance (auch per Chat-Command !dance)     | 315 - 323      |
| sign-idle      | 324   | 340            | true  | text-Loop                                |                |
| sign-start     | 341   | 342            | false | Start der "text" Sequenz                 | 343 - 357      | 

Falls du weitere Animationen hinzufügst (z. B. neue Alerts), ergänze einfach eine neue Zeile und füge denselben Key in `frameRanges` ein.

## Prioritäten und Queue
- Priorität: raid > subscriber > cheer > follower (tip/pat/dance liegen zwischen sub und cheer).
- Queue-Limit: 10 Einträge. Wenn voll, werden nur follower/cheer verworfen, niemals raid/sub.
- Coalescing: Mehr als 2 Cheers innerhalb von 5 Sekunden -> nur ein Cheer-Event (zählt zusammen).

## Sleep / Cry Logik
- Inaktivität `sleepTimeoutSec`: nach X Sekunden ohne Event -> `sleep_enter` dann `sleep_idle`.
- Wenn `sleep_idle` länger als `cryTimeoutSec`: starte Cry-Sequenz (`cry_start` -> `cry_idle` -> `cry_end`).

## Chat-Command
- `!dance` im Chat triggert die `dance` Animation (falls aktiviert). Nur Textnachrichten-Events werden ausgewertet.

## Konfiguration (StreamElements Felder)
- `assetBaseUrl`: Basis-URL deiner Frames.
- `animationSpeed`: ms pro Frame (8/9/10 fps). Standardmässig 8FPS, 125ms zwischen den frames.
- `sleepTimeoutSec`: Sekunden bis Schlaf.
- `cryTimeoutSec`: Sekunden Schlaf bis Weinen.
- `handoverDelayMs`: Pause nach einer Animation bevor die nächste startet. Alte Animation "auslaufen" lassen.
- `coalesceWindowMs`: Zeitraum für Cheer-Zusammenfassung.
- `coalesceMinCount`: Ab wie vielen Cheers im Zeitraum zusammengefasst wird.
- `maxQueueLength`: Maximale Queue-Länge (Follower/Cheer werden verworfen, nie Sub/Raid).
- `enableChatDance`: Ob `!dance` aus dem Chat reagiert. (standard aktiv)
- `head1`, `skin`: Auswahl der Assets/Overlays.
