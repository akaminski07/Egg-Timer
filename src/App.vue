<script setup>
import {onMounted, ref} from 'vue';
import { gsap } from 'gsap-trial';

const one_minute = ref("1 Minute");
const one_minute2 = ref("3 Minutes");
const one_minute3 = ref("5 Minutes");
const countdown = ref(null);
let customTime = ref("");
let time = ref("0s");
let currentRotation = 0;

const alarmSound = new Audio("src/mp3/alarm.mp3");
alarmSound.volume = 0.1;

const startTimer = (seconds) => {
  alarmSound.pause()
  alarmSound.currentTime = 0;
  let timeLeft = seconds; // Set countdown to 60 seconds
  time.value = `${timeLeft}s`; // Update button text

  // Clear any existing timer to avoid overlaps
  if (countdown.value) {
    clearInterval(countdown.value);
  }
  gsap.to(".eggImg", {
    rotation: currentRotation += 360,
    duration: timeLeft,
    ease: "easeOut",
  });

  countdown.value = setInterval(() => {

    timeLeft--;
    if (timeLeft > -1) {
      time.value = `${timeLeft}s`; // Update button text with time remaining

    } else {
      clearInterval(countdown.value); // Stop timer
      time.value = "0s"; // Reset button text
      alarmSound.play();

    }
  }, 1000); // Run every second
};
onMounted(() => {

});

</script>

<template>

  <img class="eggImg" src="/public/funny_egg.png" alt="egg" />
    <h1 class="black">Egg Timer</h1>
  <h1 class="time">{{time}}</h1>

    <div id="buttons" >
      <button @click="startTimer(60)" class="btn">{{ one_minute }}</button>
      <button @click="startTimer(3 * 60)" class="btn">{{ one_minute2 }}</button>
      <button @click="startTimer(5 * 60)" class="btn">{{ one_minute3 }}</button>
    </div>


  <input class=" mt-3 mb-3 form-control" v-model="customTime" placeholder="Custom time" />
  <button class="btn" @click="startTimer(customTime)">Set custom time</button>
</template>

<style>

.form-control{
  width: 10rem;
}

body{
  background-color: white;
}

.eggImg{
  width: 300px;
  height: 300px;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

h1 {
  margin-bottom: 20px;
  font-size: 2.8rem;
  font-family: "Jersey 10"
}

.time{
  color: #2c2c2c;
}

#buttons {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 10px;

}

.btn {
  background-color: #ff8800;
  font-weight: bold;
  color: white;
  transition: transform 0.5s ease, background-color 0.2s ease;
}

.btn:hover {
  background-color: rgba(255, 136, 0, 0.8);
  color: white;
  transform: scale(1.05);
}

.btn:active{
  transform: scale(0.95);
  background-color: rgb(255, 136, 0);
  color: white;
}

.black {
  color: #ff8800;
  font-size: 4rem;
}
</style>
