<template>
<div @submit.prevent="onSubmit">
  <button
    id="1"
    name="1"
    v-bind:class="{ button1: isActive1, button1active: !isActive1 }"
  >1</button>
  <button
    id="2"
    name="2"
    :class="{ button2: isActive2, button2active: !isActive2 }"
  >2</button>
  <button
    id="3"
    name="3"
    :class="{ button3: isActive3, button3active: !isActive3 }"
  >3</button>
  <button
    id="4"
    name="4"
    :class="{ button4: isActive4, button4active: !isActive4 }"
  >4</button>
  <button
    type="submit"
    @click="onSubmit"
  >TRY</button>
</div>
</template>

<script>
export default {
  data () {
    return {
      delay: 500,
      line: new Array(10).fill(0).map(() => Math.ceil(Math.random() * 4)),
      isActive1: true,
      isActive2: true,
      isActive3: true,
      isActive4: true,
    }
  },

  methods: {
    onSubmit() {
      for (let i = 0; i < this.line.length ; i++) {
        console.log('a')
        
        if (this.line[i] == 1){
          setTimeout(() => {
            this.isActive1 = false;
          }, i*this.delay)
          setTimeout(() => {
            this.isActive1 = true;
          }, (i+1)*this.delay)
        }
        if (this.line[i] == 2){
          setTimeout(() => {
            this.isActive2 = false;
          }, i*this.delay)
          setTimeout(() => {
            this.isActive2 = true;
          }, (i+1)*this.delay)
        }
        if (this.line[i] == 3){
          setTimeout(() => {
            this.isActive3 = false;
          }, i*this.delay)
          setTimeout(() => {
            this.isActive3 = true;
          }, (i+1)*this.delay)
        }
        if (this.line[i] == 4){
          setTimeout(() => {
            this.isActive4 = false;
          }, i*this.delay)
          setTimeout(() => {
            this.isActive4 = true;
          }, (i+1)*this.delay)
        }
      } 
    }
  }
}

</script>

<style>

.button1 {
    background-color: rgba(255, 0, 0, 0.5);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

.button1active {
    background-color: rgb(255, 0, 0);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

.button2 {
    background-color: rgba(255, 255, 0, 0.3);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

.button2active {
    background-color: rgb(255, 255, 0);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

.button3 {
    background-color: rgba(0, 0, 255, 0.5);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

.button3active {
    background-color: rgba(0, 0, 255);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

.button4 {
    background-color: rgba(0, 128, 0, 0.5);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

.button4active {
    background-color: rgba(0, 128, 0);
    color: black;
    font-size: 40px;
    padding: 100px 100px;
}

</style>