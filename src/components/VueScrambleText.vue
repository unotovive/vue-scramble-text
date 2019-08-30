<template>
  <span>{{ tweenedValue }}</span>
</template>
<script lang="ts">
import { Vue, Component, Prop, Watch } from "vue-property-decorator";
@Component({})
export default class VueTextScrambler extends Vue {
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

  private queue: any[] = [];
  private tweenedValue: string = "";
  private frame: any;
  private frameRequest: any;

  public mounted() {
    if (this.disableInitialScramble) {
      this.tweenedValue = `${this.text}`;
    } else {
      this.setText(this.tweenedValue, `${this.text}`);
    }
  }

  @Watch("text")
  public onTextChanged(newValue: string, oldValue: string) {
    this.setText(`${oldValue}`, `${newValue}`);
  }

  public setText(oldText: string, newText: string) {
    const length = Math.max(oldText.length, newText.length);
    this.queue = [];
    for (let i = 0; i < length; i++) {
      const from = oldText[i] || "";
      const to = newText[i] || "";
      const start = Math.floor(Math.random() * this.scrambleDuration);
      const end = start + Math.floor(Math.random() * this.scrambleDuration);
      this.queue.push({ from, to, start, end });
    }
    cancelAnimationFrame(this.frameRequest);
    this.frame = 0;
    this.update();
  }

  public randomChar() {
    return this.scrambleCharacterSet[
      Math.floor(Math.random() * this.scrambleCharacterSet.length)
    ];
  }
  public update() {
    let output = "";
    let complete = 0;
    for (let i = 0, n = this.queue.length; i < n; i++) {
      const { from, to, start, end, charTmp } = this.queue[i];
      let char = charTmp;
      if (this.frame >= end) {
        complete++;
        output += to;
      } else if (this.frame >= start) {
        if (!char || Math.random() < 0.28) {
          char = this.randomChar();
          this.queue[i].char = char;
        }
        output += char;
      } else {
        output += from;
      }
    }
    this.tweenedValue = output;
    if (complete !== this.queue.length) {
      this.frameRequest = requestAnimationFrame(this.update);
      this.frame++;
    }
  }
}
</script>
<style lang="scss" scoped>
</style>
