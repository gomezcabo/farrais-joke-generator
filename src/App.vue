<template>
  <div id="app">
    <!-- <textarea v-model="joke"></textarea> -->
    <div id="picture">
      <div id="quote" contenteditable="true" :style="{ 'font-size': `${fontSize}vmin` }">{{ joke }}</div>
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

export default {
  name: 'app',
  data () {
    return {
      fontSize: 8,
      joke: 'Escribe aquí tu chiste'
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
        this.downloadURI('data:' + myImage, `chiste${time}.png`);
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
  textarea {
    width: 100%;
    height: calc(100vmax - 100vmin - 20px);
  }

  #actions {
    width: calc(100vmin - 20px);
    margin: 0 auto;
    margin-left: 4px;
    margin-right: 5px;
    padding: 0 5px 0 8px;
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
    background-color: #8950ff;
    color: white;
    background-image: url('./assets/logo.png');
    background-repeat: no-repeat;
    background-size: 16% 8%;
    background-position-x: calc(98%);
    background-position-y: calc(98%);

    img {
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      z-index: 10;
    }
  }

  #quote {
    z-index: 100;
    width: 100%;
    text-align: center;
    white-space: pre-line;
  }
}

.vue-slide-bar-component {
  padding-top: 10px !important;
}

.vue-slide-bar {
  background-color: #777 !important;
}

</style>
