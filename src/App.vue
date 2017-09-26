<template lang='pug'>
  #app
    .adviser
      h3.label-start Hi, @USER
      a.btn-start(@click='showModal = true') start
      transition(name='slideUp' appear)
        form.form(v-if='showModal')
          h2.form-heading Awesome Question (as always). We have something to say... Stay tuned and JAZZUP!
          input.form-answer(type='radio'  v-for='n in 3')
          label.fotm-label(for='')
          btn.form-submit(type='submit')
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      showModal: false
    }
  }
}
</script>

<style lang='scss'>
@import url('https://fonts.googleapis.com/css?family=Roboto:400');


* {
  box-sizing: border-box;
}
html {
  font-size: 10px;
  overflow: auto;
  font-family: 'Roboto', sans-serif;
}
body {
  margin: 0;
  padding: 0;
}
.adviser {
  position: relative;
  overflow: hidden;
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  background-color: #6E97C4;
}
.btn-start {
  position: absolute;
  display: block;
  width: 18rem;
  height: 6rem;
  font-size: 2rem;
  line-height: 6rem;
  color: #F4F4F4;
  text-transform: uppercase;
  text-align: center;
  cursor: pointer;
  background-color: #FC3164;
  top: calc(60% - 6rem / 2);
  left: calc(50% - 18rem / 2);
  border-radius: 4rem;
  transition: all 0.3s ease-in-out;
  &:hover {
    background-color: darken(#FC3164, 10%);
    transition: all 0.3s ease-in-out;
  }
}
.label-start {
  position: absolute;
  display: block;
  width: 30rem;
  height: 6rem;
  font-size: 6rem;
  line-height: 6rem;
  color: #F4F4F4;
  text-transform: uppercase;
  text-align: center;
  top: calc(40% - 6rem / 2);
  left: calc(50% - 30rem / 2);
  transition: all 0.3s ease-in-out;
}
.form {
  position: absolute;
  height: 90%;
  width: 80rem;
  bottom: 0;
  left: calc(50% - 80rem / 2);
  background-color: #F4F4F4;
  border-radius: 0.5rem;
  padding: 5% 5%;
  box-shadow: 0px 2px 20px rgba(0, 0, 0, 0.25);
}
.form-heading {
  width: 100%;
  font-size: 4rem;
  font-weight: normal;
  line-height: 5rem;
  color: #142643;
  text-align: center;
  margin-bottom: 10%;
}
.input-answer {

}
.slideUp-enter-active, .slideUp-leave-active {
  transition: all .5s ease-in-out
}
.slideUp-enter, .slideUp-leave-to {
  transform: translateY(100%)
}
</style>
