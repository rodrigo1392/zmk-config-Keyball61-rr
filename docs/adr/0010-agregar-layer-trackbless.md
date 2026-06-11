# ADR 0010: Agregar layer TRACKBLESS

## Estado

Aceptado.

## Contexto

`DEFAULT` no puede quedar bloqueado por `trackball_lock` si se quiere mantener
la activacion automatica de `MOUSE` por movimiento. Para escribir sin riesgo de
mover el puntero, se necesita una capa equivalente a `QWRT` donde el trackball
si este bloqueado.

## Decision

Agregar el layer `TRACKBLESS`:

```c
#define TRACKBLESS 5
```

`TRACKBLESS` replica la distribucion de `QWRT`, pero se incluye en
`trackball_lock.layers`:

```c
layers = <1 2 3 5>;
```

Crear tap-dances para que `TRACKBLESS` pueda activarse y desactivarse desde
posiciones de pulgar:

```c
td_trackbless_space: td_trackbless_space {
    compatible = "zmk,behavior-tap-dance";
    #binding-cells = <0>;
    tapping-term-ms = <350>;
    bindings = <&kp SPACE>, <&tog TRACKBLESS>;
};
```

En `TRACKBLESS`, posiciones 55 y 58:

- tap: `SPACE`;
- doble tap: sigue enviando `SPACE`; no desactiva `TRACKBLESS`.

En `MOUSE` y `SCROLL`, posiciones 55 y 58:

- tap: no hace nada;
- doble tap: alternar `TRACKBLESS`.

En `MOUSE`, posicion 58 conserva una excepcion: tap vuelve a `QWRT` y doble tap
alterna `TRACKBLESS`.

Se elimina el tap-dance anterior `td_mouse_default`, que hacia que doble tap en
posiciones de `MOUSE` volviera a `QWRT`.

## Consecuencias

`QWRT` mantiene activacion automatica de `MOUSE` por movimiento. `TRACKBLESS`
mantiene el mismo layout de escritura, pero descarta movimiento del trackball.

La posicion 55 deja de ser `&lt MOUSE SPACE`. En `MOUSE` y `SCROLL`, las
posiciones 55 y 58 permiten alternar `TRACKBLESS`. En `TRACKBLESS`, esas
posiciones quedan como espacio para evitar salir accidentalmente de la capa
bloqueada.

En `TRACKBLESS`, las posiciones 56 y 57 quedan dedicadas a acceso de capas:

- posicion 56: hold activa `SCROLL`; doble tap alterna `SCROLL`;
- posicion 57: hold activa `SYM`; doble tap alterna `SCROLL`.

El acceso a `MOUSE` queda principalmente por `automouse-layer`.

ADR 0013 ajusta la prioridad numerica para que `SCROLL` quede por encima de
`TRACKBLESS` y pueda usarse como ruta de salida cuando `TRACKBLESS` esta activo.

## Archivos afectados

- `config/keyball61.keymap`
- `config/boards/shields/keyball61/keyball61_right.overlay`

## Reversal strategy

Eliminar `TRACKBLESS`, quitar `5` de `trackball_lock.layers` y restaurar las
posiciones 55 y 58 de `QWRT` segun el keymap anterior.
