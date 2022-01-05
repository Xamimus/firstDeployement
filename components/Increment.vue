<template>
    <div>
        <h1 @click="increment">Clique-moi !</h1>
        <h2>J'en suis déjà à {{count}}...</h2>
    </div>
</template>

<script>
export default {
  data() {
      return {
          count: 0,
      }
  },
  mounted(){
      this.$fire.firestore
      .collection('increment')
      .doc('p2FIC2VjhWcjh8QKbdu9')
      .get()
      .then((e) => {
        let data = e.data();
        this.count = data.count;
      });
  },
  methods: {
      increment(){
          this.count ++;

          this.$fire.firestore
            .collection('increment')
            .doc('p2FIC2VjhWcjh8QKbdu9')
            .set({count: this.count});
      }
  },
}
</script>

<style scoped>
h1:hover{
    transform: scale(1.02);
}
h1 {
    color: rgb(8, 2, 29);
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-size: 25;
}
h2 {
    font-size: 20;
}
</style>