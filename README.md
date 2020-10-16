# Now Playing (Widget)

A now playing widget for OBS Studio or Webpage, using Last.fm API for current playing track. 

Can work with Spotify when connected with your Last.fm account.

## 👀 Demos
* 👉 [stacked](https://now-playing-2e8d3.web.app?user=halfcube&theme=stacked)
* 👉 [ticker (marquee)](https://now-playing-2e8d3.web.app?user=halfcube&theme=ticker)

## 🛠 Configurations

| URL param | description |
|---|---|
| `user` | lastfm username |
| `theme` | theme mode |

## example

    https://now-playing-2e8d3.web.app?user=halfcube&theme=ticker

## ⚡️ System Dependencies
This project requires Node.js to build and run.

_At time of writing the latest stable version is Node.js 14.9.0_


## 🎲 Installation

Within the working directory of this project after checking it out to your local computer.

Run the following commands.

```bash
npm install
```

## 🎯 Usage

Start a local development server with the following command

```bash
npm run dev
```

To deploy the resulting code to a Web Server, the code must be compiled before deployment.

```bash
npm run build
```

The resulting code in `./dist` can be uploaded to your Web Server of choice.


## 🎓 License

MIT: https://kylewelsby.mit-license.org