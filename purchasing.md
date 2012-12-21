### In-App Purchasing

In-App Purchasing (IAP) is how your app can make money.  The OUYA Developer Kit (ODK) is designed to be both easy-to-use and secure.

##### Definitions

Product -- this is what is purchased by the user.  It has three attributes:
  Identifier -- a developer-facing identifier (unique per developer)
	Price -- the cost of the product
	Name -- the user-facing name

Purchasable -- identifies a product when making purchases

Receipt -- information about a prior purchase
	Product ID -- the product identifier
	Price -- the amount that was paid
	PurchaseDate -- when the purchase was made

Entitlements -- a product which can be purchased only once [not yet implemented].
Consumable -- a product which can be purchased repeatedly.
Subscription -- a product which will be purchased automatically on a set schedule [not yet implemented].

#### Initializing the ODK

All IAP functionality goes through the OuyaFacade object.  One of these should be created at the beginning of the application and used for all IAP requests.
```java
	// Your developer id can be found in the Developer Portal
	public static final String DEVELOPER_ID = "00000000-0000-0000-0000-000000000000";

	// Create an OuyaFacade
	private OuyaFacade OuyaFacade = new OuyaFacade();

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		OuyaFacade.init(this, DEVELOPER_ID);
		super.onCreate(savedInstanceState);
	}

Of course, when you're application is finished it's polite to inform the OuyaFacade as well:
```java
	@Override
	protected void onDestroy() {
		OuyaFacade.shutdown();
		super.onDestroy();
	}
```
Now we're ready for some real action! 

#### Creating Products

In order for users to give you money, you must first create products for them to buy!  This is done on the OUYA website via the Developer Portal.
After logging in, click on the Products menu and then the New Product link.  This will take you to a page where you can enter the product Id, price and name.

#### Getting Product Information

Once the products have been created, the application may want to query the servers for the current name/price.  This is done by requesting a product list, where products that you are interested in are specified by product id.
```java
	// This is the set of product IDs which our app knows about
	public static final List<Purchasable> PRODUCT_ID_LIST =
		Arrays.asList(new Purchasable("sharp_sword"));
```
Since this request has to travel across the Internet to the OUYA servers, the response will be returned via a callback mechanism.  It is up to your application to create an appropriate listener object.  Listeners extend the OuyaResponseListener and have three callback methods:
onSuccess	-- the request succeeded and the requested data is passed back
onFailure	-- the request failed and the error is passed back
onCancel	-- the user cancelled the request (i.e. they hit cancel when prompted for a password)

Now let's create our own listener!  In this example we're extending the CancelIgnoringOuyaResponseListener which ignores cancels, thus we only need to provide onSuccess and onFailure methods:
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
Ok, so how do we actually get the data we want?  By making a request via the OuyaFacade:
```java
	OuyaFacade.requestProductList(PRODUCT_ID_LIST, productListListener);
```
So easy!

#### Making a Purchase

Once users use your application they'll be super addicted and will eagerly buy all the products you offer!  To make that happen we must first create a new listener:
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
	OuyaFacade.requestPurchase(productToBuy, purchaseListener);
```
Now to wait for the money to start pouring in...

#### Querying Purchase Receipts

At this point we can get information on products and buy them, but what if the user had bought something in a previous play session?  The ODK provides a way to list purchase receipts, and yes this will require another listener object!
For security reasons, the receipts are returned encrypted and must be decrypted within the application itself.  To assist with this, you can use the OuyaEncryptionHelper's decryptReceiptResponse method.
Let's take a look at our listener:
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
	OuyaFacade.requestReceipts(receiptListListener);
```
The receipt decryption happens inside the app to help prevent hackers.  By moving it into each application there is no "one piece of code" they can attack to break encryption for all applications.  In the future we'll encourage developers to not even use the decryptReceiptResponse method and to move that into your application, and to perturb what it does slightly (changing for-loops to while-loops, etc) to help make things even more secure.
Currently the ODK is under heavy development so the helper method will help insulate you from our under-the-hood changes.

#### Identifying the User

If your application talks to an external server it's often necessary to get a unique identifier for the current user (i.e. to store high scores on a website).  This is done in the normal pattern of creating a listener:
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
	OuyaFacade.requestGamerUuid(gamerUuidListener);
```
Note that these game uuids are different across developers: two apps by different developers that query the uuid of the same user will get different results.
