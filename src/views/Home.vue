<template>
  <div class="about">
    <section>
      <div class="container">
        
        <div class="header-search">
          <span class="fa fa-search"></span>
          <div style="box-shadow: 2px 4px 20px -4px rgba(0,0,0,.1);">  <input type="text" name="" id="" v-model="search" v-on:keyup="searchEntry" placeholder="Search for Photos.."></div>
        </div>
        
        <div class="image-container">
          <div class="wrapper">
            <div class="masonry masonry--v" >
              <div class="masonry-brick masonry-brick--v" @click="modal(image.urls.small, image.user.name)" v-for="image in images" :key="image.id">
                <img :src="image.urls.small" class="masonry-img">
                <div class="text">
                  <h1>{{image.user.name}}</h1>
                  <h3>London, Mumbai</h3>
                  <div class="loader" v-if="loader">
                    <pulse-loader v-show="loader" class="custom-class"  :color='color' :loading='loader'></pulse-loader>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- <ul class="grid">
            <li @click="modal(image.urls.small, image.user.name)" class="gallery__image " v-for="image in images" :key="image.id">
              <img id="myImg" :src="image.urls.small" alt="Avatar" class="image">
              <div class="text">
                <h1>{{image.user.name}}</h1>
                <h3>London, Mumbai</h3>
                <div class="loader" v-if="loader">
                  <pulse-loader v-show="loader" class="custom-class"  :color='color' :loading='loader'></pulse-loader>
                </div>
              </div>
            </li>
          </ul> -->
        </div>
      </div>
      <!-- The Modal -->
      <div id="myModal" class="modal">
        <span class="close" @click="close">&times;</span>
        <img class="modal-content" id="img01">
        <div id="caption"></div>
      </div>
    </section>
  </div>
</template>
<script>
import axios from 'axios'
import { PulseLoader } from '@saeris/vue-spinners'
export default {
  components: {
    PulseLoader
  },
  name: 'list-images',
  data: function () {
    return {
      color: 'white',
      loader: false,
      search: '',
      images: []
    }
  },
  methods: {
    getImage: function () {
      this.loader = true
      const baseURI = 'https://api.unsplash.com/photos/?client_id=b9f69c845aa5a486905c09075f2ec48d4d8aec94c4630fd28e3a78ebfaf5df7d&per_page=8&order_by=latest'
      axios.get(baseURI)
      .then((result) => {
        let response = result.data
        response.forEach(element => {
          this.images.push(element)
          this.loader = false
        });
      })
    },
    modal: function (image, name) {
      let modal = document.getElementById("myModal")
      let modalImg = document.getElementById("img01")
      modal.style.display = "block";
      modalImg.src = image;
      modalImg.alt = name;
    },
    close: function () {
      let modal = document.getElementById("myModal")
      modal.style.display = 'none'
    },
    searchEntry: function () {
      this.loader = true
      let query = this.search
      const baseURI = 'https://api.unsplash.com/search/photos/?query='+`${query}`+'&client_id=b9f69c845aa5a486905c09075f2ec48d4d8aec94c4630fd28e3a78ebfaf5df7d&per_page=8&order_by=latest'
      axios.get(baseURI)
      .then((result) => {
        this.images = result.data.results
        this.loader = false
      })
    }
  },
  created () {
    this.getImage()
  },
  watch: {
    images: function () {
      if (this.images.length == 0) {
        this.getImage()
      } 
    }
  }
}
</script>

<style lang="scss" scoped>
@import url("//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css");
.modal {
    display: none;
    position: fixed;
    z-index: 1; 
    padding-top: 100px; 
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto; 
    background-color: rgba(0,0,0,0.9);
    transition: 0.3s;
  }

  .modal-content {
    margin: auto;
    display: block;
    height: 70%;
    width: 47%;
    object-fit: cover;
    border-radius: 10%;
  }

  .close {
    position: absolute;
    top: 15px;
    right: 35px;
    color: #f1f1f1;
    font-size: 40px;
    font-weight: bold;
    transition: 0.3s;
  }

  .close:hover,
  .close:focus {
    color: #bbb;
    text-decoration: none;
    cursor: pointer;
  }
  
.container {
  .header-search {
    height: 70px;
    background: #d6dde5;
    padding: 91px 118px;
    position: relative;
    
    input {
      height: 57px;
      width: 100%;
      font-size: 20px;
      border: 1px solid #dde2e9;
      border-radius: 10px;
      text-indent: 65px;
      transition: 0.3s;
      &:focus, &:active {
        z-index: 199999;
        border-radius: 10px;
        outline: none;
        box-shadow: 2px 4px 20px -4px rgba(5, 5, 5, 0.1);
        transition: 0.3s;
      }
    }
    
    .fa-search { 
      position: absolute;
      top: 116px;
      left: 138px;
    }
  }
  .image-container {

    .wrapper {
      margin: -3em auto;
      max-width: 970px;
    }

    img {
      vertical-align: middle;
      max-width: 100%;
    }

    .masonry {
      display: flex;
      width: 100%;
    }

    a {
      color: #333;
    }

    .masonry--v {
      flex-flow: column wrap;
      max-height: 1080px;
    }

    .masonry--v {
      margin-left: -8px; /* Adjustment for the gutter */
      counter-reset: brick;
    }

    .masonry-brick {
      overflow: hidden;
      border-radius: 10px;
      margin: 0 0 50px 8px;  /* Some Gutter */
      background-color: #333;
      color: white;
      position: relative;
    }

    .masonry-brick .text {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      transition: font-size 0.25s, opacity 0.25s ease-in-out;

      h1 {
        font-size: 17px;
        margin: 0;
        width: 50%;
      }

      h3 {
        margin: 0;
        font-size: 14px;
      }
    }

    @media only screen and (min-width: 1024px) {
      /* Horizontal masonry bricks on desktop-sized screen */
      .masonry-brick--h:nth-child(4n+1) {
        width: 250px;
      }
      .masonry-brick--h:nth-child(4n+2) {
        width: 325px;
      }
      .masonry-brick--h:nth-child(4n+3) {
        width: 180px;
      }
      .masonry-brick--h:nth-child(4n+4) {
        width: 380px;
      }

      /* Adjusting vertical masonry height on desktop-sized screen */
      .masonry--v {
        max-height: 1263px;
      }

      /* Vertical masonry bricks on desktop-sized screen */
      .masonry-brick--v {
        width: 33.33333%;
      }
    }

    @media only screen and (max-width: 1023px) and (min-width: 768px) {
      /* Horizontal masonry bricks on tabled-sized screen */
      .masonry-brick--h:nth-child(4n+1) {
        width: 200px;
      }
      .masonry-brick--h:nth-child(4n+2) {
        width: 250px;
      }
      .masonry-brick--h:nth-child(4n+3) {
        width: 120px;
      }
      .masonry-brick--h:nth-child(4n+4) {
        width: 280px;
      }

      /* Adjusting vertical masonry height on tablet-sized screen */
      .masonry--v {
        max-height: 2000px;
      }

      /* Vertical masonry bricks on tablet-sized screen */
      .masonry-brick--v {
        width: 50%;
      }
    }

    .masonry-img {
      object-fit: cover;
      width: 100%;
      height: 100%;
      filter: brightness(90%);
    }
  }
}
</style>