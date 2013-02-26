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

The first thing you will need to do is ensure that your application key is included in your application. You can down load the DER representation of your application key from the games area of the developer portal. To create a Java representation of the key you should use the following code:

```java
        // Create a PublicKey object from the key data downloaded from the developer portal.
        try {
            X509EncodedKeySpec keySpec = new X509EncodedKeySpec(APPLICATION_KEY);
            KeyFactory keyFactory = KeyFactory.getInstance("RSA");
            mPublicKey = keyFactory.generatePublic(keySpec);
        } catch (Exception e) {
            Log.e(LOG_TAG, "Unable to create encryption key", e);
        }
```

Once you have your key embedded you will need to create a purchase listener which handles responses from the server. Each purchase will have a unique purchase ID. In this example we use a Map of these IDs to the Product being purchased to allow us to determine what has been purchased :

```java
	CancelIgnoringOuyaResponseListener<String> purchaseListener =
		new CancelIgnoringOuyaResponseListener<String>() {
			@Override
			public void onSuccess(String response) {
				Product product;
	            try {
    	            OuyaEncryptionHelper helper = new OuyaEncryptionHelper();

        	        JSONObject response = new JSONObject(result);
            	    if(response.has("key") && response.has("iv")) {
                	    id = helper.decryptPurchaseResponse(response, mPublicKey);
                    	Product storedProduct;
	                    synchronized (mOutstandingPurchaseRequests) {
    	                    storedProduct = mOutstandingPurchaseRequests.remove(id);
        	            }
            	        if(storedProduct == null) {
	                        onFailure(
	                        	OuyaErrorCodes.THROW_DURING_ON_SUCCESS, 
	                        	"No purchase outstanding for the given purchase request",
	                        	Bundle.EMPTY);
    	                    return;
        	            }
        	            product = storedProduct;
            	    } else {
                	    product = new Product(new JSONObject(result));
                    	if(!mProduct.getIdentifier().equals(product.getIdentifier())) {
                        	onFailure(
                        		OuyaErrorCodes.THROW_DURING_ON_SUCCESS, 
                        		"Purchased product is not the same as purchase request product", 
                        		Bundle.EMPTY);
	                        return;
    	                }
        	        }
        	        
					Log.d("Purchase", "Congrats you bought: " + product.getName());
            	} catch (Exception e) {
					Log.e("Purchase", "Your purchase failed.", e);
				}
			}

			@Override
			public void onFailure(int errorCode, String errorMessage, Bundle info) {
				Log.d("Error", errorMessage);
			}
		};
```

Once we have defined the listener, we need to make the purchase. The following code provides a method which encrypts the purchase in the way the server expects;
```java
    public void requestPurchase(final Product product)
        throws GeneralSecurityException, UnsupportedEncodingException, JSONException {
        SecureRandom sr = SecureRandom.getInstance("SHA1PRNG");

        // This is an ID that allows you to associate a successful purchase with
        // it's original request. The server does nothing with this string except
        // pass it back to you, so it only needs to be unique within this instance
        // of your app to allow you to pair responses with requests.
        String uniqueId = Long.toHexString(sr.nextLong());

        JSONObject purchaseRequest = new JSONObject();
        purchaseRequest.put("uuid", uniqueId);
        purchaseRequest.put("identifier", product.getIdentifier());
        // This value is only needed for testing, not setting it results in a live purchase
        purchaseRequest.put("testing", "true"); 
        String purchaseRequestJson = purchaseRequest.toString();

        byte[] keyBytes = new byte[16];
        sr.nextBytes(keyBytes);
        SecretKey key = new SecretKeySpec(keyBytes, "AES");

        byte[] ivBytes = new byte[16];
        sr.nextBytes(ivBytes);
        IvParameterSpec iv = new IvParameterSpec(ivBytes);

        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding", "BC");
        cipher.init(Cipher.ENCRYPT_MODE, key, iv);
        byte[] payload = cipher.doFinal(purchaseRequestJson.getBytes("UTF-8"));

        cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding", "BC");
        cipher.init(Cipher.ENCRYPT_MODE, mPublicKey);
        byte[] encryptedKey = cipher.doFinal(keyBytes);

        Purchasable purchasable =
                new Purchasable(
                        product.getIdentifier(),
                        Base64.encodeToString(encryptedKey, Base64.NO_WRAP),
                        Base64.encodeToString(ivBytes, Base64.NO_WRAP),
                        Base64.encodeToString(payload, Base64.NO_WRAP) );

        synchronized (mOutstandingPurchaseRequests) {
            mOutstandingPurchaseRequests.put(uniqueId, product);
        }
        ouyaFacade.requestPurchase(purchasable, new PurchaseListener(product));
    }
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
	                JSONObject response = new JSONObject(receiptResponse);
    	            if(response.has("key") && response.has("iv")) {
        	            receipts = helper.decryptReceiptResponse(response, mPublicKey);
            	    } else {
                	    receipts = helper.parseJSONReceiptResponse(receiptResponse);
                	}
				} catch (Exception e) {
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

