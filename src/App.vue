<template lang='pug'>
  #app( @click='start' )
    .background
      .screen-outter
      .screen-inner
        .screen-overlay
        .screen
          h2.test-heading(v-if='!testOpen && !resultOpen') programming language test
          p.test-start(v-if='startOpen') press enter to start test
          span.test-team pxlhead tm
          .loading(v-if='loadingOpen')
            span.loader-heading Loading
            span.loader-bar {{ `[#######........]` }}
            span.loader-percent {{ `0%` }}
          .test(v-if='testOpen')
            h3.test-question {{ test.question }}
            .answers
              span.test-answer(v-for='(n, index) in test.answer' v-bind:class='index === answerActive - 1 ? "answer--active" : "" ') {{ `&#62; ${test.answer[index]}` }}
          .result(v-if='resultOpen')
            span.code(v-for='(n, index) in code') {{ `${code[index]} [0%]` }}
            .result-message(v-if='showResult')
              h3.result-text {{ `We decide you to dive into &#60;${result[3]}&#62; world` }}
              span.result-notification Press enter to restart
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      startOpen: true,
      loadingOpen: false,
      testOpen: false,
      resultOpen: false,
      showResult: false,
      answerActive: 1,
      test: { question: 'How many roads does the man go down?', answer: [42, 56, 74, 102] },
      code: ['Collecting you answers...', 'Compare answers with PL matrix...', 'Testing you patience...', 'Analizing...'],
      result: ['Python', 'JS', 'Java', 'BeaverScript'],
    }
  },
  methods: {
    start() {
      this.startOpen = false;
      this.loadingOpen = true;
    },
    loading() {
      this.loadingOpen = false;
      this.testOpen = true;
    },
    chooseAnswer() {
      this.answerActive++
      console.log(this.answerActive)
    },
    timer() {
      return 0;
      // let i = 1;
      // const timerId = setInterval(function() {
      //   return i;
      //   if (i == 20) clearInterval(timerId);
      //   i++;
      // }, 100);
    }
  }
}
</script>

<style lang='scss'>
@import url('https://fonts.googleapis.com/css?family=VT323');

$green: #1FF042;

html {
  font-size: 10px;
}
body {
  font-family: 'VT323', monospace;
  color: $green;
  margin: 0;
  padding: 0;
}
.background {
  position: relative;
  width: 100%;
  height: 100vh;
  background-color: #141814;
}
.screen-outter {
  position: absolute;
  top: 4%;
  left: calc(50% - 100rem / 2);
  width: 90%;
  max-width: 100rem;
  height: 92%;
  background-color: #cfc19a;
  border-radius: 4rem;
  border: 4px solid #2A2A2A;
}
.screen-inner {
  position: absolute;
  top: 8%;
  left: calc(50% - 90rem / 2);
  width: 90%;
  max-width: 90rem;
  height: 84%;
  background-color: #b9ab84;
  border-radius: 8rem;
}
.screen {
  position: absolute;
  top: 5%;
  left: 5%;
  width: 90%;
  height: 90%;
  border-radius: 50% / 4rem;
  background-color: #222;
  box-shadow: inset 0 0 10em 1em rgba(0, 0, 0, 0.5);
  border: 2px solid transparentize(#071007, 0.9);
}
.screen-overlay {
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
    background-image: linear-gradient(0deg, transparent 0%, rgba(32,128,32,0.2) 2%, rgba(32,128,32,0.8) 3%, rgba(32,128,32,0.2) 3%, transparent 100%);
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
    background-image: radial-gradient(ellipse 50% 15% at 50% 15%, rgba(255, 255, 255, 0.05), transparent), radial-gradient(ellipse 50% 10% at 50% 12%, rgba(255, 255, 255, 0.1), transparent), radial-gradient(ellipse 50% 5% at 50% 10%, rgba(255, 255, 255, 0.1), transparent), radial-gradient(ellipse 50% 3% at 50% 9%, rgba(255, 255, 255, 0.1), transparent), radial-gradient(ellipse 200% 20% at 50% 0%, rgba(0, 0, 0, 0.5), transparent), linear-gradient(0deg, rgba(0, 0, 0, 0.2) 50%, transparent 50%);
    background-size: 100%, 100%, 100%, 100%, 100%, 100% 0.25ch;
  }
}
.test-heading {
  position: absolute;
  top: 20%;
  left: calc(50% - 90% / 2);
  text-transform: uppercase;
  width: 90%;
  font-size: 7rem;
  text-align: center;
}
.test-start {
  position: absolute;
  top: 60%;
  left: calc(50% - 40% / 2);
  width: 40%;
  text-transform: uppercase;
  font-size: 30px;
  text-align: center;
  animation: blink 2s steps(1) infinite
}
.test-team {
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
  font-size: 3rem;
}
.test-question {
  font-size: 3rem;
}
.code {
  font-size: 2rem;
}
.result-text {
  font-size: 3rem;
}
.result-notification {
  font-size: 2rem;
  animation: blink 2s steps(1) infinite
}
.answer--active {
  background-color: transparentize($green, 0.8);
}
.answers {
  display: flex;
  flex-direction: column;
}
.loading {
  position: absolute;
  bottom: 10rem;
  left: 10%;
  width: 60%;
}
.loader-heading, .loader-percent {
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
