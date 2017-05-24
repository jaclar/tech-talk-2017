title: Javascript Performance Tracing
class: animation-fade
layout: true

<!-- This slide will serve as the base layout for all your slides -->
.bottom-bar[
  {{title}}
]

---

class: impact

# {{title}}
## The good parts

---

# ¿Para que sirve?

## ¡Mala performance es un bug!

- En el *backend* pone en **peligro** la **estabilidad** de la plataforma
- En el *frontend* causa una **mala experiencia** para el usario
- A veces es dificil solamente viendo el codigo, donde hay cuellos de botella en el codigo

---

# ¿Como funciona?

.col-6[

- Cada lenguaje de programación mantiene un call stack.
- Un profiler inspeciona el programa mientras que corre con mucha
  frequencia y guarda el stackframe de esos momentos.
- Con esa información se puede recuperar cuando tiempo se gastó en
  cada parte del codigo.

]

.col-6.responsive[![Call stack layout](./img/stack.png)]

---

# Tipos de Visuzalizaciones

--

.col-4[
### Timeline
]

--

.col-4[
### Flamegraph
]

--

.col-4[
### Sunburst
]


---

# Profiling en el lado del cliente (Chrome)

Placeholder: chrome profiling tab imagen

---

## CPU profiling

Placeholder para un gif que

---

## Heap snapshots

- En general se usa para detectar memory leaks.
- Espeicialmente closures pueden retener muchos objetos del garbage
  collector

Heap screenshot placeholder

---

# Profiling en el lado del servidor (node.js)

- como node.js usa V8 como engine de JavaScript podemos usar los
  mismos heramientas de Chrome DevTools
- desde v6 node implementa *Chrome Debugging Protocol* con
  `--inspect`

``` bash
┌─[niue] - [~/projects/talks/sp-tt-2017] - [Tue, 23. May, 23:08]
└─[$] node --inspect
Debugger listening on port 9229.
Warning: This is an experimental feature and could change at any time.
To start debugging, open the following URL in Chrome:
    chrome-devtools://devtools/remote/serve_file/@60cd6e859b9f5...
>

```

- pero que hacemos si tenemos codigo en produccion o node v4?

---

## dtrace, perf and friends - **OS tools to the rescue!**

- hace mucho tiempo los sistemas operativos incluyen heramientas para
  inspeccionar processos (`strace`, `dtrace`, etc)
- `node --perf-basic-prof` expone simbolos de V8

.col-6[
`dtrace`

- originalmente de *Solaris* (ahora tb. *macOS*, *freeBSD* y *Linux*
- tiene super-poderes y casi no tiene overhead
]

.col-6[
`perf`

- incluido en el kernel de *Linux*
- y funciona... quien ya tiene la suerte de trabajar con BSD o
  openSolaris en produccion?

]

--

Que se usa en Windows? .small[(Quien ya tiene la mala suerte de tener que trabjar con Windows en produccion?)]

---

## Ejemplo: `perf`

onliner:
``` bash
perf record -e cycles:u -g -- node --perf-basic-prof index.js
```

inspeccionar processo que ya esta corriendo

``` bash
perf...
```

TODO: walk through -> `cpuprofilify` -> `devtools` || `flamegraph`

---

## Real life flame graph de I am at

### Local

### Produccion

---

# References

- Call stack: https://en.wikipedia.org/wiki/Call_stack#Inspection
- Flame Graphs:
  http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html
- Linux tracers: http://www.brendangregg.com/blog/2015-07-08/choosing-a-linux-tracer.html
- `cpuprofilify`: https://github.com/thlorenz/cpuprofilify
- Chrome Debuggin Protocol: https://chromedevtools.github.io/devtools-protocol/
