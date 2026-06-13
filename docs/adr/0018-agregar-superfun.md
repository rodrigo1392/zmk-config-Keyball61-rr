# 0018. Agregar layer SUPERFUN

## Context

Se necesita una capa para atajos exclusivos que puedan ser capturados por cada
sistema operativo sin competir con combinaciones normales del teclado.

## Decision

Agregar `SUPERFUN` como segundo layer mas alto, solo por debajo de `BLOCKED`.

Activar `SUPERFUN` desde `FUN` en la posicion de `\` con hold-tap: hold activa
`SUPERFUN`, tap envia `BACKSLASH`.

Asignar `F13` a `F24` a las posiciones `1` a `0`, `Q` y `W` dentro de
`SUPERFUN`.

## Consequences

`BLOCKED` pasa de indice 7 a 8. La configuracion del PMW3610 debe bloquear el
trackball en `BLOCKED` usando el nuevo indice.

Los keycodes `F13` a `F24` son adecuados para atajos por sistema operativo porque
suelen estar libres y no dependen del layout del teclado.

## Reversal strategy

Eliminar `SUPERFUN`, restaurar `BLOCKED` a indice 7 y devolver
`trackball_lock.layers` al valor anterior.
