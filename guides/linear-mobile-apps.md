---
description: Linear navigation is the default type of navigation for Mobile apps.
---

# Linear Mobile Apps

When you design a new mobile app, one of the first chocies to make is the type of navigation it should support: linear or tabbed. This page explains all you need to know about linear apps. For more information on tabbed apps, please refer to [the relevant documentation page](tabbed-mobile-apps.md).

Linear Mobile Apps have a single navigation stack onto which new routes can be pushed. This stack can be  walked backwards by using common controls to do so on both iOS and Android.

They also have support for an optional toolbar presented at the bottom of their screen, within which secondary actions can be presented to the user. A typical toolbar would include between one and five buttons which can trigger events such as navigation event or iwa/trigger\_action events to, you guessed it, trigger any behaviour in the currently displayed IWA.

## Example configuration

```javascript
{
    // ...
    "navigation": {
        "type": "linear",
        "navigation_options": {
            "linear": {
                "launch_data": {
                    "webapp_id": "main_webapp"
                }
            }
        }
    }
}
```

