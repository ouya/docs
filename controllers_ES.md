## Controles

Una de las grandes ventajas de la consola OUYA es que los jugadores pueden usar un control *real*! El control de OUYA tiene:
- cuatro botones digitales (O, U, Y, A)
- una almohadilla digital de cuatro direcciones (D-Pad)
- dos joysticks analógicos (LS, RS)
- dos botones digitales que se activan cuando los joysticks son presionados (L3, R3)
- dos botones digitales frontales (L1, R1)
- dos gatillos analógicos (L2, R2)
- un touchpad, configurado para comportarse como entrada de ratón (Touchpad)

**Nota:** Los gatillos analógicos también actúan como botones digitales, con un umbral de 0.5 para el valor analógico.

Debido a que las interfaces del control son cruciales, hemos trabajado en hecer su vida más fácil.

##### Constantes

La clase **OuyaController** contiene constantes especificas de OUYA para botones y ejes. Abajo se muestra una pequeña selección.

```java
public static final int BUTTON_O;
public static final int BUTTON_U;
public static final int BUTTON_Y;
public static final int BUTTON_A;
```

Con estas constantes a mano, es totalmente aceptable manejar la entrada usando métodos estándar como **onKeyDown**, **onKeyUp**, o **onGenericMotionEvent**.

##### Consulta de estado en cualquier momento

Si quiere la flexibilidad extra de consultar el estado del control en cualquier momento, puede usar el resto de la clase **OuyaController**.
En primer lugar, esto significa que su actividad debe redirigir las llamadas de **onKeyDown**, **onKeyUp**, o **onGenericMotionEvent** hacia **OuyaController**:

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
    boolean handled = OuyaController.onKeyDown(keyCode, event);
    return handled || super.onKeyDown(keyCode, event);
}

@Override
public boolean onKeyUp(int keyCode, KeyEvent event) {
    boolean handled = OuyaController.onKeyUp(keyCode, event);
    return handled || super.onKeyUp(keyCode, event);
}

@Override
public boolean onGenericMotionEvent(MotionEvent event) {
    boolean handled = OuyaController.onGenericMotionEvent(event);
    return handled || super.onGenericMotionEvent(event);
}
```

Una vez que **OuyaController** esté recibiendo los eventos, podrá obtener una instancia de la clase de dos maneras: id de dispositivo (device id)  o número de jugador (player number):

```java
OuyaController c = OuyaController.getControllerByDeviceId(deviceId);
OuyaController c = OuyaController.getControllerByPlayer(playerNum);
```

Ahora es sencillo consultar los valores de botón o de eje:

```java
float axisX = c.getAxisValue(OuyaController.AXIS_LSTICK_X);
float axisY = c.getAxisValue(OuyaController.AXIS_LSTICK_Y);
boolean buttonPressed = c.getButton(OuyaController.BUTTON_O);
```

Con esto puede enfocarse en hacer un gran juego en lugar de hacerlo en el manejo de las entradas.
