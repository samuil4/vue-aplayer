---
sidebar: auto
---

# Options

::: tip Info:
Property names in HTML are case insensitive, so the browser interprets all uppercase characters as lowercase characters. 
This means that when you use a template in the DOM, the prop name of camelCase (hump nomenclature) needs to be named using its equivalent kebab-case (short-lined naming). 
If you use a string template, then this restriction does not apply.
:::

## fixed <Badge text="Optional" />

- **Type**：`boolean?`
- **Default value**：`false`
- **Description**：Enable bottom fixed mode

## mini <Badge text="Optional" />

::: tip Info:
This option controls the player to expand or collapse if the bottom mode is on
:::

- **Type**：`boolean?`
- **Default value**：`false`
- **Description**：Expand when fixed is set to `true`

## autoplay <Badge text="Optional" />

::: warning Note:
This option is not available on the mobile version, because most mobile browsers have audio autoplay disabled by default.
:::

- **Type**：`boolean?`
- **Default value**：`false`
- **Description**：Enable automatic playbac

## theme <Badge text="Optional" />

::: tip Info:
You can choose to use [color-thief](https://cdn.jsdelivr.net/npm/colorthief@2.0.2/dist/) to let the player automatically get the theme color based on the cover image.
:::

- **Type**：`string?`
- **Default value**：`#b7daff`
- **Description**：Set the player default theme color

## loop <Badge text="Optional" />

::: warning Note:
Since the player will save the user's usage habits, this option will be invalid after the player is initialized for the first time.
:::

- **Type**：`APlayer.LoopMode?`
- **Default value**：`all`
- **Description**：Set the initial loop mode of the player

```ts
declare namespace APlayer {
  export type LoopMode = 'all' | 'one' | 'none';
}
```

## order <Badge text="Optional" />

::: warning Note:
Since the player will save the user's usage habits, this option will be invalid after the player is initialized for the first time.
:::

- **Type**：`APlayer.OrderMode?`
- **Default value**：`list`
- **Description**：Set the initial sequence mode of the player

```ts
declare namespace APlayer {
  export type OrderMode = 'list' | 'random';
}
```

## preload <Badge text="Optional" />

- **Type**：`APlayer.Preload?`
- **Default value**：`auto`
- **Description**：Set the preload mode of the audio

```ts
declare namespace APlayer {
  export type Preload = 'none' | 'metadata' | 'auto';
}
```

## volume <Badge text="Optional" />

- **Type**：`number?`
- **Default value**：`0.7`
- **Description**：Set the volume of the player

## audio <Badge type="error" text="Required" />

- **Type**：`APlayer.Audio | Array<APlayer.Audio>`
- **Default value**：`undefined`
- **Description**：Set the audio object or playlist to play

```ts
declare namespace APlayer {
  export type AudioType = 'auto' | 'hls' | 'normal';
  export interface Audio {
    id?: number; // Song id
    name: string; // Song name
    artist: string; // Artist name
    url: string; // Audio url
    cover: string; // Cover image url
    lrc?: string; // Lirycs url
    theme?: string; // Single theme color, it will override the global default theme color
    type?: AudioType; // Specify the type of audio
    speed?: number; // Playback speed
  }
}
```

The differences between this [APlayer](https://github.com/MoePlayer/APlayer) and the current implentation are the extra props added  `id` and `speed`.
`id` By default, generated automatically by the player, but you can also pass `id` value.  
`speed` The property can specify the playback speed of the audio.

::: warning Note:
`id` is a unique identifier used to distinguish audio. It is not allowed to repeat. If it is repeated, it may cause the player to missbehave.   
By default, `id` is generated according to the index of the playlist audio files, and regenerates when the playlist change (add / delete).   
When you delete the audio from a playlist, it will cause the current audio `id` to not match the playlist id after deletion.
If you are using unique `url` values everithing will be ok.
If there are duplicate `url` values，you have to set the audio `id` properties to ensure that each item is unique, otherwise the player exception may occur.
:::

## customAudioType <Badge text="Optional" />

- **Type**：`{ [index: string]: Function }?`
- **Default value**：`undefined`
- **Description**：Custom audio type

📝 [example.vue](/guide/hls.html)

```vue
<template>
  <aplayer :audio="audio" :customAudioType="customAudioType" :lrcType="3" />
</template>

<script>
import Vue from 'vue';
import APlayer from '@moefe/vue-aplayer';

Vue.use(APlayer);

export default {
  data() {
    return {
      audio: {
        name: '太陽と向日葵.m3u8',
        artist: 'FLOWER',
        url: 'http://pdacsgxq7.bkt.clouddn.com/hls/sunandsunflower.m3u8',
        cover: 'http://p1.music.126.net/CoM8s2FD5q1OgG4LSSDZuw==/5945059371390886.jpg?param=300y300', // prettier-ignore
        lrc: 'http://pdacsgxq7.bkt.clouddn.com/lrc/sunandsunflower.lrc',
        type: 'customHls',
      },
      customAudioType: {
        customHls(audioElement, audio, player) {
          if (Hls.isSupported()) {
            const hls = new Hls();
            hls.loadSource(audio.url);
            hls.attachMedia(audioElement);
          } else if (
            audioElement.canPlayType('application/x-mpegURL') ||
            audioElement.canPlayType('application/vnd.apple.mpegURL')
          ) {
            audioElement.src = audio.url;
          } else {
            player.showNotice('Error: HLS is not supported.');
          }
        },
      },
    };
  },
};
</script>
```

## mutex <Badge text="Optional" />

- **Type**：`boolean?`
- **Default value**：`true`
- **Description**：Enable mutual exclusion mode

If enabled, multiple players will be blocked from playing at the same time, and other players will be paused while the current player is playing.

## lrcType <Badge text="Optional" />

- **Type**：`APlayer.LrcType?`
- **Default value**：`0`
- **Description**：Set lrc lyrics parsing mode

```ts
declare namespace APlayer {
  export enum LrcType {
    file = 3, // Indicates that the value of audio.lrc is the lrc file address, and the lrc lyrics text will be obtained by `fetch`
    html = 2, // HTML support
    string = 1, // Indicates that the value of audio.lrc is a string in lrc format, which will parse the lyrics directly through it.
    disabled = 0, // Disable lrc lyrics
  }
}
```

## listFolded <Badge text="Optional" />

::: warning Note:
Since the player will save the user's usage habits, this option will be invalid after the player is initialized for the first time.
:::

- **Type**：`boolean?`
- **Default value**：`false`
- **Description**：Fold the playlist

## listMaxHeight <Badge text="Optional" />

- **Type**：`number?`
- **Default value**：`250`
- **Description**：Set the maximum height of the playlist in pixels

## storageName <Badge text="Optional" />

- **Type**：`string?`
- **Default value**：`aplayer-setting`
- **Description**：Set the `localStorage` key to store player settings

This [APlayer](https://github.com/MoePlayer/APlayer) are a bit different when saving the settings to `localStoragea`. Current implementation saves an array of objects 
independently of each other between different instances, under normal circumstances you do not need to change this.

```js
// You can get the player settings for the current 
// instance using the instance's `currentSettings` property.
console.log(this.$refs.aplayer.currentSettings);
```
