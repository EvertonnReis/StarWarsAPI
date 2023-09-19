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
      <div class="navigation-buttons">
        <button @click="previousPage" class="previous-button">Previous Page</button>
        <button @click="nextPage" class="next-button">Next Page</button>
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
          'Beru Whitesun lars': 'https://i.pinimg.com/564x/85/42/34/8542346dfe430a520f42f4dd9e6e74fa.jpg',
          'Anakin Skywalker': 'https://www.sideshow.com/storage/product-images/912537/anakin-skywalker-artisan-edition_star-wars_square.jpg',
          'Wilhuff Tarkin': 'https://upload.wikimedia.org/wikipedia/en/5/5a/Grand_Moff_Tarkin.png',
          'Chewbacca': 'https://p2.trrsf.com/image/fget/cf/1200/1200/middle/images.terra.com/2018/04/16/chewbacca.jpg',
          'Han Solo': 'https://cinemaclassico.com/wp-content/uploads/2019/07/Star-Wars-Han-Solo-Leather-.jpg',
          'Greedo': 'https://static.onecms.io/wp-content/uploads/sites/6/2015/03/greedo-2000.jpg',
          'Jabba Desilijic Tiure': 'https://lumiere-a.akamaihd.net/v1/images/Jabba-The-Hutt_b5a08a70.jpeg?region=150%2C0%2C900%2C675',
          'Wedge Antilles': 'https://upload.wikimedia.org/wikipedia/en/4/41/Wedge_Antilles-Denis_Lawson-Star_Wars_%281977%29.jpg',
          'Jek Tono Porkins': 'https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/004baf4b-16cb-433e-8eb1-b46b5fd07029/dejwm1l-bee035ed-1e56-4d4e-8eeb-93fb5b001cbc.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzAwNGJhZjRiLTE2Y2ItNDMzZS04ZWIxLWI0NmI1ZmQwNzAyOVwvZGVqd20xbC1iZWUwMzVlZC0xZTU2LTRkNGUtOGVlYi05M2ZiNWIwMDFjYmMuanBnIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.87f3Ba9EsUTKCS6CjeBw-CwAEYxZSNmNLGSlnPA5G1E',
          'Yoda': 'https://upload.wikimedia.org/wikipedia/pt/thumb/6/6f/Yoda_Attack_of_the_Clones.png/200px-Yoda_Attack_of_the_Clones.png',
          'Palpatine': 'https://oyster.ignimgs.com/mediawiki/apis.ign.com/star-wars-episode-7/0/02/Palpatine_1.jpg?width=1280',
          'Boba Fett': 'https://ik.imagekit.io/eywz9hvpg/pow/media/gallery/6400/bof-bobafett-bust-01-85037-1639004393.jpg?v=020122040934',
          'IG-88': 'https://i.redd.it/what-was-exactly-the-difference-between-ig-88-and-ig-11-and-v0-6d3x6naan7p81.jpg?width=593&format=pjpg&auto=webp&s=3d75591aea5b960fdc35cb7f77c5f388f7948cbd',
          'Bossk': 'https://lumiere-a.akamaihd.net/v1/images/databank_bossk_01_169_c3c42fbe.jpeg?region=0%2C0%2C1560%2C878'
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
      console.log(this.currentPage)
      await this.loadCharacters();
    },
    async previousPage() {
      this.currentPage--;
      console.log(this.currentPage)
      await this.loadCharacters();
    },
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
  margin-bottom: 70px;
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
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
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
</style>
