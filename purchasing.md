# In-App Purchasing

In-App Purchasing (IAP) is how your app can make money.  The OUYA Developer Kit (ODK) is designed to be both easy to use and secure.

## Definitions

**Gamer Info** - information about the user.  Its attributes:

* *UUID* - a developer-facing identifier
* *Username* - the user's public username

**Product** - an item purchasable by the user.  Its attributes:

* *Identifier* - a developer-facing identifier (unique per developer)
* *Name* - the user-facing name
* *Description* - the general description of the product
* *Type* - the type of product
  * *Entitlement* -  a product which can be purchased only once and remains available to the game upon reinstallation
  * *Consumable* - a product which can be purchased repeatedly
  * *Subscription* - a product which will be re-purchased automatically every month 
* *Currency Code* - defines the type of currency
* *Local Price* - the cost of the product given the type of currency
* *Original Price* - the total price of the product given no sale is active
* *Percent Off* - the percentage reduced from the original price
* *Price In Cents* - the product price in US cents
* *Product Version To Bundle* - the version of the product


**Purchasable** - identifies a product when making purchases

**Receipt** - information about a prior purchase. it has eight attributes:

* *Product ID* - the product identifier
* *Currency* - the currency that the product was purchased in
* *Local Price* - the cost of the product given the type of currency
* *Price In Cents* - the product price in US cents
* *PurchaseDate* - when the purchase was made
* *GeneratedDate* - when the receipt was created
* *Gamer* - the gamer that purchased the product
* *UUID* - the identifier of the gamer that purchased the product

**Product Types** - there are currently three types of products:


**Cross Game Product** - a product of any type that if purchased, will be reported as purchased in all of your games



## Creating Products

In order for users to give you money, you must first create products for them to buy.  This is done on the OUYA website via the [Developer Portal](https://devs.ouya.tv/developers).
After logging in, click on the **Products** menu and then the **New Product** link.  This will take you to a page where you can create a new product.

## Adding products to your App

### Initializing the ODK

The first thing you will need to do is ensure that your application key is included in your application. You can down load the DER representation of your application key from the games area of the developer portal. To create a Java representation of the key you should use the following code:

```java
	byte[] loadApplicationKey() {
        // Create a PublicKey object from the key data downloaded from the developer portal.
        try {
            // Read in the key.der file (downloaded from the developer portal)
            InputStream inputStream = getResources().openRawResource(R.raw.key);
            byte[] applicationKey = new byte[inputStream.available()];
            inputStream.read(applicationKey);
            inputStream.close();
            return applicationKey;
        } catch (Exception e) {
            Log.e(LOG_TAG, "Unable to load application key", e);
        }

        return null;
    }
```

All IAP functionality goes through the **OuyaFacade** object.  The **OuyaFacade** singleton should be initialized at the beginning of the application and used for all IAP requests.
```java
	// Your developer id can be found in the Developer Portal
	public static final String DEVELOPER_ID = "00000000-0000-0000-0000-000000000000";

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		Bundle developerInfo = new Bundle();

		// Your developer id can be found in the Developer Portal
		developerInfo.putString(OuyaFacade.OUYA_DEVELOPER_ID, DEVELOPER_ID);

		// There are a variety of ways to store and access your application key.
		// Two of them are demoed in the samples 'game-sample' and 'iap-sample-app'
		developerInfo.putByteArray(OuyaFacade.OUYA_DEVELOPER_PUBLIC_KEY, loadApplicationKey());

		OuyaFacade.getInstance().init(this, developerInfo);
		super.onCreate(savedInstanceState);
	}
```
Of course, when your application is finished, it is polite to inform the **OuyaFacade** as well:
```java
	@Override
	protected void onDestroy() {
		OuyaFacade.getInstance().shutdown();
		super.onDestroy();
	}
```

Now we're ready for some real action!

### Check for OUYA hardware

Before invoking in-app-purchase methods check if you are running OUYA hardware.

```java
if (ouyaFacade.isRunningOnOUYASupportedHardware())
{
   //running on OUYA hardware
}
else
{
   //something else
}
```

See the [OUYA Everywhere](ouya-everywhere.md) document for more details.

### Getting Product Information

Once the products have been created, you may want your application to query the servers for the current name and price.  This is done by requesting a product list, where products that you are interested in are specified by product ID.
```java
	// This is the set of product IDs which our app knows about
	public static final List<Purchasable> PRODUCT_ID_LIST =
		Arrays.asList(new Purchasable("sharp_sword"));
```
Since this request has to travel across the Internet to the OUYA servers, the response will be returned via a callback mechanism.  It is up to your application to create an appropriate listener object.  Listeners extend the **OuyaResponseListener** and have three callback methods:

* **onSuccess**	- the request succeeded and the requested data is passed back
* **onFailure**	- the request failed and the error is passed back
* **onCancel**	- the user cancelled the request (for example, they hit "Cancel" when prompted for a password)

Now we will create our own listener!  In this example, we are extending the **CancelIgnoringOuyaResponseListener** which ignores cancels. Therefore, we only need to provide **onSuccess** and **onFailure** methods:
```java
	OuyaResponseListener<ArrayList<Product>> productListListener =
		new CancelIgnoringOuyaResponseListener<List<Product>>() {
			@Override
			public void onSuccess(List<Product> products) {
				for(Product p : products) {
					Log.d("Product", p.getName() + " costs " + p.getFormattedPrice());
				}
			}

			@Override
			public void onFailure(int errorCode, String errorMessage, Bundle errorBundle) {
				Log.d("Error", errorMessage);
			}
		};
```
So, how do we actually get the data we want? By making a request via **OuyaFacade**:
```java
	OuyaFacade ouyaFacade = OuyaFacade.getInstance();
	ouyaFacade.requestProductList(currentActivity, PRODUCT_ID_LIST, productListListener);
```
So easy!

### Making a Purchase

You will need to create a purchase listener which handles responses from the server. Each purchase will have a unique purchase ID.

```java
	PurchasableResponseListener purchaseListener = new PurchasableResponseListener() {
		@Override
		public void onSuccess(PurchaseResult result) {
			// If you previously stored the OrderId/ProductId combination, now is the time
			// to verify it.  See the requestPurchase section below.
			Log.d(TAG, result.getProductId() + " purchased.  Order: " + result.getOrderId());
		}

        @Override
        public void onFailure(int errorCode, String errorMessage, Bundle optionalData) {
			Log.d("Error", errorMessage);
        }

        @Override
        public void onCancel() {
			Log.d("Info", "User cancelled purchase");
        }
	}
```

Once we have defined the listener, we need to make the purchase. The following code allows the SDK to generate a unique purchase ID, but you can specify your own if you prefer.
```java
    public void requestPurchase(final Product product) {

        Purchasable purchasable = new Purchasable(product.getIdentifier());

		// If you want, you can store the OrderId of the Purchasable so that you can
		// associate the request with the successful PurchaseResult.
        OuyaFacade.getInstance().requestPurchase(currentActivity, purchasable, purchaseListener);
    }
```
Now we wait for the money to start pouring in...

### Querying Purchase Receipts

At this point, we can get information on products and purchase them, but what if the user purchased something in a previous play session?  The ODK provides a way to list purchase receipts. Yes, this will require another listener object!

Be sure to always query receipts for previous purchases and not just store your own "they bought the game" flag along with your saved-game data.  That method won't work if the user changes consoles (among many other edge cases).  See the "Remember to check receipts" section for more details.

**Note**: Only products that are entitlements are returned. This is to avoid re-awarding players consumable product purchases that have already been consumed.

Let us take a look at our listener:
```java
	// The receipt listener now receives a collection of tv.ouya.console.api.Receipt objects.
	CancelIgnoringOuyaResponseListener<Collection<Receipt>> receiptListListener =
		new CancelIgnoringOuyaResponseListener<Collection<Receipt>>() {
			@Override
			public void onSuccess(Collection<Receipt> receipts) {
				for (Receipt r : receipts) {
					Log.d("Receipt", r.getIdentifier() + " purchased for " + r.getFormattedPrice());
				}
			}

			@Override
			public void onFailure(int errorCode, String errorMessage, Bundle errorBundle) {
				Log.d("Error", errorMessage);
			}
		};
```
As usual, making the actual request is quite simple:
```java
	OuyaFacade.getInstance().requestReceipts(currentActivity, receiptListListener);
```

### Identifying the User

If your application talks to an external server, it is often necessary to get a unique identifier for the current user (for example, to store high scores on a website).  Games are able to access a gamer's unique id and username.
This is done in the normal pattern of creating a listener:
```java
	CancelIgnoringOuyaResponseListener<GamerInfo> gamerInfoListener =
		new CancelIgnoringOuyaResponseListener<GamerInfo>() {
			@Override
			public void onSuccess(GamerInfo result) {
				Log.d("UUID", "UUID is: " + result.getUuid());
				Log.d("Username", "Username is: " + result.getUsername());
			}

			@Override
			public void onFailure(int errorCode, String errorMessage, Bundle errorBundle) {
				Log.d("Error", errorMessage);
			}
		};
```
Then making the request:
```java
	OuyaFacade.getInstance().requestGamerInfo(currentActivity, gamerInfoListener);
```
**Note**: These game UUIDs are different across developers; two apps by different developers which query the UUID of the same user will get different results.

## Testing purchases

All purchases made from the same account that created the game will be free. In other words, developers can make purchases in their own games for free, but everyone else will be charged.

To delete your test purchases, visit your [products page](https://devs.ouya.tv/developers/products) in the developer portal.

To enable testing with multiple accounts we plan to allow developers to add a limited number of "tester accounts" for a given game sometime in the near future.

## Common onFailure errors

We have provided a class which will handle some of the common errors which the 
OUYA framework may report to your application. Using this helper class may cause your application to be forced into the background while the user enters their username and/or password.

To use this in your onFailure methods you should use the following code:

```java
            boolean wasHandledByAuthHelper =
                    OuyaAuthenticationHelper.
                            handleError(
                                    context, errorCode, errorMessage,
                                    optionalData, AUTHENTICATION_ACTIVITY_ID,
                                    listener );
```

Where context is the current Context, errorCode, errorMessage, and optionalData are the parameters passed to your onFailure method, AUTHENTICATION_ACTIVITY_ID is an ID which will be passed to your Activities onActivityResult method if the error handler needs to start the OUYA user authentication Activity, and listener is an OuyaResponseListener<Void> object which is called with the result of a request to add a user account if no accounts are already present on the system.

If the helper can handle the error the method will return true, if it can not it will return false.

## Use of Obfuscators (e.g. ProGuard)

If you pass your application through a system which may obfuscate your code you should ensure that your listeners
are not obfuscated. If an obfuscator changes the name of the listener methods the framework will not be able to
call your listener and so you will not be informed of the status of any requests you make.

If you are using ProGuard doing this involves adding the following lines to your ProGuard configuration file;

```
-keep public class * extends tv.ouya.console.api.CancelIgnoringOuyaResponseListener
-keep public class * implements tv.ouya.console.api.OuyaResponseListener
````

## Remember to check receipts

We've noticed a bunch of games not properly checking receipts: games should _always_ query the server for the most up-to-date receipt status.

Some of the many reasons to do so are:

- A voucher may have been redeemed while the game wasn't running.
- An entitlement may have been revoked by a customer service representative.
- A success response may not have reached your application (e.g. if the users broadband connection drops out while OUYA's servers are authorising the payment).
- The gamer cancelled his/her subscription.

An important thing to note is that a gamer's set of entitlements can change without the game running, thus doing things like saving off a "game has been purchased" flag and using that as the authoratative truth doesn't work correctly.  Be sure to query the servers to ensure users are presented with the correct set of options (eg: "buy now" or "play full game").
