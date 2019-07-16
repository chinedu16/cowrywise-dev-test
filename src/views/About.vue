<template>
  <div class="about">
    <section>
      <div class="container">
        <div class="header-search">
          <span class="fa fa-search"></span>
          <div>  <input type="text" name="" id="" placeholder="Search for Photos.."></div>
         
        </div>
        <div class="image-container">
          <div class="image-div">
            <img v-for="image in images" :key="image" :src="image" />
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import axios from 'axios'
export default {
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
    width: 100%;
    display: grid;
    justify-content: center;
    position: relative;
    top: -50px;

    .image-div{
      display: grid;
      grid-template-columns: repeat(3, 225px);
      // grid-template-rows: 0%;
      grid-gap: 40px;

      img {
        width: 225px;
        border-radius: 10px;
        // height: 250px;
      }
    }
  }
}
</style>


