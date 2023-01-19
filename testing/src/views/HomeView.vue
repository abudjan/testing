<template>
  <div class="jj">
  <div class="glav">
    <div class="gg" v-for="(item, i) in DATA" v-if="picked">
    <p class="qst">{{ item.question }}</p>
    <label :for="variant"  v-for="(variant) in item.variants">
     <p class="vrt">
      <input :name="item.id" :id="variant" :value="variant" type="radio"  v-model="picked[i].value"  >
      {{ variant }}</p> 
    </label>
    <p class="otvet" v-if = "fuckYou">{{ result[i].checked ? 'Ответ верный' : 'ответ не верный'}}</p>
  </div>
  
  <button class="btn" @click="send">Получить результат</button>
  </div>
</div>
</template>
<script>`1  q   `
 export default {
  data: () => ({

    DATA: null,
    answer: null,
    picked: null,
    result: null,
    fuckYou: false
  }),
  methods: {
    async getdata() {
      const response = await fetch(`http://localhost:3000/questions`)
      const data = await response.json()
      this.DATA = await data
      this.picked = data.map(item => ({
        value: null
      }))
    },
    async getanswer(){
      const response = await fetch(`http://localhost:3000/answers`)
      this.answer = await response.json()
    },

    send() {
        this.result = this.answer.map((item, i) => ({
          ...item,
          checked: this.picked[i].value === item.answer
        }))
        this.fuckYou = true;
        console.log(this.result[1])
    }
  },

  mounted(){
    this.getdata(),
    this.getanswer()
  }
}

</script>
<style lang="css">
body{
  background-color: rgb(225, 225, 225);
  
}
.glav{
  width: 60%;
  border: solid 3px;
  background-color: rgb(255, 255, 255);
  
  
}
.jj{
  display: flex;
  justify-content: center;
}
.gg{
    height: 500px;
    width: 80%;
    border: solid 4px rgb(126, 126, 126);
    margin-bottom: 20px;
    background-color: rgb(140, 203, 203);
    margin-left: 86px
}
 .qst{
  font-size: 35px;
  color: rgb(0, 0, 0);
  font-weight: bold;
 }
 .vrt{
font-size: 20px;
color: rgb(1, 0, 2);
 }
 .btn{
  width: 220px;
  height: 60px;
  font-size: 20px;
  border: none;
  border-radius: 10px;
  color:rgb(255, 255, 255);
  background-color: rgb(15, 179, 64);
 }
 .otvet{
  font-size: 30px;
  color: crimson;
 }
</style>
