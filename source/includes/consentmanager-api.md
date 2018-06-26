# API Reference

## consentManager

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

## addConsentDelegate

```swift
tealium?.consentManager()?.addConsentDelegate(self)
```

Registers a specific class conforming to the TealiumConsentDelegate protocol

| Parameters | Description                                  |
|------------|----------------------------------------------|
| delegate   | A class conforming to TealiumConsentDelegate |

## removeAllConsentDelegates

```swift
tealium?.consentManager()?.removeAllConsentDelegates()
```

Removes all consent delegates added by you. Retains the built-in delegate created by the TealiumConsentManagerModule class.

## setUserConsentStatus

```swift
tealium?.consentManager()?.setUserConsentStatus(.consented)
```

Sets the user's consent status. Designed to be called from your consent management preferences screen in your app when the user enables or disables tracking.

<aside class="notice">
Will always set the list of consented categories to include ALL available consent categories, if the status is <code>.consented</code>. Does not allow categories to be set selectively.
</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | `.consented`/`.notConsented` |

## setUserConsentCategories

```swift
tealium?.consentManager()?.setUserConsentCategories([.analytics,.bigData])
```

Sets the user's consent categories. Designed to be called from your consent management preferences screen in your app. 

<aside class="notice">Will always set consent status to <code>.consented</code>.</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| categories     | An array of values from the TealiumConsentCategories enum | `[.analytics,.bigData]`|

## setUserConsentStatusWithCategories

```swift
tealium?.consentManager()?.setUserConsentStatusWithCategories(status: .consented, categories: [.analytics])
```

Sets consent status and categories in a single call.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | `.consented`/`.notConsented`|
| categories     | An array of values from the TealiumConsentCategories enum | `[.analytics]`|

## getUserConsentStatus

```swift
let consentStatus: TealiumConsentStatus? = tealium?.consentManager()?.getUserConsentStatus()
```
Returns the current consent status

## getUserConsentCategories

```swift
let consentCategories: [TealiumConsentCategories]? = self.tealium?.consentManager()?.getUserConsentCategories()
```
Returns the current consent categories

## getUserConsentPreferences

```swift
let prefs: TealiumConsentUserPreferences? = tealium?.consentManager()?.getUserConsentPreferences()
```
Returns the TealiumConsentUserPreferences object (by value -> struct)

## resetUserConsentPreferences

```swift
tealium?.consentManager()?.resetUserConsentPreferences()
```
Clears all currently stored consent preferences. Reverts to "not determined" consent state, with no categories.

