<script>
    let time = 1;
    let recoveryTime = 1;
    
    const minutesToSeconds = (minutes) => minutes * 60;
    const secondsToMinutes = (seconds) => Math.floor(seconds / 60);
    const padWithZeroes = (number) => number.toString().padStart(2, '0');
    const State = {idle: 'idle', inProgress: 'in progress', resting: 'resting'};

    $: POMODORO_S = minutesToSeconds(time);
    $: SHORT_BREAK_S = minutesToSeconds(recoveryTime);
    $: RUNS = 1;
    $: console.log(`RUNS ${RUNS}`);
    
    
    const LONG_BREAK_S = minutesToSeconds(5);
    
    let currentState = State.idle;
    $: pomodoroTime = POMODORO_S;
    $: shortBreakTime = SHORT_BREAK_S;
    $: console.log(`pomodoroTime ${pomodoroTime}`);
    let completedPomodoros = 0;
    let interval;

    function formatTime(timeInSeconds) { 
      const minutes = secondsToMinutes(timeInSeconds);
      const remainingSeconds = timeInSeconds % 60;
      return `${padWithZeroes(minutes)}:${padWithZeroes(remainingSeconds)}`;
    }

    function startPomodoro() { 
      setState(State.inProgress);
      interval = setInterval(() => {
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
        //rest(LONG_BREAK_S);
        console.log("DONE MA DUDE")
        completedPomodoros = 0;
        idle();
      } else {
        rest(SHORT_BREAK_S);
      }
    }

    function rest(time){
      setState(State.resting);
      pomodoroTime = time;

      interval = setInterval(() => {
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
      pomodoroTime = POMODORO_S;
    }
    let increaseTime = () => time += 0.5    
    let decreaseTime= () => time -= 0.5
    let increaseRecoveryTime = () => recoveryTime += 0.5
    let decreaseRecoveryTime = () => recoveryTime -= 0.5
    let increaseRuns = () => RUNS += 1
    let decreaseRuns = () => RUNS -= 1


  </script>
  
  <style>
    time {
      display: block;
      font-size: 5em;
      font-weight: 300;
      margin-bottom: 0.2em;
    }
  </style>
  
  <section>
    <div>
      <button on:click="{decreaseTime}">➖</button>
      <input type=number bind:value={time} min=0>
      <button on:click="{increaseTime}">➕</button>
    </div>
    <div>
      <button on:click="{decreaseRecoveryTime}">➖</button>
      <input type=number bind:value={recoveryTime} min=0>
      <button on:click="{increaseRecoveryTime}">➕</button>
    </div>
    <div>
      <button on:click="{decreaseRuns}">➖</button>
      <input type=number bind:value={RUNS} min=1>   
      <button on:click="{increaseRuns}">➕</button>
    </div>
    <time>
      <p>Active Time</p>
      {formatTime(pomodoroTime)}
    </time>
    <time>
      <p>Recovery Time</p>
      {formatTime(shortBreakTime)}
    </time>
    <p>No. of Runs:{RUNS}</p>
    <footer>
      <button class="primary" on:click={startPomodoro} disabled={currentState !== State.idle}>start</button>
      <button on:click={cancelPomodoro} disabled={currentState !== State.inProgress}>cancel</button>
    </footer>
  </section>



