---
layout: post
title: Corazon Arco Iris
date: 2020-05-14 06:00 -0500
---
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://unpkg.com/@taufik-nurrohman/color-picker@2.0.3/color-picker.js"></script>
<link rel="stylesheet" href="https://unpkg.com/@taufik-nurrohman/color-picker@2.0.3/color-picker.min.css">
<style>
  :root {
    --size: 100px;
  }

  #app {
    display: grid;
    grid-auto-rows: row;
    grid-gap: 16px;
  }

  #container {
    display: grid;
    align-items: start;
    justify-items: center;
    height: calc(var(--size) * 1.5 * 1.41421356237);
  }

  #container * {
    box-sizing: border-box;
  }

  .heart {
    display: grid;
    transform: rotate(45deg);
  }
  .heart.active {
    animation: latir 1.2s linear infinite;
  }

  .center,
  .right,
  .left {
    position: relative;
    overflow: hidden;
  }

  .center {
    width: var(--size);
    height: var(--size);
    grid-row: 2;
    grid-column: 2;
    box-shadow: calc(var(--size) * .1) calc(var(--size) * .1) calc(var(--size) * .8);
    border-bottom-right-radius: .5em;
  }

  .right {
    width: calc(var(--size) / 2);
    height: var(--size);
    grid-row: 2;
    border-radius: var(--size) 0 0 var(--size);
    justify-self: end;
  }

  .left {
    width: var(--size);
    height: calc(var(--size) / 2);
    border-radius: var(--size) var(--size) 0 0;
    grid-column: 2;
    align-self: end;
  }

  .bars {
    display: grid;
    height: var(--size);
    grid-auto-flow: column;
    position: absolute;
    animation: rainbow 3s infinite linear;
  }

  .bars div {
    width: calc(var(--size) / 5);
  }

  @keyframes rainbow {
    from {
      right: 0px;
    }

    to {
      right: calc(var(--size) * -1.2);
    }
  }

  #reset {
    background-color: #0088ff;
    color: white;
    font-size: 1em;
    border: none;
    border-radius: .5em;
    padding: 10px 60px;
  }

  #inputs {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    justify-items: center;
    align-items: center;
  }
  #inputs p {
    grid-column: 1 / 7;
    text-align: center;
  }

  #picker {
    justify-self: center;
  }

  #picker .color-picker {
    position: static;
  }

  .color {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 4px solid rgba(0, 0, 0, 0)
  }

  .color.active {
    border: 4px dashed #0088ff;
  }

  @keyframes latir {
    0% {
      transform: rotate(45deg) scale(1);
    }
    25% {
      transform: rotate(45deg) scale(1.2);
    }
    50% {
      transform: rotate(45deg) scale(.8);
    }
    75% {
      transform: rotate(45deg) scale(1.2);
    }
    100% {
      transform: rotate(45deg) scale(1);
    }
  }
</style>

<template id="rainbow">
  <div v-bind:class="position">
    <div class="bars">
      <div v-for="(color, index) in colors"
           v-bind:key="`color-${position}-${index}`"
           v-bind:style="{ backgroundColor: color.value }"></div>
    </div>
  </div>
</template>

<div id="app">
  <div id="container">
    <div class="heart"
         v-on:click="latir"
         ref="heart">
      <rainbow-container v-if="colors.length > 0"
                         v-for="rainbow in rainbows"
                         v-bind:key="`rainbow-${rainbow.position}`"
                         v-bind:position="rainbow.position"
                         v-bind:colors="rainbow.colors"></rainbow-container>
    </div>
  </div>
  <div id="inputs">
    <p>TOMAR CUALQUIERA DE LAS ESFERAS PARA CAMBIAR LOS COLORES DEL CORAZÓN ARCO IRIS.</p>
    <div class="color"
         v-for="color in colors"
         v-bind:style="{backgroundColor: color.value }"
         v-on:click="createPicker(color, $event)"
         v-bind:class="{ active: isActive === color.name }"
         v-bind:data-color="color.value"></div>
  </div>
  <div id="picker" ref="picker"></div>
  <button id="reset"
          type="button"
          v-on:click="resetColors">RESTABLECER COLORES</button>
</div>

<script>
  const COLORS = [
    { name: "orange", value: "#ff7f00" },
    { name: "red", value: "#ff0900" },
    { name: "purple", value: "#a800ff" },
    { name: "green", value: "#00f11d" },
    { name: "yellow", value: "#ffef00" },
    { name: "blue", value: "#0088ff" },
  ]
  const RainbowContainer = {
    template: '#rainbow',
    props: {
      position: { type: String },
      colors: { type: Array }
    },
  }
  const app = new Vue({
    el: '#app',
    components: { RainbowContainer },
    data: {
      colors: [],
      isActive: '',
      picker: null
    },
    mounted() {
      this.resetColors()
    },
    methods: {
      createPicker(color, event) {
        this.isActive = color.name
        if (this.picker) this.picker.pop()
        this.picker = new CP(
          event.target,
          { e: false, parent: this.$refs.picker }
        )
        this.picker.on('change', function (r, g, b, a) {
          color.value = CP.HEX([r, g, b, a])
        })
        this.picker.enter()
      },
      getColors(order) {
        return order.map(color => {
          return this.colors.find(c => c.name === color)
        })
      },
      resetColors() {
        this.colors = COLORS.map(color => { return {...color} })
        if (this.picker) {
          this.picker.pop()
          this.isActive = ''
        }
      },
      latir(event) {
        this.$refs.heart.classList.toggle('active')
      }
    },
    computed: {
      rainbows() {
        let rightColor = ['yellow', 'blue', 'orange', 'red', 'purple', 'green', 'yellow', 'blue', 'orange']
        let centerAndLeftColor = ['red', 'purple', 'green', 'yellow', 'blue', 'orange', 'red', 'purple', 'green', 'yellow', 'blue']
        return [
          { position: "center", colors: this.getColors(centerAndLeftColor) },
          { position: "right", colors: this.getColors(rightColor) },
          { position: "left", colors: this.getColors(centerAndLeftColor) }
        ]
      }
    }
  })
</script>
