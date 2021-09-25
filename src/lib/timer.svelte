<script>
import { onMount } from 'svelte';

let myAudio;
let time = 1;
let recoveryTime = 1;

let completedPomodoros = 0;
let interval;
let increaseTime = () => time += 0.5    
let decreaseTime= () => time -= 0.5
let increaseRecoveryTime = () => recoveryTime += 0.5
let decreaseRecoveryTime = () => recoveryTime -= 0.5
let increaseRuns = () => RUNS += 1
let decreaseRuns = () => RUNS -= 1

const minutesToSeconds = (minutes) => minutes * 60;
const secondsToMinutes = (seconds) => Math.floor(seconds / 60);
const padWithZeroes = (number) => number.toString().padStart(2, '0');
const State = {idle: 'idle', inProgress: 'in progress', resting: 'resting'};
let currentState = State.idle;

$: exercise_time = minutesToSeconds(time);
$: break_time = minutesToSeconds(recoveryTime);
$: RUNS = 1;
$: pomodoroTime = exercise_time;

function formatTime(timeInSeconds) { 
  const minutes = secondsToMinutes(timeInSeconds);
  const remainingSeconds = timeInSeconds % 60;
  return `${padWithZeroes(minutes)}:${padWithZeroes(remainingSeconds)}`;
}

function startPomodoro() {       
  setState(State.inProgress);
  interval = setInterval(() => {
    if (pomodoroTime === 4) {
          myAudio.play();
    }
    if (pomodoroTime === 0) {
          completePomodoro();
    }
        pomodoroTime -= 1;
  },1000);
}

    function setState(newState){
      clearInterval(interval)
      currentState = newState;
    }

    function completePomodoro(){
      completedPomodoros++;
      if (completedPomodoros === RUNS) {
        
        completedPomodoros = 0;
        pomodoroTime = exercise_time;
        idle();
        
      } else {
        
        rest(break_time);
      }
    }

    function rest(time){
      setState(State.resting);
      pomodoroTime = time;
      interval = setInterval(() => {
        if (pomodoroTime === 4) {
          myAudio.play();
        }
        if (pomodoroTime === 0) {
            console.log("start new tabata")
            pomodoroTime = time;
          startPomodoro();
        }
        pomodoroTime -= 1;
      },1000);
    }

    function cancelPomodoro() {      
      idle();
    }

    function idle(){
      setState(State.idle);
      pomodoroTime = exercise_time;
    }
 

onMount(() => {
  myAudio = document.createElement("audio");      
  myAudio.src = "countdown.wav";      
});

</script>
  
  <style>
    time {
      display: block;
      font-size: 5em;
      font-weight: 300;
      margin-bottom: 0.2em;
    }
    .active {
      color: green;
    }
    .resting {
      color: orange;
    }
    input {
      width: 5rem;
      text-align:center;
    }  
    /* Chrome, Safari, Edge, Opera */
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }

    /* Firefox */
    input[type=number] {
      -moz-appearance: textfield;
    }
  </style>
  
  <section>
    <div>
      <button on:click="{decreaseTime}" disabled={time === 0}>➖</button>
      <input type=number bind:value={time} min=0>
      <button on:click="{increaseTime}">➕</button>
    </div>
    <div>
      <button on:click="{decreaseRecoveryTime}" disabled={recoveryTime === 0}>➖</button>
      <input type=number bind:value={recoveryTime} min=0>
      <button on:click="{increaseRecoveryTime}">➕</button>
    </div>
    <div>
      <button on:click="{decreaseRuns}" disabled={RUNS === 0}>➖</button>
      <input type=number bind:value={RUNS} min=1>   
      <button on:click="{increaseRuns}">➕</button>
    </div>    
    <time class:active={currentState === State.inProgress} class:resting={currentState === State.resting}>      
      {formatTime(pomodoroTime)}
    </time>    
    <footer>
      <button class="primary" on:click={startPomodoro} disabled={currentState !== State.idle || time === 0 || recoveryTime === 0 || RUNS === 0}>start</button>
      <button on:click={cancelPomodoro} disabled={currentState !== State.inProgress && currentState !== State.resting}>cancel</button>
    </footer>
  </section>



