Version 1.1, July 25, 2013

##Introduction

Welcome, OUYA Devs! We’re thrilled you’ve decided to develop for OUYA.

We want to provide a great experience for both gamers and developers, which is why we will review all Games and Apps (read “Games”) you submit to OUYA. Below are the guidelines we’ll use. Of course, we are open to any developer -- but we want all the games on OUYA to come together to make OUYA great.

By following the guidelines below, young grasshopper, you’ll be live on OUYA before you know it (Yeah!). We will try to keep this document as stable as possible, and communicate if we need to make changes.

##Speed of Reply

We understand your need to get feedback quickly and we are committed to making this happen as it benefits you, our developers, as well as OUYA’s gamers.

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
am display-size 1280x720
```

5. Type am display-size 1920x1080, press Enter to switch to 1080p (changes the resolution back to 1080p)
```
am display-size 1920x1080
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

##Content No-No’s

We want you to build imaginative, inventive games. To make sure OUYA has a great environment for gamers, there are a few things we don’t want to see on OUYA. First, make sure to rate your game according to OUYA’s Content Rating Guidelines below (be honest).  Then ensure your submitted screen-shots and Game Details text contains no explicit content. And finally, let’s avoid these:

* **Hate Speech**: Do not promote hatred toward an individual or group of people based on their race or ethnic origin, religion, disability, gender, age, veteran status, or sexual orientation/gender identity in your game.  If you do, we’ll reject it.

* **Violence**: Do not encourage real world violence, in any way.

* **Sexually Explicit Content**: Don’t go overboard on nudity or obscene references… you know what we mean.

* **Misrepresentation** (of your Game or Yourself): Be who you say you are. Don’t confuse gamers by being “like” other games.

* **Intellectual Property**: Do not infringe on the intellectual property rights of others including patent, trademark, trade secret, copyright, and all other proprietary rights.

* **Dangerous Products**: Do not transmit viruses, worms, malware, or any other items that may harm gamers or the OUYA platform.

* **Illegal Activities**: Use good judgment and do not break the law.

* **OUYA Trademark**: You can use the OUYA name, logo, and trademark, but you have to respect that these are OUYA’s intellectual property, so we want you to use them in a way that accurately portrays that your game is on OUYA.

##In-App Purchasing & Technical Info

You have to use OUYA’s In-App Payment API to take any payment. The In-App Payment API can be found in our OUYA Development Kit (ODK) at devs.ouya.tv.  It supports micro-transactions and full game downloads as well as any content you want to unlock.

You shouldn’t mislead users about any in-app services, goods, content or functionality.

You should lead users through a simple tutorial to ensure as many people as possible are able to enjoy your app.

No nasty bugs please! We typically reject content that regularly crashes, has awful bugs, or does not perform as described.

Your game should continue to work even if a network connection goes down (excluding any online functionality, of course).

All games must function on a retail OUYA device, without modifications to hardware or software.

The size of the initial Game’s download (the .apk file) should be 1.2 GB or less. If a Game requires additional files or assets, they should be delivered via your own servers.

Games should run in full-screen mode and be playable in 720p and 1080p (it is TV ya know).

Games should follow the OUYA [Interface Guidelines](https://devs.ouya.tv/developers/docs/interface-guidelines), including the names of the controller buttons and related UI standards.


##OUYA Content Ratings

To help expedite your game’s review, we ask that you assign a content rating to your game that reflects the appropriateness of its content for different audiences. The rating scale is as follows:

**Everyone**: Play on. This games for everyone.  Nothing in this category has objectionable material.

**9+**: Mild and infrequent as they may be, these games can contain occurrences of cartoon, fantasy, or realistic violence, as well as mature, suggestive, or horror-themed content. But your 9 year olds already watch the Local News, so odds are they're ready.

**12+**: In 4 years, they'll be driving. Today, they're ready for infrequent mild language, simulated gambling, and mature and suggestive themes, as well as more frequent and intense cartoon, fantasy, or realistic violence.

**17+**: This is the oldest rating, so you guessed it: It's got the most "stuff." And by "stuff," we mean frequent and intense offensive language, all forms of violence (cartoon, fantasy or realistic) and themes (mature, horror and suggestive), as well as sexual content, alcohol, tobacco, and drugs.

##Content Guidelines

Please see the [content submission checklist](https://s3.amazonaws.com/ouya-docs/OUYA_Dev_ReviewChecklist.pdf) for the full list. Here are some important points to keep in mind:

* Alcohol, Tobacco and Drugs: Illegal content is illegal on OUYA. That said, games that reference drugs, alcohol, or tobacco products are always rated 12+. Games that focus on drugs, alcohol, or tobacco are 17+. 
* Gambling: This ain't Vegas, so games or apps that facilitate real gambling aren't allowed in OUYA. Gambling themes and simulated gambling will always be rated 12+ or 17+.
* Hate: We at OUYA hate on hate. Please keep your speech clean and respect your fellow gamer.
* Profanity and Crude Humor: We can't control how hard you laugh, but we can promise that any crude or profane humor will always be labeled 12+ or 17+.
* Sexual and Suggestive Content: We know OUYA's one sleek, sexy console, but no: Pornography is never allowed. If games or apps suggest or make sexual references, they'll be rated 12+ or 17+. Focusing on these elements will result in a 17+ rating.
* User Generated Content and User to User Communication: To protect the community, games or apps rated "everyone" cannot host user generated content or enable communication between users. And should OUYA players want to find and communicate with one another, those games will be rated 12+ or 17+.
* Violence: We play violence safe. Games or apps that contain mild cartoon or fantasy violence are 9+, realistic or intense fantasy violence is 12+ or 17+, and graphic violence is always rated high maturity.
* The OUYA name must be fully capitalized both in-game and on the game details page.

##Communication

In the event we reject your Game -- including for reasons that may not be captured within this document -- we will let you know by email, along with a brief description of why. You will have the opportunity to resubmit your Game and/or let us know if you disagree with our decision.

So go make some great games...

For any questions or clarifications, please contact us at devsupport@ouya.tv

##Icons

* The application image that is shown in the launcher is embedded inside of the APK itself. The expected file is in res/drawable-xhdpi/ouya_icon.png and the image size must be 732x412.
 
* The icon that is shown in the app settings is embedded inside of the APK itself. The expected file is in res/drawable/app_icon.png and the image size must be 96x96.

The 732x412 icon displays in the Play section of the OUYA Launcher.

<img src="https://s3.amazonaws.com/ouya-docs/images/PlayStore_732x412.png"/>

The 732x412 icon displays in the Discover section of the OUYA Launcher.

<img src="https://s3.amazonaws.com/ouya-docs/images/Discover_732x412.png"/>

The 96x96 icon displays on some legacy Android settings pages.

<img src="https://www.dropbox.com/s/v8z4bxdcqqspcen/Settings_96x96.png"/>

##Video

* In the developer portal after you've created the game entry, an edit link will appear. You can use this edit link to alter the description, add screenshots, submit new builds, and add video for your title. Your list of games can be found <a target=_blank href="https://gamers.ouya.tv/developers/games">[here]</a>.
 
<table border="1"><tr><td><img src="http://ouya-docs.s3.amazonaws.com/images/OuyaEditGame.jpg"/></td></tr></table>

* When editing game details in the video URL field, enter a video for your title using a Vimeo URL.
 
<table border="1"><tr><td><img src="http://ouya-docs.s3.amazonaws.com/images/OuyaVideoUrl.jpg"/></td></tr></table>
