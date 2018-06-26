# TealiumConfig Methods

## setConsentLoggingEnabled

```swift
let config = TealiumConfig(...)
config.setConsentLoggingEnabled(true)
```

Enables the [Consent Logging](https://community.tealiumiq.com/t5/Universal-Data-Hub/Consent-Change-Event-Specifications/ta-p/23213) feature, which sends all consent status changes to UDH for auditing purposes.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| enabled     | Bool | true (default: false)|

## isConsentLoggingEnabled

```swift
let config = TealiumConfig(...)
let isEnabled = config.isConsentLoggingEnabled()
```
Returns the current state of the consent logging feature.

## setInitialUserConsentStatus

```swift
let config = TealiumConfig(...)
config.setInitialUserConsentStatus(.notConsented)
```
Sets the initial consent status for the user when the library starts up for the first time. If there are saved preferences, these will override any preferences passed in the config object.

<aside class="notice">Will always set the list of consented categories to include ALL available consent categories, if the status is <code>.consented</code>. Does not allow categories to be set selectively.</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| status     | A value from TealiumConsentStatus enum | .consented/.notConsented |

## setInitialUserConsentCategories

```swift
let config = TealiumConfig(...)
config.setInitialUserConsentCategories([.analytics])
```

Sets the user's initial consent categories when the library starts up for the first time. If there are saved preferences, these will override any preferences passed in the config object.

<aside class="notice">Will always set consent status to <code>.consented</code>.</aside>

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| categories     | An array of values from the TealiumConsentCategories enum | [.analytics,.bigData]|

## getInitialUserConsentStatus

```swift
let config = TealiumConfig(...)
let status: TealiumConsentStatus? = config.getInitialConsentStatus()
```
Getter for the consent status currently set on the config object.

<aside class="notice">May not match the consent status set in the consentManager if the user has changed their preferences since the library launched.</aside>


## getInitialUserConsentCategories

```swift
let config = TealiumConfig(...)
let categories: [TealiumConsentCategories]? = config.getInitialConsentCategories()
```
Getter for the consent categories currently set on the config object.

<aside class="notice">May not match the consent categories set in the consentManager if the user has changed their preferences since the library launched.</aside>

## setOverrideConsentPolicy
```swift
let config = TealiumConfig(...)
config.setOverrideConsentPolicy("mycustompolicy")
```
Overrides the default `policy` parameter. Default is `gdpr`.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| policy     | A string value describing the policy to set for consent logging purposes | "mycustompolicy"|

## getOverrideConsentPolicy

```swift
let config = TealiumConfig(...)
let currentPolicy = config.getOverrideConsentPolicy()
```
Returns the current consent policy override, if applicable. Optional.