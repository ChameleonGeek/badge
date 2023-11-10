# badge
This is a workspace to brainstorm the creation of a "geek badge" for events.  This is simply a list of hardware/functions all participants in the design of the badge can see and consider.  Once a more complete/final determination of components and functions is made, this repo will host the code for development and testing.

There is very little practical reason for the capabilities of the badge.  While it may be useful, the primary purpose is for fun, and to give event attendees the ability to hack a device meant to be toyed with.

Event organizers will provide support for the badges, helping attendees perform first boot configuration and collect the necessary information to communicate with each badge.

### Functional Components
The badge will be centered around an ESP32 and an e-ink/e-paper display.  Other hardware will be included to provide other capabilities which may or may not be used in the primary programmed configuration, but will be available for users to add functions for them via changing the main operating code on the badge.

This is just the initial list of main components.  Others will most likely be added.

ESP32 - Main processor and WiFi and Bluetooth controller

e-Ink Dispay - The most visible component, where the wearer's first name and organization can be displayed.

IRDA Transeiver - may or may not be integrated, may be substituted with IR LED and IR phototransistor or IR capabilities may be completely omitted.

Pushbuttons - At least two buttons will be needed for the menu system.

LEDs - All badges need the ability to produce some light.  This can be single color, bicolor LEDs or WS2812s.

Consider the addition of a shitty add-on connector.

### Functional Capabilities
Provide WiFi and/or Bluetooth connectivity for initial user configuration, but to also communicate with other badges and beacons at events.

Potentially provide IRDA communications, to constrain badge-to-badge communications to short range interactions.  Possibly adding capability to interact with IR-controlled devices.

e-Ink display will display the first name and organization the wearer is with.  It will also display icons to represent the specialties the wearer claims and report information related to event tasks.  Different "screens" of content displayed on the display are called "cards" in this document.

The user will be able to navigate a small menu system which includes the ability to restore it to first boot configuration (delete configuration file and any other files saved to ESP).

To extend battery life, the menu system should be able to turn WiFi and Bluetooth functionality on and off, as well as turn off/set max intensity of LEDs.

The team should decide whether the ESP should be configured to allow OTA reprogramming.  There are reasons to allow OTA, and reasons not to.

### Operation
#### First Boot/Out of the Box
The badge will boot and see if a valid configuration file is present on the ESP.  If not, it will perform first boot operations.  User will have the ability to restore the badge to first boot state via the menu system and WiFi configuration page.

The badge will host an access point, whose SSID is based on the ESP MAC Address.  The ESP will calculate a QR code with the proper network credentials and will display it on the e-Ink display.  The user can use this QR to connect to the badge for initial configuration.

The badge will provide a captive portal, hosting a web form the user can use to provide their name and organization, as well as a new SSID and WiFi passcode if desired.  These settings will be saved to the configuration file on the ESP.

Once the user has configured the badge, it will attempt to connect to the event's infrastructure to "register itself" for the event.

#### Normal operation
On boot, the badge will check if it should run first boot functions.  If not, it can display a QR code for the WiFi Access Point, which can be used to further configure the badge.

The badge will display the wearer's name and organization.  If the user has selected specialties to display, those will be displayed as icons as well.

The badge should have a couple of buttons to navigate a small menu for easy configuration.

The badge should probably have some LEDs (discrete or WS2812) just because.

If IR communication is included, the badge can periodically transmit random codes to turn down the volume of different devices.

#### Tasks/Games
##### Passing business cards
Using IR or Bluetooth, the badge will attempt to connect with other badges and try to exchange names, organizations and unique IDs.  Upon successful exchange, the badge will store a list of "contacts."  This should be restricted to very short-range sharing, meaning that the wearers should be close enough to actually interact with each other.  This short range can be via the use of IR communications or by bluetooth connection, only making connections to other badges with a very high RSSI.

Depending on selected settings, the badge can scroll through the list of names, or can simply display a count of unique contacts.

Other tasks will need to connect to Bluetooth devices with low RSSI.  Passing the business card will probably need to attempt connections with low RSSI devices and choose (based on RSSI) whether to log the contact after the exchange is finished.

##### Adding graphics
It needs to be determined if the ESP32 can perform small file transfers over Bluetooth.  If so, users will be able to create 1-bit images and transfer them to the badge, adding them to the cycle of "cards" the badge will display.  A full-screen 1-bit bitmap for a 128x296 (2.9 inch) display is only 592 bytes.

Graphics may be added via the configuration web form.

##### Where's Waldo
A Bluetooth device can be set up at the event, and will negotiate with other badges just like passing business cards.  When the badge has passed business cards with Waldo, the ESP will wait until the RSSI of Waldo reaches a low level.  At that point, somewhere between five and fifteen minutes later, the badge will display that it found Waldo.  A reward of some kind can be offered for finding Waldo.

##### Find the murderer
This is similar to the ice breaker sometimes used at other events.  This will use the same protocols as passing business cards and Where's Waldo.  If the badge has exchanged cards with the murderer, just like Where's Waldo, the badge will wait until the RSSI has reduced to a low enough level and a random period of time has elapsed, the badge will declare "I'm dead."  The goal is for "living" attendees to guess who the murderous badge belongs to.

##### Paging
Using long-range Bluetooth connections, event hosts may be able to "page" specific attendees by placing a card in the cycle to be displayed by that attendee's badge, turning on an LED, or activating a buzzer (if part of the badge).  This would require the event hosts to have a full list of attendees and associated badge identifiers.  The user may be able to remove these cards via the onboard menu system, and/or the event organizers will be able/required to.

##### Flags
Using long-range Bluetooth or WiFi communications, the badge can look for specific SSIDs.  If the SSID is detected, the badge will respond as pre-programmed.  This could be a "dinner bell," notification that specific classes are about to start, etc.  Each SSID being sought can make the badge respond for a pre-set period, or one SSID can turn something on, and another will turn it off.
