## In-App Purchasing

In-App Purchasing (IAP) is how your app can make money.  The OUYA Developer Kit (ODK) is designed to be both easy to use and secure.

#### Definitions

**Product** -- this is what is purchased by the user.  It has three attributes:
* *Identifier* -- a developer-facing identifier (unique per developer)
* *Price* -- the cost of the product
* *Name* -- the user-facing name

**Purchasable** -- identifies a product when making purchases

**Receipt** -- information about a prior purchase. it has three attributes:
* *Product ID* -- the product identifier
* *Price* -- the amount that was paid
* *PurchaseDate* -- when the purchase was made

**Entitlements** --  a product which can be purchased only once and remains available to the game upon reinstallation

**Consumable** -- a product which can be purchased repeatedly

**Subscription** -- a product which will be purchased automatically on a set schedule [not yet implemented]

#### Initializing the ODK

All IAP functionality goes through the **OuyaFacade** object.  One of these objects should be created at the beginning of the application and used for all IAP requests.
```java
	// Your developer id can be found in the Developer Portal
	public static final String DEVELOPER_ID = "00000000-0000-0000-0000-000000000000";

	// Create an OuyaFacade
	private OuyaFacade ouyaFacade = OuyaFacade.getInstance();

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		ouyaFacade.init(this, DEVELOPER_ID);
		super.onCreate(savedInstanceState);
	}
```
Of course, when your application is finished, it is polite to inform the **OuyaFacade** as well:
```java
	@Override
	protected void onDestroy() {
		ouyaFacade.shutdown();
		super.onDestroy();
	}
```
The OuyaFacade also has a testing mode where you can buy products without spending actual money.  When testing mode is enabled, the purchase confirmation dialog will be red and have the word "[TEST]" in front of messages to make sure you know what mode you're in (don't want to ship games in test mode!).
```java
	ouyaFacade.setTestMode();
```
Now we're ready for some real action! 

#### Creating Products

In order for users to give you money, you must first create products for them to buy.  This is done on the OUYA website via the [Developer Portal](https://devs.ouya.tv/developers).
After logging in, click on the **Products** menu and then the **New Product** link.  This will take you to a page where you can enter the product ID, price and name.

#### Getting Product Information

Once the products have been created, you may want your application to query the servers for the current name and price.  This is done by requesting a product list, where products that you are interested in are specified by product ID.
```java
	// This is the set of product IDs which our app knows about
	public static final List<Purchasable> PRODUCT_ID_LIST =
		Arrays.asList(new Purchasable("sharp_sword"));
```
Since this request has to travel across the Internet to the OUYA servers, the response will be returned via a callback mechanism.  It is up to your application to create an appropriate listener object.  Listeners extend the **OuyaResponseListener** and have three callback methods:

* **onSuccess**	-- the request succeeded and the requested data is passed back
* **onFailure**	-- the request failed and the error is passed back
* **onCancel**	-- the user cancelled the request (for example, they hit "Cancel" when prompted for a password)

Now we will create our own listener!  In this example, we are extending the **CancelIgnoringOuyaResponseListener** which ignores cancels. Therefore, we only need to provide **onSuccess** and **onFailure** methods:
```java
	OuyaResponseListener<ArrayList<Product>> productListListener =
		new CancelIgnoringOuyaResponseListener<ArrayList<Product>>() {
			@Override
			public void onSuccess(ArrayList<Product> products) {
				for(Product p : products) {
					Log.d("Product", p.getName() + " costs " + p.getPriceInCents());
				}
			}

			@Override
			public void onFailure(int errorCode, String errorMessage) {
				Log.d("Error", errorMessage);
			}
		};
```
So, how do we actually get the data we want? By making a request via **OuyaFacade**:
```java
	ouyaFacade.requestProductList(PRODUCT_ID_LIST, productListListener);
```
So easy!

#### Making a Purchase

Once users experience your application, they will become super addicted and eagerly buy all the products you offer!  To make that happen, we must first create a new listener:
```java
	CancelIgnoringOuyaResponseListener<Product> purchaseListener =
		new CancelIgnoringOuyaResponseListener<Product>() {
			@Override
			public void onSuccess(Product product) {
				Log.d("Purchase", "Congrats you bought: " + product.getName());
			}

			@Override
			public void onFailure(int errorCode, String errorMessage) {
				Log.d("Error", errorMessage);
			}
		};
```
With that listener defined, making the purchase is just one line:
```java
	Purchasable productToBuy = PRODUCT_ID_LIST.get(0);
	ouyaFacade.requestPurchase(productToBuy, purchaseListener);
```
Now we wait for the money to start pouring in...

#### Querying Purchase Receipts

At this point, we can get information on products and purchase them, but what if the user purchased something in a previous play session?  The ODK provides a way to list purchase receipts. Yes, this will require another listener object!

**Note**: Only products that are entitlements are returned. This is to avoid re-awarding players consumable product purchases that have already been consumed.

For security reasons, the receipts are returned encrypted and must be decrypted within the application itself.  To assist with this, you can use the **OuyaEncryptionHelper**'s **decryptReceiptResponse** method.

Let us take a look at our listener:
```java
	CancelIgnoringOuyaResponseListener<String> receiptListListener =
		new CancelIgnoringOuyaResponseListener<String>() {
			@Override
			public void onSuccess(String receiptResponse) {
				OuyaEncryptionHelper helper = new OuyaEncryptionHelper();
				List<Receipt> receipts = null;
				try {
					receipts = helper.decryptReceiptResponse(receiptResponse);
				} catch (IOException e) {
					throw new RuntimeException(e);
				}
				for (Receipt r : receipts) {
					Log.d("Receipt", "You have purchased: " + r.getIdentifier())
				}
			}

			@Override
			public void onFailure(int errorCode, String errorMessage) {
				Log.d("Error", errorMessage);
			}
		};
```
As usual, making the actual request is quite simple:
```java
	ouyaFacade.requestReceipts(receiptListListener);
```
The receipt decryption happens inside the application to help prevent hacking.  By moving the decryption into each application there is no "one piece of code" a hacker can attack to break encryption for all applications.  In the future, we will encourage developers to avoid using the **decryptReceiptResponse** method. They will need to move the method into their application, and *perturb* what it does slightly (changing for-loops to while-loops, and so forth) to help make things even more secure.
Currently, the ODK is under heavy development, so the helper method will assist in insulating you from our "under-the-hood" changes.

#### Identifying the User

If your application talks to an external server, it is often necessary to get a unique identifier for the current user (for example, to store high scores on a website).  This is done in the normal pattern of creating a listener:
```java
	CancelIgnoringOuyaResponseListener<String> gamerUuidListener =
		new CancelIgnoringOuyaResponseListener<String>() {
			@Override
			public void onSuccess(String result) {
				Log.d("UUID", "UUID is: " + result);
			}

			@Override
			public void onFailure(int errorCode, String errorMessage) {
				Log.d("Error", errorMessage);
			}
		};
```
Then making the request:
```java
	ouyaFacade.requestGamerUuid(gamerUuidListener);
```
**Note**: These game UUIDs are different across developers; two apps by different developers which query the UUID of the same user will get different results.

#### Listeners And Obfuscators (e.g. ProGuard)

If you pass your application through a system which may obfuscate your code you should ensure that your listeners
are not obfuscated. If an obfuscator changes the name of the listener methods the framework will not be able to
call your listener and so you will not be informed of the status of any requests you make.

If you are using ProGuard doing this involves adding the following lines to your ProGuard configuration file;

```
-keep public class * extends tv.ouya.console.api.CancelIgnoringOuyaResponseListener
-keep public class * implements tv.ouya.console.api.OuyaResponseListener
````

