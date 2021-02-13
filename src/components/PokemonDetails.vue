<template>
  <div class="details">
    <div class="details-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="" width="100" height="100">
      </div>
      <div v-if="pokemon" class="data">
        <h2>{{ pokemon.name }}</h2>
        <h3>Present in the following games</h3>
        <div class="games">
          <div class="game" v-for="(game, index) in pokemon.games" :key="index">{{ game }}</div>
        </div>
        <h3>Found in these cities</h3>
        <div class="cities">
          <div class="city" v-for="(city, index) in encounters" :key="index">{{ city }}</div>
        </div>
      </div>
    </div>
    <button class="close" @click="closeDetails">Close</button>
  </div>
</template>

<script>
export default {
  props: [
    'pokemonUrl',
    'imageUrl'
  ],
  data: () => {
    return {
      show: false,
      pokemon: {},
      encounters: []
    }
  },
  methods: {
    fetchData() {
      let request = new Request(this.pokemonUrl);
      fetch(request)
          .then((response) => {
            if (response.status === 200) return response.json();
          })
          .then((data) => {
            this.pokemon = data;
            this.show = true;
            this.pokemon.games = []
            data.game_indices.forEach(game_index => {
              this.pokemon.games.push(game_index.version.name.replace('-', ' '));
            });
            let encounters_request = new Request(data.location_area_encounters);
            fetch(encounters_request)
              .then((response) => {
                if (response.status === 200) return response.json()
              })
              .then((data) => {
                data.forEach((index) => {
                  this.encounters.push(index.location_area.name.replace(/-/g, ' '));
                })
              })
              .catch((error) => {
                console.throw(error)
              });
          })
          .catch((error) => {
            console.log(error);
          })
    },
    fetchEncounters() {
      let request = new Request(this.pokemon.encounterUrl);
      fetch(request)
        .then((response) => {
          if(response.status === 200) return response.json()
        })
        .then((data) => {
          console.log(data)

        })
        .catch((error) => {console.throw(error)})
    },
    closeDetails() {
      this.$emit('closeDetails');
    },
    fetchGames() {

    }
  },
  created() {
    this.fetchData();
    this.fetchEncounters();
  }
}
</script>

<style scoped>
.details {
  color: #35495e;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: fixed;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  width: calc(100% - 10px);
  height: calc(100vh - 20px);
  background: rgba(0, 0, 0, 0.7);
}

.details-view {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: relative;
  width: 100%;
  max-width: 800px;
  padding: 50px 0 0;
  background-color: whitesmoke;
  border-radius: 5px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
  0 10px 10px rgba(0, 0, 0, .2);
}

.image {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: -60px;
  width: 120px;
  height: 120px;
  background-color: #333;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
  0 10px 10px rgba(0, 0, 0, .2);
}

h2 {
  text-transform: capitalize;
}

.data {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  width: 100%;
  margin-bottom: 40px;
}


h3 {
  width: 90%;
  max-width: 650px;
  border-bottom: 1px solid #ccc;
}

.games, .cities {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  width: 90%;
  max-width: 650px;
}

.game, .city {
  margin: 0 10px 10px 0;
  padding: 5px 10px;
  border-radius: 20px;
  color: #fff;
  font-size: 0.8rem;
  letter-spacing: 2px;
  text-transform: capitalize;
}

.game {
  background-color: #0A2E50;
}

.city {
  background-color: #C73015;
}

.close {
  margin-left: 15px;
  position: relative;
  float: right;
  outline: none;
  border: solid 3px black;
  border-radius: 5px;
  background-color: #333;
  color: whitesmoke;
  padding: 10px 20px;
  margin-bottom: 20px;
  font-size: 1.2rem;
  cursor: pointer;
}
</style>
