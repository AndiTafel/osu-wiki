<!--This needs a thorough review besides the changes from the wiki status page. There are several grammar and styling issues present (e.g. formal form).-->

# Storyboard Scripting - allgemeine Regeln

![Ein Bespiel eines Skriptes im .osb.](img/SBS_Base.jpg "Ein Bespiel eines Skriptes im .osb.")

Diese Anleitung beschreibt die Zeilen des geskripteten Codes in einer .osb oder .osu Datei unter dem `[Events]`. Die Befehle in der .osb Datei wird von allen vorhandenen Schwierigkeitsstufen in einer Beatmap verwendet, während die die nur in der .osu Datei enthalten sind, nur in der gegebenen Schwierigkeitsstufe erscheinen.

## Grundregeln

### Objekte

::: alert-note
**Anmerkung:** Für Objekte in [osu!](/wiki/Game_mode/osu!) und im [Beatmapping](/wiki/Beatmapping), siehe [Hit-Objekte](/wiki/Gameplay/Hit_object)
:::

Ein [Storyboard-Objekt](/wiki/Storyboard/Scripting/Objects) ist eine Instanz eines Sprites oder eine Animation in einem Storyboard. Es können auch Töne im Storyboard eingesetzt werden, siehe den [Audio](/wiki/Storyboard/Scripting/Audio)-Leitfaden für mehr Details.

Nur PNG und JPEG werden als Bildformate für Objekte akzeptiert.	JPEGs sind verlustreich, was bedeutet, dass sie 'ne kleinere Dateigröße haben, die Pixel werden jedoch nicht exakt wie bei der Eingabe positioniert bleiben. Es wird auch keine Transparenz unterstützt. Deswegen sind sie eher als Hintergrundbilder oder für quadratische oder photo-realistische Bilder geeignet. PNGs sind verlustlos, was bedeuet, dass alle Pixel ihre exakten Positionen beibehalten, was zu einer größeren Dateigröße im Vergleich zum JPEG führt. Transparenz wird unterstützt, was sich daher am besten für Objekte/Text auf der Foreground-Ebene eignet.

Die Animationen werden dann zu einer Engine zusammengefasst, d.h. PNG-Ebenen oder Animationensfunktionen sollten nicht verwendet werden. Speichere stattdessen jedes Frame als einzelne Datei ab und füge eine Zahl zum Dateinamen hinzu, wie z. B. "sample0.png" und "sample1.png" für die 2-Frame-Animation "sample.png").

### Auflösungen

![Editor screen size. Green is screen size and Red is play area](img/SBS_SS.jpg "Editor screen size. Green is screen size and Red is playarea")

Der Editor ist 640 x 480 Pixel groß und der allgemeine Spielbereich ist 510 x 385 Pixel groß).

Koordinaten sind spezifiziert durch positive `X`-Werte nach rechts, positive `Y`-Werte nach unten mit dem Ursprung (0,0) in der oberen linken Ecke vom Bildschirm. Es ist auch möglich, Koordinaten außerhalb dieses Bereichs zu verwenden (z. B. für Sprites, die sich von außerhalb des Bildschirms ins Bild bewegen).

**Editor Koordinaten:**

| Bildfläche | x | y |
| :-: | :-: | :-: |
| Editor | 0 – 640 | 0 – 480 |
| Spielbereich | 60 – 570 | 55 – 440 |

### Ebenen

Mit Ausnahme des Overlays (welches unterhalb des Skins platziert wird) werden alle Storyboard Sprites unterhalb der [Hit Objekte](/wiki/Gameplay/Hit_object) platziert. Daher ist selbst die "höchste" (Overlay) Ebene im Storyboard immer noch über den Hit-Objekten, aber unter der Lebensbalken, dem Cursor, etc.

Es gibt fünf Storyboard-Ebenen, die in aufsteigender Reihenfolge nach Priorität:

- Background
- Fail (erscheint nur, wenn der Spieler im "Fail-Status" ist, siehe [Spielstatus](#spielstatus) weiter unten)
- Pass (erscheint nur, wenn der Spieler im "Pass-Status" ist, siehe [Spielstatus](#spielstatus) weiter unten)
- Foreground
- Overlay (wird oberhalb der Hit-Objekte angezeigt, mit Vorsicht zu verwenden)

Beachte, dass die "Fail"- und "Pass"-Ebenen nie gleichzeitig angezeigt werden, außer im Tab "Design".

Standardmäßig wird das Vorschau-Hintergrundbild (das Hintergrundbild, welches Sie in der [Songauswahl](/wiki/Client/Interface#songauswahl) sehen können) der Map unter allen anderen Ebenen platziert. Wird dassselbe Bild als Objekt im Storyboard verwendet, wird es augenblicklich nach dem Laden der Map verschwinden. Es ist üblich, das Vorschau-Hintergrundbild der Beatmap als erstes Objekt (zeitlich und in Bezug auf die Sprites) festzulegen und den "fade out"-Befehl (aufhellen) zu verwenden, um dem Publikum den Hintergrund "vorzustellen".

#### Regeln zum Thema Überlappung

- Objekte, die sich in **verschiedenen** Ebenen überlappen, werden in der oben angegebenen Reihenfolge gezeichnet (z. B. wird ein Objekt der Foreground-Ebene immer über den Objekten der Background-, Fail- und Pass-Ebenen angezeigt).
- Objekte, die sich auf **derselben** Ebene überlappen, werden in der Reihenfolge, in der sie spezifieziert werden (z. B. wenn Objekt-1 als erstes in der .osb oder .osu Datei spezifiziert wird und Objekt-2 danach, erscheinen beide Elemente in der selben Ebene und Objekt-2 wird über Objekt-1 gelegen), gezeichnet.
- Befehle aus der .osb Datei haben Vorrang gegenüber den aus der .osu Datei innerhalb der Ebenen, so als wären die Befehle aus der .osb Datei an das Ende der Befehle in der .osu Datei angehängt worden. Die Prioritäten der vier Ebenen werden jedoch dadurch nicht außer Kraft gesetzt. [Siehe dieses Beispiel](https://osu.ppy.sh/community/forums/topics/1869?start=469997).

### Spielstatus

Die Idee dahinter, weshalb ein Storyboard anstatt eines Videos benutzt werden sollte, ist **die Möglichkeit, die Elemente dynamisch an das Gameplay anzupassen**. osu! zeigt entweder nur die Pass oder die Fail Ebene an, was von der Performance des Spielers abhängt. Diese Status werden daher auch als "Fail-Status" und als "Pass-Status" bezeichnet.

Der Status **vor der ersten spielbaren Sequenz** (z. B. bevor der erste [Circle/Slider/Spinner](/wiki/Gameplay/Hit_object) erscheint):

- ist immer der Pass-Status. Die Fail Ebene wird daher nie angezeigt. Es wird empfohlen weder die Pass Ebene, noch die Fail Ebene an diesem Punkt der Beatmap zu verwenden, da man nicht wirklich vom "passen" sprechen kann.

Der Status **während der Spielzeit** ("Drain-Zeit", während der der Spieler auf Objekte klicken muss, um zu verhindern, dass sich die Lebensleiste leert):

- Pass-Status, wenn der erste farbige Combo mit einem Geki/Elite Beat! (nur 300er in dem farbige Comboyeah).
- Ansonsten Fail-Status. Beachte, dass es keinen Status für Katu/Beat! gibt, nicht so wie in den DS Spielen (indem es drei Status gab).
  - In [osu!taiko](/wiki/Game_mode/osu!taiko) entsteht der Fail-Status, wenn man beim letzten Hit Objekt gescheitert ist , ansonsten der Pass-Status.
  - In [osu!catch](/wiki/Game_mode/osu!catch) wird der Status von der vorher spielbaren Sequenz übernommen.

Der Status **während den Pausen** (zwischen dem gespielten Sequenzen):

- Pass-Status, wenn der Lebensbalken von der letzten spielbaren Sequenz mehr als die Hälfte beträgt (wenn z. B. das Symbol "O" erscheint).
- Ansonsten Fail-Status (wenn z. B. das "X" erscheint).
  - Kommt in [osu!taiko](/wiki/Game_mode/osu!taiko) zum Einsatz, wenn eine gewisse Quote bis zu einer bestimmten Zeit nicht erreicht wurde. Siehe die beiden folgenden Beispiele:
    - Beispiel A: Mit einer Genauigkeit von 96,5 %, während die Lebensleiste nur zu 40 % gefüllt ist, zeigt den Pass-Status anstatt des Fail-Status.
	- Beispiel B: Zu viele 100er in etwa 100 Noten oder ein D, während die Lebensleiste noch bei etwa 30 % ist, zeigt den Fail-Status anstatt des Pass-Status (in diesem Fall, siehe [ZUN - Maiden's Cappricio ~ Dream Battle](https://osu.ppy.sh/beatmapsets/18005#taiko/69556)).

Der Status nach der letzten spielbaren Sequenz, wenn die Beatmap mindestens eine Pause hatte:

- Pass-Status, wenn mindestens die Hälfte aller Pausen im Pass-Status waren.
- Ansonsten Fail-Status.

Der Status nach der letzten spielbaren Sequenz, wenn die Map keine Pausen hatte:

- Verhält sich wie während den Pausen.

### Zeit

![Benutzen Sie STRG+C, um den Zeitpunkt zu kopieren.](img/SBS_Time.jpg "Benutzen Sie STRG+C, um den Zeitpunkt zu kopieren.")

- Die Zeit wir in Millisekunden gemessen (1000 ms = 1 second) ab dem Start der Audiodatei (`.mp3`/`.ogg`), negative Werte für Intros sind auch möglich.
- Die Zeit im SB hängt nicht vom Zeitpunkt der Beatmap selbst ab (z. B. wie viele BPMs vorhanden sind). Daher wird empfohlen, dass die Beatmap einigermaßen gut zeitlich angepasst sein sollte, bevor am Storyboard gearbeitet wird, da es sonst schwieriger wird, diese Zeiten später richtig anpassen.
- Die Zeit ist nicht auf die Länge des Liedes eingeschränkt. Es ist möglich, dass negative Werte für Ereignisse, die vor dem Song (Intro) beginnen, und Werte, die über das letzten spielbaren Sektion oder sogar über das Ende der Audiodatei (Outro) hinausgehen, genommen werden können.
- Die Beatmap startet am frühesten Punkt oder bei 0, je nachdem was früher ist.
  - Im ersten Fall wird die `Skip`-Taste für den Benutzer angezeigt. Wenn Sie drauf klicken oder die Leertaste drücken, wird zur Zeit 0 überspringen. Das Spiel kehrt zum normalen Skip-Verhalten vor der Beatmap zurück (z.B. `Skip` erneut drücken, um direkt zum Countdown zu springen — im Gegensatz zu [Elite Beat Agents](https://de.wikipedia.org/wiki/Elite_Beat_Agents), wo das Neustarten einer Beatmap den Spieler nicht zum Zeitpunkt 0, sondern zum Anfäng zurückbringt).
- Das Spiel wird zur [Ergebnisanzeige](/wiki/Client/Interface#ergebnisanzeige) übergehen, sobald das letzte Ereignis eingetreten ist oder der Benutzer auf die Schaltfläche `Skip` klickt oder die Leertaste drückt.
  - Dies enthält Ereignisse, die auf **BEIDEN** Pass/Fail Ebenen, selbst es nur eines geben sollte, angezeigt wird.
    - Beispiel: Wenn das Fail Storyboard bis zur Zeit 20000 läuft und das Storyboard bis zur Zeit 25000 geht, dann wird das Spiel noch bis zur Zeit 25000 weiterlaufen, selbst wenn Sie gerade im Fail-Status sind. Deshalb ist es zu empfehlen beide Storyboard zur selben Zeit enden zu lassen.
  - Ereignisse bleiben weiterbestehen, selbst wenn der Spieler zu den Resultaten überspringen sollte, die Audioeffekte im Storyboard werden trotzdem fortgesetzt und abgespielt.
- Im Tab "Design" des Beatmap-Editors wird die derzeitige Zeit in Millisekunden angezeigt. Drücken Sie `Strg` + `C`, um die Zeit in die Zwischenablage zu kopieren.

## Kommentare

Einzeilge Kommentare im Stil von C können hinzugefügt werden, aber beachte, dass die möglicherweise entfernt werden, wenn die Beatmap über den Editor im Spiel gespeichert wird. Standardmäßig werden einige Kommentare verwendet, um die Befehle in die vier Ebenen zu unterteilen.

`// Dies hier ist ein Kommentar.`

Im Gegensatz zu C/C++/C#/Java können Kommentare nicht in einer Zeile platziert werden, in der schon ein gültiger Befehl vorhanden ist. Blockkommentare sind auch nicht verfügbar.
