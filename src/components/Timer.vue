<template>
  <div class="center">
    <div class="time-container">
      <span class="head-timer">
        {{
          history[history.length - 1] === "work"
            ? "Time to focus"
            : history.length === 0
            ? "Pomodoro"
            : "Take a break"
        }}
      </span>
      <h1>
        {{ minutes < 10 ? 0 : "" }}{{ minutes }} : {{ seconds < 10 ? 0 : ""
        }}{{ seconds }}
      </h1>
      <button v-if="!isStarted" @click="startMethod">
        <span>Start</span>
      </button>
      <button v-if="isStarted" @click="stopMethod">
        <span>Stop</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TimerVue",
  data() {
    return {
      remainTime: 0,
      seconds: 0,
      minutes: 0,
      history: [],
      isStarted: false,
      countdownFnc: null,
    };
  },
  methods: {
    countDown() {
      this.countdownFnc = setInterval(() => {
        if (this.remainTime > 0) {
          this.remainTime = this.remainTime - 1;
          this.minutes = Math.floor(this.remainTime / 60);
          this.seconds = this.remainTime - this.minutes * 60;
        } else {
          this.updateStep();
        }
      }, 1000);
    },
    startMethod() {
      this.isStarted = true;
      this.history.push("work");
      this.remainTime = 15;
      this.countDown();
    },
    stopMethod() {
      this.isStarted = false;
      clearInterval(this.countdownFnc);
      this.history = [];
      this.remainTime = 0;
      this.seconds = 0;
      this.minutes = 0;
    },
    updateStep() {
      let textNotif = "";
      let iconNotif = "";
      if (this.history[this.history.length - 1] === "work") {
        console.log(
          this.history,
          this.history.slice(this.history.length - 8, this.history.length - 1)
        );
        if (
          this.history
            .slice(this.history.length - 8, this.history.length - 1)
            .filter((el) => el === "work").length === 4
        ) {
          textNotif = "Take a long break";
          this.history.push("long_break");
          this.remainTime = 10;
        } else {
          textNotif = "Take a short break";
          this.history.push("short_break");
          this.remainTime = 5;
        }
      } else if (
        this.history[this.history.length - 1] ===
        ("short_break" || "long_break")
      ) {
        textNotif = "Return to work";
        this.history.push("work");
        this.remainTime = 15;
      }
      new Notification("Pomodoro", { textNotif, iconNotif });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  font-size: 5rem;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
button {
  width: 125px;
  height: 50px;
  background-color: #2c3e50;
  border-color: #fff;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  font-size: 20px;
}
button:hover {
  transform: scale(1.025);
  transition: all ease-in-out 0.25s;
}
.center {
  display: flex;
  justify-content: center;
}
.time-container {
  padding: 20px;
  width: 400px;
  background: none;
  border: 1px solid #2c3e50;
  border-radius: 5px;
}
.head-timer {
  font-size: 2rem;
}
</style>
