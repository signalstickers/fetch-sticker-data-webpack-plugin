<a href="#top" id="top">
  <img src="https://user-images.githubusercontent.com/441546/104593733-f1c59000-5624-11eb-94fb-359d2dbae9b8.png" style="max-width: 100%;"></<img>
</a>
<p align="center">
  <a href="https://www.npmjs.com/package/@signalstickers/fetch-sticker-data-webpack-plugin"><img src="https://img.shields.io/npm/v/@signalstickers/fetch-sticker-data-webpack-plugin.svg?"></a>
  <a href="https://github.com/signalstickers/fetch-sticker-data-webpack-plugin/actions"><img src="https://img.shields.io/endpoint.svg?url=https%3A%2F%2Factions-badge.atrox.dev%2Fsignalstickers%2Ffetch-sticker-data-webpack-plugin%2Fbadge%3Fref%3Dmaster&style=flat-square&label=build&logo=none"></a>
  <a href="https://david-dm.org/signalstickers/fetch-sticker-data-webpack-plugin"><img src="https://img.shields.io/david/signalstickers/fetch-sticker-data-webpack-plugin.svg?"></a>
  <a href="https://conventionalcommits.org"><img src="https://img.shields.io/badge/conventional%20commits-1.0.0-FB5E85.svg?"></a>
</p>

This Webpack plugin fetches sticker pack data from Signal's Sticker API at build-time. It ingests a
YAML file containing a set of sticker pack IDs and decryption keys and outputs a JSON file containing


## Install

```
npm i @signalstickers/fetch-sticker-data-webpack-plugin
```

## Use

The plugin accepts 2 options, `inputFile` and `outputFile`. `inputFile` should be the path to a YAML
file enumerating one or more [`StickerPackYaml`](https://github.com/signalstickers/fetch-sticker-data-webpack-plugin/blob/master/src/etc/types.ts#L9-L16)
objects. `outputFile` should be the desired path to a JSON file in the compilation's output directory
where the plugin will wite a list of [`StickerPackPartial`](https://github.com/signalstickers/fetch-sticker-data-webpack-plugin/blob/master/src/etc/types.ts#L42-L45)
objects for each entry in `inputFile`.

For each entry in `inputFile`, the plugin will

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

### Example

The following `stickers.yml` file will produce the below JSON output.

> `stickers.yml`

```yml
# Sticker pack ID
ae1d343accdec586fe19954a6be7aee9:
  # Sticker pack's decryption key.
  key: ffea9831b6af7617e4ab1b3868ce6f26f5c7c8ab3f05fb8d68b436f9253232ab
  # All other key/value pairs here are optional and will be copied to the
  # output file.
  foo: bar
  baz: qux
```

> `sticker-partials.json`

The JSON file produced by the plugin will be an array of objects. Each object will contain `manifest`
and `meta` keys. The `manifest` key will contain various data fetched from Signal about the sticker
pack, while the `meta` key will contain the sticker pack ID and key from the input file, as well as any
other data contained in the entry in the input file.

```json
[
  {
    "manifest": {
      "title": "Taffy Cat",
      "author": "Dmitrii Nechitailo",
      "cover": {
        "id": 0,
        "emoji": ""
      }
    },
    "meta": {
      "id":"ae1d343accdec586fe19954a6be7aee9",
      "key": "ffea9831b6af7617e4ab1b3868ce6f26f5c7c8ab3f05fb8d68b436f9253232ab",
      "foo": "bar",
      "baz": "qux"
    }
  }
]
```
