<template>
  <div id="app">
    <div id="particle-js"></div>
    <b-container>
      <b-row>
        <b-col class="bg" cols="12">
          <br />
          <h1>{{ ing.length == 0 ? "" : ing }}:{{ time }}</h1>
          <br />
          <h1>
            <font-awesome-icon
              icon="pause"
              v-if="status === 'play'"
              @click="pause()"
            />
            <font-awesome-icon icon="play" v-else @click="play()" />
          </h1>
        </b-col>
        <b-col cols="12">
          <h1>待辦事項</h1>
          <b-form-input
            v-model="newtodo"
            placeholder="請輸入待辦事項"
            @keydown.enter="add()"
          ></b-form-input>
          <ul>
            <li v-for="(t, idx) in todo" :key="idx">
              <h1>{{ t }}</h1>
              <b-btn variant="outline-danger" @click="remove('todo', idx)"
                >X</b-btn
              >
            </li>
          </ul>
        </b-col>
        <b-col cols="12">
          <h1>已完成事項</h1>
          <ul>
            <li v-for="(t, idx) in finish" :key="idx">
              <h1>{{ t }}</h1>
              <b-btn variant="outline-danger" @click="remove('finish', idx)"
                >X</b-btn
              >
            </li>
          </ul>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import "particles.js/particles";
var timer = 0;
const time = 5;
export default {
  name: "app",
  data() {
    return {
      newtodo: "",
      todo: [],
      status: "",
      ing: "",
      time: 5,
      finish: []
    };
  },
  methods: {
    add() {
      this.todo.push(this.newtodo);
    },
    remove(type, idx) {
      if (type == "todo") this.todo.splice(idx, 1);
      else this.finish.splice(idx, 1);
    },
    play() {
      if (this.status != "pause") {
        if (this.todo.length < 1) return;
        this.ing = this.todo[0];
        this.todo.splice(0, 1);
      }
      this.status = "play";
      timer = setInterval(() => {
        this.time--;
        if (this.time < 1) {
          clearInterval(timer);
          this.status = "";
          this.finish.push(this.ing);
          this.ing = "";
          this.time = time;
          this.play();
        }
      }, 1000);
    },
    pause() {
      this.status = "pause";
      clearInterval(timer);
    },
    initParticlesJS() {
      /* eslint-disable */
      particlesJS.load(
        "particle-js",
        "particlesjs-config.json",
        function() {
          console.log("callback - particles.js config loaded");
        }
      );
    }
  },
  watch: {
    todo(value) {
      let todo = "";
      for (let i = 0; i < value.length; i++) {
        todo += value[i] + ",";
      }
      if (todo.slice(-1) == ",") todo = todo.slice(0, -1);
      window.localStorage.setItem("todo", todo);
    }
  },
  mounted() {
    require("particles.js");
    this.$nextTick(() => {
      this.initParticlesJS();
    });
    let todo = window.localStorage.getItem("todo");
    this.todo = todo.split(",");
  }
};
</script>

<style>

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif, Microsoft JhengHei;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
}
#particle-js {
  z-index: -1;
  position: fixed;
  height: 100vh;
  width: 100vw;
  background: black;
}
</style>
