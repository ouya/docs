Chromium
========

OUYA fork of Chromium

The purpose of Chromium on Android is to achieve a hardware accelerated WebView capable of playing HTML5 games, rendering browser content, and streaming accelerated video. HTML5 game engines like Twine (text adventure games), Construct 2 (visual-scripting-engine), Gootechnologies (WebGL acceleration), and HTML5 browser games all need to make use of an HTML5 rendering engine that supports hardware acceleration. The previously mentioned engines will have their own documentation that extends this base Chromium documentation.

The steps below is an extension that begins from the Chromium trunk and make customizations to support the OUYA controller and in-app-purchases. You will need to build a Ubuntu 12.04 Linux guest in order to build Chromium from the trunk. There are a set of dependencies that need installed before you can obtain the Chromium source. Some customization to the build files and tweaks to the source code are necessary to add controller and ODK functionality.

The Chromium apps also have flags that enable or change the default behaviour. These options are configurable from a file and the flags can be hardcoded in the settings once you find the optimal flag settings. One example of Chromium flags the spatial navigation feature.

More
====


First thing, the Chromium code-base is very large. <a target=_blank href="https://code.google.com/p/chromium/codesearch#chromium/src/&q=&sq=package:chromium">[Searching]</a> the code will help reveal the size of the source. Rather than fork multiple repros, this assumes you've already cloned the chromium trunk.

These files are intended to supplement the trunk.

Some customization of the content-shell via content-flags are necessary.

Create a text file content-shell-command-line with the following content, these are the content flags.

<table border="1"><tr><td>
content_shell chrome --js-flags=--expose-gc --enable-dcheck --disable-gesture-requirement-for-media-playback --v=1 --vmodule=chunk*=1,source*=1,media*=1,webmed*=1,*=0 --webcore-log-channels=Media --no-restore-state --tablet-ui --tablet-ui --force-compositing-mode --allow-webui-compositing --enable-threaded-compositing --enable-fixed-position-compositing --enable-accelerated-overflow-scroll --enable-accelerated-scrollable-frames --enable-composited-scrolling-for-frames --enable-begin-frame-scheduling --enable-deadline-scheduling --disable-gesture-debounce --enable-gesture-tap-highlight --enable-pinch --enable-overlay-fullscreen-video --enable-overlay-scrollbars --enable-overscroll-notifications --touch-ack-timeout-delay-ms=200 --in-process-gpu --disable-gpu-shader-disk-cache --enable-viewport --enable-viewport-meta --main-frame-resizes-are-orientation-changes --disable-composited-antialiasing --ui-prioritize-in-gpu-process --profiler-timing=0 --composite-to-mailbox --gpu-driver-bug-workarounds=0,23,26,34 --disable-gpu-watchdog chrome
</td></tr><table>

Deploy the file and restart chrome-shell.

<table border="1"><tr><td>
adb push content-shell-command-line /data/local/tmp/content-shell-command-line
</td></tr><table>

The build/deploy/run command. Run from the command from the chromium/src folder.

<table border="1"><tr><td>
ninja -C out/Release -j10 content_shell_apk && build/android/adb_install_apk.py --apk ContentShell.apk --release && build/android/adb_run_content_shell
</td></tr></table>

Altered file list:

<pre>
.gclient
\---src
    +---content
    |   |   content_shell.gypi
    |   |
    |   +---public
    |   |   +---android
    |   |   |   \---java
    |   |   |       \---src
    |   |   |           \---org
    |   |   |               \---chromium
    |   |   |                   \---content
    |   |   |                       \---browser
    |   |   |                           |   ContentView.java
    |   |   |                           |   ContentViewClient.java
    |   |   |                           |   ContentViewCore.java
    |   |   |                           |
    |   |   |                           \---input
    |   |   |                                   ImeAdapter.java
    |   |   |
    |   |   \---common
    |   |           content_client.cc
    |   |
    |   +---renderer
    |   |   web_preferences.cc
    |   |
    |   \---shell
    |       \---android
    |           +---java
    |           |   +---res
    |           |   |   \---layout
    |           |   |           shell_view.xml
    |           |   |
    |           |   \---src
    |           |       \---org
    |           |           \---chromium
    |           |               \---content_shell
    |           |                       Shell.java
    |           |                       ShellManager.java
    |           |
    |           \---shell_apk
    |               |   AndroidManifest.xml
    |               |
    |               +---libs
    |               |       ouya-sdk.jar
    |               |
    |               +---res
    |               |   +---drawable
    |               |   |       app_icon.png
    |               |   |
    |               |   +---drawable-xhdpi
    |               |   |       ouya_icon.png
    |               |   |
    |               |   \---layout
    |               |           content_shell_activity.xml
    |               |
    |               \---src
    |                   \---org
    |                       \---chromium
    |                           \---content_shell_apk
    |                                   ContentShellActivity.java
    |
    +---media
    |   \---base
    |       \---android
    |           \---java
    |               \---src
    |                   \---org
    |                       \---chromium
    |                           \---media
    |                                   MediaCodecBridge.java
    |
    \---ui
        \---events
            \---keycodes
                    keyboard_code_conversion_android.cc
</pre>

<b>Turn on spatial navigation</b>

<table border="1"><tr><td>
src/content/renderer/web_preferences.cc
</td></tr>
<tr><td>
<pre>
#if defined(OS_ANDROID)
  settings->setSpatialNavigationEnabled(true);
#endif
</pre>
</td></tr><table>

<b>Quick ways to browse a specific URL</b>

<table border="1"><tr><td>
* The URL toolbar can be toggled.
<pre>
mShellManager.showToolbar();

mShellManager.hideToolbar();
</pre>
</td></tr><table>

<table border="1"><tr><td>
* Run activity manager to use an intent filter and set the URL via the command-line.
</td></tr>
<tr><td>
adb shell am start -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -n org.chromium.content_shell_apk/org.chromium.content_shell_apk.ContentShellActivity -d http://google.com
</td></tr><table>

<b>Check Capabilities</b>
<table border="1"><tr><td>
adb shell am start -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -n org.chromium.content_shell_apk/org.chromium.content_shell_apk.ContentShellActivity -d chrome://gpu
</td></tr><table>

<h1>Setting up the build environment</h1>

* Install the <a target=_blank href="http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase6-419409.html#jdk-6u45-oth-JPR">[JDK6 1.6]</a> from the Oracle archives.<br/>
<br/>

<table border="1"><tr><td><pre>
cd ~/Downloads
chmod +x ./jdk-6u45-linux-x64.bin
./jdk-6u45-linux-x64.bin
</pre></td></tr></table>

* Add Java to your environment.<br/>

<table border="1"><tr><td>
Add this to the end of the ~/.bashrc file.
</td></tr>
<tr><td><pre>
export JAVA_HOME=~/Downloads/jdk1.6.0_45
export PATH=$PATH:~/Downloads/jdk1.6.0_45/bin
</pre></td></tr></table>

* Install <a target=_blank href="https://developer.android.com/sdk/installing/studio.html">[Android Studio]</a>. This is the easiest way to get the Android Build Tools and SDK needed for the build.<br/>
<br/>

<table border="1"><tr><td><pre>
cd ~/Downloads
tar zxvf android-studio-bundle-133.970939-linux.tgz
cd ~/Downloads/android-studio/bin
./studio.sh
</pre></td></tr></table>

* Launch the SDK Manager from inside Android Studio (Tools -> Android -> SDK Manager) and configure it so it matches at least the following:<br/>
<br/>

<table border="1"><tr><td><pre>
* Android SDK Tools 22.3
* Android SDK Platform-Tools 19
* Android 4.3 (API 18) SDK Platform
* Extras Android Support Repository
* Extras Android Support Library
* Extras Google Repository
</pre></td></tr></table>

* <a target=_blank href="https://code.google.com/p/chromium/wiki/LinuxBuildInstructions">[Instructions]</a> for setting up the Chromium Build Environment.<br/>
<br/>

* Create a Virtual Machine in (VMWare Workstation/Fusion or Virtual Box) with <a target=_blank href="http://www.ubuntu.com/download/desktop">[Ubuntu 12.04 LTS Desktop 64-bit]</a> as your guest image. VMWare is recommended as you don't need to export/import to backup/restore the virtual machine image.<br/>
<br/>

* For the guest, you want about 80GB of disk space to handle compiling Chromium. You'll want 160GB if you need the space to duplicate the compiled source or to archive the folder for backup.<br/>
<br/>

* The guest needs a minimum of 2GB of RAM to compile, 4GB is recommended.<br/>
<br/>

* I typically turn off the power settings and lock screen for guests, since the host can handle the lock screen. Open All Settings-&gt;Brightness and Lock. Set inactive screen to "Never". Turn off the "Lock". Turn off "Require my password when waking up from suspend". That way your VM is always responsive and you don't miss anything while compiling.<br/>
<br/>

<b>(After you've created a VM and installed Ubuntu):</b><br/>
<br/>

* Install the build dependencies, which are listed at <a target=_blank href="http://code.google.com/p/chromium/wiki/LinuxBuildInstructionsPrerequisites">[LinuxBuildInstructionsPrerequisites]</a>.<br/>
<br/>

* Install the depot_tools utilities, a process that is documented at <a target=_blank href="http://dev.chromium.org/developers/how-tos/install-depot-tools">[install-depot-tools]</a>.<br/>

<table border="1"><tr><td><pre>
cd ~/Downloads
sudo apt-get install git
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
</pre></td></tr></table>

<h1>Getting chromium from trunk</h1>

Guide: <a target=_blank href="https://sites.google.com/a/chromium.org/dev/developers/how-tos/get-the-code">[how-to-get-the-code]</a>.<br/>
<br/>

These are the quick steps that I use. Open a terminal.

<b>Make/change to folder</b>
<table border="1"><tr><td>
<pre>
mkdir ~/Documents/chromium
cd ~/Documents/chromium
</pre>
</td></tr><table>

<b>Make sure the tools are in your path:</b>
<table border="1"><tr><td>
export PATH="$PATH":`echo ~`/Downloads/depot_tools/
</td></tr><table>

<b>Get the Android source</b>
<table border="1"><tr><td>
fetch android --nosvn=True
</td></tr><table>

<b>Sync the code</b>
<table border="1"><tr><td>
<pre>
cd ~/Documents/chromium/src
git pull --rebase origin master
gclient sync
</pre>
</td></tr><table>

<b>GIT is needed to get the source.</b>
<table border="1"><tr><td>
<pre>
sudo apt-get install git-core gitk git-gui subversion curl lib32z1 ant
</pre>
</td></tr><table>

<b>Get the dependencies, you may need to run several times if 'gclient sync' returns errors. You should see an agreement if you've run it enough times.</b>
<table border="1"><tr><td>
./build/install-build-deps.sh
</td></tr><table>

<b>Enable the Android flag, edit the .gclient file (for trunk)</b>
<table border="1"><tr><td>
vi ../.gclient
</td></tr><table>

<table border="1"><tr><td>
solutions = [{u'managed': False, u'name': u'src', u'url': u'https://chromium.googlesource.com/chromium/src.git', u'custom_deps': {}, u'deps_file': u'.DEPS.git', u'safesync_url': u''}]
target_os = ['android']
</td></tr></table>

<b>Sync the code again</b>
<table border="1"><tr><td>
<pre>
git pull --rebase origin master
gclient sync
</pre>
</td></tr><table>

<b>Make sure adb is in your path. Add the Android SDK paths for your location to the end of your ~/.bashrc file.</b>
<table border="1"><tr><td>
export PATH=$PATH:~/Downloads/android-studio/sdk/platform-tools/:~/Downloads/android-studio/sdk/tools/
</td></tr><table>

<b>Make sure device is connected to adb</b>
<table border="1"><tr><td>
adb connect IP:port
</td></tr><table>

<b>Compile</b>
<table border="1"><tr><td>
<pre>
export CHROME_SRC=~/Documents/chromium/src
. build/android/envsetup.sh
android_gyp
ninja -C out/Release content_shell_apk
</pre>
</td></tr><table>

<b>Building updates and launching:</b>

<table border="1"><tr><td>
ninja -C out/Release -j10 content_shell_apk && build/android/adb_install_apk.py --apk ContentShell.apk --release && build/android/adb_run_content_shell
</td></tr></table>

At this point, copy the files from this repository into /path/to/chrome.

Repeat the build and launching step.

And then apply the chrome-shell flags from above.

<h1Running the Chromium profiler</h1>

<b>Make sure there's an output folder and give write permissions</b>
<table border="1"><tr><td>
<pre>
adb shell mkdir sdcard/Download
chmod 777 /sdcard/Download
</pre>
</td></tr><table>

<b>Login with Super User</b>
<table border="1"><tr><td>
<pre>
adb shell
su
</pre>
</td></tr><table>

<b>Start the trace (limit to 10 seconds)</b>
<table border="1"><tr><td>
am broadcast -a org.chromium.content_shell_apk.GPU_PROFILER_START -e file /sdcard/Download/trace.txt
</td></tr><table>

<b>Stop the trace</b>
<table border="1"><tr><td>
am broadcast -a org.chromium.content_shell_apk.GPU_PROFILER_STOP
</td></tr><table>

<b>Pull the file for analysis</b>
<table border="1"><tr><td>
adb pull /sdcard/Download/trace.txt
</td></tr><table>

<b>Browse the trace file in your desktop Chrome browser</b>
<table border="1"><tr><td>
<a target=_blank href="chrome://tracing/">chrome://tracing/</a>
</td></tr><table>

Check out more info on the <a target=_blank href="Tracing tool - http://www.chromium.org/developers/how-tos/trace-event-profiling-tool/recording-tracing-runs">[tracing tool]</a>.

<b>Keyboard definitions</b>
<table border="1"><tr><td>
ui/events/keycodes/keyboard_codes_posix.h
</td></tr><table>

<b>Any KeyEvents that map to KEYCODE_UNKNOWN can be added to the mapping.</b>
<table border="1"><tr><td>
ui/events/keycodes/keyboard_code_conversion_android.cc
</td></tr><table>

<h1>IDE</h1>

* <a target=_blank href="http://www.chromium.org/developers/sublime-text">Sublime Text</a> makes a good editor for working with the Chromium source.

<h1>Debugging</h1>

* <a target=_blank href="https://developers.google.com/chrome-developer-tools/docs/remote-debugging">Debugging Chromium</a> with the Chrome extension.

* <a target=_blank href="http://www.chromium.org/developers/how-tos/debugging-on-android">Debugging on Android</a> on Android involves debugging C++ and Java components.

<h1>Remote Debugging</h1>

* <a target=_blank href="https://developers.google.com/chrome-developer-tools/docs/remote-debugging">Remote Debugging</a> is a great way to debug JavaScript on the Android device.

* <a target=_blank href="https://www.google.com/intl/en/chrome/browser/canary.html">Chrome Canary</a> is needed to do remote debugging against the trunk. You want your desktop browser to be at or a better version of Chrome than the version you are debugging.


<h1>Logging</h1>

You can filter logs by your specific process id when looking for issues.

<table border="1"><tr><td>
Find your process id - PID. Replace your.bundle.id with your package identifier to find the process.
</td></tr>
<tr><td>
adb shell ps | grep your.bundle.id
</td></tr><table>

<table border="1"><tr><td>
And then use the PID with process logging turned on to find log messages just for your application.
</td></tr>
<tr><td>
adb logcat -v process | grep THE_PROCESS_ID
</td></tr><table>

Enable logging - http://www.chromium.org/developers/how-tos/debugging-on-android#TOC-Log-output
