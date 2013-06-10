## Corona Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Corona

## Guide

### Examples

Examples are included at the base GIT path.

### Resources

Corona SDK - http://coronalabs.com/products/corona-sdk/<br/>

### Building

(Currently testing on Mac)

put the Corona Enterprise SDK into a subfolder of your Mac’s Application folder

working folder:<br/>
'''Applications/CoronaEnterprise/Samples/SimpleLuaExtension/android'''<br/>

set environment variable: ANDROID_SDK<br/>

generate local.properties:<br/>
'''android update project --path .'''

edit the build script to add paths to the includes:<br/>
'''Applications/CoronaEnterprise/Corona/android/lib/Corona/build.xml'''<br/>
<property file="${user.dir}/local.properties" /><br/>
<property file="${user.dir}/ant.properties" /><br/>
<import file="${user.dir}/custom_rules.xml" optional="true" /><br/>

create ant.properties<br/>
'''key.alias=androiddebugkey'''<br/>
'''key.store=debug.keystore'''<br/>

copy the debug keystore to the sample android folder<br/>
'''cp ~/.android/debug.keystore . '''

build the sample:<br/>
'''./build.sh ~/android/android-sdk-macosx /Applications/CoronaEnterprise'''<br/>
			when prompted enter the debug keystore and alias password “android”<br/>
			
install the sample:<br/>
'''adb install path/to/apk'''

###Recommended Tools

Outlaw IDE - http://outlawgametools.com/outlaw-code-editor-and-project-manager/