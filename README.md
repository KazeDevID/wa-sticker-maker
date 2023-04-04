<div align=center>

<img src="https://img.icons8.com/color/96/000000/whatsapp--v5.png" alt="wsf"/>

# wa-sticker-maker

wa-sticker-maker is a simple tool which allows you to create and format WhatsApp Stickers.

[![NPM](https://img.shields.io/npm/l/wa-sticker-formatter?style=flat-square&label=License)](https://github.com/KazeDevID/wa-sticker-maker/blob/master/LICENSE) [![CodeFactor](https://img.shields.io/codefactor/grade/github/KazeDevID/wa-sticker-maker?style=flat-square&label=Code%20Quality)](https://www.codefactor.io/repository/github/KazeDevID/wa-sticker-maker) [![NPM](https://img.shields.io/npm/KazeDevID/wa-sticker-maker?style=flat-square&label=Downloads)](https://npmjs.com/package/wa-sticker-maker)


</div>

# Installation

```cmd
> npm i wa-sticker-maker
```

# Usage

Wa-Sticker-Formatter provides two ways to create stickers.
The paramers are the same for both.

1. First is the Buffer, SVG String, URL, SVG String or File path of static image, GIF or Video. The second is the options. GIFs and Videos will output an animated WebP file.

2. 2nd Paramter, an object, Sticker Options accepts the following fields

`pack` - The pack name.<br>
`author` - The author name.<br>
`type` - Value from StickeTypes enum (exported). Can be 'crop' or 'full' or undefined (default).<br>
`categories` - The sticker category. Can be an array of Emojis or undefined (default).<br>
`quality` - The quality of the output file. Can be an integer from 0 to 100. Defaults to 100.
`id` - The sticker id. If this property is not defined, it will be generated.<br>
`background` - Background color in hexadecimal format or an RGBA Object. Defaults to undefined (transparent).<br>

## Import

Before using the library, you need to import it.

```TS
import { Sticker, createSticker, StickerTypes } from 'wa-sticker-maker' // ES6
// const { Sticker, createSticker, StickerTypes } = require('wa-sticker-maker') // CommonJS
```
## Using The `Sticker` constructor (Recommended)

```TS
const sticker = new Sticker(image, {
    pack: 'My Pack', // The pack name
    author: 'Me', // The author name
    type: StickerTypes.FULL, // The sticker type
    categories: ['🤩', '🎉'], // The sticker category
    id: '12345', // The sticker id
    quality: 50, // The quality of the output file
    background: '#000000' // The sticker background color (only for full stickers)
})

const buffer = await sticker.toBuffer() // convert to buffer
// or save to file
await sticker.toFile('sticker.webp')

// or get Baileys-MD Compatible Object
conn.sendMessage(jid, await sticker.toMessage())

```

You can also chain methods like this:

```TS
const buffer = await new Sticker(image)
    .setPack('My Pack')
    .setAuthor('Me')
    .setType(StickerTypes.FULL)
    .setCategories(['🤩', '🎉'])
    .setId('12345')
    .setBackground('#000000')
    .setQuality(50)
    .toBuffer()
```

The `image` (first parameter) can be a Buffer, URL, SVG string, or File path.

### SVG Example
```TS
const sticker = new Sticker(`
    <svg xmlns="http://www.w3.org/2000/svg" width="512" height="512" viewBox="0 0 512 512">
        <path d="M256 0C114.6 0 0 114.6 0 256s114.6 256 256 256 256-114.6 256-256S397.4 0 256 0zm0 464c-119.1 0-216-96.9-216-216S136.9 40 256 40s216 96.9 216 216-96.9 216-216 216z" fill="#ff0000" />
    </svg>
`, { author: 'W3' })
```

## Using the `createSticker` function

```TS
const buffer = await createSticker(buffer, options) // same params as the constructor
// NOTE: `createSticker` returns a Promise of a Buffer
```

## Options

The following options are valid:

```TS
interface IStickerConfig {
    /** Sticker Pack title*/
    pack?: string
    /** Sticker Pack Author*/
    author?: string
    /** Sticker Pack ID*/
    id?: string
    /** Sticker Category*/
    categories?: Categories[]
    /** Background */
    background?: Sharp.Color
     /** Sticker Type */
    type?: StickerTypes | string
    /* Output quality */
    quality?: number
}
```

## Sticker Types

Sticker types are exported as an enum.

```ts
enum StickerTypes {
    DEFAULT = 'default',
    CROPPED = 'crop',
    FULL = 'full'
}

```

## Background

Background can be a hex color string or a sharp color object.
```JSON
{
    "background": "#FFFFFF"
}
```
or 

```JSON  
{
    "background": {
        "r": 255,
        "g": 255,
        "b": 255,
        "alpha": 1
    }
}
```

# Metadata

Here's some basic information about WhatsApp Sticker Metadata.

In WhatsApp, stickers have their own metadata embedded in the WebP file as They hold info like the author, the title or pack name and the category.


```TS
import { extractMetadata, Sticker } from 'wa-sticker-maker'
import { readFileSync } from 'fs'

const sticker = readFileSync('sticker.webp')
let metadata = await extractMetadata(sticker) // { emojis: [], 'sticker-pack-id': '', 'sticker-pack-name': '', 'sticker-author-name': '' }

// or use the static method from the Sticker class
metadata = await Sticker.extractMetadata(sticker)

```

---
Thanks for using wa-sticker-maker!


