# cordova-plugin-jitsi-meet-sdk
Original plugin (https://github.com/Zeno97/cordova-plugin-jitsi-meet-sdk) has backward compatibility issues because it uses an outdated version of jitsi-sdk. (3.+ is too old). 
Cordova plugin for Jitsi Meet React Native SDK: https://jitsi.github.io/handbook/docs/dev-guide/dev-guide-mobile/ Mobile SDK 4.0.0 is out and contains some backwards incompatible changes, check the release notes (24 Feb 2023). 
Updated dependencies in the file `build-extras.gradle` so that the latest major version of SDK will be pulled. 

## Warning
For the plugin to work properly with the latest version, you need to update the `cordova-android` version to `11`, since sdk requires target version 32 and higher. After update you should append the next lines in your config.xml files:

```
        <resource-file src="res/android/ic_notification.png" target="app/src/main/res/drawable-ldpi/ic_notification.png" />
        <resource-file src="res/android/ic_notification.png" target="app/src/main/res/drawable-mdpi/ic_notification.png" />
        <resource-file src="res/android/ic_notification.png" target="app/src/main/res/drawable-hdpi/ic_notification.png" />
        <resource-file src="res/android/ic_notification.png" target="app/src/main/res/drawable-xhdpi/ic_notification.png" />

```
These are notifications icons for library. Without these lines the plugin will not start correctly. 
All options, feature flags and listeners are available. 

All feature flags available can be found here: https://github.com/jitsi/jitsi-meet/blob/master/react/features/base/flags/constants.js

## Supported Platforms
- __Android__ >= 23 
- __iOS__

## Installation
The plugin can be installed via Cordova-CLI

Install the latest head version
```
cordova plugin add https://github.com/vlad909/cordova-plugin-jitsi-meet-sdk.git
```
## Changes
`welcomePageEnabled: false` has removed. Now it's a feature flag "welcomepage.enabled" (look at example)

## Usage
All paramenters are optional except for room. You need to specify at least the room name.

- If serverURL is not specified by default is "https://meet.jit.si".

- All feature flags not specified are in their default value.

- All boolean options by default are false.

- All string options by default are empty.

This is the minimal setup to enter into a conference
```js
JitsiMeet.startConference(
{
    room: "MyAmazingRoom",
});
```


And this is a complete example
```js
JitsiMeet.startConference(
{
    serverURL: "https://meet.jit.si",
    room: "MyAmazingRoom",
    displayName: "Max!",
    email: "max@amazingmax.it", // change it
    audioMuted: false,
    videoMuted: false,
    welcomePageEnabled: false,
    subject: "My amazing name",
    audioOnly: false,
    //token: "your jwt token, if you have one",
    flags: {
        "chat.enabled": true,
        "invite.enabled": true,
        "kick-out.enabled": true,
        "live-streaming.enabled": true,
        "pip.enabled": true,
        "raise-hand.enabled":true,
        "recording.enabled": true,
        "video-share.enabled": true,
        "add-people.enabled": true,
        "calendar.enabled": true,
        "meeting-name.enabled": true,
        "meeting-password.enabled": true,
        "toolbox.alwaysVisible": true,
	"welcomepage.enabled": true,
 	"prejoinpage.enabled": true
    }
}, function(listener){
    // a listener has been fired!
    console.log(listener);
    
    switch(listener){
        case "CONFERENCE_JOINED":
            //Broadcasted when a conference was joined.
            break;
        case "CONFERENCE_TERMINATED":
            //Broadcasted when the active conference ends, be it because of user choice or because of a failure.
            break;
        case "CONFERENCE_WILL_JOIN":
            //Broadcasted before a conference is joined. 
            break;
        case "AUDIO_MUTED_CHANGED":
            //Broadcasted when audioMuted state changed
            break;
        case "VIDEO_MUTED_CHANGED":
            //Broadcasted when videoMuted state changed
            break;
        case "PARTICIPANT_JOINED":
            //Broadcasted when a participant has joined the conference. 
            break;
        case "PARTICIPANT_LEFT":
            //Broadcasted when a participant has joined the conference.
            break;
        case "ENDPOINT_TEXT_MESSAGE_RECEIVED":
            //Broadcasted when an endpoint text message is received. 
            break;
	case "PARTICIPANTS_INFO_RETRIEVED":
	    //Broadcasted when a RETRIEVE_PARTICIPANTS_INFO action is called. 
            break;
	case "CHAT_MESSAGE_RECEIVED":
	    //Broadcasted when a chat text message is received.
            break;
	case "CHAT_TOGGLED":
	    //Broadcasted when the chat dialog is opened or closed.
            break;
    }
});
```

## Close the conference
```js
JitsiMeet.disposeConference(function(success){
	console.log("You successfully closed your conference!");
},function(error){
	console.log("Something goes wrong. Check tab error in the console");
	console.error(error);
});
```

## Supported events

### CONFERENCE_JOINED
Broadcasted when a conference was joined. The data HashMap contains a url key with the conference URL.

### CONFERENCE_TERMINATED
Broadcasted when the active conference ends, be it because of user choice or because of a failure. The data HashMap contains an error key with the error and a url key with the conference URL. If the conference finished gracefully no error key will be present.

### CONFERENCE_WILL_JOIN
Broadcasted before a conference is joined. The data HashMap contains a url key with the conference URL.

### AUDIO_MUTED_CHANGED
Broadcasted when audioMuted state changed. The data HashMap contains a muted key with state of the audioMuted for the localParticipant.

### VIDEO_MUTED_CHANGED
Broadcasted when videoMuted state changed. The data HashMap contains a muted key with state of the videoMuted for the localParticipant.

### PARTICIPANT_JOINED
Broadcasted when a participant has joined the conference. The data HashMap contains information of the participant that has joined. Depending of whether the participant is the local one or not, some of them are present/missing. isLocal email name participantId

### PARTICIPANT_LEFT
Broadcasted when a participant has joined the conference. The data HashMap contains information of the participant that has left. Depending of whether the participant is the local one or not, some of them are present/missing. isLocal email name participantId

### ENDPOINT_TEXT_MESSAGE_RECEIVED
Broadcasted when an endpoint text message is received. The data HashMap contains a senderId key with the participantId of the sender and a message key with the content.

### PARTICIPANTS_INFO_RETRIEVED
Broadcasted when a RETRIEVE_PARTICIPANTS_INFO action is called. The data HashMap contains a participantsInfo key with a list of participants information and a requestId key with the id that was sent in the RETRIEVE_PARTICIPANTS_INFO action.

### CHAT_MESSAGE_RECEIVED
Broadcasted when a chat text message is received. The data HashMap contains a senderId key with the participantId of the sender, a message key with the content, a isPrivate key with a boolean value and a timestamp key.

### CHAT_TOGGLED
Broadcasted when the chat dialog is opened or closed. The data HashMap contains a isOpen key with a boolean value.

## Broadcasting Actions
Working in progress...

## Issues
The plugin will receive updates and fixes. Write in the Issues section for any problem.

## I am looking Freelance work!
You can email me at maxcoppola97@libero.it

### Thanks!
