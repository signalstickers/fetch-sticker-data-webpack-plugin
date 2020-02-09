<a href="#top" id="top">
  <img src="https://user-images.githubusercontent.com/441546/74094549-eb837700-4a97-11ea-867f-448675c19342.png" style="max-width: 100%;"></<img>
</a>
<p align="center">
  <a href="https://www.npmjs.com/package/@signalstickers/fetch-sticker-data-webpack-plugin"><img src="https://img.shields.io/npm/v/@signalstickers/fetch-sticker-data-webpack-plugin.svg?"></a>
  <a href="https://travis-ci.com/signalstickers/fetch-data-webpack-plugin"><img src="https://img.shields.io/travis/com/signalstickers/fetch-data-webpack-plugin?"></a>
  <a href="https://david-dm.org/signalstickers/fetch-sticker-data-webpack-plugin"><img src="https://img.shields.io/david/signalstickers/fetch-sticker-data-webpack-plugin.svg?"></a>
  <a href="https://conventionalcommits.org"><img src="https://img.shields.io/badge/conventional%20commits-1.0.0-FB5E85.svg?"></a>
</p>

This Webpack plugin fetches sticker pack data from Signal at build-time,
allowing the Signal Stickers website to load this data without having to fetch
and decrypt it from the Signal API.

# Install

```
npm i @signalstickers/fetch-sticker-data-webpack-plugin
```

# Use

The plugin accepts 2 options, `inputFile` and `outputFile`. `inputFile` should
be the path to a YAML file enumerating one or more [`StickerPackYaml`](https://github.com/signalstickers/fetch-sticker-data-webpack-plugin/blob/master/src/etc/types.ts#L9-L16)
objects. `outputFile` should be the desired path to a JSON file in the
compilation's output directory where the plugin will wite a list of [`StickerPackPartial`](https://github.com/signalstickers/fetch-sticker-data-webpack-plugin/blob/master/src/etc/types.ts#L42-L45)
objects for each entry in `inputFile`.

To enable the plugin, add it to your Webpack configuration:

```ts
import FetchStickerDataPlugin from '@signalstickers/fetch-sticker-data-webpack-plugin';

export default {
  plugins: [new FetchStickerDataPlugin({
    inputFile: 'stickers.yml',
    outputFile: 'sticker-partials.json'
  })]
}
```

**Example:**

The following `stickers.yml` file will produce the below JSON output.

> `stickers.yml`

```yml
---
ae1d343accdec586fe19954a6be7aee9:
  key: ffea9831b6af7617e4ab1b3868ce6f26f5c7c8ab3f05fb8d68b436f9253232ab
  source: ''
  tags:
    - cat
  nsfw: false
```

> `sticker-partials.json`

```json
{
  "meta": {
    "id":"ae1d343accdec586fe19954a6be7aee9",
    "key": "ffea9831b6af7617e4ab1b3868ce6f26f5c7c8ab3f05fb8d68b436f9253232ab",
    "source":"",
    "tags": [
      "cat"
    ],
    "nsfw":false
  },
  "manifest": {
    "title": "Taffy Cat",
    "author": "Dmitrii Nechitailo",
    "cover": {
      "id": 0,
      "emoji": ""
    }
  }
}
```
