Version 1.2, November 5th, 2015

##Introduction

Welcome, `Cortex TV` Devs! We’re thrilled you’ve decided to develop for `Forge TV`.

We want to provide a great experience for both gamers and developers, which is why we will review all Games and Apps (read “Games”) you submit to `Cortex TV`. Below are the guidelines we’ll use. Of course, we are open to any developer -- but we want all the games on `Cortex TV` to come together to make `Cortex TV` great.

By following the guidelines below, young grasshopper, you’ll be live on `Cortex TV` before you know it (Yeah!). We will try to keep this document as stable as possible, and communicate if we need to make changes.

##Speed of Reply

We understand your need to get feedback quickly and we are committed to making this happen as it benefits you, our developers, as well as `Cortex TV’s` gamers.

Last week, it took us three days to review an average Game.  We’ll update this weekly for you.

## Technical Guidelines

### Safe Zone
We want to provide a great experience for gamers with any type of TV. Some TVs have a substantial amount of overscan -- so much so that you can’t see the outside edge of the game at all. 

For example:

<a target=_blank href="https://s3.amazonaws.com/ouya-docs/SafeAreaTVMaxOverScanText2.png"><img width=600 src="https://s3.amazonaws.com/ouya-docs/SafeAreaTVMaxOverScanText2.png"/></a><br/>
(Above) Notice how the outer UI elements for title and buttons are being clipped because they fall outside the safe zone.
<hr>


<a target=_blank href="https://s3.amazonaws.com/ouya-docs/SafeAreaTVNoOverScanText2.png"><img width=600 src="https://s3.amazonaws.com/ouya-docs/SafeAreaTVNoOverScanText2.png"/></a><br/>
(Above) Here without the severe overscan you can see the important content that went missing along the outer edge.
<hr>


<a target=_blank href="https://s3.amazonaws.com/ouya-docs/InsideSafeArea.png"><img width=600 src="https://s3.amazonaws.com/ouya-docs/InsideSafeArea.png"/></a><br/>
(Above) You have the option to stylize content outside the safe zone by including non-essential game art or generic padding.
The green overlay denotes the safe area which can be toggled while a game is running with the commands in the following section.
<hr>


Safe zone issues are is easy to check. In the Terminal or Command Prompt:

1. Type adb shell, press Enter<br/> (this opens the shell prompt connected to the device)
```
adb shell
```

2. Type su, press Enter (grant super-user access to the activity manager)
```
su
```

3. Type am start -n tv.ouya.console/tv.ouya.console.launcher.ToggleSafeZoneActivity, press Enter to toggle (toggles a green visual overlay showing the safe zone)
```
am start -n tv.ouya.console/tv.ouya.console.launcher.ToggleSafeZoneActivity
```

4. Type am display-size 1280x720, press Enter to switch to 720p (changes the TV display to the target resolution, if the resolution is not available emulates with a black area)
```
setprop persist.tegra.hdmi.resolution 720p
```

5. Type am display-size 1920x1080, press Enter to switch to 1080p (changes the resolution back to 1080p)
```
setprop persist.tegra.hdmi.resolution 1080p
```

Here is TimG with a video tip on handling the safe area.

<table border=1>

<tr>

 <td>Check Your Safe Area (3:36)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=Dz7ZRjN3KlE" target="_blank">
<img src="http://img.youtube.com/vi/Dz7ZRjN3KlE/0.jpg" alt="Check Your Safe Area" width="240" height="180" border="10" /></a>
 </td>
 
</tr>

</table>

##Game Details 

* **Age Rating**: The review team will verify the game details age rating corresponds with the in-game content.

* **Cost**: The review team will verify the cost displays correctly on the games details page. Costs can be `FREE` (free game without in-app-purchases), `FREE*` (free game with in-app-purchases), `PRICE` (premium game without in-app-purchases, and `PRICE*` (premium game with in-app-purchases). 

* **Description**: The review team will critique the game description to be optimal. (Changes to the description are recommended and not required).

* **Downloadable**: The review team will verify the game can be downloaded from the games details page.

* **Full Screen**: The review team will verify the game runs full screen and does not use an incorrect resolution creating large black areas. This is recommended and not required.

* **Genre**: A request to the review team is needed if the game title fails to appear in the expected genres.

* **Installable**: The review team will verify the game can be installed from the games details page. 

* **Launchable**: The review team will verify the game can be launched from the games details page.

* **Video**: Premium titles are required to have a preview video.

##App Review

* **APK Size**: The maximum APK size for games under review is 1.25GB. Extra content can be downloaded by the application after launching.

* **Button Mashing**: The review team checks that mashing controller buttons at the start of the game will not crash the app. Commonly when content is loading, button mashing can crash apps under review.

* **Clear**: The review team recommends that it is clear how to begin playing the game. If the game game uses text fields, the navigation between fields should be easy and not cumbersome.

* **Coming Soon**: The review team recommends that the main menu functions properly unless any features are explicitly labeled as `coming soon`.

* **Controller**: The review team will reject any game where the controller input handling does not function properly. The review team recommends games use `OUYA-Everywhere` input. The review team requires that only controller images that represent `Cortex TV` devices are used in the game. The review team recommends that the controller input handling follow [interface-guidelines](interface-guidelines.md#user-content-interface-guidelines).

* **Corruption**: The review team recommends that the game state shouldn't be corrupted by powering off the `Cortex TV` device in the middle of the game.

* **Endurance**: The review team will recommend fixes when issues are discovered by playing the game for an extended period of time. Issues like memory leaks could crash the game after playing for long periods.

* **Exit**: The review team requires that exiting the game via the game menu works as expected. The game exit functionality should not just pause the game.

* **Legend**: The review team recommends that the game displays a legend which could be displayed in the loading screen. The legend displays a controller layout which shows how sticks and buttons map to in-game actions.

* **Menu**: The review team requires that the `DPAD` and `LEFT_STICK` are capable of navigating the in-game menus.

* **Multiplayer**: The review team checks that the maximum number of players are supported in multiplayer games without issues. The review team recommends that the game developer is responsible for extensive multiplayer testing. The review team requires that only a single controller map to each player for multiplayer games.

* **Network**: The review team requires that the game handle the loss of a network connection gracefully. If a network connection is required, the game should notify the user that a network connection is needed to continue or give the option to exit the game.

* **Orientation**: The review team checks that the game uses the `landscape-left` orientation and does not `auto-rotate` during loading or gameplay.

* **Out of Space**: For games that download content in game, the review team checks for download errors that can occur when the system is out of disk space. (Messaging space issues to the user is recommended but not required.)

* **Pause**: The review team recommends that in-game pausing when exiting to `Cortex TV` functions properly. When returning to the game, the game should resume without losing user data.

* **Performance**: The review team checks for in-game lag and will recommend fixes for any performance issues that are discovered. 

* **Phone Features**: The review team recommends that the game does not mention phone features like `accelerometer`, `tilt`, or `vibration`.

* **Readable**: The review team checks that important game text is readable from 10 feet back from the TV.

* **Runnable**: The review team will verify the game can be started without immediately crashing.

* **Save**: The review team recommends that games use a feature for saving in-game progress where applicable. If game saving is implemented, the review team will verify that saving functions correctly.

* **Social**: The review team requires that social features (Facebook/Twitter) work as expected if implemented. The game should support switching from social widgets back to the game.

* **Store**: The review team recommends the game uses an in-game store. The review team requires that the in-game store functions properly.

* **Sound**: The review team recommends the game uses stereo sound. Music and sound is required to stop playing if the game is paused when switching back to the `Cortex TV` launcher.

* **Tutorial**: The review team recommends that the game initially starts with a tutorial about the different game modes in the game.

* **Updates**: The review team requires that games continue to function after `OTA` updates. The review team will let developers know when game breaking issues are discovered when testing `OTA` updates.

##Content No-No’s

We want you to build imaginative, inventive games. To make sure `Cortex TV` has a great environment for gamers, there are a few things we don’t want to see on `Cortex TV`. First, make sure to rate your game according to `Cortex TV’s` Content Rating Guidelines below (be honest).  Then ensure your submitted screen-shots and Game Details text contains no explicit content. And finally, let’s avoid these:

* **Hate Speech**: Do not promote hatred toward an individual or group of people based on their race or ethnic origin, religion, disability, gender, age, veteran status, or sexual orientation/gender identity in your game.  If you do, we’ll reject it.

* **Violence**: Do not encourage real world violence, in any way.

* **Sexually Explicit Content**: Don’t go overboard on nudity or obscene references… you know what we mean. There should be no explicit content in the game details page or video.

* **Misrepresentation** (of your Game or Yourself): Be who you say you are. Don’t confuse gamers by being "like" other games.

* **Intellectual Property**: Do not infringe on the intellectual property rights of others including patent, trademark, trade secret, copyright, and all other proprietary rights. The game details page should only include intellectual property that you own. Emulators must not include any `ROM` files from infringing games.

* **Dangerous Products**: Do not transmit viruses, worms, malware, or any other items that may harm gamers or the `Cortex TV` platform.

* **Illegal Activities**: Use good judgment and do not break the law.

* **`Cortex TV` Trademark**: You can use the `Cortex TV` name, logo, and trademark, but you have to respect that these are `Razer’s` intellectual property, so we want you to use them in a way that accurately portrays that your game is on `Cortex TV`.

* **Gambling**: Do not use or promote any real world gambling.

* **`OUYA` Trademark**: You can use the `OUYA` name, logo, and trademark, but you have to respect that these are `Razer’s` intellectual property, so we want you to use them in a way that accurately portrays that your game is on `OUYA`.

* **Copyright**: The review team requires that all copyright dates on the details page and in game are correct.

##In-App Purchasing & Technical Info

You have to use `Cortex TV’s` In-App Payment API to take any payment. The In-App Payment API can be found in our `Cortex TV` Development Kit (ODK) at devs.ouya.tv. It supports micro-transactions and full game downloads as well as any content you want to unlock.

You shouldn’t mislead users about any in-app services, goods, content or functionality.

You should lead users through a simple tutorial to ensure as many people as possible are able to enjoy your app.

No nasty bugs please! We typically reject content that regularly crashes, has awful bugs, or does not perform as described.

Your game should continue to work even if a network connection goes down (excluding any online functionality, of course).

All games must function on a retail `Forge TV` device, without modifications to hardware or software.

The size of the initial Game’s download (the .apk file) should be 1.2 GB or less. If a Game requires additional files or assets, they should be delivered via your own servers.

Games should run in full-screen mode and be playable in 720p and 1080p (it is TV ya know).

Games should follow the `Cortex TV` [Interface Guidelines](https://devs.ouya.tv/developers/docs/interface-guidelines), including the names of the controller buttons and related UI standards.

* **IAP**: The review team requires that `IAP` functions properly in the `Cortex TV` store and in game. All purchasing for `Cortex TV` games must use the `Cortex TV` API for purchases.

* **Descriptions**: The review team requires `product` descriptions for store items are correct.

* **Readable**: The review team requires that store descriptions are readable.

* **Products**: The review team requires that `entitlements` and `consumables` are purchasable in game and unlock content as expected.

* **Price**: The review team requires that the store, in-game, and purchase prompts display the price correctly.

* **Unlocked Content**: The review team requires that purchased unlocked content functions properly.

* **Out of Memory**: The review team requires that purchases still work properly when the system is out of memory.

* **Purchase Type**: The review team requires that store items are properly labeled as `entitlement`, `consumable`, or `subscription`.

* **Check Receipts**: The review team recommends that the game checks receipts on start. Reinstalling the game should not prompt to purchase unlockable content again.

* **Vouchers**: The review team requires that vouchers unlock entitlements properly for premium games that use a promoted product.

* **Reactive**: - The review team recommends that games unlock content immediately after using vouchers. The user shouldn't be prompted to purchase entitlements after using vouchers.

* **Donations**: The review team requires that the store and game respect the guidelines for soliciting donations and purchases.

* **Buy Button**: The review team recommends including a `BUY` button on the main menu in the game.

* **Premium Titles**: The review team recommends that premium games check for receipts at the start of the game before the user enters into gameplay. Premium games should be purchased before the game can be played. If the receipt check fails to find a receipt, the main menu should show a `BUY` button before gameplay can be entered.

* **Purchase Changes**: The review team recommends honoring old purchases when a game changes purchase types. For example, if a game changes from `free to try` to a `premium` game, purchases from the old purchase type should still work.

* **Premium Content**: The review team requires that premium content purchased in premium games should function properly. 

* **Premium Receipt Checking**: The review team requires that `entitlement` purchases and unlocked content should remain unlocked after the game is reinstalled.

##Cortex TV Content Ratings

To help expedite your game’s review, we ask that you assign a content rating to your game that reflects the appropriateness of its content for different audiences. The rating scale is as follows:

**Everyone**: Play on. This games for everyone.  Nothing in this category has objectionable material.

**9+**: Mild and infrequent as they may be, these games can contain occurrences of cartoon, fantasy, or realistic violence, as well as mature, suggestive, or horror-themed content. But your 9 year olds already watch the Local News, so odds are they're ready.

**12+**: In 4 years, they'll be driving. Today, they're ready for infrequent mild language, simulated gambling, and mature and suggestive themes, as well as more frequent and intense cartoon, fantasy, or realistic violence.

**17+**: This is the oldest rating, so you guessed it: It's got the most "stuff." And by "stuff," we mean frequent and intense offensive language, all forms of violence (cartoon, fantasy or realistic) and themes (mature, horror and suggestive), as well as sexual content, alcohol, tobacco, and drugs.

##Content Guidelines

Please see the [content submission checklist](https://s3.amazonaws.com/ouya-docs/OUYA_Dev_ReviewChecklist.pdf) for the full list. Here are some important points to keep in mind:

* Alcohol, Tobacco and Drugs: Illegal content is illegal on `Cortex TV`. That said, games that reference drugs, alcohol, or tobacco products are always rated 12+. Games that focus on drugs, alcohol, or tobacco are 17+. 
* Gambling: This ain't Vegas, so games or apps that facilitate real gambling aren't allowed in `Cortex TV`. Gambling themes and simulated gambling will always be rated 12+ or 17+.
* Hate: We at `Razer` hate on hate. Please keep your speech clean and respect your fellow gamer.
* Profanity and Crude Humor: We can't control how hard you laugh, but we can promise that any crude or profane humor will always be labeled 12+ or 17+.
* Sexual and Suggestive Content: We know `Cortex TV` is one sleek, sexy console, but no: Pornography is never allowed. If games or apps suggest or make sexual references, they'll be rated 12+ or 17+. Focusing on these elements will result in a 17+ rating.
* User Generated Content and User to User Communication: To protect the community, games or apps rated "everyone" cannot host user generated content or enable communication between users. And should `Cortex TV` players want to find and communicate with one another, those games will be rated 12+ or 17+.
* Violence: We play violence safe. Games or apps that contain mild cartoon or fantasy violence are 9+, realistic or intense fantasy violence is 12+ or 17+, and graphic violence is always rated high maturity.
* The `OUYA` name must be fully capitalized both in-game and on the game details page.

##Communication

In the event we reject your Game -- including for reasons that may not be captured within this document -- we will let you know by email, along with a brief description of why. You will have the opportunity to resubmit your Game and/or let us know if you disagree with our decision.

So go make some great games...

For any questions or clarifications, please contact us at `devsupport@ouya.tv`.

##Icons

* The application image that is shown in the launcher is embedded inside of the APK itself. The expected file is in `res/drawable-xhdpi/ouya_icon.png` and the image size must be `732x412`.
 
* The icon that is shown in the app settings is embedded inside of the APK itself. The expected file is in `res/drawable/app_icon.png` and the image size must be `96x96`.

The `732x412` icon displays in the Play section of the `Cortex TV` Launcher.

<img src="https://s3.amazonaws.com/ouya-docs/images/PlayStore_732x412.png"/>

The `732x412` icon displays in the Discover section of the OUYA Launcher.

<img src="https://s3.amazonaws.com/ouya-docs/images/Discover_732x412.png"/>

The `96x96` icon displays on some legacy Android settings pages.

<img src="https://s3.amazonaws.com/ouya-docs/images/Settings_96x96.png"/>

## Video

* In the developer portal after you've created the game entry, an edit link will appear. You can use this edit link to alter the description, add screenshots, submit new builds, and add video for your title. Your list of games can be found <a target=_blank href="https://gamers.ouya.tv/developers/games">[here]</a>.
 
<table border="1"><tr><td><img src="http://ouya-docs.s3.amazonaws.com/images/OuyaEditGame.jpg"/></td></tr></table>

* When editing game details, add one or more videos into the Video urls field. Add one Vimeo url per line.
 
<table border="1"><tr><td><img src="http://ouya-docs.s3.amazonaws.com/images/VideosURLbox.png"/></td></tr></table>

## Keystore

The `keystore` is a security mechanism for your game.

The `keystore` must be used to submit your game to the store.

Updates of the game needs to use the same `keystore`.

Be sure to email yourself a backup of the keystore and password because you always need to submit updates of the game with the same `keystore`.

### Create the KeyStore

* IDE tools like [Eclipse](http://developer.android.com/tools/publishing/app-signing-eclipse.html) and [Unity](http://docs.unity3d.com/Manual/class-PlayerSettingsAndroid.html) can be used to create the `keystore`.

* Or generate a unique `keystore` for your game on the command-line. Make sure `ADB` is in your path and `keytool` is in your path.

Example `keystore` filename: `ouya_your.keystore` (put your own name here)

Example `keystore` alias: `YourAlias` (put your own alias)

Example `keystore` password: `your_password` (put your own password)

```
keytool -genkey -alias YourAlias -keystore ouya_your.keystore -keyalg RSA -keysize 2048 -validity 36135 -storepass your_password
```

And then fill out the info with `your company info`:

```
What is your first and last name?
[Unknown]: OUYA Inc
What is the name of your organizational unit?
[Unknown]: OUYA Inc
What is the name of your organization?
[Unknown]: OUYA Inc
What is the name of your City or Locality?
[Unknown]: San Mateo
What is the name of your State or Province?
[Unknown]: CA
What is the two-letter country code for this unit?
[Unknown]: US
Is CN=OUYA Inc, OU=OUYA Inc, O=OUYA Inc, L=San Mateo, ST=CA, C=US correct?
[no]: yes
Enter key password for <YourAlias>
(RETURN if same as keystore password):
Re-enter new password: 
```

### Sign APK with the Keystore

Before submitting your `APK` to the store, make sure your `APK` is signed with your `keystore`.

Make sure `jarsigner` is in your path and you are using `JDK6`.

Example `keystore` filename: `ouya_your.keystore` (put your own name here)

Example `keystore` alias: `YourAlias` (put your own alias)

Example `keystore` password: `your_password` (put your own password)

```
jarsigner -keystore ouya_your.keystore -storepass your_password -keypass your_password Your.apk YourAlias
```

### Verify Certs

The `keystore` certificate can be verified after signing the apk.

```
jarsigner -verify -certs -verbose Your.apk
```

### Publish

After signing with the `keystore`, the `APK` is ready to publish in the [developer portal](http://devs.ouya.tv).
