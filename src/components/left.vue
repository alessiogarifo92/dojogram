<template>

  <div class="left">

    <div class="pop-up" v-if="showStory == true && indexImg !== undefined"  :class="{storyOnScreen :showStory}">

        <img class="col-4" :src="stories[indexImg].profile_picture"  alt="">
        <img class="closing" @click="closeStory" src="https://image.flaticon.com/icons/png/128/753/753345.png" alt="">
    </div>

<!-- class="stories" -->
    <div>
      <!-- skeleton stories -->
      <div class="w-100 d-flex flex-row px-3" v-if="loaded == false">
        <div v-for="item in 8" :key="item.id" class="mr-5 skeleton mt-2">
          <b-skeleton type="avatar"></b-skeleton>

          <b-skeleton class="mt-2" width="150%"></b-skeleton>

        </div>
      </div>

<!--  stories -->
      <div class="stories" v-if="loaded == true">
        <div v-for="(profile, ind) in stories" :key="profile.id" class="story">
          <img @click="storyView(ind)"
            :src="profile.profile_picture" alt=""
          />
          <p>{{profile.profile_name}}</p>

        </div>
      </div>

    </div>

<!-- skeleton posts -->
    <div v-if="loaded == false">
      <b-card v-for="item in 2" :key="item.id" class="mt-4" no-body img-top>
        <div class="d-flex m-2">
          <b-skeleton type="avatar"></b-skeleton>

          <b-skeleton class="ml-2 mt-2" width="10%"></b-skeleton>
        </div>

        <b-skeleton-img card-img="top" aspect="3:1"></b-skeleton-img>
        <b-card-body>
        <b-skeleton class="mt-2" width="30%"></b-skeleton>
        <b-skeleton class="mt-2" width="30%"></b-skeleton>
        <b-skeleton class="mt-2" width="40%"></b-skeleton>
        <b-skeleton class="mt-2" width="30%"></b-skeleton>
        <b-skeleton class="mt-2" width="30%"></b-skeleton>
        <b-skeleton class="mt-2" width="30%"></b-skeleton>
        </b-card-body>
      </b-card>

    </div>

<!--  posts -->
    <div v-if="loaded == true">
      <div v-for="(post,index) in posts" :key="post.id" class="post">
        <div class="friendProf">
          <img
            :src="post.profile_picture"
            alt="Pippo franco"
          />
          <p>{{post.profile_fullname}}</p>
        </div>

        <div class="mainPic">
          <img
            :src="post.post_image"
            alt=""
          />
        </div>

        <div class="reactions">
          <!-- @dblclick for double click event -->
          <i v-if="liked == false" @dblclick="addLike()" class="far fa-heart"></i>
          <i v-else class="fa fa-heart"  @dblclick="addLike()"  style="color:red;"></i>
          <i class="far fa-comment"></i>
        </div>

        <div class="likes">
          <img
          v-if="post.likes.length !== 0 "
            :src="post.likes[0].profile_picture"
            alt=""
          />
          <!-- add if conditions above and below because if array empty didnt work -->
          <p v-if="post.likes.length !== 0">Piace a <strong>{{post.likes[0].username}}</strong> e <strong>altri {{post.likes.length - 1}}</strong></p>
        </div>

        <div class="myComment">
          <p><strong>{{post.profile_name}}</strong> {{post.post_text}}</p>
        </div>

        <p class="mostraComm" @click="show()">Mostra tutti e <span>{{post.comments.length}}</span> i commenti</p>

        <div class="comment" :key="i" v-for="(comment,i) in post.comments">
          <!-- condition is false display only to i<=2 otherwise all comment -->
           <span v-if="isActive == false? i<=2: comment"><strong>{{comment.username}}</strong> {{comment.text}}</span>
        </div>

        <div class="time-ago">
          <span>
            posted
            <time-ago :datetime="new Date(post.date.date)"></time-ago>
            ago
          </span>

        </div>

        <div class="publicComment">
          <input @keyup.enter="addComment(index)" v-model="comment" type="text" name="" value="" placeholder="Scrivi un commento..."><button @click="addComment(index)" type="button" name="button">Pubblica</button>
        </div>
      </div>
    </div>


  </div>
</template>

<script>
import axios from 'axios';
import TimeAgo from 'vue2-timeago';
export default {
  name: "left",
  components: {
  TimeAgo,
  },
  // props: {
  //   msg: String,
  // },
  data(){
    return{
      stories:null,
      posts:null,
     isActive: false,
     loaded:false,
     showStory: false,
     indexImg:null,
     comment:'',
     liked: false
    }
  },
  mounted() {
    axios.get("https://flynn.boolean.careers/exercises/api/boolgram/profiles")
       .then(response => {
         this.stories = response.data;
           console.log(this.stories);
       })
// non mettere virgola tra funzione e funzione senno da errore
     axios.get("https://flynn.boolean.careers/exercises/api/boolgram/posts")
      .then(response => {
        this.posts = response.data;
          console.log(this.posts);
      })

      setTimeout(() => { this.loaded = true }, 4000)

  },

  methods: {
    show(){
        this.isActive =!this.isActive;
      },

    storyView(ind){
      this.indexImg= ind;
      this.showStory = true;
      console.log(this.indexImg);
    },

    closeStory(){
      this.showStory= false;
    },

// add comments method(push an array to get the new info displayed)
    addComment(index){
      if (this.comment !== '') {
        this.posts[index].comments.push( {
          text:this.comment,
          username: 'Alessio G'
        });
        this.comment = ""
        console.log(this.comment,index);
      }

    },

    addLike(){
      this.liked = !this.liked;
    }
  }
};
</script>

<style scoped lang="scss">
.left {
  width: 70%;

  .pop-up{
    display:flex;
    justify-content:center;
    align-items:center;
    background-color: rgba(0,0,0,0.7);
    height:100%;
    width:100%;
    position:absolute;
    top: 70px;
    left: 0;
    overflow-y: hidden;
    z-index:1;

    .closing{
      position:absolute;
      top:45px;
      right:450px;
      width:30px;
      height:30px;
    }

    img{
      top: calc(32% - 225px);
      width:500px;
      height:450px;
    }
  }

  .stories {
    border: 1px solid lightgray;
    padding: 15px 10px;
    overflow-x: auto;
    max-height: 95px;
    display: flex;
    justify-content: flex-start;
    box-shadow: 0px 0px 13px lightgrey;

    .skeleton{
      max-width:100px;
      margin-right:15px;
      overflow-x: hidden;
      }
    .story{
      max-width:100px;
      margin-right:15px;
      .storyOnScreen{
        position: absolute;
        background:white;
        width:300px;
        height:300px;
      }
      img {
        width: 50px;
        height: 50px;
        border-radius: 50%;
        margin: 0 10px;
        padding: 3px;
        border: 2px solid crimson;
      }
      p{
        font-size:11px;
        font-weight:bold;
      }
    }

  }
  // hide scrollbar but keep funtionality
  .stories::-webkit-scrollbar {
    display: none;
  }

  .post {
    border: 1px solid lightgray;
    // height: 400px;
    // width:100%;
    margin-top: 30px;

    .friendProf {
      display: flex;
      justify-content: flex-start;
      align-items: flex-end;
      margin: 10px;

      img {
        width: 50px;
        height: 50px;
        border-radius: 50%;
        margin-right: 10px;
      }
    }

    .mainPic {
      img {
        width: 100%;
        height:fit-content;
      }
    }

    .reactions {
      display: flex;
      margin: 10px;

      i {
        font-size: 20px;
      }

      .fa-comment {
        margin-left: 10px;
      }
    }

    .likes{
      display:flex;
      margin-left:10px;
      font-size:10px;
      img{
        width: 20px;
        height: 20px;
        border-radius: 50%;
        margin-right:10px;
      }
    }

    .myComment{
      margin-left:10px;
      text-align:left;
      font-size:12px;
      strong{
        margin-right: 10px;
      }
    }

    .mostraComm{
      color:gray;
      text-align: left;
      margin-left: 10px;
    }

    .comment{
      display: flex;
      align-items: flex-start;
      margin-left: 10px;
      font-size: 13px;

      span{
        margin: 1px 10px 0 0;
      }
    }

    .time-ago{
      color: #657786;
      text-align: left;
      margin: 10px 20px;
      font-size: 13px;
    }

    .publicComment{
      margin-top:15px;
      input{
        width:79.3%;
        border: 1px solid lightgray;
        border-right:none;
        border-left:none;
        padding-top:5px;
        outline:none;
        padding-left: 10px;
      }
      button{
        border:1px solid lightgray;
        border-left:none;
        border-right:none;
        color:blue;
        background-color:white;
        padding-top:5px;
        width: 20.7%;
      }
    }

  }
}
</style>
