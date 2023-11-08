# p2p Screensharing
Chat and screenshare peer-to-peer, without a server, no account needed.

## Introduction
Backed by no server, p2p Screensharing runs entirely in the browser to chat and screenshare with a peer. It supports both mobile and desktop (screen capture may not be available on mobile).

## Public Version
A public version of the app is available [here](https://screenshare.wdh.gg).

Once you have opened it on your browser, you can create a room, choose a username and share the room's link with the person you'd like to chat/screenshare with.

Screensharing is optional, you may start streaming at any time, stop streaming, change the window/screen that you're sharing. Audio capture may not be available, depending on your browser.

## Self Hosting
To self host p2p Screensharing, follow these 4 simple steps:

1. Clone the repository
```
git clone https://github.com/WilliamDavidHarrison/p2p-screensharing
```

2. Change directory
```
cd p2p-screensharing
```

3. Install dependencies
```
npm install
```
npm start -- --port 3000 --host
```

> You can replace `3000` with whatever port you want the server to run on.

4. Open your browser
Open http://localhost:3000 in your browser and you will see your application ready to go!
