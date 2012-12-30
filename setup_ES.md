## Instrucciones de instalación para el ODK de OUYA

##### MacOS
Descargue e instale el  Android SDK y sus herrmientas a su Mac o PC, siguiendo las instrucciones en http://developer.android.com/sdk/index.html. Cuando llegue a la parte donde ejecuta android sdk instale lo siguiente:
- Herramientas, incluyendo ambas herramientas - Android SDK Tools y Android SDK Platform -
- Android 4.1(API 16): SDK Platform
- Android 4.0 (API 14): SDK Platform
- Extras: Android Support Library

Note que en lugar del android sdk, puede que sea necesario utilizar el siguiente comando:
```bash
./android sdk
```

Luego de ejecutar la herramienta SDK Manager, instale el runtime de Java  si le es solicitado.

Puede necesitar añadir algunas rutas al PATH. Asumiendo que ha colocado la carpeta del sdk  en la siguiente ubicación: ~/android/android-sdk-macosx, abra una terminal y agregue las tres líneas siguientes a su .bashrc:
```bash
export PATH=$PATH:~/android/android-sdk-macosx/tools
export PATH=$PATH:~/android/android-sdk-macosx/platform-tools
export ANDROID_HOME=~/android/android-sdk-macosx
```

##### Windows
A ser descrito

##### Eclipse
A ser descrito

##### Desarrollando con el ODK
Utilice el API de Android nivel 16 (Android 4.1 "Jelly Bean") cuando desarrolle para la consola OUYA.

Para utilizar las APIs de OUYA necesita incluir ouyasdk.jar a las librerías de su proyecto, así como guava-r09.jar y commons-lang-2.6.jar. Estas se encuentran el directorio libs.

Para más información de las APIs disponibles por favor consulte la documentación de referencia de la API.

Para ejecutar el código de ejemplo, abra el proyecto que se encuentra en iap-sample-app y siga las instrucciones del archivo README.txt.

Para que su aplicación o juego sea reconocido como hecho para OUYA, necesitará incluir una categoria intent de OUYA en la entrada de su actividad principal en el manifiesto. Utilice “ouya.intent.category.GAME” o “ouya.intent.category.APP”.
```xml
<activity android:name=".GameActivity" android:label="@string/app_name">
  <intent-filter>
    <action android:name="android.intent.action.MAIN"/>
    <category android:name="android.intent.category.LAUNCHER"/>
    <category android:name="ouya.intent.category.GAME"/>
  </intent-filter>
</activity>
```

La imagen de la aplicación que es mostrada en el launcher esta embebida dentro del APK. El archivo esperado se encuentra en res/drawable-xhdpi/ouya_icon.png y la imagen debe ser 732x412 para juegos o 412x412 para aplicaciones.

#### Substitutos para el Hardware

Para poder comenzar a desarrollar software antes de tener acceso a una consola OUYA, puede utilizar el emulador de Android o una tablet estándar con Android.


##### Software

El harware de la consola OUYA tiene incluido el launcher de OUYA, pero cuando utilice un emulador o una tablet con Android necesitará instalar el launcher manualmente. Ese archivo esta incluido en el paquete OUYA ODK.

Para instalar el launcher ejecute:
```bash
adb install -r ouya-framework.apk
adb install -r ouya-launcher.apk
```

Si el launcher de OUYA no ha sido instalado algunas características del ODK no funcionarán correctamente.

##### Emulador

Si está usando el emulador, configure el dispositivo virtual de Android de la siguiente manera:
- Resolution: 1920x1080 o 1280x720, según sea necesario.
- Hardware Back/Home keys: yes (necesitará agregar esto a los parámetros de hardware)
- DPad support: yes (necesitará agregar esto a los parámetros de hardware)
- Target: Android 4.1 - API Level 16
- CPU/ABI: Intel Atom x86
- Device ram size: 1024

Recomendamos el uso de las extenciones Intel Atom x86 CPU/ABI e Intel's HAXM para asegurar que el desempeño del emulador sea el adecuado para desarrollo de juegos. Si está desarrollando código de bajo nivel debe recordar que el dispositivo está basado en ARM y por lo tanto debe desarrollar para la arquitectura ARM y utilizar un emulador AVD con el CPU/ABI asignado a la arquitectura ARM.

La consola OUYA no tiene botones de hardware para "Atrás" o "menú", y los juegos no deben confiar en la presencia de estos. Asignando las teclas de hardware a las propiedades del emulador oculta la barra de navegacion de Android, permitiendo que el emulador ocupe completamente una pantalla de 1920x1080 o 1280x720 de la misma manera en que la consola OUYA lo hace.

Cuando se desarrolla con el emulador, no es posible emular completamente los botones y capacidades del control de OUYA.

##### Tablet Android

Si está utilizando una tablet estándar de Android, recomendamos utilizar una que tenga una resolución tan cercana como sea posible a 1920x1080 o 1280x720. Note que la barra de navegacion de Android consumirá espacio de la pantalla del dispositivo, lo cual no ocurrirá en la consola OUYA.

El control de OUYA combina un control estándar (dos joysticks, un D-Pad, cuatro botones, dos botones frontales y dos gatillos) con un touchpad. Para pruebas recomendamos utilizar el control de Xbox 360 con cable USB conbinado con un touchpad o ratón para probar la interacción de los joysticks y botones. 

##### Manejo de Resolución de Pantalla.

La consola OUYA sólo soporta  salida de 720P o 1080P.

Para juegos basados en OpenGL, recomendamos crear un buffer de renderizado de 1920x1080 si está dirigido a 1080P o 1280x720 para 720P. Si esto no encaja con la resolución de pantalla de su dispositivo, la ventana de juego puede aparecer dentro de una ventana o escalada dependiendo del comportamiento determinado por el creador.

Para juegos utilizando Android UI Framework, siga las mejores prácticas de Android para desarrollo de aplicaciones orientadas a pantallas xhdpi-large y tvdpi-large. Para mayor informacion dirijase a [developer.android.com](http://developer.android.com).
