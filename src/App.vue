<template>
  <div class="piano">
    <h1>Web电子琴</h1>
    <div class="keys">
      <button
        v-for="note in notes"
        :key="note"
        class="key"
        @mousedown="playNote(note)"
        @mouseup="stopNote"
        @mouseleave="stopNote"
      >
        {{ note }}
      </button>
    </div>
  </div>
</template>

<script>
import * as Tone from "tone";

export default {
  name: "App",
  data() {
    return {
      synth: null, // 合成器实例
      notes: ["C4", "D4", "E4", "F4", "G4", "A4", "B4", "C5"], // 音符集合
    };
  },
  methods: {
    async playNote(note) {
      // 初始化音频上下文和合成器（首次交互时）
      if (!this.synth) {
        console.log("Initializing audio context...");
        await Tone.start(); // 激活 Tone.js 的音频上下文
        this.synth = new Tone.Synth().toDestination();
      }
      console.log(`Playing note: ${note}`);
      this.synth.triggerAttack(note); // 播放音符
    },
    stopNote() {
      if (this.synth) {
        this.synth.triggerRelease(); // 停止音符
      }
    },
  },
};
</script>

<style scoped>
.piano {
  text-align: center;
  margin-top: 50px;
  font-family: Arial, sans-serif;
}
.keys {
  display: flex;
  justify-content: center;
  gap: 10px;
}
.key {
  width: 60px;
  height: 200px;
  background: white;
  border: 2px solid black;
  border-radius: 5px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  user-select: none;
  display: flex;
  justify-content: center;
  align-items: flex-end;
}
.key:active {
  background: #f0f0f0;
}
</style>
