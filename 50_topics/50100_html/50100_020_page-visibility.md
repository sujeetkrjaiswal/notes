# Page Visibility

You cannot rely on pagehide, beforeunload, and unload events to fire on mobile platforms. This is not a bug in your favorite browser; this is due to how all mobile operating systems work. An active application can transition into a “background state” via several routes:

* The user can click on a notification and switch to a different app.
* The user can invoke the task switcher and move to a different app.
* The user can hit the “home” button and go to homescreen.
* The OS can switch the app on users behalf - e.g. due to an incoming call.

Once the application has transitioned to background state, it may be killed without any further ceremony - e.g. the OS may terminate the process to reclaim resources, the user can swipe away the app in the task manager. As a result, you should assume that “clean shutdowns” that fire the pagehide, beforeunload, and unload events are the exception, not the rule.

To provide a reliable and consistent user experience, both on desktop and mobile, the application must use Page Visibility API and execute its session save and restore logic whenever visibilityChange state changes. This is the only event your application can count on.

```javascript
var pageVisibility = document.visibilityState;

// subscribe to visibility change events
document.addEventListener('visibilitychange', function() {
  // fires when user switches tabs, apps, goes to homescreen, etc.
  if (document.visibilityState == 'hidden') {
    // ...
  }

  // fires when app transitions from prerender, user returns to the app tab.
  if (document.visibilityState == 'visible') {
    // ...
   }
});
```

