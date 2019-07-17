<template>
  <div class="about">
    <section>
      <div class="container">
        
        <div class="header-search">
          <span class="fa fa-search"></span>
          <div>  <input type="text" name="" id="" v-model="search" v-on:keyup="searchEntry" placeholder="Search for Photos.."></div>
        </div>
        <div class="image-container">
          <ul class="grid">
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
            
          </ul>
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
      color: '#3daff9',
      loader: false,
      search: '',
      images: []
    }
  },
  methods: {
    getImage: function () {
      this.loader = true
      const baseURI = 'https://api.unsplash.com/photos/?client_id=b9f69c845aa5a486905c09075f2ec48d4d8aec94c4630fd28e3a78ebfaf5df7d'
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
      let query = this.search
      const baseURI = 'https://api.unsplash.com/search/photos/?query='+`${query}`+'&client_id=b9f69c845aa5a486905c09075f2ec48d4d8aec94c4630fd28e3a78ebfaf5df7d'
      axios.get(baseURI)
      .then((result) => {
        this.images = result.data.results
      })
    }
  },
  created () {
    this.getImage()
  }
}
</script>

<style lang="scss" scoped>
@import url("//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css");
.container {
  .header-search {
    height: 118px;
    background: #dde2e9;
    padding: 91px 118px;
    position: relative;
    
    input {
      height: 57px;
      width: 100%;
      font-size: 20px;
      border: 1px solid #dde2e9;
      border-radius: 10px;
      text-indent: 65px;
      
      &:focus, &:active {
        z-index: 199999;
        border-radius: 10px;
        outline: none;
        box-shadow: 0 8px 6px -6px black;
      }
    }
    
    .fa-search { 
      position: absolute;
      top: 116px;
      left: 138px;
    }
  }
  .image-container {
    z-index: 1;
    top: -70px;
    display: flex;
    justify-content: center;
    position: relative;
    .grid {
      display: grid;
      grid-gap: 30px 50px;
      grid-template-columns: 225px 225px 225px;
      grid-auto-flow: dense;

      .gallery__image{
        background: linear-gradient(to bottom, rgba(0,0,0,0) 0%,rgba(0,0,0,0.4) 100%); /* W3C */
        cursor: pointer;
        border-radius: 10px;
        img {
          &:hover{
            opacity: 0.7s;
          }
        }
        .text {
          position: absolute;
          color: #fff;
          margin-left: 20px;

          h1 {
            margin: 0px;
            font-size: 18px;
            font-weight: bold;
            width: 50%;
            text-transform: capitalize;
          }
          h3 {
            margin: 0px;
            font-size: 11px;
            font-weight: 500;
            text-transform: capitalize;
          }
        }
      }
    }
  }
}

ul {
  margin: 0;
  padding: 0;
}
li {
  list-style: none;  
  display: flex;
  align-items: flex-end;
}  
img {
  position:relative;
  z-index:-1;
  border-radius: 10px;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: 0.3s;
}

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
  background-color: rgb(0,0,0);
  background-color: rgba(0,0,0,0.9); 
}

.modal-content {
  margin: auto;
  display: block;
  width: 80%;
  max-width: 72%;
  object-fit: cover;
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

</style>


