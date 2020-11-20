<template>
  <div id="app" :class="{started}">

    <button @click="start" id="roll" class="btn">
      <Sync />
    </button>

    <button @click="changeMode" id="mode" class="btn">
      <Plus v-if="mode === 'plus'" />
      <Minus v-if="mode === 'minus'" />
      <Times v-if="mode === 'multi'" />
    </button>

    <section>
      <div class="grid" v-if="started && progress < 100">
        <div class="top">{{a}}</div>
        <div class="top">{{sign}}</div>
        <div class="top">{{b}}</div>

        <div class="full center">=</div>

        <div class="full bottom">
          <input v-model="res">
        </div>
      </div>

      <div class="win" v-if="started && progress === 100">
        <img :src="gif()" alt="Victoire !" />
      </div>
    </section>

    <div class="progress" v-if="progress < 100">
      <div :style="`width:${progress}%`"/>
    </div>

  </div>
</template>

<style src="./style.scss" lang="scss" />

<script>
import Sync from './icon/sync-alt-duotone.svg'
import Plus from './icon/plus-solid.svg'
import Minus from './icon/minus-solid.svg'
import Times from './icon/times-solid.svg'

export default {
  name: 'App',

  components:{
    Sync,
    Plus,
    Minus,
    Times
  },

  mounted(){
    this.start()
    /*const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

    // new speech recognition object
    this.recognition = new SpeechRecognition()
    this.recognition.lang = 'fr-FR'
    this.recognition.continuous = true

    // This will run when the speech recognition service returns a result
    this.recognition.onstart = function() {
      console.log("Voice recognition started. Try speaking into the microphone.")
    }

    this.recognition.onresult = this.onResult
    this.recognition.start()*/
  },

  data: () => ({
    started: false,
    score: 0,
    max: 5,
    a: 1,
    b: 1,
    mode: window.localStorage.getItem('mode') || 'plus',
    res: ''
  }),

  computed:{
    solution(){
      if(this.mode === 'minus') return this.a - this.b
      if(this.mode === 'multi') return this.a * this.b
      return this.a + this.b
    },

    sign(){
      if(this.mode === 'minus') return '-'
      if(this.mode === 'multi') return 'x'
      return '+'
    },

    progress(){
      return Math.round((this.score / this.max) * 100)
    },
  },

  watch:{
    res(next){
      let result = parseInt(next, 10)
      const good = this.solution === result

      if(!good) return

      this.goodSound()
      this.updateScore(1)

      setTimeout(() => {
        this.res = ''
        this.roll()
      }, 500)
    },

    mode(next){
      window.localStorage.setItem('mode', next)
    }
  },

  methods: {
    start(){
      this.started = true
      this.score = 0
      this.roll()
    },

    changeMode(){
      if(this.mode === 'plus'){
        this.mode = 'minus'
      }else
      if(this.mode === 'minus'){
        this.mode = 'multi'
      }else{
        this.mode = 'plus'
      }

      this.start()
    },

    roll: function(){
      let a = this.random(1, 10)
      let b = this.random(1, 10)

      if(this.mode === 'minus' && a < b) [a, b] = [b, a]

      this.a = a
      this.b = b
    },

    random(min, max){
      return Math.round(min + Math.random() * (max - min))
    },

    /*onResult(event){
      let transcript = event.results[event.results.length-1][0].transcript

      let result = parseInt(transcript, 10)
      const good = this.solution === result
      console.log(transcript, result, this.solution, good)

      this.updateScore(good ? 1 : -1)
      if(good){
        this.goodSound()
        this.roll()
      }else{
        this.badSound()
      }
    },*/

    updateScore(next){
      this.score = this.score + next < 0 ? 0 : this.score + next
      if(this.score === this.max) this.win()
    },

    goodSound(){
      const a = new Audio('/sfx_coin_double3.wav')
      a.play()
    },

    badSound(){
      const a = new Audio('/sfx_sounds_error9.wav')
      a.play()
    },

    finalSound(){
      const a = new Audio('/sfx_sounds_pause7_out.wav')
      a.play()
    },

    win(){
      this.finalSound()
    },

    gif(){
      const images = [
        'https://media4.giphy.com/media/6fScAIQR0P0xW/giphy.gif',
        'https://media.giphy.com/media/QBC5foQmcOkdq/source.gif',
        'https://media.giphy.com/media/kBZBlLVlfECvOQAVno/source.gif',
        'https://media.giphy.com/media/eoxomXXVL2S0E/giphy.gif',
        'https://media.giphy.com/media/3ohzdRPaqZC8TIfdyE/source.gif',
        'https://media.giphy.com/media/iFVLGbPAg9AiB08Vbo/source.gif',
        'https://media.giphy.com/media/ieawleTV0wPiBH1HgM/source.gif',
        'https://media.giphy.com/media/3xz2BCohVTd7h2Kvfi/source.gif',
        'https://media.giphy.com/media/TmVqs1EEKnFjq/source.gif',
        'https://media.giphy.com/media/TmVqs1EEKnFjq/source.gif',
        'https://media.giphy.com/media/26xBENWdka2DSvvag/source.gif',
        'https://media.giphy.com/media/mPIA4KZVXv0ty/source.gif',
        'https://giphy.com/gifs/sandiegozoo-reaction-happy-lol-3bzBb1sBpPpgXKdLrs',
        'https://media.giphy.com/media/iaktyrUiG659e/source.gif',
        'https://media.giphy.com/media/gYZ7qO81g4dt6/source.gif',
        'https://media.giphy.com/media/4Zo41lhzKt6iZ8xff9/giphy.gif',
        'https://media.giphy.com/media/Y4pAQv58ETJgRwoLxj/source.mp4',
        'https://media.giphy.com/media/xTiTnf9SCIVk8HIvE4/source.gif',
        'https://media.giphy.com/media/9IRX12VhoXoR2/giphy.gif',
        'https://media.giphy.com/media/RQSuZfuylVNAY/source.gif',
        'https://media.giphy.com/media/Yjc9l1Q6Al1DO/source.gif',
        'https://media.giphy.com/media/1LweXxLwVT0J2/source.gif',
      ]

      const src = images[this.random(0, images.length-1)]
      console.log(src)

      return src
    }
  }
}
</script>
