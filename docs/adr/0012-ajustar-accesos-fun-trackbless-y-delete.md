# ADR 0012: Ajustar accesos FUN, TRACKBLESS y DELETE

## Estado

Aceptado.

## Contexto

La tecla 54 estaba dedicada a `SNIPE`, pero `FUN` necesita un acceso global
momentaneo y persistente. Ademas, `TRACKBLESS` no debe activarse por doble tap
desde cualquier layer, porque eso introduce cambios accidentales en capas de
trabajo normal.

## Decision

Definir `hold_fun_toggle`:

```c
hold_fun_toggle: hold_fun_toggle {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    tapping-term-ms = <200>;
    flavor = "hold-preferred";
    bindings = <&mo>, <&td_fun_toggle>;
};
```

Usar la posicion 54 en todos los layers con:

```c
&hold_fun_toggle FUN 0
```

El hold activa `FUN` momentaneamente. El doble tap alterna `FUN` mediante
`td_fun_toggle`.

Limitar el doble tap de posiciones 55 y 58 para alternar `TRACKBLESS` a:

- `TRACKBLESS`;
- `SCROLL`.
- `MOUSE`.

En `MOUSE`, la posicion 58 conserva tap para volver a `QWRT`, y doble tap
alterna `TRACKBLESS`.

En `SCROLL`, asignar tambien `DELETE` a la tecla fisica `]`, igual que en
`SYM`.

## Consecuencias

`FUN` queda disponible desde cualquier capa sin depender de `SNIPE`. El acceso a
`TRACKBLESS` por doble tap queda limitado a capas donde tiene sentido operar el
modo bloqueado o salir del modo puntero.

`SNIPE` deja de tener acceso directo desde la posicion 54.

## Archivos afectados

- `config/keyball61.keymap`

## Reversal strategy

Restaurar la posicion 54 a `SNIPE` y devolver los tap-dances de `TRACKBLESS` a
los layers donde estaban antes.
