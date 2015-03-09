# Java Engine

# Audience #

This document is for developers that uses Java to make apps for Android. The docs target developers using Android Studio, Eclipse, or IntelliJ.

## Releases

Java apps/games use the `ouya-sdk.jar` library included in the `ODK` downloadable from the [OUYA developer portal](http://devs.ouya.tv).

## Xiaomi Libraries

[Back to general info](enable_xiaomi_support.md#xiaomi-libraries)

Place the Xiaomi libraries in the following destinations:

* `assets/MiGameCenterSDKService.apk`

* `libs/SDK_MIBOX_2.0.1.jar`

## Xiaomi Required Permissions

[Back to general info](enable_xiaomi_support.md#xiaomi-required-permissions)

Xiaomi's SDK requires several additional permissions in `AndroidManifest.xml` in order to work.
```java
	<uses-permission android:name="com.xiaomi.sdk.permission.PAYMENT"/>
    <uses-permission android:name="android.permission.GET_TASKS"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
```

## Xiaomi Initialization

[Back to general info](enable_xiaomi_support.md#xiaomi-initialization)

* Using `Java`, prepare a `Bundle` with additional `key/value` pairs which are used to initialize the `OuyaFacade`.

```java
	// Your developer id can be found in the Developer Portal
	public static final String DEVELOPER_ID = "00000000-0000-0000-0000-000000000000";

	// Both of these values will be emailed to you by the OUYA team after you've been 
	// selected by the OUYA team
	public static final String XIAOMI_APP_ID = "0000000000000";
	public static final String XIAOMI_APP_KEY = "000000000000000000";

	// All product IDs that might be used
	public static final String[] ALL_PRODUCT_IDS = new String[] {
		"long_sword",
		"sharp_axe",
		"100_extra_lives"
	}

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		Bundle developerInfo = new Bundle();

		// "tv.ouya.developer_id"
		developerInfo.putString(OuyaFacade.OUYA_DEVELOPER_ID, DEVELOPER_ID);
		
		developerInfo.putByteArray(OuyaFacade.OUYA_DEVELOPER_PUBLIC_KEY, loadApplicationKey());

		// We must tell the OuyaFacade that we can use the Xiaomi market for purchases.
		
		// "com.xiaomi.app_id"
		developerInfo.putString(OuyaFacade.XIAOMI_APPLICATION_ID, XIAOMI_APP_ID);
		
		// "com.xiaomi.app_key"
		developerInfo.putString(OuyaFacade.XIAOMI_APPLICATION_KEY, XIAOMI_APP_KEY);
		
		// "tv.ouya.product_id_list"
		developerInfo.putStringArray(OuyaFacade.OUYA_PRODUCT_ID_LIST, ALL_PRODUCT_IDS);

		OuyaFacade.getInstance().init(this, developerInfo);
		super.onCreate(savedInstanceState);
	}
```

## Disable Xiaomi Screensaver

[Back to general info](enable_xiaomi_support.md#disable-xiaomi-screensaver)

* The screensaver should be disabled while your game is running. Invoke `View.setKeepScreenOn(true)` to disable the screensaver. Here is a common scenario of an activity loading the layout and using the content View to disable the screensaver.

```java
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		FrameLayout content = (FrameLayout)findViewById(android.R.id.content);
		if (null != content) {
			// Disable screensaver
			content.setKeepScreenOn(true);
		}
	}
```

## Create a Xiaomi-specific icon

[Back to general info](enable_xiaomi_support.md#create-a-xiaomi-specific-icon)

The `ouya_xiaomi_icon.png` 284x160 icon should be places in `res/drawable-xhdpi/ouya_xiaomi_icon.png`.

## Localization Resources

[Back to general info](enable_xiaomi_support.md#localization-resources)

Java uses standard [Android localization](http://developer.android.com/guide/topics/resources/localization.html) for localized string resources.

Strings can be placed in designated folders which are automatically selected using the current language.

* `res/values/strings.xml` (Default)

* `res/values-de/strings.xml` (Dutch)

* `res/values-en/strings.xml` (English)

* `res/values-es/strings.xml` (Spanish) 

* `res/values-fr/strings.xml` (French)

* `res/values-it/strings.xml` (Italian)

* `res/values-zh-rCN/strings.xml` (Simplified Chinese)

[Localization Example](https://github.com/ouya/ouya-sdk-examples/tree/master/Android/AndroidExit)

# Java Examples #

# Virtual Controller Example #

The virtual controller example exercises the new OUYA-Everywhere input. The button names and images are now accessible from the API. And the virtual controller buttons highlight with multiple controllers for supported controllers. TextViews display the incoming keycode values and the remapped keycodes after the OuyaInputMapper has remapped the input.

The `AndroidVirtualController` source code can be found within the [ouya-sdk-examples](https://github.com/ouya/ouya-sdk-examples/tree/master/Android/AndroidVirtualController).

![image alt text](image_0.png)

# Android Virtual Controller Project #

The project has a small number of key files that makes the example work.

![image alt text](image_1.png)

## AndroidManifest.xml ##

Specifies the target Android API level of 16 and the starting activity.

## MainActivity.java ##

The starting and only activity in the project responsible for the logic to display text, buttons, and toggle image visibility based on input.

## DebugInput.java ##

A debug class for displaying keycode and axis value input in human-readable format in the logcat.

## ouya-sdk.jar ##

The Java library released through the ODK in the developer portal which provides access to the OUYA SDK.

## activity_main.xml ##

The Android layout that specifies the position and content that displays text and images.

## drawables ##

The drawable resources hold the icons and controller images used in the example.

# Example Code #

## MainActivity ##

The MainActivity extends the OuyaActivity from the ouya-sdk.jar for the easiest way to add OUYA-Everywhere input.

```
public class MainActivity extends OuyaActivity {
```

## setDrawable ##

Accepts the ImageView that will display the button image and the keyCode id for the corresponding button image.

```
	private void setDrawable(ImageView imageView, int keyCode) {
		ButtonData data = OuyaController.getButtonData(keyCode);
		if (null != data) {
			imageView.setImageDrawable(data.buttonDrawable);
		}
	}
```

## onCreate ##

Loads the layout and gets the references to the ImageView controls that will handle toggling button visibility when toggled.

```
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		
		setContentView(R.layout.activity_main);
		super.onCreate(savedInstanceState);
		
		txtSystem = (TextView)findViewById(R.id.txtSystem);
		txtController = (TextView)findViewById(R.id.txtController);
		imgButtonMenu = (ImageView)findViewById(R.id.imgButtonMenu);
		txtKeyCode = (TextView)findViewById(R.id.txtKeyCode);
		imgControllerO = (ImageView)findViewById(R.id.imgControllerO);
		imgControllerU = (ImageView)findViewById(R.id.imgControllerU);
		imgControllerY = (ImageView)findViewById(R.id.imgControllerY);
		imgControllerA = (ImageView)findViewById(R.id.imgControllerA);
		imgControllerL1 = (ImageView)findViewById(R.id.imgControllerL1);
		imgControllerL2 = (ImageView)findViewById(R.id.imgControllerL2);
		imgControllerL3 = (ImageView)findViewById(R.id.imgControllerl3);
		imgControllerR1 = (ImageView)findViewById(R.id.imgControllerR1);
		imgControllerR2 = (ImageView)findViewById(R.id.imgControllerR2);
		imgControllerR3 = (ImageView)findViewById(R.id.imgControllerR3);
		imgControllerDpadDown = (ImageView)findViewById(R.id.imgControllerDpadDown);
		imgControllerDpadLeft = (ImageView)findViewById(R.id.imgControllerDpadLeft);
		imgControllerDpadRight = (ImageView)findViewById(R.id.imgControllerDpadRight);
		imgControllerDpadUp = (ImageView)findViewById(R.id.imgControllerDpadUp);
		imgControllerMenu = (ImageView)findViewById(R.id.imgControllerMenu);
		imgButtonA = (ImageView)findViewById(R.id.imgButtonA);
		imgDpadDown = (ImageView)findViewById(R.id.imgDpadDown);
		imgDpadLeft = (ImageView)findViewById(R.id.imgDpadLeft);
		imgDpadRight = (ImageView)findViewById(R.id.imgDpadRight);
		imgDpadUp = (ImageView)findViewById(R.id.imgDpadUp);
		imgLeftStick = (ImageView)findViewById(R.id.imgLeftStick);
		imgLeftBumper = (ImageView)findViewById(R.id.imgLeftBumper);
		imgLeftTrigger = (ImageView)findViewById(R.id.imgLeftTrigger);
		imgButtonO = (ImageView)findViewById(R.id.imgButtonO);
		imgRightStick = (ImageView)findViewById(R.id.imgRightStick);
		imgRightBumper = (ImageView)findViewById(R.id.imgRightBumper);
		imgRightTrigger = (ImageView)findViewById(R.id.imgRightTrigger);
		imgLeftThumb = (ImageView)findViewById(R.id.imgLeftThumb);
		imgRightThumb = (ImageView)findViewById(R.id.imgRightThumb);
		imgButtonU = (ImageView)findViewById(R.id.imgButtonU);
		imgButtonY = (ImageView)findViewById(R.id.imgButtonY);
```

Input can be event based, or spawn a thread to set visibility on an interval. The menu button is pressed and the visibility needs to be cleared after an interval rather than waiting for an event to clear it.

```
    	// spawn thread to toggle menu button
        Thread timer = new Thread()
        {
	        public void run()
	        {
	        	while (mWaitToExit)
	        	{
	        		if (mMenuDetected != 0 &&
	        			mMenuDetected < System.nanoTime())
	        		{
	        			mMenuDetected = 0;
	        			Runnable runnable = new Runnable()
	        			{
		        			public void run()
		        			{
		        				imgButtonMenu.setVisibility(View.INVISIBLE);
		        			}
	        			};
	        			runOnUiThread(runnable);
	        			
	        		}
	        		try
	        		{
	        			Thread.sleep(50);
	        		}
	        		catch (InterruptedException e)
	        		{
	        		}
		        }
			}
        };
		timer.start();
```
 

## onStart ##

Initialization displays build information and sets the drawable button images from the new api.

```
	@Override
	protected void onStart() {
		super.onStart();
		txtSystem.setText("Brand=" + android.os.Build.BRAND + " Model=" + android.os.Build.MODEL + " Version=" + android.os.Build.VERSION.SDK_INT);
		
		setDrawable(imgControllerO, OuyaController.BUTTON_O);
		setDrawable(imgControllerU, OuyaController.BUTTON_U);
		setDrawable(imgControllerY, OuyaController.BUTTON_Y);
		setDrawable(imgControllerA, OuyaController.BUTTON_A);
		setDrawable(imgControllerL1, OuyaController.BUTTON_L1);
		setDrawable(imgControllerL2, OuyaController.BUTTON_L2);
		setDrawable(imgControllerL3, OuyaController.BUTTON_L3);
		setDrawable(imgControllerR1, OuyaController.BUTTON_R1);
		setDrawable(imgControllerR2, OuyaController.BUTTON_R2);
		setDrawable(imgControllerR3, OuyaController.BUTTON_R3);
		setDrawable(imgControllerDpadDown, OuyaController.BUTTON_DPAD_DOWN);
		setDrawable(imgControllerDpadLeft, OuyaController.BUTTON_DPAD_LEFT);
		setDrawable(imgControllerDpadRight, OuyaController.BUTTON_DPAD_RIGHT);
		setDrawable(imgControllerDpadUp, OuyaController.BUTTON_DPAD_UP);
		setDrawable(imgControllerMenu, OuyaController.BUTTON_MENU);
	}
```

## onGenericMotionEvent ##

The axis events arrive with onGenericMotionEvent after the OUYA-Everywhere has remapped the input.

```
	@Override
	public boolean onGenericMotionEvent(MotionEvent motionEvent) {
		float lsX = motionEvent.getAxisValue(OuyaController.AXIS_LS_X);
	    float lsY = motionEvent.getAxisValue(OuyaController.AXIS_LS_Y);
	    float rsX = motionEvent.getAxisValue(OuyaController.AXIS_RS_X);
	    float rsY = motionEvent.getAxisValue(OuyaController.AXIS_RS_Y);
	    float l2 = motionEvent.getAxisValue(OuyaController.AXIS_L2);
	    float r2 = motionEvent.getAxisValue(OuyaController.AXIS_R2);
```

## onKeyDown ##

When a button is pressed the corresponding image is highlighted. When the system button is detected, the image is highlighted for an interval.

```
	@Override
	public boolean onKeyDown(int keyCode, KeyEvent keyEvent) {
		switch (keyCode)
		{
		case OuyaController.BUTTON_L1:
			imgLeftBumper.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_L3:
			imgLeftStick.setVisibility(View.INVISIBLE);
			imgLeftThumb.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_R1:
			imgRightBumper.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_R3:
			imgRightStick.setVisibility(View.INVISIBLE);
			imgRightThumb.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_O:
			imgButtonO.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_U:
			imgButtonU.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_Y:
			imgButtonY.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_A:
			imgButtonA.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_DOWN:
			imgDpadDown.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_LEFT:
			imgDpadLeft.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_RIGHT:
			imgDpadRight.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_UP:
			imgDpadUp.setVisibility(View.VISIBLE);
			return true;
		case OuyaController.BUTTON_MENU:
			imgButtonMenu.setVisibility(View.VISIBLE);
			mMenuDetected = System.nanoTime() + 1000000000;
			return true;
		}
		return true;
	}
```

## onKeyUp ##

When the button is no longer pressed the ImageView for the highlighted button is hidden.

```
	@Override
	public boolean onKeyUp(int keyCode, KeyEvent keyEvent) {
		switch (keyCode)
		{
		case OuyaController.BUTTON_L1:
			imgLeftBumper.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_L3:
			imgLeftStick.setVisibility(View.VISIBLE);
			imgLeftThumb.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_R1:
			imgRightBumper.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_R3:
			imgRightStick.setVisibility(View.VISIBLE);
			imgRightThumb.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_O:
			imgButtonO.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_U:
			imgButtonU.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_Y:
			imgButtonY.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_A:
			imgButtonA.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_DOWN:
			imgDpadDown.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_LEFT:
			imgDpadLeft.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_RIGHT:
			imgDpadRight.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_DPAD_UP:
			imgDpadUp.setVisibility(View.INVISIBLE);
			return true;
		case OuyaController.BUTTON_MENU:
			//wait 1 second
			return true;
		}
		return true;
	}
```
