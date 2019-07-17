<template>
  <div class="about">
    <section>
      <div class="container">
        <div class="header-search">
          <span class="fa fa-search"></span>
          <div>  <input type="text" name="" id="" placeholder="Search for Photos.."></div>
        </div>
        <div class="image-container">
          <ul class="grid">
            <vcl-facebook v-if="!images"></vcl-facebook>
            <li v-else  @click="modal(image)" class="gallery__image " v-for="image in images" :key="image">
              <img id="myImg" :src="image" alt="Avatar" class="image">
              <div class="text">
                <h1>Flying Kites</h1>
                <h3>London, Mumbai</h3>
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
import { VclFacebook } from 'vue-content-loading';
import axios from 'axios'
export default {
  components: {
    VclFacebook
  },
  name: 'list-images',
  data: function () {
    return {
      images: []
    }
  },
  methods: {
    getImage: function () {
      const baseURI = 'https://api.unsplash.com/photos/?client_id=b9f69c845aa5a486905c09075f2ec48d4d8aec94c4630fd28e3a78ebfaf5df7d'
      axios.get(baseURI)
      .then((result) => {
        let response = result.data
        response.forEach(element => {
          console.log(element.urls.small)
          this.images.push(element.urls.small)
        });
      })
    },
    modal: function (image) {
      let modal = document.getElementById("myModal")
      let modalImg = document.getElementById("img01")
      modal.style.display = "block";
      modalImg.src = image;
    },
    close: function () {
      var span = document.getElementsByClassName('close')[0];
      let modal = document.getElementById("myModal")
      modal.style.display = 'none'
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
    z-index: -999999999;
    input {
      height: 57px;
      width: 100%;
      font-size: 20px;
      border: 1px solid #dde2e9;
      border-radius: 10px;
      text-indent: 65px;
      
      &:focus, &:active {
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
    top: -50px;
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
          }
          h3 {
            margin: 0px;
            font-size: 11px;
            font-weight: 500;
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
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.9); /* Black w/ opacity */
}

/* Modal Content (image) */
.modal-content {
  margin: auto;
  display: block;
  width: 80%;
  // height: 520px;
  max-width: 72%;
  object-fit: cover;
}

/* Caption of Modal Image */
#caption {
  margin: auto;
  display: block;
  width: 80%;
  max-width: 700px;
  text-align: center;
  color: #ccc;
  padding: 10px 0;
  height: 150px;
}

/* Add Animation */
.modal-content, #caption {  
  -webkit-animation-name: zoom;
  -webkit-animation-duration: 0.6s;
  animation-name: zoom;
  animation-duration: 0.6s;
}

@-webkit-keyframes zoom {
  from {-webkit-transform:scale(0)} 
  to {-webkit-transform:scale(1)}
}

@keyframes zoom {
  from {transform:scale(0)} 
  to {transform:scale(1)}
}

/* The Close Button */
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

/* 100% Image Width on Smaller Screens */
@media only screen and (max-width: 700px){
  .modal-content {
    width: 100%;
  }
}
</style>


