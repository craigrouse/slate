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
