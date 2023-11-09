# p2p Screensharing
Chat and screenshare peer-to-peer, without a server, no account needed.

###### This repository is a forked and updated version of *[purplnay/p2p-screensharing](https://github.com/purplnay/p2p-screensharing)*.

## Introduction
Backed by no server, p2p Screensharing runs entirely in the browser to chat and screenshare with a peer. It supports both mobile and desktop (screen capture may not be available on mobile).

## Public Version
A public version of the app is available [here](https://screenshare.wdh.gg).

Once you have opened it on your browser, you can create a room, choose a username and share the room's link with the person you'd like to chat/screenshare with.

Screensharing is optional, you may start streaming at any time, stop streaming, change the window/screen that you're sharing. Audio capture may not be available, depending on your browser.

## Self Hosting
> **Important**
> This application requires a domain name to work properly. It will most likely not work with an IP address like `0.0.0.0:3000`.

### 1. Clone the repository
```
git clone https://github.com/WilliamDavidHarrison/p2p-screensharing
```

### 2. Change directory
```
cd p2p-screensharing
```

### 3. Install dependencies
```
npm install
```

### 4. Start the server
```
npm start -- --port 3000 --host
```

> You can replace `3000` with whatever port you want the server to run on.

### 5. Open your browser
Open http://localhost:3000 in your browser and you will see your application ready to go!
