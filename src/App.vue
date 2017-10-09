<template lang='pug'>
  #app.screen
    .screen-outer
      .screen-glare
      .screen-inner
        .start(v-if='phase == 0')
          h1.start-title programming language test
          p.start-notification(v-if='!loading') press enter to start test
          span.team pxlhead tm
          .loader(v-if='loading')
            span.loader-title Loading
            span.loader-bar {{ loaderBar }}
        .test(v-if='phase == 1')
          h3.test-question {{ activeTest.question }}
          .test-answers
            span.answer(v-for='(answer, index) in activeTest.answers'
              :class='{ "answer--active": index == activeAnswer }'
              @click='activeAnswer = index') {{ answer }}
        .result(v-if='phase == 2')
          .loader(v-if='loading')
            span.loader-title {{ loaderText }}
          .result-message(v-else)
            h3.result-text Try not. Do {{ result }} – or do not {{ result }}. There is no try.
            span.result-notification Enter press to restart and be mindful.
</template>

<script>
import testData from './testData'

export default {
  name: 'app',

  data() {
    return {
      phase: 0,
      loading: false,
      loaderBar: '',
      loaderPhrases: [
        'Collecting your answers...',
        'Comparing answers with PL matrix...',
        'Testing your patience...',
        'Analizing...'
      ],
      activePhrase: 0,
      activeAnswer: 0,
      answersStack: [],
      result: ''
    }
  },

  mounted() {
    document.addEventListener('keydown', this.onKeydown)
  },

  computed: {
    activeTest() {
      let result = testData
      this.answersStack.forEach(answer => {
        result = result[answer]
      })
      if (typeof result !== 'string') return result
      this.showResult(result)
      return {}
    },
    loaderText() {
      if (this.activePhrase == -1) return
      return this.loaderPhrases[this.activePhrase]
    }
  },

  methods: {
    showResult(result) {
      this.phase = 2
      this.loading = true
      this.result = result

      const timerId = setInterval(() => this.activePhrase++, 1500)
      setTimeout(() => {
        clearInterval(timerId)
        this.loading = false
        this.activePhrase = 0
      }, this.loaderPhrases.length * 1500)
    },
    onKeydown(e) {
      if (this.phase == 0 && e.keyCode == 13) {
        this.loading = true
        const timerId = setInterval(() => this.loaderBar += '#', 500)
        setTimeout(() => {
          clearInterval(timerId)
          this.loading = false
          this.phase = 1
        }, 3000)
      } else if (this.phase == 1) {
        if (e.keyCode == 13) {
          this.answersStack.push(this.activeAnswer)
          this.activeAnswer = 0
        } else if (e.keyCode == 38) {
          this.activeAnswer = this.activeAnswer > 0
          ? this.activeAnswer - 1
          : this.activeTest.answers.length - 1
        } else if (e.keyCode == 40) {
          this.activeAnswer = this.activeAnswer < this.activeTest.answers.length - 1
          ? this.activeAnswer + 1
          : 0
        }
      } else if (e.keyCode == 13) {
        this.phase = 0
        this.answersStack = []
        this.loaderBar = ''
      }
    }
  }
}
</script>

<style lang='scss'>
@import url('https://fonts.googleapis.com/css?family=VT323');

html {
  font-size: 10px;
}
body {
  font-family: 'VT323', monospace;
  color: #1FF042;
  margin: 0;
  padding: 0;
}
.screen {
  position: relative;
  width: 100%;
  height: 100vh;
  background-color: #141814;
}
.screen-outer {
  position: absolute;
  top: 5%;
  left: calc(50% - 120rem / 2);
  width: 90%;
  max-width: 120rem;
  height: 90%;
  background-color: #C9D9D3;
  border-radius: 50% / 4rem;
  border: 4px solid #2A2A2A;
}
.screen-inner {
  position: absolute;
  top: 7.5%;
  left: 7.5%;
  width: 85%;
  height: 85%;
  border-radius: 50% / 4rem;
  background-color: #222;
  box-shadow: inset 0 0 10em 1em rgba(0, 0, 0, 0.5);
  border: 2px solid transparentize(#071007, 0.9);
}
.screen-glare {
  position: absolute;
  top: 5%;
  left: 5%;
  width: 90%;
  height: 90%;
  border-radius: 50% / 4rem;
  opacity: 0.4;
  z-index: 1000;
  background: radial-gradient(944.09px at 50% 0%, #102710 0%, rgba(13, 17, 13, 0.23) 100%);
  &::before {
    position: absolute;
    content: '';
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 50% / 4rem;
    z-index: 1001;
    background-image: linear-gradient(0deg,
                      transparent 0%,
                      rgba(32,128,32,0.2) 2%,
                      rgba(32,128,32,0.8) 3%,
                      rgba(32,128,32,0.2) 3%,
                      transparent 100%);
    background-repeat: no-repeat;
    animation: flash 7.5s linear 0s infinite;
  }
  &::after {
    position: absolute;
    content: '';
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 50% / 4rem;
    z-index: 1001;
    background-image: radial-gradient(ellipse 50% 15% at 50% 15%, rgba(255, 255, 255, 0.05), transparent),
                      radial-gradient(ellipse 50% 10% at 50% 12%, rgba(255, 255, 255, 0.1), transparent),
                      radial-gradient(ellipse 50% 5% at 50% 10%, rgba(255, 255, 255, 0.1), transparent),
                      radial-gradient(ellipse 50% 3% at 50% 9%, rgba(255, 255, 255, 0.1), transparent),
                      radial-gradient(ellipse 200% 20% at 50% 0%, rgba(0, 0, 0, 0.5), transparent),
                      linear-gradient(0deg, rgba(0, 0, 0, 0.2) 50%, transparent 50%);
    background-size: 100%, 100%, 100%, 100%, 100%, 100% 0.25ch;
  }
}
.start-title {
  position: absolute;
  top: 20%;
  left: calc(50% - 90% / 2);
  text-transform: uppercase;
  width: 90%;
  font-size: 7rem;
  text-align: center;
}
.start-notification {
  position: absolute;
  top: 60%;
  left: calc(50% - 40% / 2);
  width: 40%;
  text-transform: uppercase;
  font-size: 30px;
  text-align: center;
  animation: blink 2s steps(1) infinite
}
.team {
  position: absolute;
  text-transform: uppercase;
  bottom: 4rem;
  right: 10%;
  font-size: 2rem;
}
.test {
  position: relative;
  padding: 10%;
  width: 80%;
  height: 70%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.result {
  position: relative;
  padding: 10%;
  width: 80%;
  height: 70%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.test-question {
  font-size: 5rem;
}
.result-text {
  font-size: 4rem;
}
.result-notification {
  font-size: 3rem;
  animation: blink 2s steps(1) infinite
}
.answer--active {
  background-color: transparentize(#1FF042, 0.8);
}
.test-answers {
  font-size: 3rem;
  display: flex;
  flex-direction: column;
}
.answer {
  font-size: 3.5rem;
}
.loader {
  position: absolute;
  bottom: 10rem;
  left: 10%;
  width: 80%;
}
.loader-title {
  font-size: 4rem;
  margin: 0 1rem;
}
.loader-bar {
  font-size: 3rem;
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
}

@keyframes flash {
  0% { background-position: 0 -100vh; }
  50%,100% { background-position: 0 100vh; }
}
</style>
