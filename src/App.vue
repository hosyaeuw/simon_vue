<template>
  <div class="contrainer">
    <div class="circle">
      <CircleChunk
        :ref="colors[idx - 1].value"
        @click="cl(colors[idx - 1])"
        :color="colors[idx - 1].value"
        v-for="idx in 4"
        :key="idx"
      />
    </div>
    <div class="menu">
      <div class="menu__round">Round {{ roundNumber }}</div>
      <button @click="gameStart" class="menu__btn">Start</button>
      <div class="menu__radio_btns">
        <label>
          <input :disabled="isRoundStart" ref="radio_easy" type="radio" name="difficulty" value="1500" checked>
          Easy
        </label>
        <label>
          <input :disabled="isRoundStart" ref="radio_medium" type="radio" name="difficulty" value="1000">
          Medium
        </label>
        <label>
          <input :disabled="isRoundStart" ref="radio_hard" type="radio" name="difficulty" value="400">
          Hard
        </label>
      </div>
    </div>
  </div>
</template>

<script>
import CircleChunk from './components/CircleChunk'

export default {
  name: 'App',
  data: () => (
    {
      colors: [
        {
          value: 'red',
          audio: require('./assets/1.ogg')
        },
        {
          value: 'blue',
          audio: require('./assets/2.ogg')
        },
        {
          value: 'green',
          audio: require('./assets/3.ogg')
        },
        {
          value: 'yellow',
          audio: require('./assets/4.ogg')
        }
      ],
      round: [],
      roundNumber: 1,
      userClickCount: 0,
      isRoundStart: false,
      isGameStart: false,
      duration: 0
    }
  ),
  methods: {
    cl: function (colorObj) {
      const audio = new Audio(colorObj.audio)
      audio.play()
      if (this.isGameStart) {
        if (this.round[this.userClickCount] === colorObj.value) {
          this.userClickCount += 1
          if (this.userClickCount === this.round.length) {
            this.roundNumber++
            this.userClickCount = 0
            setTimeout(this.start, 1500)
          }
        } else {
          alert('Проиграл')
          this.roundNumber = 1
          this.isGameStart = false
        }
      }
    },
    start: function () {
      this.round = []
      const duration = this.duration
      for (let i = 0; i < this.roundNumber + 1; i++) {
        const color = this.colors[Math.floor(Math.random() * this.colors.length)].value
        this.round.push(color)
        const elem = this.$refs[color].$el
        setTimeout(function () {
          elem.classList.add('active')
          setTimeout(function () {
            elem.classList.remove('active')
          }, duration / 2)
        }, duration * i)
      }
    },
    gameStart: function () {
      this.roundNumber = 1
      this.duration = +[
        this.$refs.radio_easy,
        this.$refs.radio_medium,
        this.$refs.radio_hard].filter(elem => elem.checked)[0].value
      this.isGameStart = true
      this.start()
    }
  },
  components: {
    CircleChunk
  }
}
</script>

<style>
  #app{
    height: 100%;
  }

  * {
    padding: 0;
    margin: 0;
    list-style: none;
    outline: none;
    box-sizing: border-box;
  }

  html {
    -ms-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%;
    height: 100%;
  }

  body {
    height: 100%;
    -moz-osx-font-smoothing: grayscale;
    -webkit-font-smoothing: antialiased;
    color: black;
    background: #eff3f6f8;
    font-family: "Noto Sans", Arial;
  }

  .contrainer{
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .menu{
    text-align: center;
  }

  .menu .menu__round{
    margin-bottom: 32px;
  }

  .menu .menu__btn{
    padding: 12px 36px;
    border-radius: 15px;
    border: 2px solid black;
    cursor: pointer;
    margin-bottom: 32px;
  }

  .menu .menu__radio_btns{
    text-align: left;
    height: 100px;
    display: flex;
    justify-content: space-around;
    flex-direction: column;
  }

  .circle{
    display: flex;
    flex-wrap: wrap;
    width: 400px;
    height: 400px;
    margin-right: 100px;
  }

  @media only screen and (min-device-width : 320px) and (max-device-width : 480px) {
  .contrainer {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .circle{
    width: 240px;
    height: 240px;
    margin-right: 0;
    margin-bottom: 52px;
  }
}
</style>
