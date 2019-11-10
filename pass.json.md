[**back**](./instructions.md#instructions)

# Top Level Keys

## Standard Keys

Information that is required for all passes.

| Key name           |        Type        | Description                                                                                                                                  |
| ------------------ | :----------------: | :------------------------------------------------------------------------------------------------------------------------------------------- |
| description        | localizable string | Required. Brief description of the pass, used by the iOS accessibility technologies.                                                         |
|                    |                    | Don’t try to include all of the data on the pass in its description, just include enough detail to distinguish passes of the same type.      |
| formatVersion      |      integer       | Required. Version of the file format. The value must be 1.                                                                                   |
| organizationName   | localizable string | Required. Display name of the organization that originated and signed the pass.                                                              |
| passTypeIdentifier |       string       | Required. Pass type identifier, as issued by Apple. The value must correspond with your signing certificate.                                 |
| serialNumber       |       string       | Required. Serial number that uniquely identifies the pass. No two passes with the same pass type identifier may have the same serial number. |
| teamIdentifier     |       string       | Required. Team identifier of the organization that originated and signed the pass, as issued by Apple.                                       |

## Asociated App Keys

Information about an app that is associated with a pass.

| Key name                   |       Type       | Description                                                                                                                               |
| -------------------------- | :--------------: | :---------------------------------------------------------------------------------------------------------------------------------------- |
| appLaunchURL               |      string      | Optional. A URL to be passed to the associated app when launching it.                                                                     |
|                            |                  | The app receives this URL in the application:didFinishLaunchingWithOptions: and application:openURL:options: methods of its app delegate. |
|                            |                  | If this key is present, the associatedStoreIdentifiers key must also be present.                                                          |
| associatedStoreIdentifiers | array of numbers | Optional. A list of iTunes Store item identifiers for the associated apps.                                                                |
|                            |                  | Only one item in the list is used—the first item identifier for an app compatible with the current device.                                |
|                            |                  | If the app is not installed, the link opens the App Store and shows the app. If the app is already installed, the link launches the app.  |

## Companion App Keys

Custom information about a pass provided for a companion app to use.

| Key name |     Type      | Description                                                                                                                          |
| -------- | :-----------: | :----------------------------------------------------------------------------------------------------------------------------------- |
| userInfo | any JSON data | Optional. Custom information for companion apps. This data is not displayed to the user.                                             |
|          |               | For example, a pass for a cafe could include information about the user’s favorite drink and sandwich in a machine-readable form for |
|          |               | the companion app to read, making it easy to place an order for “the usual” from the app. Available in iOS 7.0.                      |

## Expiration Keys

Information about when a pass expires and whether it is still valid.
A pass is marked as expired if the current date is after the pass’s expiration date, or if the pass has been explicitly marked as voided.

| Key name       |         Type          | Description                                                                                          |
| -------------- | :-------------------: | :--------------------------------------------------------------------------------------------------- |
| expirationDate | W3C date, as a string | Optional. Date and time when the pass expires.                                                       |
|                |                       | The value must be a complete date with hours and minutes, and may optionally include seconds.        |
|                |                       | Available in iOS 7.0.                                                                                |
| voided         |        Boolean        | Optional. Indicates that the pass is void—for example, a one time use coupon that has been redeemed. |
|                |                       | Available in iOS 7.0.                                                                                |

## Relevance Keys

information about where and when a pass is relevant.

| Key name     |              Type               | Description                                                                                                          |
| ------------ | :-----------------------------: | :------------------------------------------------------------------------------------------------------------------- |
| beacons      |  array of beacon dictionaries   | Optional. Beacons marking locations where the pass is relevant.                                                      |
|              |                                 | For these dictionaries’ keys, see [Beacon Dictionary Keys.](#beacon-dictionary-keys)                                 |
|              |                                 | Available in iOS 7.0.                                                                                                |
| locations    | array of locations dictionaries | Optional. Locations where the pass is relevant. For example, the location of your store.                             |
|              |                                 | For these dictionaries’ keys, see [Location Dictionary Keys.](#location-dictionary-keys)                             |
| maxDistance  |             number              | Optional. Maximum distance in meters from a relevant latitude and longitude that the pass is relevant.               |
|              |                                 | This number is compared to the pass’s default distance and the smaller value is used.                                |
|              |                                 | Available in iOS 7.0.                                                                                                |
| relevantDate |      W3C date, as a string      | Recommended for event tickets and boarding passes; otherwise optional. Date and time when the pass becomes relevant. |
|              |                                 | For example, the start time of a movie.                                                                              |
|              |                                 | The value must be a complete date with hours and minutes, and may optionally include seconds.                        |

## Style Keys

Keys that specify the pass style
Provide exactly one key—the key that corresponds with the pass’s type. The value of this key is a dictionary containing the keys in [Pass Structure Dictionary Keys.](#pass-structure-dictionary-keys)

| Key name     |           Type            | Description                               |
| ------------ | :-----------------------: | :---------------------------------------- |
| boardingPass | pass-structure dictionary | OInformation specific to a boarding pass. |
| coupon       | pass-structure dictionary | OInformation specific to a coupon.        |
| eventTicket  | pass-structure dictionary | OInformation specific to a event ticket.  |
| generic      | pass-structure dictionary | OInformation specific to a generic pass.  |
| storeCard    | pass-structure dictionary | OInformation specific to a store card.    |

## Visual Appearance Keys

Keys that define the visual style and appearance of the pass.
With the release of iOS 9, there are two ways to display a barcode:

-   The barcodes key (new and required for iOS 9 and later)
-   The barcode key (for iOS 8 and earlier)

To support older versions of iOS, use both keys. The system automatically selects the barcodes array for iOS 9 and later and uses the barcode dictionary for iOS 8 and earlier.

| Key name           |             Type              | Description                                                                                                                                |
| ------------------ | :---------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------- |
| barcode            |      barcode dictionary       | Optional. Information specific to the pass’s barcode. For this dictionary’s keys, see [Barcode Dictionary Keys.](#beacon-dictionary-keys)  |
|                    |                               | Note:Deprecated in iOS 9.0 and later; use barcodes instead.                                                                                |
| barcodes           | array of barcode dictionaries | Optional. Information specific to the pass’s barcode. The system uses the first valid barcode dictionary in the array.                     |
|                    |                               | Additional dictionaries can be added as fallbacks. For this dictionary’s keys, see [Barcode Dictionary Keys.](#beacon-dictionary-keys)     |
|                    |                               | Note:Deprecated in iOS 9.0 and later; use barcodes instead.                                                                                |
| backgroundColor    |      color, as a string       | Optional. Background color of the pass, specified as an CSS-style RGB triple. For example, rgb(23, 187, 82).                               |
| foregroundColor    |      color, as a string       | Optional. Foreground color of the pass, specified as a CSS-style RGB triple. For example, rgb(100, 10, 110).                               |
| groupingIdentifier |            string             | Optional for event tickets and boarding passes; otherwise not allowed. Identifier used to group related passes.                            |
|                    |                               | If a grouping identifier is specified, passes with the same style, pass type identifier, and grouping identifier are displayed as a group. |
|                    |                               | Otherwise, passes are grouped automatically.                                                                                               |
|                    |                               | Use this to group passes that are tightly related, such as the boarding passes for different connections of the same trip.                 |
|                    |                               | Available in iOS 7.0.                                                                                                                      |
| labelColor         |      color, as a string       | Optional. Color of the label text, specified as a CSS-style RGB triple. For example, rgb(255, 255, 255).                                   |
|                    |                               | If omitted, the label color is determined automatically.                                                                                   |
| logoText           |      localizable string       | Optional. Text displayed next to the logo on the pass.                                                                                     |
| suppressStripShine |            Boolean            | Optional. If true, the strip image is displayed without a shine effect. The default value prior to iOS 7.0 is false.                       |
|                    |                               | In iOS 7.0, a shine effect is never applied, and this key is deprecated.                                                                   |

## Web Service Keys

Information used to update passes using the web service.
If a web service URL is provided, an authentication token is required; otherwise, these keys are not allowed.

| Key name            |  Type  | Description                                                                                                                                                                                                                                                      |
| ------------------- | :----: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| authenticationToken | string | The authentication token to use with the web service. The token must be 16 characters or longer.                                                                                                                                                                 |
| webServiceURL       | string | The URL of a web service that conforms to the API described in [PassKit Web Service Reference.](https://developer.apple.com/library/archive/documentation/UserExperience/Reference/PassKit_Bundle/Chapters/TopLevel.html#//apple_ref/doc/uid/TP40012026-CH2-SW1) |
|                     |        | The web service must use the HTTPS protocol; the leading https:// is included in the value of this key.                                                                                                                                                          |
|                     |        | On devices configured for development, there is UI in Settings to allow HTTP web services.                                                                                                                                                                       |

## NFC-Enabled Pass Keys

NFC-enabled pass keys support sending reward card information as part of an Apple Pay transaction.

---

**NOTE**

Important: NFC-enabled pass keys are only supported in passes that contain an Enhanced Passbook/NFC certificate. For more information, contact merchant support at https://developer.apple.com/contact/passkit/.

---

Passes can send reward card information to a terminal as part of an Apple Pay transaction. This feature requires a payment terminal that supports NFC-entitled passes. Specifically, the terminal must implement the Value Added Services Protocol.

Passes provide the required information using the nfc key. The value of this key is a dictionary containing the keys described in [NFC Dictionary Keys.](#nfc-dictionary-keys) This functionality allows passes to act as the user’s credentials in the context of the NFC Value Added Service Protocol. It is available only for storeCard style passes.

| Key name |      Type      | Description                                                                                                                                            |
| -------- | :------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| nfc      | nfc dictionary | Optional. Information used for Value Added Service Protocol transactions. For this dictionary’s keys, see [NFC Dictionary Keys.](#nfc-dictionary-keys) |
|          |                | Available in iOS 9.0.                                                                                                                                  |
