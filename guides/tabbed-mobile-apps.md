---
description: >-
  Tabbed apps display a set of pre-configured tabs with each one providing a
  distinct functionality to the app, by way of an Integrated Web App.
---

# Tabbed Mobile Apps

Each tab displays a preconfigured IWA according to the app's configuration data.

## A few facts about tabs in Mobile Apps

### Tabs are silos

Tab IWAs will not be able to inter-communicate between tabs, other than via events through the native app container such as:

* Navigate to a document in tab “Calculator”
* Switch to tab “Calculator”
* Fundamentally, each tab will behave as though it was an independent IWA centric app as described above, with the exception that it can call events on the tabbed container.

### Each tab has \(and manages\) its own navigation stack

_On iOS_: When using the ‘back’ button causes the tab to navigate to the root of it’s stack, the back button will be hidden or disabled and and no further back capability will apply. e.g. Tab changes will not be included in the back stack.

_On Android_: Each tab will handle it’s own backstack BUT, because the back button is ubiquitous on most android versions, Tab changes will also be incorporated into the back stack.

When triggering a "back" navigation, the pattern used is:

* exhaust the entire back stack in the current tab.
* When/If the Tab backstack stack is exhausted – go the the "initial" tab for the app, typically a "Home" tab, and keep walking back through its navigation stack
* When the entry \(e.g. "Home"\) tab exhausts its backstack, triggering another "back" navigation will cause the app to exit back to the device state prior to the app's launch.

### No secondary actions presented

Tabbed apps display a tab bar and do not currently have support for displaying a toolbar, which means they do not support secondary actions. If secondary actions are defined inthe app configuration, they will be ignored.

Primary actions are supported the same way as for Linear apps.

## Tab-related events

### iwa/navigation event

The iwa/navigation event will have an optional “target” property added which will identify the target tab by tab id. This will be ignored in non-tabbed apps.

### app/switchtotab event

The “switchtotab” event includes a mandatory “id” attribute which identifies the target tab.

The “switchtotab” event has the following optional attributes :

* reset: if true, the tab will reload its initial state.

## Showing/hiding tabs

It is possible to hide/show the tab bar based on the configuration of the currently displayed IWA.

The IWA configuration data can indicate whether an IWA will hide/show the tabbar by default.

Individual IWA route definitions may also override the hide/show state of the tabbar. A practical example of this may be for authentication purposes. The first tab \(we’ll call it "Home"\) may launch as a login screen and then transition to the main app \("Main"\) screen once authentication has occurred. The Login IWA that is first launched may indicate that the tab bar should be hidden. The "Main" IWA may indicate that it should be shown. Once authentication is completed, the tab bar with all associated tabs will appear as the tab transitions to display the Main IWA. Going "back" to the Login IWA will cause the tabbar to be hidden again.

## Example configurations

### App configuration

```javascript
{
    // ...
    "navigation" : {
        "type" : "tabbed",
        "navigation_options" : {
            "tabbed" : {
                "launch_tab_id" : "t1",
                "tabs" : {
                    "t1" : {
                        "presentation" : {
                            "icon_name": "pencil",
                            "title": {
                                "key": "tab-t1-title",
                                "translations": {
                                     "en": "Main"
                                }
                            }
                        },
                        "launch_data" : { "webapp_id" : "main_webapp" }
                    },
                    "t2" : {
                        "presentation" : {
                            "icon_name": "clock-o",
                            "title": {
                                "key": "tab-t2-title",
                                "translations": {
                                     "en": "2nd"
                                }
                            }
                        },
                        "launch_data" : { "webapp_id" : "2nd_webapp" }
                    }
                },
                "order" : [ "t1", "t2" ]
            }
        }
    }
}
```

### IWA configuration

```javascript
{
    "id": "login",
    "show_tabs": true,
    "routes": {
        "/": {
            "show_tabs": false,
            "actions": {
                "primary": []
            }
        },
        "/menu": {
            "actions": {
                "primary": []
            }
        }
    }
}
```



