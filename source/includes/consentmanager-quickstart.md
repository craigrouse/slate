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