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