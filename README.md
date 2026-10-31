# SSS Seemannschaft 25 Fragen

Trainer für die 25 häufigsten Prüfungsfragen im Fach **Seemannschaft** des
Sportseeschifferscheins. Die Fragen kommen in zufälliger Reihenfolge, die
Antwort wird frei formuliert und gegen feste Schlüsselelemente geprüft.
Danach erscheint immer die Musterlösung.
Falsche Antworten kommen in derselben Runde nochmals, höchstens zweimal.

Die App läuft ohne Server, ohne Anmeldung und nach dem ersten Aufruf auch
ohne Netz. Gestaltung übernommen aus dem SSS-Trainer Seemannschaft.

---

## Auf GitHub veröffentlichen

1. Auf GitHub ein neues Repository anlegen, z. B. `sss-25-fragen`.
2. **Add file → Upload files** wählen und den **Inhalt** dieses Ordners
   hineinziehen: `index.html`, `manifest.webmanifest`, `sw.js`, `README.md`,
   `.nojekyll` und den Ordner `icons`. Dann **Commit changes**.
   Hinweis: `.nojekyll` ist eine versteckte Datei. Auf dem Mac blendet
   **Cmd + Shift + .** sie im Finder ein. Fehlt sie, läuft die App trotzdem.
3. Im Repository **Settings → Pages** öffnen. Unter **Build and deployment**
   als Quelle **Deploy from a branch** wählen, Branch **main**, Ordner
   **/ (root)**, speichern.
4. Nach ein bis zwei Minuten ist die App erreichbar unter
   `https://<benutzername>.github.io/sss-25-fragen/`.

Alle Pfade sind relativ, die App läuft deshalb auch in einem Unterordner.

## Als App installieren

- **iPhone/iPad**: Adresse in Safari öffnen → Teilen → „Zum Home-Bildschirm“.
- **Android**: in Chrome öffnen → Menü ⋮ → „App installieren“.
- **Desktop (Chrome/Edge)**: Installations-Symbol rechts in der Adresszeile.

Nach dem ersten Aufruf liegt alles auf dem Gerät. Die App startet dann auch
im Flugmodus.

## Örtlich ausprobieren

`index.html` doppelklicken. Die Datei ist in sich geschlossen: Stylesheet,
Programm und alle 25 Fragen stecken darin. Installation und Offlinebetrieb
funktionieren erst über eine Webadresse (GitHub Pages oder z. B.
`python3 -m http.server 8080` in diesem Ordner).

---

## Aufbau

```
index.html             Die ganze App: Stylesheet, Programm, Fragen (JSON-Block)
manifest.webmanifest   Angaben für die Installation als App
sw.js                  Offlinebetrieb
icons/                 App-Symbole (192, 512, maskable, Apple)
.nojekyll              GitHub soll die Dateien unverändert ausliefern
```

## Fragen bearbeiten

Die Fragen stehen in `index.html` im Block
`<script id="fragen-daten" type="application/json">`. Je Frage:

```json
{
  "id": "f05", "nummer": 5,
  "frage": "Welche Funktion hat ein Treibanker?",
  "loesung": ["Ein Treibanker reduziert …"],
  "anmerkung": "optional, erscheint unter der Musterlösung",
  "hinweis": "Denken Sie an einen Fallschirm – nur im Wasser.",
  "mindestanzahl": 2,
  "elemente": [
    { "label": "Verringert die Drift", "pflicht": true,
      "treffer": ["drift", "brems", "verlangsamt"] }
  ]
}
```

- **`loesung`**: eine Zeile = Absatz, mehrere Zeilen = Aufzählung.
- **`elemente`**: die Schlüsselelemente. `pflicht: true` = muss vorkommen.
  `verneinend: true`, wenn das Element selbst eine Verneinung enthält
  („kein Tampen in der Schraube“), dann entwertet ein „nicht“ den Treffer nicht.
- **`treffer`**: Wörter oder Wortfolgen, die das Element ausdrücken.
  Gross-/Kleinschreibung, Umlaute und ß spielen keine Rolle.
  - `wort` – unter 4 Zeichen exakt, 4 Zeichen am Wortanfang, 5–6 Zeichen auch
    als Wortteil, ab 7 Zeichen als Wortteil mit Tippfehler-Toleranz
    (7–9 Zeichen 1 Fehler, ab 10 Zeichen 2 Fehler)
  - `=wort` ganzes Wort exakt · `^wort` Wortanfang · `~wort` ganzes Wort mit
    Toleranz · `#100-150` Zahl im Bereich
  - Wortfolgen: Wörter in dieser Reihenfolge, bis zu zwei Wörter dazwischen
- **`mindestanzahl`**: so viele Elemente braucht „Richtig“ (zusätzlich zu allen
  Pflichtelementen). „Teilweise“ ab der Hälfte davon.

Nach einer Änderung in `sw.js` die Zeile `const BESTAND = "sss25-v1";`
hochzählen (`v2`, `v3` …), damit installierte Apps die neue Fassung laden.

## Bewertung und Wiederholung

| Ergebnis  | Bedingung                                                 |
| --------- | --------------------------------------------------------- |
| Richtig   | alle Pflichtelemente und mindestens `mindestanzahl`       |
| Teilweise | mindestens die Hälfte von `mindestanzahl`                 |
| Falsch    | weniger                                                   |

Die Automatik erkennt Begriffe, keine Begründungen. Mit „Doch richtig“ lässt
sich die Bewertung überstimmen.

Falsch, teilweise, gelöst oder übersprungen: Die Frage wird frühestens nach
drei anderen Fragen wieder gestellt, höchstens zweimal. Danach bleibt sie
offen und erscheint im Ergebnis. „Fehler nochmals üben“ startet eine Runde nur
mit diesen Fragen.

## Datengrundlage

„SSS Seemannschaft – 25 häufigsten Fragen“, © SegelnAG. Fragen und
Musterlösungen sind wörtlich übernommen, mit zwei Ausnahmen:

- **Frage 24**: Die Vorlage nennt bei blauen Abgasen „unverbrannter
  Treibstoff“. In der App steht „verbranntes Motoröl“. Die Korrektur ist in
  der App als Anmerkung ausgewiesen.
- **Frage 13**: Die Musterlösung bleibt unverändert (ABC). Eine Anmerkung weist
  darauf hin, dass aktuelle Reanimationsleitlinien bei Erwachsenen mit der
  Herzdruckmassage beginnen.

## Datenschutz

Keine Übertragung, kein Tracking. Gespeichert wird nur die zuletzt gewählte
Rundengrösse im `localStorage` des Browsers.
