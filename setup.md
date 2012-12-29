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

Recomendamos el uso de las extenciones Intel Atom x86 CPU/ABI e Intel's HAXM para asegurar que el desempeño del emulador sea el adecuado para desarrollo de juegos.....
We recommend the use of the Intel Atom x86 CPU/ABI and Intel's HAXM extensions to ensure the emulator performance is adequate for game development. If you are developing low level code you should note that the device is ARM based and therefore you should develop for the ARM architecture and use an emulator AVD with the CPU/ABI set to an ARM architecture.

The OUYA console does not have hardware buttons for back or menu, and games should not rely on the presence of these. Setting the hardware keys emulator property hides the Android navigation bar, enabling the emulator to fill the entire 1920x1080 or 1280x720 screen in the same way the OUYA console does.

When developing with the emulator, it is not possible to fully emulate the OUYA controller buttons and features. 

##### Android Tablet

If using a standard Android tablet, we recommend using a tablet with a usable display resolution as close as possible to 1920x1080 or 1280x720. Note that the Android navigation bar will consume some of the screen on standard tablets, which will not be the case for the OUYA console.

The OUYA game controller combines a standard controller (two joysticks, a D-Pad, four game buttons, two shoulder buttons, and two triggers) with a touchpad. For testing, we recommend using the Xbox 360 wired USB controller combined with a mouse or touchpad for testing joystick and game button interaction. 

##### Screen Resolution Handing

The OUYA console supports 720P or 1080P output only.

For OpenGL-based games, we recommend creating a render buffer that's 1920x1080 if targeting 1080P or 1280x720 if targeting 720P. If this does not match the display resolution of your device, the game screen may be windowed or scaled depending on the behavior determined by the device manufacturer.

For games using the Android UI Framework, follow the Android best practices for developing applications for xhdpi-large and tvdpi-large displays. See [developer.android.com](http://developer.android.com) for more information.
