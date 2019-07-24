---
description: >-
  Tabbed apps display a set of pre-configured tabs with each one providing a
  distinct functionality to the app, by way of an Integrated Web App.
---

# Tabbed Mobile Apps

Each tab displays a preconfigured IWA according to the app's configuration data.

## A few facts about tabs in Mobile Apps

### Tabs are fixed for an entire app

The definition of tabs for an app is loaded when the app starts and cannot be dynamically modified.

This means it is impossible to, for example, change which tabs are displayed after a user has logged in.

It is however possible to show and hide tabs \(e.g. to show them to users only after they have logged in\) by using the "show\_tabs" configuration parameter in app data and webapp.json files. See "The tab bar can be hidden".

It is also possible to display/hide individual tabs. See "Invididuals tabs can be hidden" below.

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

### The tab bar can be hidden

The entire tab bar can be hidden using the "show\_tabs" property in both the app-level App Data and the IWA-level webapp.json file, in which case individual routes can decide to show/hide the entire tab bar. See examples below.

### Individual tabs can be hidden

It is possible to configure tabs which are:

* not displayed by default
* can be displayed/hidden at any time

Typical use-cases for hiding individual tabs are:

* a top-level area of an app which should not be available to users until they perform a particular action
* configuring many tabs for all types of users and displaying select subsets of the tabs based on the type of user once they are identified \(e.g. after logging in\).

You can display/hide individual tabs at any time by using the [app/set\_tab\_appearance event](../iwa-framework/events/app.md#set_tab_appearance).

## Tab-related events

### iwa/navigation event

The iwa/navigation event will have an optional “target” property added which will identify the target tab by tab id. This will be ignored in non-tabbed apps.

### app/switchtotab event

The “switchtotab” event includes a mandatory “id” attribute which identifies the target tab.

The “switchtotab” event has the following optional attributes :

* reset: if true, the tab will reload its initial state.

## Showing/hiding tabs

### The tab bar can be hidden

#### By default, in App Data configuration

The entire tab bar can be hidden using the "show\_tabs" property in the app-level App Data.

#### For any route, in IWA configuration

It is also possible to hide/show the tab bar based on the configuration of the currently displayed IWA.

The IWA configuration data can indicate whether an IWA will hide/show the tab bar by default.

Individual IWA route definitions may also override the hide/show state of the entire tab bar.

A practical example of this may be for authentication purposes. The first tab \(we’ll call it "Home"\) may launch as a login screen and then transition to the main app \("Main"\) screen once authentication has occurred. The Login IWA that is first launched may indicate that the tab bar should be hidden. The "Main" IWA may indicate that it should be shown. Once authentication is completed, the tab bar with all associated tabs will appear as the tab transitions to display the Main IWA. Going "back" to the Login IWA will cause the tabbar to be hidden again.

### Individual tabs can be hidden

It is possible to configure individual tabs which are:

* not displayed by default
* can be displayed/hidden at any time

Typical use-cases for hiding individual tabs are:

* a top-level area of an app which should not be available to users until they perform a particular action
* configuring many tabs for all types of users and displaying select subsets of the tabs based on the type of user once they are identified \(e.g. after logging in\).

You can display/hide individual tabs at any time by using the [app/set\_tab\_appearance event](../iwa-framework/events/app.md#set_tab_appearance).

## Example configurations

### App Data configuration

```javascript
{
    // ...
    "navigation" : {
        "type" : "tabbed",
        "navigation_options" : {
            "tabbed" : {
                "show_tabs": false,
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
                        "hidden": true,
                        "launch_data" : { "webapp_id" : "2nd_webapp" }
                    }
                },
                "order" : [ "t1", "t2" ]
            }
        }
    }
}
```

### IWA configuration \(webapp.json file\)

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



