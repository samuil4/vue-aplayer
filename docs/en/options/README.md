---
sidebar: auto
---

# 选项

::: tip Info:
HTML 中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。  
这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名。如果你使用字符串模板，那么这个限制就不存在了。
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

::: warning 注意
由于大多数移动端浏览器禁止了音频自动播放，所以该选项在移动端无效
:::

- **Type**：`boolean?`
- **Default value**：`false`
- **Description**：是否开启自动播放

## theme <Badge text="Optional" />

::: tip Info:
你可以选择引入 [color-thief](https://cdn.jsdelivr.net/npm/colorthief@2.0.2/dist/) 让播放器根据封面图片自动获取主题颜色
:::

- **Type**：`string?`
- **Default value**：`#b7daff`
- **Description**：设置播放器默认主题颜色

## loop <Badge text="Optional" />

::: warning 注意
由于播放器会保存用户的使用习惯，所以播放器首次初始化之后该选项将失效
:::

- **Type**：`APlayer.LoopMode?`
- **Default value**：`all`
- **Description**：设置播放器的初始循环模式

```ts
declare namespace APlayer {
  export type LoopMode = 'all' | 'one' | 'none';
}
```

## order <Badge text="Optional" />

::: warning 注意
由于播放器会保存用户的使用习惯，所以播放器首次初始化之后该选项将失效
:::

- **Type**：`APlayer.OrderMode?`
- **Default value**：`list`
- **Description**：设置播放器的初始顺序模式

```ts
declare namespace APlayer {
  export type OrderMode = 'list' | 'random';
}
```

## preload <Badge text="Optional" />

- **Type**：`APlayer.Preload?`
- **Default value**：`auto`
- **Description**：设置音频的预加载模式

```ts
declare namespace APlayer {
  export type Preload = 'none' | 'metadata' | 'auto';
}
```

## volume <Badge text="Optional" />

- **Type**：`number?`
- **Default value**：`0.7`
- **Description**：设置播放器的音量

## audio <Badge type="error" text="必填" />

- **Type**：`APlayer.Audio | Array<APlayer.Audio>`
- **Default value**：`undefined`
- **Description**：设置要播放的音频对象或播放列表

```ts
declare namespace APlayer {
  export type AudioType = 'auto' | 'hls' | 'normal';
  export interface Audio {
    id?: number; // 音频 id
    name: string; // 音频名称
    artist: string; // 音频艺术家
    url: string; // 音频播放地址
    cover: string; // 音频封面
    lrc?: string; // lrc 歌词
    theme?: string; // 单曲主题色，它将覆盖全局的默认主题色
    type?: AudioType; // 指定音频的Type
    speed?: number; // 单曲播放速度
  }
}
```

这里与 [APlayer](https://github.com/MoePlayer/APlayer) 不同的是新增了 `id` 和 `speed` 属性。  
`id` 默认情况下由播放器自动生成，你也可以手动传一个 `id` 来覆盖它。  
`speed` 属性可以指定该音频的播放速度。

::: warning 注意
`id` 是用来区分音频的唯一标识，不允许重复，如果出现重复可能会导致播放器出现异常。  
默认情况下 `id` 是根据播放列表的索引生成，当播放列表发生变化时 (新增/删除) 会重新生成。  
当你从播放列表中删除音频时，由于播放列表发生了变化，所以会导致当前音频的 `id` 与删除后的播放列表不匹配。
出现这种情况时，会降级根据 `url` 更新当前音频的信息，如果播放列表中每一项的 `url` 都是唯一的，那么不会有问题。
如果有重复的 `url`，你必须设置音频的 `id` 属性，以确保每一项都是唯一的，否则播放器可能出现异常。
:::

## customAudioType <Badge text="Optional" />

- **Type**：`{ [index: string]: Function }?`
- **Default value**：`undefined`
- **Description**：自定义音频 Type

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
- **Description**：是否开启互斥模式

如果开启则会阻止多个播放器同时播放，当前播放器播放时暂停其他播放器

## lrcType <Badge text="Optional" />

- **Type**：`APlayer.LrcType?`
- **Default value**：`0`
- **Description**：设置 lrc 歌词解析模式

```ts
declare namespace APlayer {
  export enum LrcType {
    file = 3, // 表示 audio.lrc 的值是 lrc 文件地址，将通过 `fetch` 获取 lrc 歌词文本
    html = 2, // 不支持 html 用法
    string = 1, // 表示 audio.lrc 的值是 lrc 格式的字符串，将直接通过它解析歌词
    disabled = 0, // 禁用 lrc 歌词
  }
}
```

## listFolded <Badge text="Optional" />

::: warning 注意
由于播放器会保存用户的使用习惯，所以播放器首次初始化之后该选项将失效
:::

- **Type**：`boolean?`
- **Default value**：`false`
- **Description**：是否折叠播放列表

## listMaxHeight <Badge text="Optional" />

- **Type**：`number?`
- **Default value**：`250`
- **Description**：设置播放列表最大高度，单位为像素

## storageName <Badge text="Optional" />

- **Type**：`string?`
- **Default value**：`aplayer-setting`
- **Description**：设置存储播放器设置的 `localStorage` key

这里与 [APlayer](https://github.com/MoePlayer/APlayer) 有所不同，在 `localStorage` 中保存的是对象数组  
不同的实例之间互不影响，一般情况下你不需要修改此项。

```js
// 你可以使用实例的 `currentSettings` 属性获取当前实例的播放器设置
console.log(this.$refs.aplayer.currentSettings);
```
