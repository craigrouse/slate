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

## setConsentStatus

```swift
tealium?.consentManager()?.setConsentStatus(.consented)
```

Sets the user's current consent status. Designed to be called from your consent management preferences screen in your app when the user enables or disables tracking.

<aside class="notice">
Will always set the list of consented categories to include ALL available consent categories, if the status is <code>.consented</code>. Does not allow categories to be set selectively.
</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | `.consented`/`.notConsented` |

## setConsentCategories

```swift
tealium?.consentManager()?.setConsentCategories([.analytics,.bigData])
```

Sets the user's current consent categories. Designed to be called from your consent management preferences screen in your app. 

<aside class="notice">Will always set consent status to <code>.consented</code>.</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| categories     | An array of values from the TealiumConsentCategories enum | `[.analytics,.bigData]`|

## setConsentStatusWithCategories

```swift
tealium?.consentManager()?.setConsentStatusWithCategories(status: .consented, categories: [.analytics])
```

Sets consent status and categories in a single call.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | `.consented`/`.notConsented`|
| categories     | An array of values from the TealiumConsentCategories enum | `[.analytics]`|

## currentConsentStatus

```swift
let consentStatus: TealiumConsentStatus? = tealium?.consentManager()?.currentConsentStatus()
```
Returns the current consent status

## currentConsentCategories

```swift
let consentCategories: [TealiumConsentCategories]? = self.tealium?.consentManager()?.currentConsentCategories()
```
Returns the current consent categories

## currentConsentPreferences

```swift
tealium?.consentManager()?.currentConsentPreferences -> TealiumConsentUserPreferences
```
Returns the TealiumConsentUserPreferences object (by value -> struct)

## clearStoredConsentPreferences

```swift
tealium?.consentManager()?.clearUserConsentPreferences()
```
Clears all currently stored consent preferences. Reverts to "not determined" consent state, with no categories.

