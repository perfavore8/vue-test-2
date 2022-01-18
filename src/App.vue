<template>
<div>
  <button
  class="button1"
  >1</button>
  <button
  class="button2"
  >2</button>
  <button
  class="button3"
  >3</button>
  <button
  class="button4"
  >4</button>
</div>
</template>

<script>
export default {
    data () {
      line:[1,2,3,4]
    }
}
</script>

<style>

.button1 {
    background-color: rgba(255, 0, 0, 0.5);
    color: black;
}

.button2 {
    background-color: rgba(255, 255, 0, 0.5);
    color: black;
}

.button3 {
    background-color: rgba(0, 0, 255, 0.5);
    color: black;
}

.button4 {
    background-color: rgba(0, 128, 0, 0.5);
    color: black;
}

</style>