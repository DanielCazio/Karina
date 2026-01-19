Bildfragmente & Blendmodes mit p5.js

## Einführung

### Benötigte Vorkenntnisse

Dieses Tutorial baut auf Inhalten auf, die im Kurs bereits behandelt wurden. Hilfreich sind insbesondere:

- **Basics**  
- **Arbeiten mit Bildern**
- **Arrays**
- **Schleifen**

Zur Vertiefung der hier genutzten Blend Modes empfiehlt sich außerdem ein Blick in die offizielle Referenz:
- https://p5js.org/reference/p5/blendMode/

---

### Projektidee

In diesem Projekt erzeugen wir eine experimentelle Bildkomposition aus Fragmenten eines Fotos. Ein zufällig gewähltes Hintergrundbild wird in viele kleine Ausschnitte zerlegt, leicht verschoben und mit unterschiedlichen Blend Modes erneut auf die Leinwand gesetzt.

Das Ergebnis erinnert an eine digitale Collage, bei der das Originalbild sich visuell auflöst, aber noch erkennbar bleibt.

---

### Lernziele

Nach diesem Tutorial ist es möglich:

- mit Offscreen-Canvas (`createGraphics`) zu arbeiten
- Blend Modes gezielt und kreativ einzusetzen
- mehrere Bilder vorzuladen und zufällig auszuwählen
- Bildausschnitte gezielt zu kopieren
- Zufall als gestalterische Variationen zu nutzen

---

## Das Projekt Schritt für Schritt

### Ausschnitt 1: Bild laden und vorbereiten

Ziel dieses Schrittes ist es, ein Bild korrekt zu laden und als Grundlage für die weitere Arbeit zu verwenden. Zunächst arbeiten wir mit einem einzelnen Bild, um den Ablauf möglichst übersichtlich zu halten.

```js
let img;

function preload() {
  img = loadImage('./pics/heidelbeere.png');
}

function setup() {
  createCanvas(img.width, img.height);
  image(img, 0, 0);
}
```

#### Aufgabe

- Erstelle nun ein Array aus mindestens drei Bildern;
- Wähle später ein Bild zufällig aus dem Array aus.

<details>
<summary> Hinweis</summary>
Nutze `loadImage()` und `random()`.
</details>

<details>
<summary> Musterlösung</summary>

```js
let imgs = [];
let img;

function preload() {
  imgs.push(loadImage('./pics/heidelbeere.png'));
  imgs.push(loadImage('./pics/granatapfel.png'));
  imgs.push(loadImage('./pics/zitronen.png'));
}

function setup() {
  img = random(imgs);
  createCanvas(img.width, img.height);
  image(img, 0, 0);
}
```
</details>

> **Hinweis**  
> Dateinamen sollten ausschließlich aus Kleinbuchstaben bestehen. Großbuchstaben oder Sonderzeichen können dazu führen, dass Bilder nicht geladen werden.

---

### Ausschnitt 2: Offscreen-Canvas (MiniCanvas)

In diesem Projekt arbeiten wir nicht nur auf dem Haupt-Canvas, sondern erzeugen mehrere **MiniCanvas**, die einzelne Bildfragmente enthalten. Ein MiniCanvas wird mit `createGraphics()` erstellt und funktioniert wie eine eigene Zeichenfläche im Hintergrund:

```js
let miniCanvas = createGraphics(sw, sh);
```

Nun kopieren wir einen Ausschnitt des Bildes auf den MiniCanvas:

```js
miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw - 30, sh - 30);
```

#### Aufgabe

- Erstelle mehrere MiniCanvas (Diese Fragmente werden im nächsten Schritt visuell weiter verändert);
- Platziere sie versetzt auf dem Haupt-Canvas, sodass das Originalbild als Hintergrund sichtbar bleibt.

<details>
<summary> Hinweis</summary>
Nutze eine `for`-Schleife und zufällige Koordinaten für `sx` und `sy`.
</details>

---

### Ausschnitt 3: Blend Mode

Blend Modes bestimmen, wie Farben übereinander verrechnet werden, wenn sich mehrere Bildebenen überschneiden. In p5.js wird dafür die Funktion `blendMode()` verwendet. Alle verfügbaren Modi sind in der offiziellen Referenz dokumentiert:
https://p5js.org/reference/p5/blendMode/

Blend Modes können sowohl auf das Haupt-Canvas als auch auf ein MiniCanvas angewendet werden. 

#### Aufgabe 

- Erzeuge einen MiniCanvas
- Wende einen festen Blend Mode darauf an (z. B. `MULTIPLY` oder `ADD`)

---

#### Hauptaufgabe

Nun erweitern wir das Prinzip:

- Erstelle ein Array mit mehreren Blend Modes;
- Wähle für jedes Fragment einen zufälligen Blend Mode (z. B. MULTIPLY oder ADD) und wende ihn auf den jeweiligen MiniCanvas an.

<details>
<summary> Hinweis</summary>
Ein Array könnte z. B. `BLEND`, `ADD`, `MULTIPLY`, `EXCLUSION` enthalten.
</details>

<details>
<summary> Musterlösung</summary>

```js
let filter = [BLEND, ADD, DARKEST, EXCLUSION, MULTIPLY, HARD_LIGHT];
miniCanvas.blendMode(random(filter));
```
</details>

---

## 3. Musterlösung 

<details>
<summary> Vollständige Musterlösung</summary>

```js
let img;
let imgs = [];

function preload() {
  imgs.push(loadImage('./pics/heidelbeere.png'));
  imgs.push(loadImage('./pics/granatapfel.png'));
  imgs.push(loadImage('./pics/zitronen.png'));
}

function setup() {
  img = random(imgs);
  createCanvas(img.width, img.height);

  let filter = [BLEND, ADD, DARKEST, EXCLUSION, MULTIPLY, HARD_LIGHT];

  tint(250, 160);
  image(img, 0, 0);
  noTint();

  for (let i = 0; i < 50; i++) {
    let sw = 150;
    let sh = 150;

    let sx = random(0, width - sw);
    let sy = random(0, height - sh);

    let x = sx + random(-10, 10);
    let y = sy + random(-10, 10);

    let miniCanvas = createGraphics(sw, sh);
    miniCanvas.blendMode(random(filter));
    miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw - 30, sh - 30);

    image(miniCanvas, x, y);

    stroke(100);
    noFill();
    rect(x, y, sw, sh);
  }
}
```
</details>

---

## 4. Fazit & Ausblick

In diesem Tutorial wurde gezeigt, wie sich Bilder mithilfe von Offscreen-Canvas und Blend Modes fragmentieren und neu zusammensetzen lassen.

Mögliche Erweiterungen:

- Wechsel der Bilder über Tastatureingaben statt Zufall
- Interaktive Neugenerierung der Fragmente per Mausklick
- Animation der Fragmente im `draw()`-Modus
- Eigene Parameter (Anzahl, Größe, Blend Modes) über Slider steuern

Das Projekt kann als Ausgangspunkt für eigene experimentelle Bildarbeiten dienen und flexibel weiterentwickelt werden.

---
