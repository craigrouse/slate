# Enums and Constants

## TealiumConsentCategories

### Enum Values

```swift
.analytics
.affiliates
.displayAds
.email
.personalization
.search
.social
.bigData
.mobile
.engagement
.monitoring
.cdm
.cdp
.cookiematch
.misc
```

This enum contains the complete list of supported consent categories.
<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

### all

```swift
let allCategories: [TealiumConsentCategories] = TealiumConsentCategories.all()
```
Returns the complete list of supported consent categories.

### consentCategoriesStringArrayToEnum

```swift
let selectCategories: [TealiumConsentCategories] = TealiumConsentCategories.consentCategoriesStringArrayToEnum(["analytics","bigData"])
```
Converts an array of strings containing valid consent categories to an array of enum values. Invalid string values will be omitted from the resulting array.

| Parameters | Description                            | Example Value            |
|------------|----------------------------------------|--------------------------|
| [String]     | An array of strings to convert into an array of TealiumConsentCategories | ["analytics", "bigData"]|

## TealiumConsentStatus

### notDetermined

```swift
let status: TealiumConsentStatus = .notDetermined
```

The `notDetermined` status is the default setting for the consent manager. In this state, the consent manager will queue events locally until an affirmative `consented`/`notConsented` status is provided.

### consented
```swift
let status: TealiumConsentStatus = .consented
```

The `consented` status is set when the user has consented to tracking. In this state, the consent manager allow all tracking calls to continue as normal.

### notConsented
```swift
let status: TealiumConsentStatus = .notConsented
```

The `notConsented` status is set when the user has declined tracking. In this state, the consent manager will drop all tracking calls and halt further processing by the SDK.

## TealiumConsentConstants (Internal)

```swift
public enum TealiumConsentConstants {
    static let consentCategoriesKey = "consent_categories"
    static let trackingConsentedKey = "tracking_consented"
    static let consentGrantedEventName = "grant_full_consent"
    static let consentDeclinedEventName = "decline_consent"
    static let consentPartialEventName = "grant_partial_consent"
    static let privacySettingEvent = "update_consent_cookie"
    static let moduleName = "consentmanager"
    static let consentLoggingEnabled = "consent_manager_enabled"
    static let consentStatus = "consent_status"
    static let policyKey = "policy"
    static let defaultPolicy = "gdpr"
}
```

This enum is used internally by the SDK to provide a central place to configure static key names.

## TealiumConsentTrackAction (Internal)

### trackingAllowed
```swift
let action: TealiumConsentTrackAction = .trackingAllowed
```

Used internally by the consent manager to determine that tracking is allowed, and tracking calls should pass through the module for further processing.

### trackingForbidden
```swift
let action: TealiumConsentTrackAction = .trackingForbidden
```
Used internally by the consent manager to determine that tracking is forbidden, and tracking calls should be blocked.

### queue
```swift
let action: TealiumConsentTrackAction = .queue
```
Used internally by the consent manager to indicate that the consent state is currently not determined, and tracking calls should be queued locally until the consent state is known.