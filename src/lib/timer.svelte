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
    <input type=number bind:value={time} min=0>
    <input type=number bind:value={recoveryTime} min=0>
    <input type=number bind:value={RUNS} min=1>        
    <time>
      {formatTime(pomodoroTime)}
    </time>
    <footer>
      <button class="primary" on:click={startPomodoro} disabled={currentState !== State.idle}>start</button>
      <button on:click={cancelPomodoro} disabled={currentState !== State.inProgress}>cancel</button>
    </footer>
  </section>