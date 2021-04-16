<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <BloccoPrincipale msg="Alessio funziona" />
  </div>
</template>

<script>
// @ is an alias to /src
import BloccoPrincipale from "@/components/BloccoPrincipale.vue";

export default {
  name: "Home",
  components: {
    BloccoPrincipale,
  },
};
</script>
