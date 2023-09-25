<template>
  <div class="home">
    <div class="character-list">
      <div
        v-for="(character, index) in characters"
        :key="index"
        class="character-card"
      >
        <img :src="character.image" alt="Character Image" class="character-image" />
        <h2 class="character-name">{{ character.name.toLowerCase() }}</h2>
        <p class="character-description">{{ character.description }}</p>
        <button type="button" class="btn btn-dark" @click="openModal(character)">Details</button>
      </div>
      <div class="navigation-buttons">
        <button @click="previousPage" class="previous-button">Previous Page</button>
        <button @click="nextPage" class="next-button">Next Page</button>
      </div>
      <div class="modal" v-if="showModal">
        <div class="modal-content">
          <h1>{{ selectedCharacter.name.toLowerCase() }}</h1>
          <h3>Films</h3>
          <ul class="film-list">
            <li v-for="(film, index) in selectedCharacter.films" :key="index" class="film-item">
              <template v-if="filmList[film]">
                <img :src="filmList[film].image" alt="Film Image" width="100" height="100">
                <span class="title-modal">{{ filmList[film].title }}</span>
              </template>
            </li>
          </ul>
          <h3>Planet</h3>
          <div v-if="planetDetails">
            <template v-if="planetList[selectedCharacter.planet]">
              <img :src="planetList[selectedCharacter.planet].image" alt="Planet Image" width="350" height="350">
              <span class="title-modal" style="margin-left: 10px;">{{ planetDetails.name }}</span>
            </template>
            <template v-else>
              <span>Planeta não encontrado.</span>
            </template>
          </div>
          <button @click="closeModal" class="btn btn-dark">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      characters: [],
      currentPage: 1,
      selectedCharacter: null,
      showModal: false,
      filmList: {
        'https://swapi.dev/api/films/1/':{
          title: 'A New Hope',
          image: 'src/img/movies/episodeIV.jpg'
        },
        'https://swapi.dev/api/films/2/':{
          title: 'The Empire Strikes Back',
          image: 'src/img/movies/episodeV.jpg'
        },
        'https://swapi.dev/api/films/3/':{
          title: 'Return of the Jedi',
          image: 'src/img/movies/episodeVI.jpg'
        },
        'https://swapi.dev/api/films/4/':{
          title: 'The Phantom Menace',
          image: 'src/img/movies/episodeI.jpg'
        },
        'https://swapi.dev/api/films/5/':{
          title: 'Attack of the Clones',
          image: 'src/img/movies/episodeII.jpg'
        },
        'https://swapi.dev/api/films/6/':{
          title: 'Revenge of the Sith',
          image: 'src/img/movies/episodeIII.jpg'
        },
      },
      planetList: {
        'https://swapi.dev/api/planets/1/': {
          name: 'Tatooine',
          image: 'src/img/planets/Tatooine.jpeg'
        },
        'https://swapi.dev/api/planets/2/': {
          name: 'Alderaan',
          image: 'src/img/planets/Aldera_City.png'
        },
        'https://swapi.dev/api/planets/3/': {
          name: 'Yavin IV',
          image: 'src/img/planets/yavin_iv.jpg'
        },
        'https://swapi.dev/api/planets/5/': {
          name: 'Dagobah',
          image: 'src/img/planets/Dagobah.jpg'
        },
        'https://swapi.dev/api/planets/7/': {
          name: 'Endor',
          image: 'src/img/planets/Endor.jpg'
        },
        'https://swapi.dev/api/planets/9/': {
          name: 'Coruscant',
          image: 'src/img/planets/Coruscant.jpg'
        },
        'https://swapi.dev/api/planets/11/': {
          name: 'Geonosis',
          image: 'src/img/planets/Geonosis.jpeg'
        },
      },
      planetDetails: null
      };
  },
  async mounted() {
    await this.loadCharacters();
  },
  methods: {
    async loadCharacters() {
      try {
        const response = await axios.get(`https://swapi.dev/api/people/?page=${this.currentPage}`);
        const charactersData = response.data.results;
        
        const characterImages = {
          'Luke Skywalker': 'src/img/people/1.jpg',
          'C-3PO': 'src/img/people/2.jpg',
          'R2-D2': 'src/img/people/3.jpg',
          'Darth Vader': 'src/img/people/4.jpg',
          'Leia Organa': 'src/img/people/5.jpg',
          'Owen Lars': 'src/img/people/6.jpg',
          'Beru Whitesun lars': 'src/img/people/7.jpg',
          'R5-D4': 'src/img/people/8.jpg',
          'Biggs Darklighter': 'src/img/people/9.jpg',
          'Obi-Wan Kenobi': 'src/img/people/10.jpg',
          'Anakin Skywalker': 'src/img/people/11.jpg',
          'Wilhuff Tarkin': 'src/img/people/12.jpg',
          'Chewbacca': 'src/img/people/13.jpg',
          'Han Solo': 'src/img/people/14.jpg',
          'Greedo': 'src/img/people/15.jpg',
          'Jabba Desilijic Tiure': 'src/img/people/16.jpg',
          'Wedge Antilles': 'src/img/people/18.jpg',
          'Jek Tono Porkins': 'src/img/people/19.jpg',
          'Yoda': 'src/img/people/20.jpg',
          'Palpatine': 'src/img/people/21.jpg',
          'Boba Fett': 'src/img/people/22.jpg',
          'IG-88': 'src/img/people/23.jpg',
          'Bossk': 'src/img/people/24.jpg',
          'Lando Calrissian': 'src/img/people/25.jpg',
          'Lobot': 'src/img/people/26.jpg',
          'Ackbar': 'src/img/people/27.jpg',
          'Mon Mothma': 'src/img/people/28.jpg',
          'Arvel Crynyd': 'src/img/people/29.jpg',
          'Wicket Systri Warrick': 'src/img/people/30.jpg',
          'Nien Nunb': 'src/img/people/31.jpg',
          'Qui-Gon Jinn': 'src/img/people/32.jpg',
          'Nute Gunray': 'src/img/people/33.jpg',
          'Finis Valorum': 'src/img/people/34.jpg',
          'Padmé Amidala': 'src/img/people/35.jpg',
          'Jar Jar Binks': 'src/img/people/36.jpg',
          'Roos Tarpals': 'src/img/people/37.jpg',
          'Rugor Nass': 'src/img/people/38.jpg',
          'Ric Olié': 'src/img/people/39.jpg',
          'Watto': 'src/img/people/40.jpg',
          'Sebulba': 'src/img/people/41.jpg',
          'Quarsh Panaka': 'src/img/people/42.jpg',
          'Shmi Skywalker': 'src/img/people/43.jpg',
          'Darth Maul': 'src/img/people/44.jpg',
          'Bib Fortuna': 'src/img/people/45.jpg',
          'Ayla Secura': 'src/img/people/46.jpg',
          'Ratts Tyerel': 'src/img/people/47.jpg',
          'Dud Bolt': 'src/img/people/48.jpg',
          'Gasgano': 'src/img/people/49.jpg',
          'Ben Quadinaros': 'src/img/people/50.jpg',
          'Mace Windu': 'src/img/people/51.jpg',
          'Ki-Adi-Mundi': 'src/img/people/52.jpg',
          'Kit Fisto': 'src/img/people/53.jpg',
          'Eeth Koth': 'src/img/people/54.jpg',
          'Adi Gallia': 'src/img/people/55.jpg',
          'Saesee Tiin': 'src/img/people/56.jpg',
          'Yarael Poof': 'src/img/people/57.jpg',
          'Plo Koon': 'src/img/people/58.jpg',
          'Mas Amedda': 'src/img/people/59.jpg',
          'Gregar Typho': 'src/img/people/60.jpg',
          'Cordé': 'src/img/people/61.jpg',
          'Cliegg Lars': 'src/img/people/62.jpg',
          'Poggle the Lesser': 'src/img/people/63.jpg',
          'Luminara Unduli': 'src/img/people/64.jpg',
          'Barriss Offee': 'src/img/people/65.jpg',
          'Dormé': 'src/img/people/66.jpg',
          'Dooku': 'src/img/people/67.jpg',
          'Bail Prestor Organa': 'src/img/people/68.jpg',
          'Jango Fett': 'src/img/people/69.jpg',
          'Zam Wesell': 'src/img/people/70.jpg',
          'Dexter Jettster': 'src/img/people/71.jpg',
          'Lama Su': 'src/img/people/72.jpg',
          'Taun We': 'src/img/people/73.jpg',
          'Jocasta Nu': 'src/img/people/74.jpg',
          'R4-P17': 'src/img/people/75.jpg',
          'Wat Tambor': 'src/img/people/76.jpg',
          'San Hill': 'src/img/people/77.jpg',
          'Shaak Ti': 'src/img/people/78.jpg',
          'Grievous': 'src/img/people/79.jpg',
          'Tarfful': 'src/img/people/80.jpg',
          'Raymus Antilles': 'src/img/people/81.jpg',
          'Sly Moore': 'src/img/people/82.jpg',
          'Tion Medon': 'src/img/people/83.jpg',
        };

        this.characters = charactersData.map(character => ({
          name: character.name,
          description: `Height: ${character.height}cm, Mass: ${character.mass}kg`,
          image: characterImages[character.name] || '',
          films: character.films,
          planet: character.homeworld
        }));

      } catch (error) {
        console.error('Erro ao obter dados dos personagens da API:', error);
      }
    },
    async nextPage() {
      this.currentPage++;
      console.log(this.currentPage)
      await this.loadCharacters();
    },
    async previousPage() {
      this.currentPage--;
      console.log(this.currentPage)
      await this.loadCharacters();
    },
    async openModal(character){
      this.selectedCharacter = character;
      this.showModal = true;

      try {
        const response = await axios.get(character.planet);
        this.planetDetails = response.data;
      } catch (error) {
        console.error('Erro ao obter dados do planeta da API:', error);
      }

    },
    closeModal(){
      this.selectedCharacter = null;
      this.showModal = false;
    }
  },
};

</script>

<style>
@import '../assets/star-warsfont.css';
.home {
  text-align: center;
}

.character-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 30px;
  margin-top: 10px;
}

.character-card {
  margin: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  width: 300px;
  text-align: center;
  background-color: #f9f9f9;
}

.character-image {
  max-width: 300px;
  max-height: 200px;
}

.character-name {
  font-size: 20px;
  margin: 10px;
  font-style: italic;
}

.character-description {
  font-size: 16px;
}

.next-button{
  margin: 10px;
  margin-top: 100px;
  max-width: 100px;
  max-height: 200px;
  position: absolute;
  right: 0;
}

.previous-button{
  margin: 10px;
  margin-top: 100px;
  max-width: 100px;
  max-height: 200px;
  position: absolute;
  left: 0;
}

.navigation-buttons {
  display: flex;
  justify-content: center;
  padding: 10px;
}

.modal {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  max-width: 80%;
  max-height: 80%;
  overflow: auto;
  text-align: center;
}

.modal button {
  margin-top: 10px;
}

.film-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.film-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.title-modal{
  margin-left: 10px;
}

</style>
