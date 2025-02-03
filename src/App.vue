<script setup>
import {onMounted, ref} from 'vue';
import { gsap } from 'gsap-trial';



const countdown = ref(null);
let customTime = ref("");
let time = ref("0s");
const timeline = gsap.timeline({ repeat: -1, yoyo: true});



const alarmSound = new Audio("/Egg-Timer/alarm.mp3");
alarmSound.volume = 0.1;

const startTimer = (seconds) => {

  clearInterval(countdown.value)
  alarmSound.pause();
  alarmSound.currentTime = 0;
  let timeLeft = seconds;
  time.value = `${timeLeft}s`;

  if (countdown.value) {
    clearInterval(countdown.value);
  }

  gsap.set(".eggImg", { rotation: 0 });
  gsap.to(".eggImg", {
    rotation: 360,
    duration: timeLeft,
    ease: "easeOut",
  });


  countdown.value = setInterval(() => {
    timeLeft--;
    if (timeLeft > 0) {
      time.value = `${timeLeft}s`;
    } else {
      clearInterval(countdown.value);
      time.value = "0s";
      alarmSound.play();
    }
  }, 1000);
};

onMounted(() => {
  timeline.to(".eggImg", {
    y: 40,
    duration: 1, // Adjust speed
    ease: "power1.inOut"
  });
});

</script>

<template>

  <img class="eggImg" src="/public/funny_egg.png" alt="egg" />
    <h1 class="black">Egg Timer</h1>
  <h1 class="time">{{time}}</h1>


    <div id="buttons" >
      <button @click="startTimer(60)" class="btn">1 Min.</button>
      <button @click="startTimer(3 * 60)" class="btn">2 Min.</button>
      <button @click="startTimer(5 * 60)" class="btn">3 Min.</button>
    </div>


  <input class=" mt-3 mb-3 form-control" v-model="customTime" placeholder="Time (s)" />
  <button class="btn" style="width: 8rem" @click="startTimer(customTime)">Set</button>
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
  font-size: 20px;
  color: white;
  transition: transform 0.5s ease, background-color 0.2s ease;
  font-family: "Jersey 10";

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
