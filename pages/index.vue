<template>
  <div class="container">
    <div>
      <div v-if="notStart">
        <h1>ハイ・アンド・ロー</h1>
        <div class="links">
          <a target="_blank" @click="start()" class="button--green">スタート</a>
        </div>
      </div>
      <div v-else>
        <h1 class="resultText">{{ message }}</h1>
        <img v-bind:src="torannpu" width="200" height="300"/>
        <div><p>このカードよりHighかLowを予測してね！</p></div>
        <div v-if="reset" class="links">
          <a target="_blank" @click="reStart()" class="button--green">もう一度プレーする</a>
        </div>
        <div class="links" v-else>
          <a target="_blank" @click="high()" class="button--green">High↑</a>
          <a target="_blank" @click="low()" class="button--green">Low↓</a>
        </div>
        <div class="mt-3" v-if="countState">あなたが当てた回数は{{ count }}回です</div>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },

  data() {
    return {
      max : 13,
      min: 1,
      notStart: true,
      torannpu: "",
      oldNumber: "",
      newNumber: "",
      result:"",
      message:"",
      reset:"",
      countState:"",
      count:0
    }
  },

  methods: {
    start(){
      this.notStart = false
      this.oldNumber = Math.floor(Math.random() *  (this.max - this.min + 1) + this.min)
      this.torannpu = "/torannpu" + this.oldNumber + ".png"
    },

    high(){
      this.newNumber = Math.floor(Math.random() * (this.max - this.min + 1) + this.min)
      this.torannpu = "/torannpu" + this.newNumber + ".png"
      if(this.newNumber > this.oldNumber){
        this.result = true
        this.count += 1
        this.oldNumber = this.newNumber
      }else{
        this.result = false
      }
      this.check()
    },

    low(){
      this.newNumber = Math.floor(Math.random() * (this.max - this.min + 1) + this.min)
      this.torannpu = "/torannpu" + this.newNumber + ".png"
      if(this.newNumber < this.oldNumber){
        this.result = true
        this.count += 1
        this.oldNumber = this.newNumber
      }else{
        this.result = false
      }
      this.check()
    },

    check(){
      if(this.result){
        this.message = "予想正解です！"
        this.torannpu = "/torannpu" + this.oldNumber + ".png"
      }else{
        this.message = "予想失敗です！"
        this.countState = true
        this.reset = true
      }
    },

    reStart(){
      this.message = ""
      this.oldNumber = ""
      this.newNumber = ""
      this.countState = ""
      this.count = 0
      this.reset = ""
      this.start()
    },

  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.resultText {
  color: red
}
</style>
