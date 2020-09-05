Firebase Cloud Messaging Quickstart
===================================

The Firebase Cloud Messaging quickstart demonstrates how to:
- Request permission to send app notifications to the user.
- Receive FCM messages using the Firebase Cloud Messaging JavaScript SDK.

Introduction
------------

[Read more about Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)

Getting Started
---------------

1. Create your project in the Firebase Console by following [**Step 1: Create a Firebase Project**](https://firebase.google.com/docs/web/setup/#create-firebase-project)
1. Register a web app by following [**Step 2: Register your app with Firebase**](https://firebase.google.com/docs/web/setup/#create-firebase-project).
     1. You don't need to add Hosting right now, and you can skip the "Add Firebase SDK" step in the console's "Add Firebase to your web app" flow.
     1. Remember to click "Register App" or "Continue to console" at the bottom of the "Add Firebase to your web app" flow.
1. Open Project and go to **Project settings > Cloud Messaging** and there in section **Web configuration** click **Generate key pair** button.
1. Copy public key and in `index.html` file replace `<YOUR_PUBLIC_VAPID_KEY_HERE>` with your key.
1. You must have the [Firebase CLI](https://firebase.google.com/docs/cli/) installed. If you don't have it install it with `npm install -g firebase-tools` and then configure it with `firebase login`.
1. On the command line run `firebase use --add` and select the Firebase project you have created.
1. On the command line run `firebase serve -p 8081` using the Firebase CLI tool to launch a local server.
1. Open [http://localhost:8081](http://localhost:8081) in your browser.
1. Click **REQUEST PERMISSION** button to request permission for the app to send notifications to the browser.
1. Use the generated Instance ID token (IID Token) to send an HTTP request to FCM that delivers the message to the web application, inserting appropriate values for [`YOUR-SERVER-KEY`](https://console.firebase.google.com/project/_/settings/cloudmessaging) and `YOUR-IID-TOKEN`.

NOTE: If your payload has a `notification` object, `setBackgroundMessageHandler` will not trigger. Read [here](https://firebase.google.com/docs/cloud-messaging/js/receive) for more information.

### HTTP
```
POST /fcm/send HTTP/1.1
Host: fcm.googleapis.com
Authorization: key=YOUR-SERVER-KEY
Content-Type: application/json

{
  "notification": {
    "title": "Portugal vs. Denmark",
    "body": "5 to 1",
    "icon": "firebase-logo.png",
    "click_action": "http://localhost:8081"
  },
  "to": "YOUR-IID-TOKEN"
}
```

### Fetch
```js
var key = 'YOUR-SERVER-KEY';
var to = 'YOUR-IID-TOKEN';
var notification = {
  'title': 'Portugal vs. Denmark',
  'body': '5 to 1',
  'icon': 'firebase-logo.png',
  'click_action': 'http://localhost:8081'
};

fetch('https://fcm.googleapis.com/fcm/send', {
  'method': 'POST',
  'headers': {
    'Authorization': 'key=' + key,
    'Content-Type': 'application/json'
  },
  'body': JSON.stringify({
    'notification': notification,
    'to': to
  })
}).then(function(response) {
  console.log(response);
}).catch(function(error) {
  console.error(error);
})
```

### cURL
```
curl -X POST -H "Authorization: key=AAAArY6Tlhk:APA91bHpo8MWju3Gl0bhh9KpEjdVu_tLrPOhbgdF3WS5EKifU9Tf4sOOpbYARzrGOS-nxsXnzqrIKKl7Sc3OAFVOrhiwcd2BCMAcTta47dJP5HOVhlOk5Nyn-S_TTYSduAkyOWzMKdTm" -H "Content-Type: application/json" -d '{
  "data": {
    "message":{
    "title": "Хо-Хо",
    "body": "Хватит терпеть",
    "icon": "firebase-logo.png",
  }},
  "to": "fb96xi9GOB5oSScUK54GgK:APA91bFedhQfTwDxu1hNrlz-l1UrwP9x6zOx4ettHINMPA_8kJWvVQo2JpSzBRiiKzH1kiSgXaTDTnE1bRZaoqqIQ7umo2gzw5C6XDBTp7iQmDzZJM2Xmblwqct20lH23oakvPdIVXlH"
}' "https://fcm.googleapis.com/fcm/send"
```

### App focus
When the app has the browser focus, the received message is handled through
the `onMessage` callback in `index.html`. When the app does not have browser
focus then the `setBackgroundMessageHandler` callback in `firebase-messaging-sw.js`
is where the received message is handled.

The browser gives your app focus when both:

1. Your app is running in the currently selected browser tab.
2. The browser tab's window currently has focus, as defined by the operating system.

Support
-------

https://firebase.google.com/support/

License
-------

© Google, 2016. Licensed under an [Apache-2](../LICENSE) license.
