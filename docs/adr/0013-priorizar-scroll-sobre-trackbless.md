# ADR 0013: Priorizar SCROLL sobre TRACKBLESS

## Estado

Aceptado.

## Contexto

`TRACKBLESS` bloquea el trackball y replica `QWRT`. La tecla 56 en
`TRACKBLESS` ya activa `SCROLL`, pero `TRACKBLESS` tenia un numero de capa mas
alto que `SCROLL`. En ZMK, la capa activa con numero mas alto resuelve los
bindings, por lo que `SCROLL` no podia exponer sus teclas mientras
`TRACKBLESS` seguia activo.

Eso dejaba sin una ruta practica para salir de `TRACKBLESS` usando el doble tap
de las posiciones 55 o 58 definido en `SCROLL`.

## Decision

Reordenar las capas para que:

```c
#define TRACKBLESS 5
#define SNIPE      6
#define SCROLL     7
```

Actualizar `keyball61_right.overlay` para mantener el PMW3610 sincronizado:

```c
scroll-layers = <7>;

trackball_lock {
    layers = <1 2 3 5>;
};
```

## Consecuencias

Cuando `TRACKBLESS` esta activo, la tecla 56 puede activar `SCROLL` con
prioridad suficiente para que sus bindings sean visibles. Desde ahi, doble tap
en las posiciones 55 o 58 alterna `TRACKBLESS` y permite salir del modo
bloqueado.

El cambio acopla nuevamente el orden del keymap con los numeros usados por el
overlay del trackball.

## Reversal strategy

Restaurar `SCROLL` como capa 5, `TRACKBLESS` como capa 7, y devolver
`scroll-layers` y `trackball_lock.layers` a esos numeros.
