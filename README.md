# Sistemas-Fisicos-2

## Bitacora de investigación David Vanegas Londoño

# 🎵 Cyberpunk Live Coding en Strudel

> Proyecto académico de composición mediante **Live Coding** utilizando **Strudel**.
>
> **Objetivo:** crear un paisaje sonoro inspirado en la estética *cyberpunk* utilizando múltiples capas de sonido que puedan modificarse en tiempo real durante la interpretación.

---

#  Introducción

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

#  Objetivos

- Crear una pieza con varias capas sonoras.
- Experimentar con Live Coding.
- Manipular parámetros musicales en tiempo real.
- Comprender la organización modular de Strudel.
- Construir una interpretación dinámica sin detener la música.

---

#  Configuración inicial

La velocidad de la composición se estableció en:

```javascript
setcpm(36)
```

Lo que equivale aproximadamente a **144 BPM**, un tempo adecuado para música electrónica y synthwave.

---

#  Mezclador

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

#  Bajo

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

#  Armonía

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

#  Melodía

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

#  Arpegio

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

#  Pad Atmosférico

El pad utiliza notas largas con mucho ataque y reverb.

Su función principal es generar profundidad y sensación espacial.

Puede permanecer apagado durante gran parte de la pieza y activarse únicamente en el clímax.

---

#  Percusión

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

#  Controles en Tiempo Real

Uno de los aspectos más importantes del Live Coding es modificar parámetros mientras la música continúa sonando.

## Abrir el filtro

```javascript
.lpf(slider(1200,300,5000))
```

Permite transformar progresivamente el timbre del sintetizador.

---

## Cambiar la octava

Toda la melodía puede desplazarse una octava.

```javascript
const OCTAVE = 1
```

o

```javascript
const OCTAVE = -1
```

---

## Transportar toda la pieza

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

## Cambiar sintetizadores

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

## Inicio

Solo armonía.

```
CHORDS = 1
```

---

## Segunda sección

Se incorpora el bajo.

```
BASS = 1
```

---

## Tercera sección

Entra la batería.

```
DRUMS = 1
```

---

## Cuarta sección

Comienza la melodía principal.

```
LEAD = 1
```

---

## Quinta sección

Se añade el arpegio.

```
ARP = 1
```

---

## Clímax

Se activa el pad.

```
PAD = 1
```

---

## Final

Reducir el filtro.

Apagar la batería.

Dejar únicamente el pad y la melodía.

Volver a introducir la batería para cerrar la interpretación.

---

#  Experimentación

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

#  Aprendizajes

A través de este proyecto fue posible comprender que el **Live Coding** no consiste únicamente en programar música, sino en interpretar código como si fuera un instrumento musical.

La organización modular mediante capas independientes facilita experimentar con diferentes estructuras, modificar parámetros en tiempo real y construir una interpretación dinámica donde la composición evoluciona constantemente durante la ejecución.

---

#  Tecnologías utilizadas

- **Strudel**
- JavaScript
- Roland TR-909 Samples
- Sintetizadores integrados de Strudel

---

#  Resultado

El resultado final es una composición de estilo **cyberpunk**, con una estructura flexible y preparada para Live Coding, donde cada elemento puede modificarse en tiempo real para crear nuevas variaciones durante la interpretación.
