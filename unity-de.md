1. Download
Wir brauchen verschiedene Dateien, um mit Unity für die Ouya entwickeln zu können:
- Java JDK 1.6.045 32bit(!!!)
- Android SDK mit API 16
- Android NDK
und natürlich das Ouya Plugin von GitHub, welches dort als zip heruntergeladen werden kann.

2. Plugin
Nachdem alles heruntergeladen wurde entzippen wir das Plugin an einen zentralen Ort. Darin befinden sich folgende Dateien:
 

In Unity öffnen wir nun ein neues Projekt (auch wenn es sich um ein bestehendes Spiel handelt). Wir navigieren zu den entzippeden Dateien, denn hierbei handelt es sich um ein Unity Projekt.
Nachdem Unity das Projekt geöffnet hat, starten wir Unity neu, um die Plugins zu laden. Wichtig: Nichts im Projekt verändern, sonst funktioniert es nicht mehr!

 

Nach dem Neustart befinden sich in der Menüleiste von Unity zwei neue Reiter "OUYA" und "NGUI". Wir klicken auf "OUYA" und Exportieren das "Core Package", das "StarterKit Package" und das "Examples Package". Es empfiehlt sich, diese an einen Ort zu speichern, da wir diese gleich benötigen.
Nun öffnen wir mit Unity unser Spiel, das wir auf die Ouya-Konsole bringen wollen. Nachdem das Projekt geladen hat, öffnen wir den Windows Explorer und navigieren zu dem Ordner, indem wir die Packages exportiert haben. Diese ziehen wir einfach in die Assets unseres Spiels.
Nach einem etwas länger dauerndem Import wird Unity wieder neugestartet. Um zu überprüfen, ob alles geklappt hat gehen wir unter "Edit" dann "Project Settings" und wählen "Input". Wenn das Plugin richtig importiert wurde, gibt es hier nun 121 Axen (für die möglichen 4 Controller, die man mit der Ouya verbinden kann). Das Plugin ist nun korrekt importiert und kann eingerichtet werden.

3. Einrichten des Plugins, Installation
Zunächst stellen wir fest, ob unsere gewählte Plattform Android ist, das die Ouya-Konsole auf Android basiert. Seit Mai ist das Android Plugin kostenlos für Unity verfügbar, und somit ist auch die Entwicklung für die Ouya kostenfrei.
Dazu gehen wir unter "File" dann "Build Settings" und wählen dann Android aus. Anschließend klicken wir auf "Switch Platform", sofern Android nicht schon ausgewählt ist.

 

Nun installieren wir die Android SDK. Dies hier zu erklären, würde den Rahmen des Tutorials sprengen, es gibt aber genügend Tutorials zu diesem Thema im Internet. Nach erfolgreicher Installation der SDK, und dem Download der API 16 (die wir für die OUYA brauchen), muss der Pfad der SDK noch in Unity angegeben werden: Und zwar einmal für Unity und für das Ouya-Plugin.

Zunächst öffnen wir "Edit" und "Preferences". Im sich öffnenden Fenster wählen wir den Reiter "External Tools", und können hier den Pfad der Android SDK angeben.

 

Nun öffnen wir das "Ouya Panel". Dazu gehen wir unter "Window" in Unity auf "Open Ouya Panel". Das sich öffnende Fenster ist das Kontrollzentrum für die Ouya-Konsole. Hier kann überprüft werden, ob der Code stimmt und (für Unity Pro) auch direkt die apk-Datei erstellt werden. Uns interessiert zunächst der Button "Android SDK", da hier nocheinmal der Pfad für die SDK angeben werden muss. Wenn dies erfolgreich war, werden die Pfadangaben scharz gedruckt, sollte etwas fehlen (zum Beispiel die API16) wird dies als grau angezeigt.

 

Nun installieren wir die heruntergeladene Java JDK und die Android NDK. Nach der Installation wird im Ouya-Panel unter dem jeweiligen Button der Pfad der installierten Dateien angeben - das heißt wir sollten uns diesen bei der Installation möglichst merken. Wenn wir alles richtig gemacht haben, ist kein Pfad schwarz gedruckt - dann hat das Ouya-Panel alle Dateien gefunden.
Bei "Android NDK" könnte der Pfad zu "NDK Make" graugedruckt sein: Dazu gibt es den Button "Select NDK Make Path...". Die make.exe lag bei mir an einer anderen Stelle als vorgesehen, vielleicht ist das bei euch ja auch der Fall.

 

Nun müssen nur noch Einstellungen in den Build Settings von Android vorgenommen werden, dann können wir auch schon für die Ouya builden.

4. Build Settings
Wir gehen zurück zu den Build Settings, und öffnen dort die "Player Settings" von Android. Hier können nun alle für einen Android Build relevanten Dinge, wie der Name der App oder das Icon festgelegt werden. Auch hier empfiehlt es sich anderweitig nachzusehen, was alles gebraucht wird. Wir sehen uns nur die Einstellungen an, bei denen sich die Ouya von einem normalen Android Build unterscheidet. Unter "Resolution and Presentation" muss die Orientation auf "Landscape Left" gestellt werden - alles andere würde unser Spiel auf dem Kopf starten lassen.
Unter "Other Settings" muss das API Level auf 16 gestellt werden. Dazu wählen wir unter "Minimum API Level" die Android Version 4.1 "Jelly Bean" aus, bei der es sich um Version 16 handelt.
Nun sollten wir noch den Bundle Identifier eingeben und unserem Spiel einen Namen und eine Firma geben - das ist das mindeste was für einen Android Build gebraucht wird.

5. OUYA Szene
Bevor wir das Spiel builden können müssen wir noch die Ouya-Szene mit dem Ouya-GameObject in unser Projekt einfügen. Diese finden wir in den Assets unter "OUYA" -> "StarterKit" -> "Scenes" und dann "SceneInit". Diese Szene öffnen wir und bearbeiten das "OuyaGameObject". Erstens entfernen wir den Haken bei "Use Legacy Input" (da wir später einen OuyaInput in unseren Scripten benutzen) und geben unter "DEVELOPER_ID" unser ID an, die wir beim Einloggen in unser Developer Konto bei devs.ouya.tv auf dem Startbildschirm präsentiert bekommen. Nun müssen wir noch beim "OuyaSceneInit" Objekt die erste Szene unseres Spieles angeben, die beim Starten des Spiels geladen werden soll.

 

Die fertige Szene wird gespeichert und in den "Player Settings" vor alle andern gesetzt. Nun wird beim Starten des Spiels als erstes die OuyaInitScene geladen, die sich mit unserem Entwicklerkonto verbindet und dann automatisch das eigentliche Spiel lädt.

6. Builden
Das wars auch schon, nun können wir unser Projekt im "OuyaPanel" mit den PlayerSettings verknüpfen um den Build zu ermöglichen - Dazu wird unter dem Button "OUYA" der Button "Sync Bundle ID" ausgewäht. Das Ouya-Plugin synchronisiert nun den Namen und den Bundle Identifier mit dem Android Manifest, welches für die Ouya benötigt wird.

 

Anschließend können wir auf "Compile" klicken um unser Projekt von dem Ouya-Plugin überprüfen zu lassen. Sollten in der Console keine kritischen Fehler auftauchen, ist das Projekt fertig zum exportieren. Besitzer einer Pro-Lizenz können dies direkt aus dem Ouya-Panel heraus, Standartbenutzern ist das nicht erlaubt. Das macht allerdings nichts, denn das Ouya-Panel arbeitet genauso mit dem Android-Builder von Unity. Wir gehen also wieder in die "Player Settings" und klicken dort auf den Button "Builden". Die erzeugte apk-Datei kann auf die Ouya geladen werden und dort erfolgreich gestartet werden.


Fertig! Dein Game läuft nun auf der OUYA. Im nächsten Teil kümmern wir uns um den Input der Ouya-Controller, 
