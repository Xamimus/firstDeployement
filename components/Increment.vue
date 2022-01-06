<template>
  <div>
    <div class="container">
      <div class="col-6 offset-3">
        <div class="card shadow p-3 rounded mt-5">
          <div class="card-body hstack gap-5">
            <button class="btn btn-outline-success" @click="increment">
              <h5>Clique-moi !</h5>
            </button>
            <h5>
              <strong id="count">{{ count }}</strong>
            </h5>
          </div>
          <div class="mt-3 mx-auto">
            <a
              href="https://github.com/Xamimus/firstDeployement"
              target="_blank"
              style="color: grey"
              ><svg
                class="w-6 h-6 text-gray-600 hover:text-gray-800 button--github"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                aria-hidden="true"
                role="img"
                width="32"
                height="32"
                preserveAspectRatio="xMidYMid meet"
                viewBox="0 0 24 24"
              >
                <path
                  d="M12 2.247a10 10 0 0 0-3.162 19.487c.5.088.687-.212.687-.475c0-.237-.012-1.025-.012-1.862c-2.513.462-3.163-.613-3.363-1.175a3.636 3.636 0 0 0-1.025-1.413c-.35-.187-.85-.65-.013-.662a2.001 2.001 0 0 1 1.538 1.025a2.137 2.137 0 0 0 2.912.825a2.104 2.104 0 0 1 .638-1.338c-2.225-.25-4.55-1.112-4.55-4.937a3.892 3.892 0 0 1 1.025-2.688a3.594 3.594 0 0 1 .1-2.65s.837-.262 2.75 1.025a9.427 9.427 0 0 1 5 0c1.912-1.3 2.75-1.025 2.75-1.025a3.593 3.593 0 0 1 .1 2.65a3.869 3.869 0 0 1 1.025 2.688c0 3.837-2.338 4.687-4.563 4.937a2.368 2.368 0 0 1 .675 1.85c0 1.338-.012 2.413-.012 2.75c0 .263.187.575.687.475A10.005 10.005 0 0 0 12 2.247z"
                  fill="currentColor"
                /></svg
            ></a>
          </div>
        </div>
      </div>
    </div>
    <div class="container login-container">
        <div v-if="!logged">
            <button class="btn btn-outline-success" @click="toggleLogin">Connexion</button>
        </div>
				<div v-if="logged">
            <button class="btn btn-outline-danger" @click="toggleLogin">Déconnexion</button>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0,
			logged: false,
    };
  },
  mounted() {
    this.$fire.firestore
      .collection("increment")
      .doc("p2FIC2VjhWcjh8QKbdu9")
      .get()
      .then((e) => {
        let data = e.data();
        this.count = data.count;
      });

      let collection = this.$fire.firestore.collection('increment');

      try{
          collection.onSnapshot((data) => {
              data.forEach(d => {
                    this.count = d.data().count;
              })
          })
      } catch (e) {
          console.error(e)
      }

      this.$fireModule.auth().onAuthStateChanged((user) => {
			if (user) {
					console.log('Toggle Login')
					var uid = user.uid;
					this.logged = true;
			} else {
					console.log('Logging out')
					this.logged = false;
			}
			});
  },
  methods: {
    increment() {
        if(!this.logged) {
						alert("Merci de vous connecter pour pouvoir liker.")
            return;
        };

      this.count ++;

      this.$fire.firestore
        .collection("increment")
        .doc("p2FIC2VjhWcjh8QKbdu9")
        .set({ count: this.count });
    },
    toggleLogin() {
        if(!this.logged){
					this.$fireModule.auth().signInAnonymously().then(() =>
						console.log('Logged')
					).catch((error) => {
						console.error(error)
						})
				} else {
					this.$fireModule.auth().signOut().then(() => {
						alert("Vous avez bien été déconnecté.")
					}).catch((error) => {
						console.error(error)
						})
				}
    }
  },
};
</script>

<style scoped>
    #count{
        font-size: 25;
    }
		.login-container{
			display: flex;
			justify-content: center;
			align-items: center;
			margin-top: 20px;
		}
</style>