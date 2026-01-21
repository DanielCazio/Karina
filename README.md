# Bildfragmente & Blend Mode 
*[Projektarbeit WiSe25/26 - Karina Koval]*

## Einführung

<div class="panel panel-default">
    <div class="panel-heading">
        <h4 class="panel-title">Benötigte Vorkenntnisse:</h4>
    </div>
   <div class="panel-body">
<span class="label label-warning">empfohlen</span>
<ul class="list-group">
  <li class="list-group-item list-group-item-warning">
    <a href="./01_Zeichenbefehle.ipynb">Zeichenbefehle</a>
    <a href="./02_Kontur_Fuellung_Farben.ipynb">Kontur/Fuellung/Farben</a>
  </li>
  <li class="list-group-item list-group-item-warning">
    <a href="./03_Basics Datentypen und Variablen.ipynb">Datentypen</a>
    <a href="./04_Basics Bedingte Anweisungen.ipynb">if/else</a>
    <a href="./07_Basics Schleifen.ipynb">for/while</a>
  </li>
  <li class="list-group-item list-group-item-info">
    <a href="../../09_Basics Arrays.ipynb">Arrays</a>
  </li>
</ul>
        </div>
    </div>
</div>

Zur Vertiefung der hier genutzten **Blend Modes** empfiehlt sich außerdem ein Blick in die offizielle Referenz:
- https://p5js.org/reference/p5/blendMode/

### Projektidee

In diesem Projekt erzeugen wir eine experimentelle Bildkomposition aus Fragmenten eines Fotos. Ein zufällig gewähltes Hintergrundbild wird in viele kleine Ausschnitte zerlegt, leicht verschoben und mit unterschiedlichen Blend Modes erneut auf die Leinwand gesetzt.

Das Ergebnis erinnert an eine digitale Collage, bei der das Originalbild sich visuell auflöst, aber noch erkennbar bleibt.

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

> **Hinweis**  
> Dateinamen sollten ausschließlich aus Kleinbuchstaben bestehen. Großbuchstaben oder Sonderzeichen können dazu führen, dass Bilder nicht geladen werden.

<details>
<summary> Musterlösung</summary>

```js
let img;
let imgs = [];

function preload() {
  imgs.push(loadImage('./pics/heidelbeere.png'));
  imgs.push(loadImage('./pics/granatapfel.png'));
  imgs.push(loadImage('./pics/zitronen.png'));
}

function setup() {
// zufälliges Bild für den Hintergrund
  img = random(imgs);
  createCanvas(img.width, img.height);
  image(img, 0, 0);
}
```
</details>


### Ausschnitt 2: Offscreen-Canvas (MiniCanvas)

In diesem Projekt arbeiten wir nicht nur auf dem Haupt-Canvas, sondern erzeugen mehrere **MiniCanvas**, die einzelne Bildfragmente enthalten. Ein MiniCanvas wird mit `createGraphics()` erstellt und funktioniert wie eine eigene Zeichenfläche im Hintergrund:

```js
let miniCanvas = createGraphics(sw, sh);

```

Nun kopieren wir einen Ausschnitt des Bildes auf den MiniCanvas:

```js
miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw, sh);

```

#### Aufgabe

- Erstelle mehrere MiniCanvas (Diese Fragmente werden im nächsten Schritt visuell weiter verändert);
- Platziere sie versetzt auf dem Haupt-Canvas, sodass das Originalbild als Hintergrund sichtbar bleibt.

> **Hinweis**  
> Nutze eine `for`-Schleife für die Erstellung mehrere MiniCanvas.
> `tint()` kann genutzt werden, um den Hintergrund leicht transparent darzustellen, sodass sich die MiniCanvas visuell besser abheben.

```js
let img;
let imgs = []; 

function preload() {
  imgs.push(loadImage('./pics/heidelbeere.png'));
  imgs.push(loadImage('./pics/granatapfel.png'));
  imgs.push(loadImage('./pics/zitronen.png'));
}

function setup() {
  // zufälliges Bild für den Hintergrund
  img = random(imgs); 
  createCanvas(img.width, img.height);
  image(img, 0, 0);
  
  // auszuschneidenden Bereich (miniCanvas) definieren
  let sw = 150; // Breite 
  let sh = 150; // Höhe 
    
  let sx = random(0, width - sw); // Quelle X
  let sy = random(0, height - sh);// Quelle Y
    
  let x = sx + random(-10,10); // Ziel X
  let y = sy + random(-10,10); // Ziel Y
    
  // Offscreen-Fragment
  let miniCanvas = createGraphics(sw, sh);

  // Bildausschnitt in miniCanvas
  miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw, sh);  
  image(miniCanvas, x, y);
    
  // Zur besseren Übersicht können die einzelnen MiniCanvas 
  // mit einem einfachen Rahmen  visualisiert werden
  stroke(100);
  noFill();
  rect(x, y, sw, sh); 
}
```
<details>
<summary> Musterlösung</summary>

```js
let img;
let imgs = [];

function preload() {
  imgs.push(loadImage('./pics/heidelbeere.png'));
  imgs.push(loadImage('./pics/granatapfel.png'));
  imgs.push(loadImage('./pics/zitronen.png'));
}

function setup() {
// zufälliges Bild für den Hintergrund
  img = random(imgs);
  createCanvas(img.width, img.height);

// Hintergrund  
  tint(250, 160);
  image(img, 0, 0);
  noTint();

  for (let i=0; i<50; i++) {
    
    // auszuschneidenden Bereich (miniCanvas) definieren
    let sw = 150; // Breite 
    let sh = 150; // Höhe 
    
    let sx = random(0, width - sw); // Quelle X
    let sy = random(0, height - sh);// Quelle Y
    
    let x = sx + random(-10,10); // Ziel X
    let y = sy + random(-10,10); // Ziel Y

    // Offscreen-Fragment
    let miniCanvas = createGraphics(sw, sh);

    // Bildausschnitt in miniCanvas
    miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw, sh);  
    image(miniCanvas, x, y);

    // Rahmen 
    stroke(100);
    noFill();
    rect(x, y, sw, sh); 
    }
}
```
</details>

### Ausschnitt 3: Blend Mode

Blend Modes bestimmen, wie Farben übereinander verrechnet werden, wenn sich mehrere Bildebenen überschneiden. In p5.js wird dafür die Funktion `blendMode()` verwendet. Alle verfügbaren Modi sind in der offiziellen Referenz dokumentiert:
https://p5js.org/reference/p5/blendMode/

Blend Modes können sowohl auf das Haupt-Canvas als auch auf ein MiniCanvas angewendet werden. 

#### Aufgabe 

- Erzeuge einen MiniCanvas und wende einen festen Blend Mode darauf an 

> **Hinweis**  
> Ein Blend Mode könnte z. B. `BLEND`, `ADD`, `MULTIPLY`, `EXCLUSION` enthalten.

<details>
<summary> Musterlösung</summary>

```js
let img;
let imgs = [];

function preload() {
  imgs.push(loadImage('./pics/heidelbeere.png'));
  imgs.push(loadImage('./pics/granatapfel.png'));
  imgs.push(loadImage('./pics/zitronen.png'));
}

function setup() {
// zufälliges Bild für den Hintergrund
  img = random(imgs);
  createCanvas(img.width, img.height);

  // Hintergrund  
  tint(250, 160);
  image(img, 0, 0);
  noTint();
    
    // auszuschneidenden Bereich (miniCanvas) definieren
    let sw = 150; // Breite 
    let sh = 150; // Höhe 
    
    let sx = random(0, width - sw); // Quelle X
    let sy = random(0, height - sh);// Quelle Y
    
    let x = sx + random(-10,10); // Ziel X
    let y = sy + random(-10,10); // Ziel Y

    // Offscreen-Fragment
    let miniCanvas = createGraphics(sw, sh);

    // fester Blend Mode
    miniCanvas.blendMode(MULTIPLY);

    // Bildausschnitt in miniCanvas
    miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw, sh);  
    image(miniCanvas, x, y);

    // Rahmen 
    stroke(100);
    noFill();
    rect(x, y, sw, sh); 
}
```
</details>

#### Hauptaufgabe

Nun erweitern wir das Prinzip:

- Erstelle ein Array mit mehreren Blend Modes;
- Wähle für jedes Fragment einen zufälligen Blend Mode und wende ihn auf den jeweiligen MiniCanvas an.

> **Hinweis**  
> `blendMode()` beeinflusst immer alle Zeichenbefehle, die danach ausgeführt werden. Damit jedes Bildfragment tatsächlich einen eigenen zufälligen Blend Mode erhält, wird `blendMode()` im MiniCanvas zweimal gesetzt: einmal zum Zurücksetzen und einmal direkt vor dem Zeichnen des Fragments.

<details>
<summary> Musterlösung</summary>

```js
let img;
let imgs = [];

function preload() {
  imgs.push(loadImage('./pics/heidelbeere.png'));
  imgs.push(loadImage('./pics/granatapfel.png'));
  imgs.push(loadImage('./pics/zitronen.png'));
}

function setup() {
// zufälliges Bild für den Hintergrund
  img = random(imgs);
  createCanvas(img.width, img.height);

    let filter = [BLEND, ADD, DARKEST, EXCLUSION, MULTIPLY, HARD_LIGHT];

  // Hintergrund  
  tint(250, 160);
  image(img, 0, 0);
  noTint();

  for (let i=0; i<50; i++) {
    
    // auszuschneidenden Bereich (miniCanvas) definieren
    let sw = 150; // Breite 
    let sh = 150; // Höhe 
    
    let sx = random(0, width - sw); // Quelle X
    let sy = random(0, height - sh);// Quelle Y
    
    let x = sx + random(-10,10); // Ziel X
    let y = sy + random(-10,10); // Ziel Y

    // Offscreen-Fragment
    let miniCanvas = createGraphics(sw, sh);

    //auf BLEND zurücksetzen
    miniCanvas.blendMode( random(filter) );

    // Bildausschnitt in miniCanvas
    miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw, sh);  
    miniCanvas.blendMode(random(filter)); // zufälligen BlendMode für miniCanvas
    miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw, sh);  
    image(miniCanvas, x, y);
   
     // Rahmen 
    stroke(100);
    noFill();
    rect(x, y, sw, sh); 
     }
}
```
</details>

---

## Fazit & Ausblick

In diesem Tutorial wurde gezeigt, wie sich Bilder mithilfe von Offscreen-Canvas und Blend Modes fragmentieren und neu zusammensetzen lassen.

Mögliches für eine Erweiterung: Verzerrte Fragmente

- Statt den Bildausschnitt 1:1 zu kopieren, kann die Quellgröße bewusst verkleinert werden. Dadurch wird der Bildausschnitt im MiniCanvas leicht vergrößert dargestellt.

```js
miniCanvas.image(img, 0, 0, sw, sh, sx, sy, sw - 30, sh - 30);
```

Andere mögliche Erweiterungen:

- Wechsel der Bilder über Tastatureingaben statt Zufall
- Interaktive Neugenerierung der Fragmente mit der Maus 
- Animation der Fragmente im `draw()`-Modus

Das Projekt kann als Ausgangspunkt für eigene experimentelle Bildarbeiten dienen und flexibel weiterentwickelt werden.
