# Upgrading to the new OUYA ODK 2.0
## Step 1: Change how OuyaFacade is initialized.
The **OuyaFacade** now requires a bundle to be initialized.  This is to facilitate the later addition of extra initialization values with as few code changes for you as possible.

To run on OUYA devices, all new games must initialize the **OuyaFacade** with the following values:

```java
	// Your developer id can be found in the Developer Portal
	public static final String DEVELOPER_ID = "00000000-0000-0000-0000-000000000000";

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		Bundle developerInfo = new Bundle();

		// Your developer id can be found in the Developer Portal
		developerInfo.putString(OuyaFacade.OUYA_DEVELOPER_ID, DEVELOPER_ID);

		// Your application key can be found on the Developer Portal as 'Signing Key'.
		// There are a variety of ways to store and access it.  
		// - two different ways are demoed in the samples 'game-sample' and 'iap-sample-app'
		developerInfo.putByteArray(OuyaFacade.OUYA_DEVELOPER_PUBLIC_KEY, APPLICATION_KEY);
		OuyaFacade.getInstance().init(this, developerInfo);

		super.onCreate(savedInstanceState);
	}
```

## Step 2: Let the **OuyaFacade** know when you're processing an activity result.

The **OuyaFacade** now occasionally uses activities that must return a result code, therefore we now require any Activity making a request must call **OuyaFacade.processActivityResult**.  

*IMPORTANT*: This step is already handled for you if inherit from OuyaActivity and call its **onActivityResult**.

```java
	@Override
	public void onActivityResult(int resultCode, int requestCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		OuyaFacade.getInstance().processActivityResult(resultCode, requestCode, data);
	}

```


## Step 3: Some of the listeners have changed.
The listeners for **requestReceipts**, **requestPurchase**, and **requestProductList** have changed.

####Receipt listeners:

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

####Purchase listeners:

```java
	PurchasableResponseListener purchaseListener = new PurchasableResponseListener() {
		@Override
		public void onSuccess(PurchaseResult result) {
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

####ProductList listeners

```java
	OuyaResponseListener<List<Product>> productListListener =
		new CancelIgnoringOuyaResponseListener<ArrayList<Product>>() {
			@Override
			public void onSuccess(ArrayList<Product> products) {
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

Note: That's a **List** now, instead of an **ArrayList**

## Step 4: **OuyaFacade** requests now require the currect activity.
Each of the request functions [**requestProductList, requestPurchase, requestReceipts, requestGamerUuid, requestGamerInfo**] now require a reference to the current activity.  Updating this is as easy as just adding the current activity to the parameter list when calling one of those functions.

```java
	OuyaFacade ouyaFacade = OuyaFacade.getInstance();
	ouyaFacade.requestProductList(currentActivity, PRODUCT_ID_LIST, productListListener);
```

## Step 5: Let us help you with encrypting your purchase requests!

Making a purchase is now as easy as

```java
    public void requestPurchase(final Product product) {

        Purchasable purchasable = new Purchasable(product.getIdentifier());

    	// If you want, you can store the OrderId of the Purchasable so that you can
    	// match the request with the PurchaseResult.
        OuyaFacade.getInstance().requestPurchase(currentActivity, purchasable, purchaseListener);
    }
```
