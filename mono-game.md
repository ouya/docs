## MonoGame Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame

### Developer Support Hangouts

2013:

<table border=1>
<tr><td>OUYA Hangout July 15th (1:00:00)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=zWYVYmk6luc" target="_blank">
<img src="http://img.youtube.com/vi/zWYVYmk6luc/0.jpg" alt="OUYA Hangout July 15th," width="240" height="180" border="10" /></a></td> 
<td></td></tr></table>

## Guide

### Examples

Examples are included at the base GIT path.

<table border=1>

 <tr>

 <td>Virtual Controller Example (41:37)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=ultZwdGizqM" target="_blank">
<img src="http://img.youtube.com/vi/ultZwdGizqM/0.jpg" alt="Virtual Controller Example" width="240" height="180" border="10" /></a>
 </td>
 
  <td></td>
 
 </tr> 

</table>

<b><a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame/InAppPurchases">InAppPurchases</a></b> - In-App-Purchase Example

<b><a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame/SetResolutions">SetResolutions</a></b> - Set Resolutions Example

<b><a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame/VirtualController">VirtualController</a></b> - Maps OUYA controllers to multiple virtual controllers

### Signing

The auto update process requires that you use the same keystore in your app and every update.

If you lose your keystore, it requires users to uninstall the game and redownload to get the next update.

If you keep using the same keystore after that, the auto updating will continue as intended.

In MonoGame, unload your project and edit the CSPROJ. Add the following settings to your valid keystore:

```xml
<AndroidKeyStore>True</AndroidKeyStore>   
<AndroidSigningKeyStore>PathToYourKeyStoreFile\key.keystore</AndroidSigningKeyStore>
<AndroidSigningStorePass>my_password_here</AndroidSigningStorePass>
<AndroidSigningKeyAlias>Aranda</AndroidSigningKeyAlias>
<AndroidSigningKeyPass>same_password_here</AndroidSigningKeyPass>
```

It's important that your key signing, your package name, bundle identifiers, developer id, and the account that submits the game all matches.

The above has to true for the in-app-purchases to function, since decryption depends on using the right signing key.

### Resources

MonoGame Download - http://monogame.codeplex.com/<br/>
ODK Bindings for C# - https://github.com/slygamer/ouya-csharp<br/>
Mono for Android - http://xamarin.com/monoforandroid<br/>
Installation Instructions - http://docs.xamarin.com/guides/android/getting_started/installation<br/>
MonoGame GIT for latest tools - https://github.com/mono/MonoGame<br/>
MonoGame OUYA Examples - https://github.com/ouya/ouya-sdk-examples<br/>
MonoGame Content Builder - https://github.com/mono/MonoGame/wiki/MonoGame-Content-Builder<br/>
Docs and Tutorials - http://www.monogame.net/documentation<br/>

### Building

Open and build ODK Bindings for C# in Xamarin (ouya-csharp/Ouya.Console.Api.csproj)<br/>
Save the Ouya.Console.Api.dll from the release folder for the next step.

After installing Mono for Android it adds templates to Visual Studio. So all you need to do is from Visual Studio create a New Project and pick the option MonoGame for OUYA.

In the new project remove the DLL reference to Ouya.Console.Api.dll and

Add a reference to the Ouya.Console.dll that was built in the previous steps.

Use the MonoGame Content Build MGCB to convert assets to .xnb content.
