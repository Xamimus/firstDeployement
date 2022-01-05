<template>
    <div>
        <h1>Clique-moi !</h1>
        <button @click="increment">{{count}}</button>
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
    console.log('mounted');
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