<template>
  <div id="app">
    <div class="main-title">
        <h1>Favourite Beers</h1>
    </div>
    <div class="main-content">
        <ul class="list-beer">
            <li v-for="(beer, index) in beers" @click="toggleViewData(index)">
                <span class="beer-title">
                    <font-awesome-icon icon="angle-right" v-bind:id="'angle-right-' + index"/>
                    <font-awesome-icon class="angle-down" icon="angle-down" v-bind:id="'angle-down-' + index"/>
                    {{ beer.name }}
                </span>
                <div class="beer-content"  v-bind:id="'content-' + index">
                    <img class="rectangle" :src="beer.image" />
                    <span class="beer-description">{{beer.description}}</span>
                </div>
                <hr class="beer-separator">
            </li>
        </ul>
    </div>
</div>
</template>

<script>
export default {
    name: 'app',
    data() {
      return {
        mostrar: false,
        beers: []
      }
    },
    methods: {
      //here methods of object
      toggleViewData: function(index) {
        this.mostrar = !this.mostrar;
        var content = document.getElementById('content-' + index);
        var angleDown = document.getElementById('angle-down-' + index);
        var angleRight = document.getElementById('angle-right-' + index);
        if(this.mostrar){
            content.style.display = 'flex';
            angleDown.style.display = 'inline';
            angleRight.style.display = 'none';
        }
        else {
            content.style.display = 'none';
            angleDown.style.display = 'none';
            angleRight.style.display = 'inline';
        }
      }
    },
    mounted() {
      this.$http.get('https://api.punkapi.com/v2/beers?per_page=10')
        .then(response => {
          if(response.status === 200) {
            this.beers = response.data.map(beer => {
              return {
                name: beer.name,
                description: beer.description,
                image: beer.image_url
              }
            })
          }
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
</script>
<style>
    .main-title {
        font-family: Roboto;
        font-size: 64px;
        font-weight: 300;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.06;
        letter-spacing: normal;
        color: #152935;
    }
    .rectangle {
        width: 75px;
        height: 88px;
        border-radius: 2px;
        background-color: #d8d8d8;
        margin-right: 3%;
    }
    .list-beer {
        text-align: left;
        list-style: none;
    }
    .beer-title {
        width: 100%;
        height: 24px;
        font-family: Roboto;
        font-size: 16px;
        font-weight: bold;
        font-style: normal;
        font-stretch: normal;
        line-height: 1.5;
        letter-spacing: normal;
        color: #517489;
    }
    .beer-content {
        display: none;
    }
    .beer-description {
        width: 100%;
    }
    .angle-down {
        display: none;
    }
</style>
