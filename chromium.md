Chromium
========

OUYA fork of Chromium


First thing, the Chromium code-base is very large. Rather than fork multiple repros, this assumes you've already cloned the chromium trunk.

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

* <a target=_blank href="https://code.google.com/p/chromium/wiki/LinuxBuildInstructions">[Instructions]</a> for setting up the Chromium Build Environment.<br/>
<br/>

* Create a Virtual Machine in (VMWare Workstation/Fusion or Virtual Box) with <a target=_blank href="http://www.ubuntu.com/download/desktop">[Ubuntu 12.04 LTS Desktop 64-bit]</a> as your guest image. VMWare is recommended as you don't need to export/import to backup/restore the virtual machine image.<br/>
<br/>

* For the guest, you want about 80GB of disk space to handle compiling Chromium. You'll want 160GB if you need the space to duplicate the compiled source or to archive the folder for backup.<br/>
<br/>

* The guest needs a minimum of 2GB of RAM to compile, 4GB is recommended.<br/>
<br/>

<b>(After you've created a VM and installed Ubuntu):</b><br/>
<br/>

* Install the build dependencies, which are listed at <a target=_blank href="http://code.google.com/p/chromium/wiki/LinuxBuildInstructionsPrerequisites">[LinuxBuildInstructionsPrerequisites]</a>.<br/>
<br/>

* Install the depot_tools utilities, a process that is documented at <a target=_blank href="http://dev.chromium.org/developers/how-tos/install-depot-tools"><[install-depot-tools]</a>.<br/>
<br/>

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

<b>Get the dependencies</b>
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

* <a target=_blank href="http://www.chromium.org/developers/how-tos/debugging-on-android">Debugging Chromium</a> on Android involves debugging C++ and Java components.
