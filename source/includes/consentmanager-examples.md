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

![Simple](images/simple-gif.gif)

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

![Group-Based](images/grouped-gif.gif)

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

![Category-Based](images/individual-gif.gif)