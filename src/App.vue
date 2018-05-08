<template>
  <div id="app">
    <!-- <textarea v-model="joke"></textarea> -->
    <div
      id="picture"
      :style="selectedColor"
    >
      <div id="quote" contenteditable="true" :style="{ 'font-size': `${fontSize}vmin` }">
        {{ joke }}
      </div>
      <img id="logo" src="./assets/logo.svg"/>
    </div>
    <div id="colors">
      <div
        class="color"
        v-for="color of colors"
        :key="color.code"
        :style="color"
        @click="selectedColor = color"
      ></div>
    </div>
    <div id="actions">
      <!-- <div class="action" @click="fontSize += 2"> + </div>
      <div class="action" @click="fontSize -= 2"> – </div> -->
      <vue-slide-bar
        id="slider"
        :min="5"
        :max="20"
        v-model="fontSize"
        :processStyle="{ backgroundColor: 'white' }"
        :tooltipStyles="{ backgroundColor: 'white', borderColor: 'white', color: 'black' }"
      />
      <div class="action" @click="exportImage">Guardar</div>
    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas'
import VueSlideBar from 'vue-slide-bar'

const colors = [
  { backgroundColor: '#333333', color: '#ffffff' },
  { backgroundColor: '#ffffff', color: '#333333' },
  { backgroundColor: '#47abbb', color: '#ffffff' },
  { backgroundColor: '#49a8fd', color: '#ffffff' },
  { backgroundColor: '#7b4596', color: '#ffffff' },
]

export default {
  name: 'app',
  data () {
    return {
      fontSize: 8,
      joke: 'Escribe aquí tu chiste',
      colors,
      selectedColor: colors[2]
    }
  },
  methods: {
    exportImage () {
      const div = document.getElementById('picture')

      html2canvas(div, {
        allowTaint: false,
        useCORS: false
      }).then(canvas => {
        var myImage = canvas.toDataURL();
        const time = new Date().getTime();
        this.downloadURI(myImage, `chiste${time}.png`);
      });
    },
    downloadURI(uri, name) {
      var link = document.createElement("a");
      link.download = name;
      link.href = uri;
      document.body.appendChild(link);
      link.click();
    }
  },
  components: {
    VueSlideBar
  }
}
</script>

<style lang="scss">
$primaryColor: #d1ecff;

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 10px;
  background-color: #444;
  font-family: 'Roboto', sans-serif
}

#app {
  #actions {
    width: calc(100vmin - 20px);
    margin: 0 auto;
    padding: 0 0px 0 12px;
    display: flex;
    justify-content: space-between;
    align-items: flex-end;

    #slider {
      margin-bottom: 2px;
      width: 100%;
      margin-right: 20px;
    }

    .action {
      display: inline-block;
      padding: 5px 10px;
      background-color: white;
      color: #333;
      font-size: 1.2rem;
      text-align: center;
      cursor: pointer;
      border-radius: 3px;
      position: relative;
      top: -2px;
    }
  }

  #colors {
    display: flex;
    margin: 0 auto;
    width: calc(100vmin - 20px);
    margin-top: 10px;
    margin-bottom: 20px;

    .color {
      flex: 1;
      height: 46px;
      cursor: pointer;
      background-color: #eee;
      border-radius: 4px;

      &:not(:last-child) {
        margin-right: 10px;
      }
   }
  }

  #picture {
    font-family: 'Acme', sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    width: calc(100vmin - 20px);
    height: calc(100vmin - 20px);
    position: relative;
    margin: 0 auto;
    margin-bottom: 0px;
    background-color: #47abbb;
    color: white;
    position: relative;

    img {
      position: absolute;
      bottom: 1%;
      right: 1.5%;
      width: 20%;
    }
  }

  #quote {
    z-index: 100;
    width: 100%;
    text-align: center;
    white-space: pre-line;
    position: relative;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.51);
  }
}

.vue-slide-bar-component {
  padding-top: 10px !important;
}

.vue-slide-bar {
  background-color: #777 !important;
}

</style>
