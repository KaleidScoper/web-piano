<template>
  <div class="piano">
    <h1>Web电子琴</h1>
    <button v-if="!isAudioContextStarted" @click="initializeAudio">
      点击以启用音频
    </button>
    <div v-else class="keys">
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
      synth: null,
      notes: ["C4", "D4", "E4", "F4", "G4", "A4", "B4", "C5"],
      isAudioContextStarted: false,
    };
  },
  methods: {
    async initializeAudio() {
      try {
        console.log("Initializing audio context...");
        await Tone.start(); // 激活音频上下文
        this.isAudioContextStarted = true;
        this.synth = new Tone.Synth().toDestination();
        console.log("Audio context initialized.");
      } catch (error) {
        console.error("Error initializing audio context:", error);
      }
    },
    playNote(note) {
      try {
        if (this.isAudioContextStarted) {
          console.log(`Playing note: ${note}`);
          this.synth.triggerAttack(note); // 播放音符
        }
      } catch (error) {
        console.error("Error while playing note:", error);
      }
    },
    stopNote() {
      try {
        if (this.synth) {
          this.synth.triggerRelease();
        }
      } catch (error) {
        console.error("Error while stopping note:", error);
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
