---
layout: post
title: kobo-eReader-Exporter
img: "assets/post-imgs/kobo-eReader-Exporter.png"
date: 27 September 2021
tags: [SideProject]
---

<p align='left'>
  A GUI application that makes you export data from the Kobo eReader database., built on Electron and Vue.js.
</p>

<br>

<!-- downloads -->
<a href="https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter/releases">
<img src="https://img.shields.io/github/downloads/benxuhuang/electron-vue-kobo-eReader-exporter/total.svg?style=flat" alt="downloads" style="padding: 0;" />
</a>
<!-- version -->
<a href="https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter/releases">
<img src="https://img.shields.io/github/release-pre/benxuhuang/electron-vue-kobo-eReader-exporter.svg?style=flat" alt="latest version" style="padding: 0;" />
</a>
<!-- platform -->
<a href="https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter">
<img src="https://img.shields.io/badge/platform-macOS-lightgrey.svg?style=flat" alt="platform" style="padding: 0;" />
</a>
<a href="https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter">
<img src="https://img.shields.io/badge/platform-windows-lightgrey.svg?style=flat" alt="platform" style="padding: 0;" />
</a>

<br>

![image](https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter/raw/main/.github/upload.png)

![image](https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter/raw/main/.github/words.png)

![image](https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter/raw/main/.github/bookmarks.png)

<br>

## How to install and use the application

1. [Download and install the application](https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter/releases)
2. Copy `KoboReader.sqlite` file from your eReader's drive: KOBOeReader > .kobo > KoboReader.sqlite to your computer
3. Open `electron-vue-kobo-eReader-exporter` application
4. Upload `KoboReader.sqlite` file

#### Note: Where do I find the KoboReader.sqlite file? 

`KoboReader.sqlite` is located on your Kobo device.
Connect your eReader to the computer, and open the folder ".kobo". 
There you should find your "KoboReader.sqlite" or "KoboReader" file 
(you will only see the extension .sqlite if you have that option enabled on your PC). 
If you can't find your .kobo folder, that's because it is hidden on your computer. 
Don't worry! 
The following links will explain what you need to do to see the hidden folders and files:

- [Windows explanation](https://www.computerhope.com/issues/ch000516.htm)

- [MacOS explanation](https://appleinsider.com/articles/18/07/27/how-to-see-hidden-files-and-folders-in-macos)


## How to build

### Required

- [Nodejs](https://github.com/nodejs/node)
- [Electron](https://github.com/electron/electron)
- [Electron-vue](https://github.com/SimulatedGREG/electron-vue)
- [Node-sqlite3](https://github.com/mapbox/node-sqlite3)

### Build steps

- Clone the project via this Terminal command:

```sh
git clone https://github.com/benxuhuang/electron-vue-kobo-eReader-exporter
```

- Build

```bash
npm install

npm run dev

npm run build
```

