<script setup>
//Imports
import {onMounted, ref, computed} from 'vue';
import { gsap } from 'gsap-trial';

const countdown = ref(null);
let customTime = ref("");
let time = ref("0s");
const timeline = gsap.timeline({ repeat: -1, yoyo: true});
const timerToggled = ref(true);
let errorText = ref(false);
let errorMessage = ref("Invalid Input!");

//Audio
const alarmSound = new Audio(`${import.meta.env.BASE_URL}alarm.mp3`);
alarmSound.volume = 0.1;

//Convert time to minutes
const formattedTime = computed(() => {
  const timeLeft = parseInt(time.value) || 0; // Ensure it's a number
  const minutes = Math.floor(timeLeft / 60);
  const seconds = timeLeft % 60;
  return `${minutes}m ${seconds}s`;
});
//Start timer when a button is clicked
const startTimer = (seconds) => {
  resetGSAP();
  toggleButton()
  customTime.value = ""
  let timeLeft = seconds;
  time.value = seconds;

  setTimeout(() => {
    errorText.value = false;
  }, 800);

  //Clear previous interval
  if (countdown.value) {
    clearInterval(countdown.value);
  }

  if (isNaN(seconds) || seconds <= 0) {
    time.value = "0s";
    showError();
    timerToggled.value = true;
    return;
  }
  //Egg rotating when timer is ticking
  gsap.set(".eggImg", { rotation: 0 });
  gsap.to(".eggImg", {
    rotation: 360,
    duration: timeLeft,
    ease: "easeOut",
  });

  //Make the timer tick per second
  countdown.value = setInterval(() => {
    timeLeft--;
    if (timeLeft > 0) {
      time.value = `${timeLeft}s`;
    } else {
      clearInterval(countdown.value);
      time.value = "0s";
      timerToggled.value = true;
      alarmSound.play();
    }
  }, 1000);
};
//Egg jumping up and down animation
function eggJumping(){
  timeline.fromTo(".eggImg", {
    y: 20,
    duration: 0.8,
    ease: "power1.inOut"
  },{
    y: -20,
    duration: 0.8,
    ease: "power1.inOut"
  });
}
//Reset animation to make it smooth
function resetGSAP(){
  gsap.killTweensOf(".eggImg", "rotation");
  gsap.set(".eggImg", {rotation: 0})
  clearInterval(countdown.value)
  alarmSound.pause();
  alarmSound.currentTime = 0;
}
onMounted(() => {
  //Start egg jump animation on entry
  eggJumping();
});

function toggleButton() {
  resetGSAP();
  timerToggled.value = !timerToggled.value;
  time.value = "0s";
}

function showError(){
  if(errorText.value === false){
    errorText.value = !errorText.value;
  }
}
const isEggCracked = ref(false);  // Example condition

// Function to return the image source based on the condition
const eggImageSrc = computed(() => {
  return isEggCracked.value ? '/public/funny_egg2.png' : '/public/funny_egg.png';
});

</script>
<template>
  <img class="eggImg" src="/public/funny_egg.png"  alt="egg" />
    <h1 class="timerTitle">Egg Timer</h1>
  <h1 class="time">{{ formattedTime }}</h1>
    <div id="buttons" >
      <button @click="startTimer(60)" class="btn">1 Min.</button>
      <button @click="startTimer(3 * 60)" class="btn">3 Min.</button>
      <button @click="startTimer(5 * 60)" class="btn">5 Min.</button>
    </div>
  <input
      class="mt-3 form-control"
      v-model="customTime"
      :placeholder="errorText ? errorMessage : 'Time (s)'"
  />
  <button
      class="btn mt-3" style="width: 8.5rem" v-if="timerToggled" @click="startTimer(customTime);">Set</button>
  <button class="btn mt-3" style="width: 8.5rem; background-color: #ff4216" v-else @click="; toggleButton()">Stop</button>
</template>
<style>

.form-control{
  width: 8.5rem;
  font-size: 20px;
  border: 2px solid #ff8800;
}

body{
  background-color: #fff1dc;
  font-family: "Jersey 10";
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
}

.time{
  color: #2c2c2c;
  font-size: 3rem;
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

.timerTitle {
  color: #ff8800;
  font-size: 4rem;
}
</style>
