<template>
  <div class="about">
    <h1 class="title-h1">Star Wars Api</h1>
    <div>
      <p class="description">
        Este é um projeto desenvolvido em Vue.js, que faz a listagem dos personagens de Star Wars, vindo da SWAPI (API do Star Wars).
        <br>Feito todo em forma de cards e trazendo algumas informações sobre os personagens.
      </p>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    justify-content: center;
  }
  .title-h1{
    display: flex;
    align-items: center;
    justify-content: center;
    color: yellow;
  }
  .description{
    margin-top: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: yellow;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 20px;
  }
}
</style>
