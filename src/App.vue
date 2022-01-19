<template>
<div class="column">
  <div class="row">
    <button
      id="1"
      name="1"
      class="button button1"
      :disabled="!isRun || linePlay"
      @click="buttonClick(1)"
      :class="{ button1active: !buttons[0] }"
    ></button>
    <button
      id="2"
      name="2"
      class="button button2"
      :disabled="!isRun || linePlay"
      @click="buttonClick(2)"
      :class="{ button2active: !buttons[1] }"
    ></button>
    <div class="interface column">
      <button
        type="submit"
        class="button-start"
        @click="onSubmit"
        :disabled="isRun"
      ><strong>START</strong></button>
      <label class="label">Difficulty</label>
      <div class="row-levels">
        <input type="radio" id="min" name="difficulty" value=1500 v-model=delay>
        <label class="label" >min</label>
        <input type="radio" id="mid" name="difficulty" value=1000 v-model=delay>
        <label class="label" >mid</label>
        <input type="radio" id="max" name="difficulty" value=400 v-model=delay>
        <label class="label" >max</label>
      </div>
      <label class="round">Rounds: {{this.round-1}}</label>
    </div>  
  </div>
  <div class="row">
    <button
      id="3"
      name="3"
      class="button button3"
      :disabled="!isRun || linePlay"
      @click="buttonClick(3)"
      :class="{ button3active: !buttons[2] }"
    ></button>
    <button
      id="4"
      name="4"
      class="button button4"
      :disabled="!isRun || linePlay"
      @click="buttonClick(4)"
      :class="{ button4active: !buttons[3] }"
    ></button>
  </div>
</div>
</template>

<script>
import ButtonSound1 from '@/assets/1.ogg';
import ButtonSound2 from '@/assets/2.ogg';
import ButtonSound3 from '@/assets/3.ogg';
import ButtonSound4 from '@/assets/4.ogg';

const ButtonSounds = [ButtonSound1, ButtonSound2, ButtonSound3, ButtonSound4]

const sleep = (delay) => new Promise( res => 
  setTimeout(() => {
    res();
  }, delay)
);

export default {
  data () {
    return {
      delay: "1500",
      line: [this.getRandomButtonNumber()],
      buttons: [true, true, true, true],
      turns: [],
      round: 1,
      turnPart: 0,
      isRun: false,
      linePlay: false,
    }
  },

  methods: {
    getRandomButtonNumber() {
      return Math.round(Math.random() * 3);
    },
    async buttonClick(index) {
      this.buttons = this.changeColor(index - 1, false, true);
      await sleep(this.delay)
      this.buttons = this.changeColor(index - 1, true);
      const isRightTurn = this.line[this.turnPart] === (index - 1);
      if (isRightTurn) {
        this.turnPart += 1
      }else {
        alert('You lose');
        this.turnPart = 0
        this.isRun = false;
        this.round = 1
        this.line = [this.getRandomButtonNumber()]
      }
      if (this.turnPart == this.round) {
        this.turnPart = 0
        this.round += 1;
        this.line = [...this.line, this.getRandomButtonNumber()];
        await sleep(this.delay/10)
        this.onSubmit();
      } 
    },
    changeColor(idx, state, shouldPlaySound = false) {
      if (shouldPlaySound) {
        const audio = new Audio(ButtonSounds[idx]);
        audio.play();
      }
      return this.buttons.map( (val, index) => index === idx ? state : val )
    },
    onSubmit() {
      this.isRun = true;
      this.linePlay = true;
      // eslint-disable-next-line
      this.line.reduce( async (prev, current) => {
        await prev;
        this.buttons = this.changeColor(current, false, true);
        return new Promise( res => {
          setTimeout( () => {
            this.buttons = this.changeColor(current, true);
            sleep(this.delay/10).then( () => {
              res();
            } )
          }, this.delay );
        });
      }, Promise.resolve() )
      sleep(this.delay * this.line.length).then(() => {
        this.linePlay = false;
      })
    }
  }
}
// .button, .red, .active
</script>

<style>

.button {
    color: black;
    font-size: 40px;
    padding: 100px;
    margin: 1px;
}

.button1 {
    background-color: rgba(255, 0, 0, 0.5);

}

.button1active {
    background-color: rgb(255, 0, 0);
}

.button2 {
    background-color: rgba(255, 255, 0, 0.3);
}

.button2active {
    background-color: rgb(255, 255, 0);
}

.button3 {
    background-color: rgba(0, 0, 255, 0.5);
}

.button3active {
    background-color: rgba(0, 0, 255);
}

.button4 {
    background-color: rgba(0, 128, 0, 0.5);
}

.button4active {
    background-color: rgba(0, 128, 0);
}

.column {
  display: flex;
  flex-direction: column;
}

.row {
  display: flex;
  flex-direction: row;
  max-height: 200px;
  margin: 3px;
  max-width: 50%;
}

.interface{
  display: flex;
  margin: 100px 100px 100px 200px;
  height:40px;
  width: 40px;
}

.button-start{
  background-color: yellowgreen;
  font-size: 20px;
  text-align: center;
  width: fit-content;
  padding: 30px 50px;
  margin: 10px;
  border-radius: 10px;
  border: 0;
}

.label{
    font-size: larger;
    font-family: sans-serif;
    margin-right: 15px;
    margin-left: 2px;
}

.round{
    margin-top: 10px;
    font-size: larger;
    font-family: sans-serif;
    display: flex;
    height: inherit;
    width: max-content;
}

.row-levels {
  display: flex;
  flex-direction: row;
}

</style>