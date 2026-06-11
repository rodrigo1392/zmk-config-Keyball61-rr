# ADR 0014: Ajustar salida MOUSE/SCROLL

## Estado

Aceptado.

## Contexto

`MOUSE` necesitaba una forma directa de entrar a `SCROLL` desde la posicion 57,
tanto momentanea como persistente. Ademas, despues de alternar `SCROLL`, otro
doble tap en la misma posicion debia volver a `QWRT`.

En `SCROLL`, `BSPC` seguia heredandose como backspace, pero en esta capa resulta
mas util que sea `DEL`.

## Decision

En `MOUSE`, posicion 57:

```c
&hold_scroll_toggle SCROLL 0
```

En `SCROLL`, posicion 57:

```c
&td_scroll_qwrt
```

`td_scroll_qwrt` no hace nada con un tap y vuelve a `QWRT` con doble tap:

```c
bindings = <&none>, <&to DEFAULT>;
```

En `SCROLL`, la posicion fisica de `BSPC` envia `DEL`.

## Consecuencias

Desde `MOUSE`, mantener la posicion 57 activa `SCROLL` momentaneamente. Doble
tap en esa posicion alterna `SCROLL`. Ya en `SCROLL`, otro doble tap en la
misma posicion vuelve a `QWRT`.

`SCROLL` deja de heredar backspace en la posicion `BSPC` y pasa a enviar delete.

## Reversal strategy

Restaurar la posicion 57 de `MOUSE` y `SCROLL` a `&trans`, quitar
`td_scroll_qwrt`, y devolver la posicion `BSPC` de `SCROLL` a `&trans`.
