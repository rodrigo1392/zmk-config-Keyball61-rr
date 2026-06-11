# ADR 0008: Ajustar trackball y controles basicos sobre base QWRT

## Estado

Aceptado.

## Contexto

Despues de volver al comportamiento base del repo original, se necesitan pocos
ajustes ergonomicos para empezar una nueva configuracion desde una base simple:

- mayor respuesta del trackball;
- activacion de mouse por movimiento con umbral explicito;
- acceso momentaneo a `SYM` desde la posicion 57;
- espacio directo en la posicion 58;
- `Enter` en `SYM` desde la posicion 58;
- clicks y multimedia basicos en `MOUSE`.

## Decision

Configurar el PMW3610 con:

```conf
CONFIG_PMW3610_CPI=1200
CONFIG_PMW3610_CPI_DIVIDOR=1
CONFIG_PMW3610_AUTOMOUSE_TIMEOUT_MS=700
CONFIG_PMW3610_MOVEMENT_THRESHOLD=1200
```

Modificar `QWRT`:

- posicion 57: `&mo SYM`;
- posicion 58: `&kp SPACE`.

Modificar `SYM`:

- posicion 58: `&kp ENTER`.

Modificar `MOUSE`:

- `A`: click derecho;
- `S`: click medio;
- `D`: click izquierdo;
- `6`: bajar volumen;
- `7`: subir volumen;
- `8`: pausa/play;
- `9`: cancion anterior;
- `0`: cancion siguiente.

## Consecuencias

El trackball queda mas sensible que la base original y exige mas movimiento para
activar automouse. La capa base recupera espacio directo en la posicion 58 y
usa la posicion 57 como acceso momentaneo a simbolos.

La capa `MOUSE` vuelve a tener acciones frecuentes sin reintroducir los modos
experimentales de toggle, lock o tap-dance.

## Archivos afectados

- `config/keyball61.keymap`
- `config/boards/shields/keyball61/keyball61_right.conf`

## Reversal strategy

Para volver al comportamiento base, restaurar `CONFIG_PMW3610_CPI_DIVIDOR=4`,
`CONFIG_PMW3610_MOVEMENT_THRESHOLD=0` y copiar las posiciones afectadas desde
`zmk-config-Keyball61`.
