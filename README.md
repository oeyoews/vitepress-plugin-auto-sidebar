<h2 align='center'><samp>vite-plugin-vitepress-auto-sidebar</samp></h2>

<p align='center'>Automatically generate VitePress sidebar</p>

<p align='center'>
  <a href='https://www.npmjs.com/package/@limy-org/vite-plugin-vitepress-auto-sidebar'>
    <img src='https://img.shields.io/npm/v/@limy-org/vite-plugin-vitepress-auto-sidebar?color=222&style=flat-square'>
  </a>
  <a href='https://github.com/mingyuLi97/vite-plugin-vitepress-auto-sidebar/blob/master/LICENSE'>
    <img src='https://img.shields.io/badge/license-MIT-blue.svg'>
  </a>
</p>

## Features

- 🪄 Automatic sidebar generation
- ✨ Title of files synchronized with sidebar
- 🚀 Automatic reload when files are deleted or title is modified
- ✔️ Customizable configuration options (custom directory names, custom file sorting).

## Installation

```bash
# pnpm
pnpm i @limy-org/vite-plugin-vitepress-auto-sidebar
# yarn
yarn add @limy-org/vite-plugin-vitepress-auto-sidebar
# npm
npm install @limy-org/vite-plugin-vitepress-auto-sidebar
```

## Usage

```ts
// .vitepress/config.ts
import VitePluginAutoSidebar from "@limy-org/vite-plugin-vitepress-auto-sidebar";
export default defineConfig({
  vite: {
    plugins: [
      VitePluginAutoSidebar({
        sidebarResolved(value) {
          console.log(value);
          // do sort
          value["/dir2/"][0].items?.sort((a, b) => a.text - b.text);
          // rename
          value["/dir2/"][0].text = 'sorted'
        },
        ignores: ["index.md"],
        root: process.cwd(),
      }),
    ],
  },
});
```

## How it work

```
├── .vitepress
├── dir1
│   ├── dir1-1
│   │   ├── 1.md
│   │   ├── 2.md
│   │   └── dir1-1-1
│   │       └── 1.md
│   └── dir1-2
│       └── 1.md
├── dir2
│   └── 2-2
│       ├── 2.md
│       └── 3.md
├── index.md
├── node_modules
└── package.json

TO


{
  "/dir2/": [
    {
      "text": "2-2",
      "collapsed": false,
      "items": [
        {
          "text": "2-2",
          "link": "/dir2/2-2/3.md"
        },
        {
          "text": "222",
          "link": "/dir2/2-2/2.md"
        }
      ]
    }
  ],
  "/dir1/": [
    {
      "text": "dir1-2",
      "collapsed": false,
      "items": [
        {
          "text": "1-2",
          "link": "/dir1/dir1-2/1.md"
        }
      ]
    },
    {
      "text": "dir1-1",
      "collapsed": false,
      "items": [
        {
          "text": "2222",
          "link": "/dir1/dir1-1/2.md"
        },
        {
          "text": "1-1",
          "link": "/dir1/dir1-1/1.md"
        },
        {
          "text": "dir1-1-1",
          "collapsed": false,
          "items": [
            {
              "text": "1-1-1",
              "link": "/dir1/dir1-1/dir1-1-1/1.md"
            }
          ]
        }
      ]
    }
  ]
}
```
