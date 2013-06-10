## MonoGame Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame

## Guide

### Examples

Several examples are included in the ouya-unity-plugin package.

<table border=1>

 <tr>

 <td>Virtual Controller Example<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=ultZwdGizqM" target="_blank">
<img src="http://img.youtube.com/vi/ultZwdGizqM/0.jpg" alt="Virtual Controller Example" width="240" height="180" border="10" /></a>
 </td>
 
  <td></td>
 
 </tr> 

</table>

### Resources

MonoGame Download - http://monogame.codeplex.com/
ODK Bindings for C# - https://github.com/slygamer/ouya-csharp
Mono for Android - http://xamarin.com/monoforandroid
Installation Instructions - http://docs.xamarin.com/guides/android/getting_started/installation
MonoGame GIT for latest tools - https://github.com/mono/MonoGame
MonoGame OUYA Examples - https://github.com/ouya/ouya-sdk-examples
MonoGame Content Builder - https://github.com/mono/MonoGame/wiki/MonoGame-Content-Builder
Docs and Tutorials - http://www.monogame.net/documentation

### Building

Open and build ODK Bindings for C# in Xamarin (ouya-csharp/Ouya.Console.Api.csproj)
Save the Ouya.Console.Api.dll from the release folder for the next step.

After installing Mono for Android it adds templates to Visual Studio. So all you need to do is from Visual Studio create a New Project and pick the option MonoGame for OUYA.

In the new project remove the DLL reference to Ouya.Console.Api.dll and

Add a reference to the Ouya.Console.dll that was built in the previous steps.

Ceate a content project with XNA and import into the MonoGame project to support loading content types like PNG files.