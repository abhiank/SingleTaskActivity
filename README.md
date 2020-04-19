# SingleTaskActivity
This is a sample app to demonstrate the behaviour of launcher activity with singleTask launch mode

**The scenario is this -**

There is a `MainActivity` which is the launcher activity. For a certain requirement of wanting to keep only one instance of the main activity alive across tasks, we mark its launch mode as `singleTask`. This fulfills that requirement and only one instance exists. 

Now other activity also need to be opened for other features etc which would stack up on top of the `MainActivity`. The problem which happens is that if we close the app by pressing home and restart the app using the app icon then all activities on top of MainActivity are killed by called `onDestroy` and `onDetachedFromWindow` and `MainActivity` is brought to front. `MainActivity` recives a call to `onNewIntent` but the same instance comes to front and a new one is not created. The anomoly to this is that if we open the app from the recents menu, then this does not happen and the stack remains intact and the newly opened activity remains on top. 

To reproduce, 

1. Open app
2. Click on "Open Activity 2"
3. `RegularActivity` opens
4. Click home
5. Reopen app by clicking on app icon from launcher app
6. Observe that Regular activity has been killed and MainActivity is now open. 

OR 

Repeat Steps 1-4.   
5. Go to recents  
6. Click on app  
7. Observe that the stack is intact and Regular activity is on top. 

This is most likely because this seems to the default behaviour of `singleTask`.

This has been discussed on the following stackoverflow threads - 

* [https://stackoverflow.com/questions/2417468/android-bug-in-launchmode-singletask-activity-stack-not-preserved
]()
* [https://stackoverflow.com/questions/61117733/why-is-launcher-activity-with-launchmode-singletask-always-pushed-to-top-of-ba?noredirect=1#comment108455060_61117733]()
