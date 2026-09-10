# Sistemas-Fisicos-2

## Bitacora de investigación David Vanegas Londoño

# <Fase de investigación>

# 🎵 Cyberpunk Live Coding en Strudel

> Proyecto académico de composición mediante **Live Coding** utilizando **Strudel**.
>
> **Objetivo:** crear un paisaje sonoro inspirado en la estética *cyberpunk* utilizando múltiples capas de sonido que puedan modificarse en tiempo real durante la interpretación.

---

##  Introducción

El proyecto consiste en desarrollar una composición interactiva mediante **Strudel**, donde la música no es una secuencia estática, sino un sistema que puede modificarse mientras está sonando.

La pieza está organizada en diferentes capas independientes:

-  Percusión
-  Bajo
-  Armonía
-  Melodía
-  Pad ambiental
-  Efectos

Cada capa puede activarse o desactivarse durante la ejecución para construir la composición progresivamente.

---

###  Objetivos

- Crear una pieza con varias capas sonoras.
- Experimentar con Live Coding.
- Manipular parámetros musicales en tiempo real.
- Comprender la organización modular de Strudel.
- Construir una interpretación dinámica sin detener la música.

---

###  Configuración inicial

La velocidad de la composición se estableció en:

```javascript
setcpm(36)
```

Lo que equivale aproximadamente a **144 BPM**, un tempo adecuado para música electrónica y synthwave.

---
### Codigo

```js
//--------------------------------------------------------
// CYBERPUNK LIVE CODING
//--------------------------------------------------------

setcpm(36) // 144 BPM

//--------------------------------------------------------
// MEZCLADOR
//--------------------------------------------------------

const DRUMS = 1
const BASS = 1
const CHORDS = 1
const LEAD = 1
const ARP = 0
const PAD = 0

//--------------------------------------------------------
// PARÁMETROS MODIFICABLES
//--------------------------------------------------------

const OCTAVE = 0
const TRANSPOSE = 0

//--------------------------------------------------------
// ESCALA
//--------------------------------------------------------

const prog =
"<[e2] [c2] [g1] [d2]>"

const chords =
"<[e3 g3 b3 d4] [c3 e3 g3 b3] [g2 b2 d3 f3] [d3 f3 a3 c4]>"

//--------------------------------------------------------
// BAJO
//--------------------------------------------------------

$bass:

note(prog.transpose(TRANSPOSE))

.sound("supersaw")

.lpf(slider(700,200,2500))

.attack(0.02)

.release(0.35)

.room(.3)

.postgain(BASS?0.35:0)


//--------------------------------------------------------
// ACORDES
//--------------------------------------------------------

$chords:

note(chords.transpose(TRANSPOSE))

.sound("gm_epiano1")

.attack(slider(.18,0,1))

.release(1)

.room(slider(1.2,0,3))

.lpf(slider(1600,400,5000))

.postgain(CHORDS?0.45:0)


//--------------------------------------------------------
// MELODÍA PRINCIPAL
//--------------------------------------------------------

let lead =

"<[e5 g5 b5 g5] [e5 a5 g5 e5] [d5 f#5 a5 g5] [b4 d5 e5 g5]>"

$lead:

note(lead
.transpose(OCTAVE*12+TRANSPOSE))

.sound("sawtooth")

.attack(slider(.04,0,.4))

.decay(slider(.35,.1,1))

.sustain(.2)

.release(.25)

.delay(slider(.25,0,.7))

.room(slider(1.4,0,3))

.lpf(slider(2200,500,5000))

.postgain(LEAD?0.55:0)


//--------------------------------------------------------
// ARPEGIO
//--------------------------------------------------------

let arp =

"<[e5 b5 g5 b5] [c6 g5 e5 g5] [d5 a5 f#5 a5] [b5 g5 d5 g5]>"

$arp:

note(arp.transpose(TRANSPOSE))

.sound("triangle")

.attack(.01)

.release(.15)

.delay(.35)

.room(1.5)

.lpf(slider(2800,800,6000))

.postgain(ARP?0.25:0)


//--------------------------------------------------------
// PAD
//--------------------------------------------------------

$pad:

note(chords.transpose(12+TRANSPOSE))

.sound("sawtooth")

.attack(2)

.release(3)

.room(3)

.lpf(slider(900,300,2000))

.postgain(PAD?0.2:0)


//--------------------------------------------------------
// BATERÍA
//--------------------------------------------------------

$drums:

stack(

s("bd")
.beat("0,4,8,12",16),

s("cp")
.beat("4,12",16),

s("hh")
.beat("0,2,4,6,8,10,12,14",16),

s("oh")
.beat("7,15",16)

)

.bank("RolandTr909")

.postgain(DRUMS?0.9:0)
```

---
###  Mezclador

Una de las primeras decisiones fue crear un pequeño mezclador utilizando variables.

```javascript
const DRUMS = 1
const BASS = 1
const CHORDS = 1
const LEAD = 1
const ARP = 0
const PAD = 0
```

Esto permite activar o desactivar instrumentos sin modificar el resto del código.

Por ejemplo:

```javascript
const PAD = 0
```

El pad permanece apagado.

Durante la interpretación puede cambiarse simplemente por:

```javascript
const PAD = 1
```

y comenzará a sonar inmediatamente.

---

###  Bajo

El bajo sigue las fundamentales de la progresión armónica.

```javascript
<f1 g1 e1 a1>
```

Para hacerlo más expresivo se añadió un filtro de paso bajo controlado mediante un slider.

```javascript
.lpf(slider(700,200,2500))
```

Durante la interpretación es posible abrir o cerrar el filtro para modificar el carácter del sonido.

---

###  Armonía

La armonía utiliza acordes menores con séptimas para producir un ambiente oscuro característico del estilo cyberpunk.

Ejemplo:

```javascript
[e3 g3 b3 d4]
[c3 e3 g3 b3]
[g2 b2 d3 f3]
[d3 f3 a3 c4]
```

El piano eléctrico incorpora:

- Attack suave
- Reverb
- Filtro LPF

Todo ello controlable mediante sliders.

---

###  Melodía

La melodía principal fue diseñada como un patrón fácilmente modificable.

```javascript
<
[e5 g5 b5 g5]
[e5 a5 g5 e5]
[d5 f#5 a5 g5]
[b4 d5 e5 g5]
>
```

Al estar almacenada en una variable, puede modificarse durante la ejecución sin detener la música.

Por ejemplo:

```javascript
[e5 g5 b5 d6]
```

genera una variación inmediata.

---

###  Arpegio

El arpegio añade movimiento constante al fondo.

```javascript
<
[e5 b5 g5 b5]
[c6 g5 e5 g5]
[d5 a5 f#5 a5]
[b5 g5 d5 g5]
>
```

Esta capa puede activarse únicamente en determinadas partes de la interpretación para incrementar la intensidad.

---

###  Pad Atmosférico

El pad utiliza notas largas con mucho ataque y reverb.

Su función principal es generar profundidad y sensación espacial.

Puede permanecer apagado durante gran parte de la pieza y activarse únicamente en el clímax.

---

###  Percusión

La batería utiliza el banco Roland TR-909.

```javascript
stack(
s("bd"),
s("cp"),
s("hh"),
s("oh")
)
```

Los distintos instrumentos siguen patrones independientes que pueden modificarse durante la ejecución.

---

###  Controles en Tiempo Real

Uno de los aspectos más importantes del Live Coding es modificar parámetros mientras la música continúa sonando.

#### Abrir el filtro

```javascript
.lpf(slider(1200,300,5000))
```

Permite transformar progresivamente el timbre del sintetizador.

---

### Cambiar la octava

Toda la melodía puede desplazarse una octava.

```javascript
const OCTAVE = 1
```

o

```javascript
const OCTAVE = -1
```

---

### Transportar toda la pieza

Es posible cambiar instantáneamente la tonalidad.

```javascript
const TRANSPOSE = 2
```

o

```javascript
const TRANSPOSE = -2
```

Esto afecta simultáneamente a bajo, armonía, melodía y arpegios.

---

### Cambiar sintetizadores

Durante la interpretación también pueden probarse distintos sonidos.

```javascript
.sound("sawtooth")
```

↓

```javascript
.sound("triangle")
```

↓

```javascript
.sound("supersaw")
```

↓

```javascript
.sound("square")
```

Cada sintetizador cambia completamente el color de la composición.

---

#  Desarrollo de la Interpretación

La presentación puede construirse de forma progresiva.

### Inicio

Solo armonía.

```
CHORDS = 1
```

---

### Segunda sección

Se incorpora el bajo.

```
BASS = 1
```

---

### Tercera sección

Entra la batería.

```
DRUMS = 1
```

---

### Cuarta sección

Comienza la melodía principal.

```
LEAD = 1
```

---

### Quinta sección

Se añade el arpegio.

```
ARP = 1
```

---

### Clímax

Se activa el pad.

```
PAD = 1
```

---

### Final

Reducir el filtro.

Apagar la batería.

Dejar únicamente el pad y la melodía.

Volver a introducir la batería para cerrar la interpretación.

---

##  Experimentación

Durante el desarrollo se realizaron distintas pruebas modificando:

- Frecuencia del filtro LPF
- Ataque y liberación de los sintetizadores
- Cantidad de reverb
- Delay
- Cambio de sintetizadores
- Transposición
- Variaciones melódicas
- Activación y desactivación de capas
- Construcción progresiva de la pieza

Esto permitió comprobar cómo pequeñas modificaciones producen cambios significativos en la textura sonora sin detener la ejecución.

---

##  Aprendizajes

A través de este proyecto fue posible comprender que el **Live Coding** no consiste únicamente en programar música, sino en interpretar código como si fuera un instrumento musical.

La organización modular mediante capas independientes facilita experimentar con diferentes estructuras, modificar parámetros en tiempo real y construir una interpretación dinámica donde la composición evoluciona constantemente durante la ejecución.

---

###  Tecnologías utilizadas

- **Strudel**
- JavaScript
- Roland TR-909 Samples
- Sintetizadores integrados de Strudel

---

###  Resultado

El resultado final es una composición de estilo **cyberpunk**, con una estructura flexible y preparada para Live Coding, donde cada elemento puede modificarse en tiempo real para crear nuevas variaciones durante la interpretación.



## Entrega 2

### Código Strudel
```js
//--------------------------------------------------------
// CYBERPUNK LIVE CODING
// STRUDEL → OSC → TOUCHDESIGNER
//--------------------------------------------------------

setcpm(36) // 144 BPM


//--------------------------------------------------------
// PARÁMETRO OSC PARA TOUCHDESIGNER
//--------------------------------------------------------

const { visualid } = createParams('visualid')


//--------------------------------------------------------
// MEZCLADOR
//--------------------------------------------------------

const DRUMS = 1
const BASS = 1
const CHORDS = 1
const LEAD = 1
const ARP = 0
const PAD = 0


//--------------------------------------------------------
// PARÁMETROS MODIFICABLES
//--------------------------------------------------------

const OCTAVE = 0
const TRANSPOSE = 0


//--------------------------------------------------------
// ESCALA / PROGRESIÓN
//--------------------------------------------------------

const prog =
"<[e2] [c2] [g1] [d2]>"

const chords =
"<[e3 g3 b3 d4] [c3 e3 g3 b3] [g2 b2 d3 f3] [d3 f3 a3 c4]>"


//========================================================
// BAJO
//========================================================

const bass =

note(
  prog.transpose(TRANSPOSE)
)

.sound("supersaw")

.lpf(
  slider(700,200,2500)
)

.attack(0.02)

.release(0.35)

.room(.3)

.postgain(
  BASS ? 0.35 : 0
)

// TOUCHDESIGNER
.visualid("bass")


//========================================================
// ACORDES
//========================================================

const harmony =

note(
  chords.transpose(TRANSPOSE)
)

.sound("gm_epiano1")

.attack(
  slider(.18,0,1)
)

.release(1)

.room(
  slider(1.2,0,3)
)

.lpf(
  slider(1600,400,5000)
)

.postgain(
  CHORDS ? 0.45 : 0
)

// TOUCHDESIGNER
.visualid("harmony")


//========================================================
// MELODÍA
//========================================================

const melodyPattern =

"<[e5 g5 b5 g5] [e5 a5 g5 e5] [d5 f#5 a5 g5] [b4 d5 e5 g5]>"


const melody =

note(
  melodyPattern
  .transpose(OCTAVE * 12 + TRANSPOSE)
)

.sound("sawtooth")

.attack(
  slider(.04,0,.4)
)

.decay(
  slider(.35,.1,1)
)

.sustain(.2)

.release(.25)

.delay(
  slider(.25,0,.7)
)

.room(
  slider(1.4,0,3)
)

.lpf(
  slider(2200,500,5000)
)

.postgain(
  LEAD ? 0.55 : 0
)

// TOUCHDESIGNER
.visualid("melody")


//========================================================
// LEAD
//========================================================


const leadPattern =

"<[b5 e6 g6 e6] [g5 c6 e6 c6] [a5 d6 f#6 d6] [g5 b5 e6 b5]>"


const lead =

note(
  leadPattern
  .transpose(OCTAVE * 12 + TRANSPOSE)
)

.sound("sawtooth")

.attack(
  slider(.02,0,.25)
)

.decay(
  slider(.25,.05,.8)
)

.sustain(.15)

.release(.2)

.delay(
  slider(.3,0,.7)
)

.room(
  slider(1.2,0,3)
)

.lpf(
  slider(3000,800,6000)
)

.postgain(
  LEAD ? 0.30 : 0
)

// TOUCHDESIGNER
.visualid("lead")


//========================================================
// ARPEGIO
//========================================================

const arpPattern =

"<[e5 b5 g5 b5] [c6 g5 e5 g5] [d5 a5 f#5 a5] [b5 g5 d5 g5]>"


const arp =

note(
  arpPattern.transpose(TRANSPOSE)
)

.sound("triangle")

.attack(.01)

.release(.15)

.delay(.35)

.room(1.5)

.lpf(
  slider(2800,800,6000)
)

.postgain(
  ARP ? 0.25 : 0
)

// TOUCHDESIGNER
.visualid("arp")


//========================================================
// PAD
//========================================================
//
// El PAD comparte la familia armónica,
// pero sigue siendo un evento separado.
//========================================================

const pad =

note(
  chords.transpose(12 + TRANSPOSE)
)

.sound("sawtooth")

.attack(2)

.release(3)

.room(3)

.lpf(
  slider(900,300,2000)
)

.postgain(
  PAD ? 0.2 : 0
)

// TOUCHDESIGNER
.visualid("harmony")


//========================================================
// BATERÍA
//========================================================


//--------------------------------------------------------
// KICK
//--------------------------------------------------------

const drum_bd =

s("bd")
  .beat("0,4,8,12",16)
  .bank("RolandTr909")
  .postgain(DRUMS ? 0.9 : 0)
  .visualid("drum_bd")


//--------------------------------------------------------
// CLAP
//--------------------------------------------------------

const drum_cp =

s("cp")
  .beat("4,12",16)
  .bank("RolandTr909")
  .postgain(DRUMS ? 0.9 : 0)
  .visualid("drum_cp")


//--------------------------------------------------------
// HI-HAT CERRADO
//--------------------------------------------------------

const drum_hh =

s("hh")
  .beat("0,2,4,6,8,10,12,14",16)
  .bank("RolandTr909")
  .postgain(DRUMS ? 0.9 : 0)
  .visualid("drum_hh")


//--------------------------------------------------------
// HI-HAT ABIERTO
//--------------------------------------------------------

const drum_oh =

s("oh")
  .beat("7,15",16)
  .bank("RolandTr909")
  .postgain(DRUMS ? 0.9 : 0)
  .visualid("drum_oh")


//========================================================
// AUDIO + OSC → TOUCHDESIGNER
//========================================================

$:stack(

  //------------------------------------------------------
  // DRUMS
  //------------------------------------------------------

  drum_bd,
  drum_bd.osc(),

  drum_cp,
  drum_cp.osc(),

  drum_hh,
  drum_hh.osc(),

  drum_oh,
  drum_oh.osc(),


  //------------------------------------------------------
  // BASS
  //------------------------------------------------------

  bass,
  bass.osc(),


  //------------------------------------------------------
  // HARMONY
  //------------------------------------------------------

  harmony,
  harmony.osc(),

  pad,
  pad.osc(),


  //------------------------------------------------------
  // MELODY
  //------------------------------------------------------

  melody,
  melody.osc(),


  //------------------------------------------------------
  // ARPEGGIO
  //------------------------------------------------------

  arp,
  arp.osc(),


  //------------------------------------------------------
  // LEAD
  //------------------------------------------------------

  lead,
  lead.osc()

)
```

<img width="1918" height="861" alt="image" src="https://github.com/user-attachments/assets/31ab2234-218f-48e8-bb6f-e5654839f4ad" />

como se ve en la imagen ahí se denota el trabajo y resultado de la experiencia sonora en Touch.



# Entrega 3

## el codigo de strudel es el mismo pero si se descargan los archivos de Touch se muestra como funcionan los controles 


# Proyecto estaciones en el mar

<img width="1682" height="808" alt="image" src="https://github.com/user-attachments/assets/0675cd1d-60b2-41d6-a94d-108697f2b7e3" />

Codigos de las estaciones:

VERANO:

```js
setcpm(132/4)

stack(
  // 🌊 Olas / ambiente marino
  note("<[c4,e4,g4,b4] [d4,f4,a4,c5]>")
    .s("sawtooth")
    .lpf(sine.range(700,2200).slow(4))
    .gain(.16)
    .room(.7),

  // ☀️ Melodía tropical principal
  note("e5 g5 a5 b5 a5 g5 e5 d5 e5 g5 a5 c6 b5 a5 g5 e5")
    .s("square")
    .decay(.12)
    .sustain(.15)
    .release(.08)
    .gain(.24)
    .pan(sine.range(-.7,.7).slow(2)),

  // 🌴 Bajo tropical
  note("e2 e2 g2 a2 ~ a2 g2 e2 d2 d2 f2 a2 g2 e2")
    .s("sawtooth")
    .lpf(700)
    .decay(.2)
    .sustain(.35)
    .gain(.38),

  // 🥁 Kick
  s("bd*4")
    .gain(.8),

  // 🪘 Percusión con mucho movimiento
  s("~ cp ~ cp ~ cp [cp cp] ~")
    .gain(.55),

  s("hh*8")
    .gain(.32)
    .pan(sine.range(-1,1).fast(2)),

  // 🌴 Shaker / sensación de baile
  s("[~ sd] [sd ~] [~ sd] [sd sd]")
    .gain(.28),

  // ✨ Brillos como gotas de agua
  note("c6 e6 g6 b6")
    .s("triangle")
    .delay(.35)
    .room(.8)
    .gain(.12)
)

```

PRIMAVERA:

```js
setcpm(72/4)

stack(

  // 🌸 BASE PRINCIPAL — tu idea original
  note("<c4M d4m e4m f4M>")
    .s("triangle")
    .slow(8)
    .room(0.8)
    .lpf(1500)
    .gain(.30),

  note("[c5 e5 g5 a5]")
    .s("kalimba")
    .slow(2)
    .delay(0.5)
    .room(0.8)
    .gain(.20),


  // 🌊 OLEAJE — notas largas que acompañan la armonía
  note("<c3 g3 e3 g3 d3 a3 f3 a3>")
    .s("sine")
    .slow(8)
    .attack(1)
    .release(2)
    .room(1)
    .gain(.10)
    .pan(sine.range(-.35,.35).slow(8)),


  // 🌱 SEGUNDA MELODÍA — aparece poco a poco
  note("e5 ~ g5 ~ a5 g5 e5 ~ d5 ~ e5 g5 ~ a5")
    .s("triangle")
    .slow(4)
    .attack(.5)
    .release(1)
    .room(.9)
    .gain(.11),


  // ☀️ MELODÍA CÁLIDA — sensación de sol y primavera
  note("g5 a5 b5 ~ a5 g5 e5 ~ g5 a5 c6 ~ b5 a5")
    .s("sine")
    .slow(6)
    .attack(.7)
    .release(1.5)
    .room(1)
    .gain(.08),


  // ✨ REFLEJOS SOBRE EL AGUA
  note("[c6 ~ e6] [g6 ~ a6] [e6 ~ g6] [b5 ~ e6]")
    .s("kalimba")
    .slow(4)
    .delay(.7)
    .room(1)
    .gain(.07)
    .pan(sine.range(-.6,.6).slow(8)),


  // 🌬️ BRISA MARINA
  s("~ hh ~ ~ hh ~ [hh hh] ~")
    .gain(.055)
    .lpf(3000)
    .room(1),


  // 🐚 PEQUEÑOS DESTELLOS
  note("e6 ~ ~ g6 ~ a6 ~ ~")
    .s("triangle")
    .slow(3)
    .delay(.8)
    .room(1)
    .gain(.05)
)

```

INVIERNO:
```js
const ratchet = register('ratchet', (pat) => pat.sometimes(ply(2)))

setcpm(65)

arrange(

  // ❄️ INTRO — Nieve cayendo
  [4,
    stack(

      // Textura muy suave
      s("~ ~ ~ ~")
        .gain(0.08),

      // Notas largas y profundas
      note("<c3 ~ g2 ~>")
        .slow(4)
        .sound("sine")
        .gain(0.18),

      // Melodía cristalina
      note("<e4 ~ d4 ~>")
        .slow(4)
        .sound("triangle")
        .gain(0.15)
    )
  ],

  // 🌨️ DESARROLLO — Paisaje invernal
  [4,
    stack(

      // Pulso muy discreto
      s("~ ~ bd ~")
        .gain(0.12),

      // Bajo profundo
      note("<c2 ~ g1 ~>")
        .slow(2)
        .sound("sine")
        .gain(0.22),

      // Acordes / atmósfera
      note("<c4 e4 g4 ~>")
        .slow(4)
        .sound("triangle")
        .gain(0.16),

      // Pequeñas notas como copos de nieve
      note("e5 ~ ~ d5 ~ ~ g5 ~")
        .slow(2)
        .sound("sine")
        .gain(0.12)
    )
  ],

  // 🌙 NOCHE — Más profunda
  [4,
    stack(

      // Pulso casi imperceptible
      s("~ bd ~ ~")
        .gain(0.10),

      // Bajo
      note("<c2 ~ g1 ~ a1 ~ e2 ~>")
        .slow(2)
        .sound("sine")
        .gain(0.24),

      // Melodía principal
      note("<e4 g4 a4 g4 e4 d4 c4 ~>")
        .slow(2)
        .sound("triangle")
        .gain(0.19),

      // Notas agudas muy ocasionales
      note("~ ~ c5 ~ ~ e5 ~ ~")
        .slow(2)
        .sound("sine")
        .gain(0.10)
    )
  ],

  // 🕯️ FINAL — La nieve se desvanece
  [6,
    stack(

      // Solo una pulsación ocasional
      s("~ ~ ~ bd")
        .gain(0.07),

      // Nota grave sostenida
      note("<c2 ~ g1 ~>")
        .slow(4)
        .sound("sine")
        .gain(0.16),

      // Melodía final
      note("<e4 ~ d4 ~ c4 ~ g3 ~>")
        .slow(4)
        .sound("triangle")
        .gain(0.13)
    )
  ]
)
```

OTOÑO:

```js
const ratchet = register('ratchet', (pat) => pat.sometimes(ply(2)))

setcpm(75)

arrange(

  // 🍁 INTRO — Brisa fresca
  [4,
    stack(

      s("~ ~ ~ ~")
        .gain(0.08),

      note("<d2 ~ a1 ~>")
        .slow(4)
        .sound("sine")
        .gain(0.16),

      note("<d4 ~ f4 ~>")
        .slow(4)
        .sound("triangle")
        .gain(0.14),

      note("~ a4 ~ ~")
        .slow(4)
        .sound("sine")
        .gain(0.08),

      // 🌊 Pequeño movimiento del agua
      note("~ d5 ~ ~")
        .slow(4)
        .sound("sine")
        .delay(0.4)
        .gain(0.04)
    )
  ],

  // 🌬️ CRECIMIENTO — El viento comienza
  [4,
    stack(

      s("bd ~ ~ ~")
        .gain(0.10),

      note("<d2 ~ a1 ~>")
        .slow(3)
        .sound("sine")
        .gain(0.18),

      note("<d4 f4 ~ a4>")
        .slow(4)
        .sound("triangle")
        .gain(0.16),

      note("~ a4 ~ ~")
        .slow(3)
        .sound("sine")
        .gain(0.09),

      // 🍂 Brisa que empieza a moverse
      note("d5 ~ f5 ~")
        .slow(4)
        .sound("sine")
        .gain(0.05)
        .room(0.6)
    )
  ],

  // 🍂 RÁFAGA — Intensa pero lenta
  [4,
    stack(

      s("bd ~ ~ ~")
        .gain(0.16),

      s("~ ~ sd ~")
        .gain(0.12),

      // Bajo con notas largas
      note("<d2 ~ a1 ~>")
        .slow(2)
        .sound("sine")
        .gain(0.21),

      // Melodía lenta — SE MANTIENE
      note("<d4 ~ f4 ~ a4 ~ c5 ~>")
        .slow(3)
        .sound("triangle")
        .gain(0.20),

      // Capa profunda
      note("<a3 ~ f3 ~>")
        .slow(4)
        .sound("sine")
        .gain(0.12),

      // 🌊 Detalle de oleaje durante la ráfaga
      note("a4 ~ c5 ~ d5 ~ c5 ~")
        .slow(3)
        .sound("sine")
        .gain(0.06)
        .delay(0.35)
    )
  ],

  // 🌫️ TRANSICIÓN — El oleaje sigue después de la ráfaga
  [2,
    stack(

      // El pulso desaparece lentamente
      s("bd ~ ~ ~")
        .gain(0.07),

      note("<a3 ~ f3 ~>")
        .slow(3)
        .sound("sine")
        .gain(0.09),

      // 🍂 Últimas notas de la ráfaga
      note("<a4 ~ c5 ~>")
        .slow(3)
        .sound("triangle")
        .gain(0.10)
        .room(0.8),

      // 🌊 Eco que conecta con el descenso
      note("d5 ~ a4 ~")
        .slow(3)
        .sound("sine")
        .delay(0.6)
        .gain(0.05)
    )
  ],

  // 🌫️ DESCENSO — El viento desaparece
  [4,
    stack(

      s("~ ~ ~ ~")
        .gain(0.06),

      note("<d2 ~ a1 ~>")
        .slow(4)
        .sound("sine")
        .gain(0.15),

      note("<f4 ~ e4 ~ d4 ~>")
        .slow(4)
        .sound("triangle")
        .gain(0.13),

      note("~ a4 ~ ~")
        .slow(4)
        .sound("sine")
        .gain(0.07),

      // 🌊 Oleaje que permanece
      note("~ d5 ~ a4")
        .slow(4)
        .sound("sine")
        .delay(0.5)
        .gain(0.04)
    )
  ],

  // 🌬️ SEGUNDA RÁFAGA — Movimiento lento
  [4,
    stack(

      s("bd ~ ~ ~")
        .gain(0.15),

      s("~ ~ sd ~")
        .gain(0.11),

      note("<d2 ~ a1 ~>")
        .slow(2)
        .sound("sine")
        .gain(0.19),

      note("<d4 ~ f4 ~ a4 ~ f4 ~>")
        .slow(3)
        .sound("triangle")
        .gain(0.18),

      note("<a3 ~ c4 ~>")
        .slow(4)
        .sound("sine")
        .gain(0.10),

      // 🍂 Pequeña melodía adicional
      note("a4 ~ c5 ~ d5 ~")
        .slow(3)
        .sound("sine")
        .gain(0.06)
        .delay(0.35)
    )
  ],

  // 🌫️ TRANSICIÓN FINAL — La ráfaga se aleja
  [2,
    stack(

      s("~ bd ~ ~")
        .gain(0.06),

      note("<a3 ~ f3 ~>")
        .slow(3)
        .sound("sine")
        .gain(0.07),

      note("d5 ~ c5 ~ a4 ~")
        .slow(3)
        .sound("triangle")
        .gain(0.08)
        .room(0.8),

      note("f5 ~ d5 ~")
        .slow(4)
        .sound("sine")
        .delay(0.7)
        .gain(0.04)
    )
  ],

  // 🍁 FINAL — Hojas cayendo
  [4,
    stack(

      s("~ ~ ~ ~")
        .gain(0.05),

      note("<d2 ~ ~ ~>")
        .slow(4)
        .sound("sine")
        .gain(0.12),

      note("<f4 ~ e4 ~ d4 ~>")
        .slow(4)
        .sound("triangle")
        .gain(0.10),

      note("~ a4 ~ ~")
        .slow(4)
        .sound("sine")
        .gain(0.06),

      // 🍂 Último reflejo
      note("d5 ~ ~ ~")
        .slow(4)
        .sound("sine")
        .delay(0.8)
        .gain(0.035)
    )
  ]
)

```

