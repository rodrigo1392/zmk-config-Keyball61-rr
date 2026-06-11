# ADR 0015: Ajustar FUN numerico y scroll en MOUSE

## Estado

Aceptado.

## Contexto

`MOUSE` necesita otro acceso directo a `SCROLL` desde `;`. Ademas, `FUN`
necesita una distribucion numerica y de operadores sobre teclas existentes del
lado derecho.

## Decision

En `MOUSE`, `;` activa `SCROLL` momentaneamente:

```c
&mo SCROLL
```

En `FUN`, asignar:

| Tecla base | FUN |
| --- | --- |
| `]` | `0` |
| `n` | `1` |
| `m` | `2` |
| `h` | `3` |
| `j` | `4` |
| `k` | `5` |
| `y` | `6` |
| `u` | `7` |
| `i` | `8` |
| `6` | `9` |
| `7` | `-` |
| `8` | `+` |
| `9` | `*` |
| `o` | `/` |
| `.` | `=` |

## Consecuencias

`MOUSE` gana acceso momentaneo a `SCROLL` desde la fila home derecha. `FUN`
conserva las teclas F existentes en el lado izquierdo y agrega numeros y
operadores en el lado derecho.

## Reversal strategy

Restaurar esas posiciones de `MOUSE` y `FUN` a `&trans`.
