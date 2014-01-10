## Development Notes for Custom Launchers

Custom launchers provide alternatives for our user-base and help spur innovation.  

One requirement for custom launchers, is that they recognize when a system update is available and take the user to the Update screen.  The notification of an available update is done via a sticky intent.  Some sample code for this is:

```
private static final String SYSTEM_UPDATE_AVAILABLE_ACTION = "tv.ouya.update.UPDATE_AVAILABLE_ACTION";

@Override
protected void onResume() {
    super.onResume();

    Intent updateIntent = registerReceiver(null, new IntentFilter(SYSTEM_UPDATE_AVAILABLE_ACTION));
    if (updateIntent != null) {
        new AlertDialog.Builder(this)
                .setTitle("Update Available")
                .setMessage("Go update now!")
                .setCancelable(false)
                .setPositiveButton("OK", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse("ouya://launcher/update")));
                    }
                })
                .create()
                .show();
    }
}
```


To help make them more usable, we've exposed the ability for them to open up certain activities in the standard OUYA launcher:

# Pair Controllers

To be opened when no input devices are present, allows users to pair new controllers.

```
startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse("ouya://launcher/manage/controllers/pairing")));
```

# Manage

Displays the OUYA Manage menu, allows users to change account/system settings.

```
startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse("ouya://launcher/manage")));
```

# Network Setup

Open the Network screen directly (this is also one of the options within the Manage menu).

```
startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse("ouya://launcher/manage/network")));
```

# Discover

Displays the OUYA Discover screen, allows users to browse/download new games.

```
startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse("ouya://launcher/discover")));
```
