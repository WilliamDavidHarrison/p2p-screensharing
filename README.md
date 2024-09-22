# p2p Screensharing
Chat and screenshare peer-to-peer, without a server, no account needed.

![Screenshot](/media/screenshot.png)

###### This repository is a forked and updated version of *[purplnay/p2p-screensharing](https://github.com/purplnay/p2p-screensharing)*.

## Introduction
Backed by no server, p2p Screensharing runs entirely in the browser to chat and screenshare with a peer. It supports both mobile and desktop (screen capture may not be available on mobile).

## Self Hosting
> [!IMPORTANT]
> This application requires a domain name to work properly. It will most likely not work with an IP address like `0.0.0.0:3000`.

### Pterodactyl
If you are looking to host this app using your Pterodactyl panel, you can find the pre-made egg [here](https://github.com/wdhdev/eggs/tree/main/software/p2p-screensharing).

---

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
