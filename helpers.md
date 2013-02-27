## Helper classes methods

We have provided a number of helper classes and methods in the ODK designed to make developing applications easier

##### Storing game data

The **OuyaFacade** class contains two methods which allow games to store and retrieve 
small Strings which will be may be persisted even if the game is uninstalled and re-installed. 

These methods should not be used to store large amounts of data, and the data stored may be deleted if it is either excessively large or remains unused whilst other data from other games is used.

To store some data use the method :

```java
	putGameData(String name, String value)
```

To retrieve the data use the method :

```java
	String getGameData(String name)
```

If no data is available a null will be returned.

The data saved will be tied to an Android package name. It will not be possible for two games downloaded from the OUYA store to access the data from each other (because package names have to be unique for each game in the store), but it will be possible for side-loaded applications to use the same package name as your game and so could potentially manipulate it. Because of this you should not use this method for critical data without adding some form of check to the data to ensure the data has not been tampered with by a side-loaded application.