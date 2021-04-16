<template>
  <div class="container">
    <!-- {{msg}} -->
    <left />
    <right />
  </div>
</template>

<script>
import right from "@/components/right.vue";
import left from "@/components/left.vue";

export default {
  name: "BloccoPrincipale",
  props: {
    msg: String,
  },
  components: {
    left,
    right,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container {
  display: flex;
  justify-content: space-around;
}
</style>
