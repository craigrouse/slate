---
title: Consent Manager Module

language_tabs: # must be one of https://git.io/vQNgJ
  - swift: Swift
  - objective_c: Objective-C
  - java: Java
  - kotlin: Kotlin

toc_footers:
  - <a href='https://community.tealiumiq.com'> > Tealium Learning Community</a>

includes:

search: true
---

# Mobile Consent Manager

This module provides a convenient implementation of the [Tealium Consent Manager](https://community.tealiumiq.com/t5/iQ-Tag-Management/Explicit-Consent-Prompt-Manager/ta-p/22714), which can help your app to comply with legal obligations, such as GDPR. Once implemented, your user will be able to easily consent to or decline tracking, and the Tealium SDK will store and respect their choice from that point on.

## How It Works

The Consent Manager module is enabled by default. In its default state, before any consent preferences have been set, it will queue all events received prior to the initial consent being granted by the user (the "not determined" state). It will continue doing this until such point as the user consents, or revokes their consent. At the point where the app user makes a selection, the following will happen:

### If the user consents to tracking

* All previously-queued tracking calls will be sent
* The user's consent state will be stored permanently, and will be checked each time a new tracking call is made
* If the user has set customized preferences, and consented only to specific categories, these categories will be respected when tags or connectors are fired in Tealium iQ and EventStream respectively
* If [Consent Logging](https://community.tealiumiq.com/t5/Universal-Data-Hub/Consent-Change-Event-Specifications/ta-p/23213) is enabled, a single tracking call will be sent to UDH, to keep track of the user's consent preferences for auditing purposes only.

### If the user grants partial consent (selected categories)
* If [Consent Logging](https://community.tealiumiq.com/t5/Universal-Data-Hub/Consent-Change-Event-Specifications/ta-p/23213) is enabled, a single tracking call will be sent to UDH, to keep track of the user's consent preferences for auditing purposes only

### If the user declines tracking

* All previously-queued tracking calls will be purged from the queue
* All future tracking calls will be canceled
* No events will be sent to the UDH. Declined consent state cannot be tracked in the UDH.

# Quick Start
This quick start guide shows the basic API usage only. Please see API reference for full description of API methods.

## Consent Granted

```swift
let config = TealiumConfig(...)
self.tealium?.consentManager()?.setConsentStatus(.consented)
```
User granted tracking consent (all categories)

## Consent Declined

```swift
let config = TealiumConfig(...)
self.tealium?.consentManager()?.setConsentStatus(.notConsented)
```
User declined tracking consent (all categories)

## Partial Consent Granted

```swift
let config = TealiumConfig(...)
self.tealium?.consentManager()?.setConsentCategories([.analytics,.bigData,.monitoring])
```
User granted consent only for specified tracking categories

# API Reference

## Consent Manager Core API

```swift
class MyClass {
	var teal: Tealium?
	
	public init () {
        let config = TealiumConfig(account: "<youraccount>",
                                   profile: "demo",
                                   environment: "dev",
                                   datasource: "abc123",
                                   optionalData: nil)
		
		self.teal = Tealium(config) { 
			// Note: this is inside the completion block after initialization
		
			// self.teal is guaranteed to be initialized inside the completion block
			self.teal?.consentManager()?.### // where ### is any public API method on the consentManager object
			
		}
	
	}

}
```

This code shows how to access the consent manager

## Add Consent Delegate

```swift
tealium?.consentManager()?.addConsentDelegate(self)
```

Registers a specific class conforming to the TealiumConsentDelegate protocol

| Parameters | Description                                  |
|------------|----------------------------------------------|
| delegate   | A class conforming to TealiumConsentDelegate |

## Remove All Consent Delegates

```swift
tealium?.consentManager()?.removeAllConsentDelegates()
```

Removes all consent delegates added by you. Retains the built-in delegate created by the TealiumConsentManagerModule class.

## Set Consent Status

```swift
tealium?.consentManager()?.setConsentStatus(TealiumConsentStatus)
```

Sets the user's current consent status. Designed to be called from your consent management preferences screen in your app when the user enables or disables tracking.

<aside class="notice">
Will always set the list of consented categories to include ALL available consent categories, if the status is <code>.consented</code>. Does not allow categories to be set selectively.
</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | .consented/.notConsented |

## Set Consent Categories

```swift
tealium?.consentManager()?.setConsentCategories([TealiumConsentCategories])
```

Sets the user's current consent categories. Designed to be called from your consent management preferences screen in your app. 

<aside class="notice">Will always set consent status to <code>.consented</code>.</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| categories     | An array of values from the TealiumConsentCategories enum | [.analytics,.bigData]|

## Set Consent Status With Categories

```swift
tealium?.consentManager()?.setConsentStatusWithCategories(status: .consented, categories: [.analytics])
```

Sets consent status and categories in a single call.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | `.consented`/`.notConsented`|
| categories     | An array of values from the TealiumConsentCategories enum | [.analytics,.bigData]|

## Current Consent Status

```swift
tealium?.consentManager()?.currentConsentStatus() -> TealiumConsentStatus
```
Returns the current consent status

## Current Consent Categories

```swift
tealium?.consentManager()?.currentConsentCategories() -> [TealiumConsentCategories]
```
Returns the current consent categories

## Current Consent Preferences

```swift
tealium?.consentManager()?.currentConsentPreferences -> TealiumConsentUserPreferences
```
Returns the TealiumConsentUserPreferences object (by value -> struct)

## Clear Stored Consent Preferences

```swift
tealium?.consentManager()?.clearUserConsentPreferences()
```
Clears all currently stored consent preferences. Reverts to "not determined" consent state, with no categories.

# TealiumConfig Methods

## Enable Consent Logging

```swift
let config = TealiumConfig(...)
config.setConsentLoggingEnabled(true)
```

Enables the [Consent Logging](https://community.tealiumiq.com/t5/Universal-Data-Hub/Consent-Change-Event-Specifications/ta-p/23213) feature, which sends all consent status changes to UDH for auditing purposes.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| enabled     | Bool | true (default: false)|

## Consent Logging Enabled?

```swift
let config = TealiumConfig(...)
let isEnabled = config.isConsentLoggingEnabled()
```
Returns the current state of the consent logging feature.

## Set Consent Status

```swift
let config = TealiumConfig(...)
config.setConsentStatus(TealiumConsentStatus)
```
Sets the initial consent status for the user when the library starts up for the first time. If there are saved preferences, these will override any preferences passed in the config object.

<aside class="notice">Will always set the list of consented categories to include ALL available consent categories, if the status is <code>.consented</code>. Does not allow categories to be set selectively.</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | .consented/.notConsented |

## Set Consent Categories

```swift
let config = TealiumConfig(...)
config.setConsentCategories([TealiumConsentCategories])
```
Sets the user's initial consent categories when the library starts up for the first time. If there are saved preferences, these will override any preferences passed in the config object.

<aside class="notice">Will always set consent status to <code>.consented</code>.</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| categories     | An array of values from the TealiumConsentCategories enum | [.analytics,.bigData]|

## Get Consent Status

```swift
let config = TealiumConfig(...)
let status: TealiumConsentStatus = config.getConfigConsentStatus()
```
Getter for the consent status currently set on the config object.

<aside class="notice">May not match the consent status set in the consentManager if the user has changed their preferences since the library launched.</aside>


## Get Consent Categories

```swift
let config = TealiumConfig(...)
let categories: [TealiumConsentCategories] = config.getConfigConsentCategories()
```
Getter for the consent categories currently set on the config object.

<aside class="notice">May not match the consent categories set in the consentManager if the user has changed their preferences since the library launched.</aside>

## Override Policy
```swift
let config = TealiumConfig(...)
config.setOverrideConsentPolicy("mycustompolicy")
```
Overrides the default `policy` parameter. Default is `gdpr`.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| policy     | A string value describing the policy to set for consent logging purposes | "mycustompolicy"|

## Get Policy Override

```swift
let config = TealiumConfig(...)
let currentPolicy = config.getConfigConsentPolicy()
```
Returns the current consent policy override, if applicable. Optional.

# Delegate Methods (TealiumConsentManagerDelegate)

## willDropTrackingCall

```swift
func willDropTrackingCall(_ request: TealiumTrackRequest) {
        print("**** Tracking call DROPPED ******")
        print(request.data)
    }
```
This method is called whenever the consent manager is going to drop a tracking call (i.e. the user has declined tracking consent).

## willQueueTrackingCall

```swift
func willQueueTrackingCall(_ request: TealiumTrackRequest) {
        print("**** Tracking call Queued ******")
        print(request.data)
    }
```
This method is called whenever the consent manager is in the "not determined" state, and tracking calls are being queued locally until the user grants or declines consent.

## willSendTrackingCall

```swift
func willSendTrackingCall(_ request: TealiumTrackRequest) {
        print("**** Tracking call Sent - 1st Helper ******")
        print(request.data)
    }
```
This method is called whenever the user has consented to tracking, and a tracking call is about to pass through the consent manager without being blocked.

## consentStateChanged

```swift
func consentStateChanged(_ state: TealiumConsentStatus) {
    print(state)
}
```
This method is called whenver the consent state has been successfully updated.

## userConsentedToTracking

```swift
func userConsentedToTracking() {
        print("User Consented")
    }
```

This method is called whenever the consent state is changed to `.consented`.

## userOptedOutOfTracking

```swift
func userOptedOutOfTracking() {
        print("User Declined Tracking")
    }
```
This method is called whenever the user declines/revokes consent (consent status: `.notConsented`).

## userChangedConsentCategories

```swift
func userChangedConsentCategories(categories: [TealiumConsentCategories]) {
        print("Categories Changed")
    }
```
This method is called whenever the user updates their consent categories preferences.

# Implemented Code Examples

## Use Case 1: Simple Opt-in

```swift
    func setConsentStatusSimple(_ consented: Bool) {
        let status: TealiumConsentStatus = consented ? .consented: .notConsented 
        self.tealium?.consentManager()?.setConsentStatus(status)
    }
```

This example shows you how to give your users a simple "Opt-in/Opt-out" option. If the user consents, they will be automatically opted-in to all tracking categories. If they revoke their consent, no categories will be active, and no tracking calls will be sent.

Define and call the following method in your Tealium Helper class when your app user consents to or declines tracking. If the user consents to tracking, the Consent Manager will automatically opt them in to all tracking categories.

## Use Case 2: Grouped Opt-in

```swift
func updateConsentPreferences(_ dict: [String: Any]) {
    if let status = dict["consentStatus"] as? String {
        var tealiumConsentCategories = [TealiumConsentCategories]()
        let tealiumConsentStatus = (status == "consented") ? TealiumConsentStatus.consented : TealiumConsentStatus.notConsented
        if let categories = dict["consentCategories"] as? [String] {
            tealiumConsentCategories = TealiumConsentCategories.consentCategoriesStringArrayToEnum(categories)
        }
        self.tealium?.consentManager()?.setConsentStatusWithCategories(status: tealiumConsentStatus, categories: tealiumConsentCategories)
    }
}

// example function to define groups of categories, and set the consent preferences in the consentManager API. Should be called when the user saves their preferences

func setUserConsentPreferences(){
	// define a dictionary containing grouped categories
	let consentGroups = ["Off" : [],
	        "Performance": ["analytics", "monitoring", "big_data", "mobile", "crm"],
	        "Marketing": ["analytics", "monitoring", "big_data", "mobile", "crm", "affiliates", "email", "search", "engagement", "cdp"],
	        "Personalized Advertising": ["analytics", "monitoring", "big_data", "mobile", "crm", "affiliates", "email", "search", "engagement", "cdp", "display_ads", "personalization", "social", "cookiematch", "misc"]]
	        
	// this is the user's selected preferences level. Would usually be dynamic in a real app
	let userSelection = "Marketing"
	
	if let userList = consentGroups[userSelection] {
	  let settingsDict = ["consentStatus": "consented", "consentCategories": userList]
	  
	  updateConsentPreferences(settingsDict)
	}
}

```

This example shows a category-based consent model, where tracking categories are grouped into a smaller number of higher-level categories, defined by you. For example, you may choose to group the Tealium consent categories "big_data", "analytics", and "monitoring" under a single category called "performance". This may be easier for the user than selecting from the full list of categories. You may choose to represent this in a slider interface, ranging from least-permissive to most-permissive (all categories).

## Use Case 3: Category-based Opt-in

```swift
// helper method to call Tealium consentManager API
func updateConsentPreferences(_ dict: [String: Any]) {
    if let status = dict["consentStatus"] as? String {
        var tealiumConsentCategories = [TealiumConsentCategories]()
        let tealiumConsentStatus = (status == "consented") ? TealiumConsentStatus.consented : TealiumConsentStatus.notConsented
        if let categories = dict["consentCategories"] as? [String] {
            tealiumConsentCategories = TealiumConsentCategories.consentCategoriesStringArrayToEnum(categories)
        }
        self.tealium?.consentManager()?.setConsentStatusWithCategories(status: tealiumConsentStatus, categories: tealiumConsentCategories)
    }
}

// example function to take a list of categories and pass to the Tealium consentManager API

func setUserConsentPreferences(_ categories: [String]){

	let settingsDict = ["consentStatus": "consented", "consentCategories": categories]  
	updateConsentPreferences(settingsDict)
}

```

This example shows a category-based consent model where the user must explicitly select each category from the full list of categories. The default state is "Not Yet Determined", which will queue hits until the user provides their consent. If the user consents to any category, events are de-queued, and the consented category data is appended to each tracking call.