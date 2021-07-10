<template>
  <div class="circle_chunk" :class="color"></div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'red',
      validator: function (value) {
        return ['red', 'green', 'blue', 'yellow'].indexOf(value) !== -1
      }
    }
  }
}
</script>

<style>
  .circle_chunk{
    width: 200px;
    height: 200px;
    border-top-left-radius: 100%;
  }

  .circle_chunk:hover,
  .active{
    filter: blur(2px);
    cursor: pointer;
    border: 3px solid black;
  }

  .red{
    background: rgb(255, 120, 120);
  }

  .red:active{
    background: rgb(252, 80, 80);
  }

  .yellow{
    background: rgb(255, 166, 120);
    transform: rotate(180deg);
  }
  .yellow:active{
    background: rgb(255, 126, 80);
  }

  .blue{
    background: rgb(120, 120, 255);
    transform: rotate(90deg);
  }
  .blue:active{
    background: rgb(80, 80, 255);
  }

  .green{
    background: rgb(120, 255, 120);
    transform: rotate(270deg);
  }
  .green:active{
    background: rgb(80, 255, 80);
  }

  @media only screen and (min-device-width : 320px) and (max-device-width : 480px) {
    .circle_chunk{
      width: 120px;
      height: 120px;
    }
  }
</style>
