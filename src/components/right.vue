<template>
  <div class="right">
    <div class="top-right">
      <p>Suggeriti per te</p>
      <p><strong>Mostra tutti</strong></p>
    </div>

<!-- condizione per mostrare api solo dopo 4 secondi di caricamento -->
    <div v-if="loaded == true" class="suggested">
      <div v-for="person in suggested" :key="person.id" class="person">
        <img
          :src="person.profile_picture"
          alt=""
        />

        <p><strong>{{person.profile_name}}</strong></p>

        <p style="color:blue;">Segui</p>
      </div>

    </div>

  <!-- imposto che inizialmente si debba vedere lo skeleton -->
    <div v-if="loaded == false">
      <div v-for="item in 8" :key="item.id" class="skeleton mt-2">
        <b-skeleton type="avatar"></b-skeleton>

        <b-skeleton class="mt-2" width="30%"></b-skeleton>

        <b-skeleton class="mt-2" width="30%"></b-skeleton>
      </div>
    </div>


  </div>
</template>

<script>
import axios from 'axios';
// import "skeleton-screen-css";

export default {
  name: "right",
  // props: {
  //   msg: String,
  // },
  data(){
    return{
      suggested:null,
      loaded:false
    }
  },
  mounted() {
      axios.get("https://flynn.boolean.careers/exercises/api/boolgram/profiles")
         .then(response => {
           this.suggested = response.data;
           console.log(this.suggested);
         })
  // inserisco timeout per mostrare skeleton per 4 secondi
    setTimeout(() => { this.loaded = true }, 4000);
  }

};
</script>

<style scoped lang="scss">
.right {
  width:25%;
  padding: 60px 30px 0 22px;

  .top-right{
    display:flex;
    justify-content: space-between;
    font-size: 15px;
  }

  .suggested{
    display:flex;
    flex-direction: column;
    align-items: flex-start;

    .person{
      width: 100%;
      margin-bottom: 10px;
      display: flex;
      flex-direction: row;
      justify-content: space-between;

      p{
        padding-top:10px;
        font-size: 13px;
      }

      img{
        width: 40px;
        height: 40px;
        border-radius: 50%;
      }
    }
  }
}

.skeleton{
  width: 100%;
  margin-bottom: 10px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>
