---
title: 'Mac OS X: Tweaking '"Open With"'
---

### Clean up duplicate apps in "Open With"

    /System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user

    killall Finder

### Delete an app from "Open With"

* Right-click on app you want to delete and click on *Show Package Contents*.
* Open *Info.plist* inside of the *Contents* folder.
* Look for *CFBundleTypeExtensions* section and remove `<string>ext</string>` line.
* `killall Finder`

### Add an app to "Open With"

* Right-click on app you want to add and click on *Show Package Contents*.
* Open *Info.plist* inside of the *Contents* folder.
* Look for *CFBundleTypeExtensions* section and add `<string>ext</string>` line.
* `killall Finder`

    /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -f /Applications/Name.app/

    killall Finder

### Set an app as the default option in "Open With"

* Right-click on the file and choose *Get Info*.
* Select the app you want as the default under the *Open with:* section, and click on *Change All*.
