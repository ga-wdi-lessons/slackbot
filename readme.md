# Let’s Build a Slackbot

## Learning Objectives

- Initialize a node project with `npm init`
- Install dependencies with `npm install --save`
- Explain what WebSockets are and why you would use them
- Make requests to the Slack API using `request` module

## Framing

### What’s a Slackbot?

>Bot users have many of the same qualities as their human counterparts: they 
have profile photos, names, and bios, they exist in the team directory, they 
can be direct messaged or mentioned, they can post messages and upload files, 
and they can be invited to and kicked out of channels and private groups.

>The biggest difference between bot users and regular users is that instead 
of interacting with a team via one of Slack's mobile or desktop apps, bot 
users are controlled programmatically via a bot user token that accesses one 
or more of Slack's APIs.

[source](https://api.slack.com/bot-users)

### What can a Slackbot do?

- _Anything_
- Deploy code
- http://www.slackbotlist.com/

## How SlackBots work

### 1. Obtain an API Token

- Create a new bot here https://ga-students.slack.com/apps/new/A0F7YS25R-bots
- Click "Add bot integration"
- Copy API Token
  - We’ll need this later

### 2. Retrieve WebSocket URL

#### What’s a websocket?

WebSockets provide a persistent two-way connection between the client and
server. Instead of polling for data, or requesting it explicitly ($.ajax),
the server can send events over the websocket connection.

This is ideal for realtime applications like:
- Chat
- Collaborative editing (Google Docs)
- Multiplayer online games


```
$ mkdir my-slackbot
$ cd my-slackbot
$ npm init
$ npm install --save request
$ touch app.js
```

```js
// app.js

var request = require("request");
var url = "https://slack.com/api/rtm.start?token=" + process.env.token;
request( url, function( err, response, body ){
    var websocketUrl = JSON.parse( body ).url;
    console.log(websocketUrl)
});
```

```
$ token=your-token-here node app.js
```

### 3. Connect to the WebSocket URL

```
$ npm install --save ws
```

```js
// app.js

var request = require("request");
var WebSocket = require("ws");

var url = "https://slack.com/api/rtm.start?token=" + process.env.token;
request( url, function( err, response, body ){
    var websocketUrl = JSON.parse( body ).url;
    var ws = new WebSocket(websocketUrl);
});
```

### 4. Listen for and respond to events

```js
// app.js

var request = require("request");
var WebSocket = require("ws");

var url = "https://slack.com/api/rtm.start?token=" + process.env.token;
request( url, function( err, response, body ){
    var websocketUrl = JSON.parse( body ).url;
    var ws = new WebSocket(websocketUrl);
    ws.on("message", function(message){
      console.log(message);
    })
});
```

## You do: Interact with your slackbot

- Send it a DM
- Mention it in a channel it’s in
- Type anything

>How do you know when the `message` is a direct message?

## Simplifying things

I built a mini-framework for building slackbots: https://github.com/ga-dc/slipshod

Create a new application, and build something awesome with your new slackbot!

## Demo Time ?!
