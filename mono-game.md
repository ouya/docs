## MonoGame Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame

### Forums

@OUYA - (MonoGame on OUYA Forums) - http://forums.ouya.tv/categories/monogame-on-ouya

@MonoGame - (MonoGame Forums) - http://community.monogame.net/category/ouya

@MonoGame - (Archived MonoGame Forums) - http://monogame.codeplex.com/discussions/topics/5559/ouya

@Xamarin - (Xamarin Forums) - http://forums.xamarin.com

### Developer Support Hangouts

2013:

OUYA Hangout July 15th (1:00:00)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=zWYVYmk6luc" target="_blank">
<img src="http://img.youtube.com/vi/zWYVYmk6luc/0.jpg" alt="OUYA Hangout July 15th," width="240" height="180" border="10" /></a>

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

### Build environment setup

MonoGame has an environmental build settings for advanced users to configure.
```
ouya/Resources/environment.txt
```

These settings aren't for everygame, but the following environmental tweaks reduce the amount of garbage collection thrashing.

```
MONO_GC_PARAMS=soft-heap-limit=512m,nursery-size=64m,evacuation-threshold=66,major=marksweep,concurrent-sweep
```


### Resources

MonoGame Download - http://monogame.codeplex.com/<br/>
(old) ODK Bindings for C# - https://github.com/slygamer/ouya-csharp<br/>
Mono for Android - http://xamarin.com/monoforandroid<br/>
Installation Instructions - http://docs.xamarin.com/guides/android/getting_started/installation<br/>
MonoGame GIT for latest tools - https://github.com/mono/MonoGame<br/>
MonoGame OUYA Examples - https://github.com/ouya/ouya-sdk-examples<br/>
MonoGame Content Builder - https://github.com/mono/MonoGame/wiki/MonoGame-Content-Builder<br/>
Docs and Tutorials - http://www.monogame.net/documentation<br/>
Binding JARs - http://docs.xamarin.com/guides/android/advanced_topics/java_integration_overview/binding_a_java_library_(.jar)/<br/>

## Examples

### Virtual Controller

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame/InputView) example shows 4 images of the OUYA Controller which moves axises and highlights buttons when the physical controller is manipulated.

![VirtualController image](ouya-everywhere-monogame/image_7.png)

### In-App-Purchases

The [In-App-Purchase](https://github.com/ouya/ouya-sdk-examples/tree/master/MonoGame/InAppPurchases) example uses the ODK to access gamer info, purchasing, and receipts.

![IAP image](ouya-everywhere-monogame/image_8.png)

## Xiaomi Initialization

[Back to general info](enable_xiaomi_support.md#xiaomi-initialization)

`MonoGame` supports initialization strings similar to `Java` syntax to make the game compatible with OUYA Everywhere devices.

* `tv.ouya.developer_id` - The developer UUID can be found in the [developer portal](http://devs.ouya.tv) after logging in.

* `com.xiaomi.app_id` - The Xiaomi App Id is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `com.xiaomi.app_key` - The Xiaomi App Key is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `tv.ouya.product_id_list` - The product id list is a comma separated list of product ids that can be purchased in the game.

```c#
			Bundle developerInfo = new Bundle();

			byte[] applicationKey = null;

			// load the application key from assets
			try {
				AssetManager assetManager = ApplicationContext.Assets;
				AssetFileDescriptor afd = assetManager.OpenFd("key.der");
				int size = 0;
				if (null != afd) {
					size = (int)afd.Length;
					afd.Close();
					using (Stream inputStream = assetManager.Open("key.der", Access.Buffer))
					{
						applicationKey = new byte[size];
						inputStream.Read(applicationKey, 0, size);
						inputStream.Close();
					}
				}
			} catch (Exception e) {
				Log.Error (TAG, string.Format("Failed to read application key exception={0}", e));
			}

			if (null != applicationKey) {
				Log.Debug (TAG, "Read signing key");
				developerInfo.PutByteArray (OuyaFacade.OUYA_DEVELOPER_PUBLIC_KEY, applicationKey);
			} else {
				Log.Error (TAG, "Failed to authorize with signing key");
				Finish ();
				return;
			}

			string[] PURCHASABLES =
			{
				"long_sword",
				"sharp_axe",
				"cool_level",
				"awesome_sauce",
				"__DECLINED__THIS_PURCHASE",
			};

			developerInfo.PutString(OuyaFacade.OUYA_DEVELOPER_ID, "00000000-0000-0000-0000-000000000000");
			developerInfo.PutString(OuyaFacade.XIAOMI_APPLICATION_ID, "0000000000000");
			developerInfo.PutString(OuyaFacade.XIAOMI_APPLICATION_KEY, "000000000000000000");
			developerInfo.PutStringArray(OuyaFacade.OUYA_PRODUCT_ID_LIST, Game1.PURCHASABLES);

			_ouyaFacade = OuyaFacade.Instance;
			_ouyaFacade.Init(this, developerInfo);
```
