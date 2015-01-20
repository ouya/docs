# Community Content

The Community Content API manages discovery, storage and publishing of user generated content for your app.

## Limitations

### Total file size

While we do not limit what you can put into a piece of content, or even limit how many files you include, there are total size restrictions.  Currently the total content size is restricted to **1MB**, but we do compress all files into a single ZIP prior to uploading, so you may be able to fit more than 1MB of uncompressed content into a mod.  **Note this does not include the file size of the screenshot.**

## Initialization

The Community Content API is automatically initialized when the **OuyaFacade** is initialized within the ODK.  After initializing the **OuyaFacade**, you’ll be able to access the **OuyaContent** class:

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        // ...Standard ODK Initialization...
        
        OuyaContent.getInstance();
    }
```

### Waiting for OuyaContent

To ensure that the API has finished initializing, you can use the **OuyaContent.isInitialized()** method, or you can register the **OuyaContent.InitListener** interface to be notified when initialization has completed.  The listener's **onInitialized()** method will be called upon registering if initialization happened prior to the registration call.

```java
    private OuyaContent.InitListener mInitListener = new OuyaContent.InitListener() {
        @Override
        public void onInitialized() {
            // Good to go!
        }
        
        @Override
        public void onDestroyed() {
            // Disconnected
        }
    };

    private void initCommunityContent() {
        OuyaContent.getInstance().registerInitializedListener(mInitListener);
    }
```

### API Availability

Community Content allows your users to interact with your app on a creative level, expanding it in new ways. While this creative interaction is largely constructive, it does allow for potentially mature (or immature) content to be published as well. Community Content is not moderated prior to publishing to the DISCOVER store, so if a user has enabled the Age Gate parental control, then the API will not be accessible to games or the DISCOVER store. Your app should properly disable or hide access to Community Content using the available **OuyaContent.isAvailable()** method:

```java
    public void loadMods() {
        if(OuyaContent.getInstance().isAvailable()) {
            // ... load mods from OuyaContent ...
        } else {
            // ... disable mod functionality ...
        }
    }
```

## Creating Content

To create content you can use the method **OuyaContent.create()** which returns an **OuyaMod** object for you to fill up:

```java
    private void newMod() {
        final OuyaMod mod = OuyaContent.getInstance().create();
    }
```

### Editing

After creating the mod, you'll find there aren't any methods for actually modifying the contents outright.  To stage a new revision, you'll need to call **OuyaFacade.edit()** to get an instance of the **OuyaMod.Editor** class.  From here, you can set all of the metadata, add files and set a screenshot.  You can leave an editor open but you must call **OuyaMod.Editor.save()** in order to commit the changes.

```java
    private OuyaMod mMod;
    
    private void editMod() {
        final OuyaMod.Editor editor = mMod.edit();
        
        // Required fields:
        editor.setTitle("Custom Level");
        editor.setCategory("level");
        editor.setDescription("This is my custom level");
        
        // Add optional tags:
        editor.addTag("space");
        editor.addTag("king of the hill");
        
        // Add optional metadata for your own display:
        editor.setMetadata("difficulty=4;theme=space;mode=koth");
        
        // ...
    }
```

In order to save content, you must include at least one file and only one screenshot.  Since files and screenshots are managed by the OUYA Framework, you must request a file through the **OuyaMod.Editor** class:

```java
    OuyaContent.SaveListener mSaveListener = ...;
    
    private void editMod() {
        // ...
        
        // Add a file:
        final OutputStream file = editor.newFile("level.dat");
        // write to the file
        file.close();
        
        // Add a screenshot
        final Bitmap screenshot = getScreenshot();
        editor.addScreenshot(screenshot);
        
        // Commit the changes and close the editor
        editor.save(mSaveListener);
    }
```

Note that while the API has been designed to allow for multiple screenshots, and the framework will accept and save all the screenshots provided, only a single screenshot will ever be displayed or published to the DISCOVER store at this time.

## Searching for Content

To get a list of published mods from the DISCOVER store, you can use the **OuyaContent.search()** method:

```java
    private static final int SEARCH_LIMIT = 10;
    
    private OuyaContent.SearchListener mSearchListener;
    
    private void searchMods() {
        // Simple search:
        OuyaContent.getInstance().search(OuyaContent.SortMethod.Rating, mSearchListener);
        
        // Specify your own limit:
        OuyaContent.getInstance().search(OuyaContent.SortMethod.Rating, SEARCH_LIMIT, mSearchListener);
        
        // Specify a limit and an offset (in the event you're paging through content)
        int currentOffset = 20;
        OuyaContent.getInstance().search(OuyaContent.SortMethod.Rating, SEARCH_LIMIT, currentOffset, mSearchListener);
        
        // Limit results to a specific category
        OuyaContent.getInstance().search(OuyaContent.SortMethod.Rating, "level", mSearchListener);
        
        // Limit results to a specific category and specify a result limit
        OuyaContent.getInstance().search(OuyaContent.SortMethod.Rating, "level", SEARCH_LIMIT, mSearchListener);
        
        // All together, now!
        OuyaContent.getInstance().search(OuyaContent.SortMethod.Rating, "level", SEARCH_LIMIT, currentOffset, mSearchListener);
    }
```

The callback interface **OuyaContent.SearchListener** will provide the total number of available mods in the event that there are more than the requested amount:

```java
    private OuyaContent.SearchListener mSearchListener = new OuyaContent.SearchListener() {
        public onResults(List<OuyaMod> mods, int totalResults) {
            // handle results...
        }
        
        public onError(int code, String reason) {
            // handle error...
        }
    };
```

## Downloading Content

Once a user has decided they want a piece of content, you can call **OuyaMod.download()** to initiate the download and automatic unpacking of the content for use:

```java
    private OuyaContent.DownloadListener mDownloadListener;
    
    private void getMod(OuyaMod mod) {
        mod.download(mDownloadListener);
    }
```

The **OuyaContent.DownloadListener** interface contains methods for progress and completion and can also be used to watch downloads that were already in progress.  You can check to see if content is currently downloading by utilizing **OuyaMod.isDownloading()**:

```java
    private OuyaContent.DownloadListener mDownloadListener;
    
    private void monitor(OuyaMod mod) {
        if(mod.isDownloading()) {
            mod.registerDownloadListener(mDownloadListener);
        }
    }
```

**OuyaContent.DownloadListener.onProgress()** will report the progress of a download as a percentage based on the total download size.

## Listing and Loading Installed Content

To view and load content for usage in your app, you can use **OuyaContent.getInstalled()** to get the list of mods:

```java
    private void loadMods() {
        OuyaContent.getInstance().getInstalled(mSearchListener);
    }
```

As stated before, files are managed by the Framework, so you'll need to load them through the **OuyaMod** class:

```java
    private void loadMod(OuyaMod mod) {
        // List filenames in mod (if necessary):
        final List<String> filenames = mod.getFilename();
        
        // Open a file
        final InputStream file = mod.openFile("level.dat");
        // ...read file...
        file.close();
    }
```

## Rating and Reporting

### Rating

Aside from creating content, users can also interact with Community Content by providing ratings for published content on a 1-to-5-star scale just as they do with apps.  At the time of this writing it is up to you, the app developer, to provide a button to rate a piece of content as we do not include a button for it within its details page on the DISCOVER store. You do not have to worry about creating a UI for your users to rate the content, all you need to do is call the method **OuyaMod.rate()** to call up the system rate dialog:

```java
    public void rateMod(OuyaMod mod) {
        mod.rate();
    }
```

### Reporting

Since published Community Content is not moderated, there is the potential for a piece of content to contain offensive material. The API provides the ability for users to flag a piece of published content for review.  While we do provide a Report button on the details page for content, you can also allow users to report from within your app using the **OuyaMod.flag()** method.  Like the Rate dialog, you do not need to create a UI since **OuyaMod.flag()** calls up a system dialog:

```java
    public void reportMod(OuyaMod mod) {
        mod.flag();
    }
```

## Example
There is now a sample Community Content app distributed in the ODK. It is located in the `Samples/cc-sample` folder. This app allows the user to create graphical emblems and share them.
