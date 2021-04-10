<template>
  <div class="traffic-light">
    <div class="timer">Осталось <span style="font-weight: 600">{{ leftTime }}</span> секунд</div>
    <div
      v-for="item in lights"
      :class="{
        circle: true,
        [item.name]: activeLight.name === item.name,
        'circle-change': (activeLight.name === 'green' && item.name === 'green') && leftTime <= 3
      }"
    />
    <div class="buttons">
      <button @click="saveState">Сохранить состояние</button>
      <button @click="clearState">Очистить</button>
    </div>
    <div v-if="state">
      <p>Текущее состояние: {{ state }}</p>
      <p>Оно будет использоваться каждый раз независимо от пути, до момента очистки</p>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      const lights = [
        {
          name: 'red',
          timing: 4
        },
        {
          name: 'yellow',
          timing: 3
        },
        {
          name: 'green',
          timing: 5
        }
      ];

      const name = document.location.pathname.replace('/', '');
      const state = localStorage.getItem('trafficLight') ? JSON.parse(localStorage.getItem('trafficLight')) : '';

      let activeLight = name ? lights[lights.map((item) => item.name).indexOf(name)] : lights[0];
      let timer = 0

      if (state) {
        activeLight = state.activeLight;
        timer = state.timer;
      }

      return {
        lights,
        activeLight,
        timer,
        state
      }
    },
    methods: {
      saveState() {
        const { timer, activeLight } = this;
        const data = JSON.stringify({
          timer,
          activeLight
        });

        localStorage.setItem('trafficLight', data);

        this.state = data;
      },
      clearState() {
        localStorage.setItem('trafficLight', '');

        this.state = '';
      }
    },
    computed: {
      leftTime() {
        return this.activeLight.timing - this.timer;
      }
    },
    mounted() {
      setInterval(() => {
        const { lights, activeLight } = this;

        this.timer++;

        // If timer seconds = light timing, change light to next
        if (this.timer === activeLight.timing) {
          // Index of current light
          const index = lights.map((item) => item.name).indexOf(activeLight.name);

          // Change active light
          this.activeLight = lights[index < (lights.length - 1) ? (index + 1) : 0];

          // Clear timer
          this.timer = 0;

          history.pushState(null, null, '/' + this.activeLight.name);
        }
      }, 1000);
    }
  }
</script>

<style lang="scss" scoped>
  .traffic-light {
    margin: 0 auto;
    padding: 10px;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .timer {
      margin-bottom: 20px;
    }

    .buttons {
      margin-top: 20px;
    }

    .circle {
      width: 100px;
      height: 100px;

      border-radius: 50%;

      background-color: #35495e;
    }

    .circle-change {
      animation: blink .5s infinite;
    }

    .red {
      background-color: red;
    }

    .yellow {
      background-color: gold;
    }

    .green {
      background-color: green;
    }
  }

  @keyframes blink {
    0% { opacity: 1.0; }
    100% { opacity: 0.0; }
    0% { opacity: 1.0; }
  }
</style>
