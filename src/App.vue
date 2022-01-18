<template>
<div class="column">
  <div class="row">
    <button
      id="1"
      name="1"
      class="button button1"
      :disabled="!isRun"
      @click="buttonClick(1)"
      :class="{ button1active: !buttons[0] }"
    ></button>
    <button
      id="2"
      name="2"
      class="button button2"
      :disabled="!isRun"
      @click="buttonClick(2)"
      :class="{ button2active: !buttons[1] }"
    ></button>
    <div class="interface column">
      <button
        type="submit"
        @click="onSubmit"
        :disabled="isRun"
      >TRY</button>
      <div class="column">
        <input type="radio" label="min" value="min" />
        <label>min</label>
        <input type="radio" label="mid" value="mid" />
        <label>mid</label>
        <input type="radio" label="max" value="max" />
        <label>max</label>
      </div>
    </div>  
  </div>
  <div class="row">
    <button
      id="3"
      name="3"
      class="button button3"
      :disabled="!isRun"
      @click="buttonClick(3)"
      :class="{ button3active: !buttons[2] }"
    ></button>
    <button
      id="4"
      name="4"
      class="button button4"
      :disabled="!isRun"
      @click="buttonClick(4)"
      :class="{ button4active: !buttons[3] }"
    ></button>
  </div>
</div>
</template>

<script>
import ButtonSound1 from '@/assets/1.ogg';
// import ButtonSound2 from '@/assets/2.ogg';
// import ButtonSound3 from '@/assets/3.ogg';
// import ButtonSound4 from '@/assets/4.ogg';

const sleep = (delay) => new Promise( res => 
  setTimeout(() => {
    res();
  }, delay)
);

export default {
  data () {
    return {
      delay: 1500,
      line: [this.getRandomButtonNumber()],
      buttons: [true, true, true, true],
      turns: [],
      round: 1,
      turnPart: 0,
      isRun: false
    }
  },

  methods: {
    getRandomButtonNumber() {
      return Math.round(Math.random() * 3);
    },
    async buttonClick(index) {
      this.buttons = this.changeColor(index - 1, false, true);
      await sleep(500)
      this.buttons = this.changeColor(index - 1, true);
      const isRightTurn = this.line[this.turnPart] === (index - 1);
      if (isRightTurn) {
        this.turnPart += 1
      }else {
        alert('You lose');
        this.isRun = false;
      }
      if (this.turnPart == this.round) {
        this.turnPart = 0
        this.round += 1;
        this.line = [...this.line, this.getRandomButtonNumber()];
        await sleep(500)
        this.onSubmit();
      } 
    },
    changeColor(idx, state, shouldPlaySound = false) {
      if (shouldPlaySound) {
        const audio = new Audio(ButtonSound1);
        audio.play();
      }
      return this.buttons.map( (val, index) => index === idx ? state : val )
    },
    onSubmit() {
      this.isRun = true;
      // eslint-disable-next-line
      this.line.reduce( async (prev, current) => {
        await prev;
        this.buttons = this.changeColor(current, false, true);
        return new Promise( res => {
          setTimeout( () => {
            this.buttons = this.changeColor(current, true);
            sleep(500).then( () => {
              res();
            } )
          }, 500 );
        });
      }, Promise.resolve() )
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
    transform: rotateZ(45deg);
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
}

.interface{
  display: flex;
  margin: 100px 100px 100px 200px;
  height:40px;
  width: 40px;
}

</style>