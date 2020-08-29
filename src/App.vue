<template>
  <div id="app" :class="{started}">

    <section>
      <div class="grid" v-if="started && progress < 100">
        <div>{{a}}</div>
        <div>x</div>
        <div>{{b}}</div>
      </div>

      <div class="win" v-if="started && progress === 100">
        <img :src="gif()" />
      </div>
    </section>

    <div class="progress" v-if="started">
      <div :style="`width:${progress}%`"/>
    </div>

    <button @click="start" id="roll">Lancer</button>

  </div>
</template>

<script>
export default {
  name: 'App',

  mounted(){
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

    // new speech recognition object
    this.recognition = new SpeechRecognition()
    this.recognition.lang = 'fr-FR'
    this.recognition.continuous = true

    // This will run when the speech recognition service returns a result
    this.recognition.onstart = function() {
      console.log("Voice recognition started. Try speaking into the microphone.")
    }

    this.recognition.onresult = this.onResult
    this.recognition.start()
  },

  data: () => ({
    started: false,
    score: 3,
    max: 5,
    a: 1,
    b: 1
  }),

  computed:{
    solution(){
      return this.a * this.b
    },

    progress(){
      return Math.round((this.score / this.max) * 100)
    },
  },

  methods: {
    start(){
      this.started = true
      this.score = 0
      this.roll()
    },

    roll: function(){
      this.a = this.random(1, 10)
      this.b = this.random(1, 10)
    },

    random(min, max){
      return Math.round(min + Math.random() * (max - min))
    },

    onResult(event){
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
    },

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


<style lang="scss">
// #0C0636,#095169,#059B9A,#53BA83,#9FD86B,#0C0636,#095169,#059B9A
body{
  margin: 0;
  font-family: 'Roboto', sans-serif;
}

#app{
  background: aqua;

  &.started{
    section{
      height: calc(100vh - 50px);
    }
  }
}

#roll{
  position: absolute;
  top: 5px;
  left: 5px;
}

section{
  height: 100vh;
  background: #0C0636;
  font-size: 12rem;
  text-align: center;
  color: #9FD86B;
}

.grid{
  height: 100%;
  display: grid;
  grid-template-columns: 2fr 1fr 2fr;
  text-align: center;
  align-items: center;
}

.progress{
  background: #059B9A;

  div{
    transition: all 350ms;
    background: #095169;
    height: 50px;
  }
}

.win{
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;

  img{
    max-height: 25vh;
  }
}

</style>
