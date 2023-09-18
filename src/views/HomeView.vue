<template>
  <div class="home">
    <div class="character-list">
      <div
        v-for="(character, index) in characters"
        :key="index"
        class="character-card"
      >
        <img :src="character.image" alt="Character Image" class="character-image" />
        <h2 class="character-name">{{ character.name }}</h2>
        <p class="character-description">{{ character.description }}</p>
      </div>
      <button @click="nextPage" class="next-button">Próxima Página</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      characters: [],
      currentPage: 1
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
          'Luke Skywalker': 'https://media.contentapi.ea.com/content/dam/star-wars-battlefront-2/images/2019/08/swbf2-refresh-hero-large-heroes-page-luke-skywalker-16x9-xl.jpg.adapt.crop1x1.320w.jpg',
          'Leia Organa': 'https://i.pinimg.com/236x/55/87/e1/5587e12ecf397985ea4efd0df24cb25d.jpg',
          'Darth Vader': 'https://i.pinimg.com/564x/52/97/c9/5297c9e8792188ea0383f854cd98ec39.jpg',
          'Owen Lars': 'https://lumiere-a.akamaihd.net/v1/images/owen-lars-main_08c717c8.jpeg?region=8%2C0%2C774%2C774',
          'C-3PO': 'https://i.pinimg.com/564x/3d/75/57/3d7557db8207fd6c85782c28895f455e.jpg',
          'R2-D2': 'https://i.pinimg.com/236x/76/dc/dd/76dcdd8155085664f3ba632ed002e5d8.jpg',
          'Obi-Wan Kenobi': 'https://i.pinimg.com/564x/a0/68/b5/a068b5ae8742339172e81c418cef5a05.jpg',
          'R5-D4': 'https://i.pinimg.com/564x/47/0f/4f/470f4fe1c0a899608873ad6fb7350fa3.jpg',
          'Biggs Darklighter': 'https://themovieandthemuse.files.wordpress.com/2016/12/biggs-darklighter.jpg?w=640',
          'Beru Whitesun lars': 'https://i.pinimg.com/564x/85/42/34/8542346dfe430a520f42f4dd9e6e74fa.jpg'
        };

        this.characters = charactersData.map(character => ({
          name: character.name,
          description: `Height: ${character.height}cm, Mass: ${character.mass}kg`,
          image: characterImages[character.name] || '' 
        }));

      } catch (error) {
        console.error('Erro ao obter dados dos personagens da API:', error);
      }
    },
    async nextPage() {
      this.currentPage++;
      await this.loadCharacters();
    },
  },
};

</script>

<style>
.home {
  text-align: center;
}

.character-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
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
  margin: 10px 0;
}

.character-description {
  font-size: 16px;
}
</style>
