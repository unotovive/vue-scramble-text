# vue-scramble-text

* Vue.jsでString配列を渡すといい感じにシャカシャカしてくれるコンポーネントライブラリ
* Component library for Vue.js. Pass string array for text shuffled effect.

# What is this 
[![Image from Gyazo](https://i.gyazo.com/6d3b481669a7f206bd27421354a7dbae.gif)](https://gyazo.com/6d3b481669a7f206bd27421354a7dbae)

# How to use
1. install package
`yarn add vue-scramble-text`

2. import in .vue file and use as HTML tag (Typescript example) 
```Vue
<template>
  <div id="app">
    <vue-scramble-text :text="'I love Vue.js & typescript ♡♡♡♡'" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import VueScrambleText from "./components/VueScrambleText.vue";
@Component({
  components: {
    VueScrambleText
  }
})
export default class App extends Vue {}
</script>
``` 

3. Change text
* component can operate automatically If text has changed.

4. Other settings(API)
```typescript
  @Prop()
  public text!: string;
  // この値を変更すると表示テキストがシャッフルされながら変わります。
  // Text for display. You can change this dynamically

  @Prop()
  public disableInitialScramble!: boolean;
  // これをtrueに設定すると初期シャカシャカが無効になります。
  // disable nitial suffle.

  @Prop({ default: 20 })
  public scrambleDuration!: number;
  // １シャカシャカにかかる時間です。
  // Duration time for 1 change action

  @Prop({ default: "!<>-_\\/[]{}—=+*^?#________" })
  public scrambleCharacterSet!: string;
  // シャカシャカ時に表示されるテキストです（こっからランダム）
  // A-Zにしてもそれっぽい
  // text for display while suffling
```