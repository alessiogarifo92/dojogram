<template>
  <div id="app">
    <div id="nav">
      <h1>Boolgram</h1>

      <div class="input-group">
        <input
          class="form-control py-2 rounded-pill mr-1 pr-5"
          type="search"
          placeholder="search"
        />
        <span class="input-group-append">
          <button class="btn rounded-pill border-0 ml-n5" type="button">
            <i class="fa fa-search"></i>
          </button>
        </span>
      </div>

      <div class="right-nav">
        <i class="far fa-heart"></i>

        <i class="fas fa-home"></i>

        <img id="profilePic" alt="Profile Pic" src="@/assets/AvatarMaker.svg" />
      </div>
      <!-- <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> -->
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px 30px 30px 4px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-bottom: 1px solid lightgray;
  overflow: hidden;
  position: fixed;
  background-color: white;
  width: 100%;
  height: 70px;
  z-index: 100;
  h1 {
    font-family: "Brush Script MT", cursive;
  }

  .input-group {
    width: 300px;
  }

  .right-nav {
    display: flex;
    align-items: center;
    i {
      font-size: 21px;
    }
    i.fas.fa-home {
      margin: 0 15px;
    }
    #profilePic {
      width: 50px;
      height: 50px;
      border-radius: 50%;
    }
  }
}

.home {
  padding-top: 100px;
}
</style>
