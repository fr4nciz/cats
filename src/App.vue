<template>
  <div id="app">

    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Produtos Front</a>
      </div>
    </nav>

    <div class="container">

      <form>

        <label>Nome</label>
        <input type="text" placeholder="Nome">
        <label>Quantidade</label>
        <input type="number" placeholder="QTD">
        <label>Valor</label>
        <input type="text" placeholder="Valor">

        <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>

      </form>

      <table>

        <thead>

          <tr>
            <th>NOME</th>
            <th>QTD</th>
            <th>VALOR</th>
            <th>OPÇÕES</th>
          </tr>

        </thead>

        <tbody>

          <tr v-for="(breed, index) in breeds" :key="index">
            <th>{{breed.name}}</th>

            <td>
              <button class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
            </td>

          </tr>

          
          <tr v-for="item of images" :key="item.id">
            <th><img :src="item.url" width="200" /></th>     

          </tr>

        </tbody>

      </table>

    </div>

  </div>
</template>

<script>
  import axios from 'axios'
  // import Dogs from './services/dogs'

  export default {
    data() {
      return {
        items: [],
        country_flag_url: "",
        images: [],
        breeds: [],
        selected_breed: {},
        current_image: {}
      }
    },
    created() {
      this.getBreeds();
      this.getImages();
    },

    methods: {
      async getBreeds() {
        try {
          axios.defaults.headers.common['x-api-key'] =
            "DEMO-API-KEY" // Substitua isso por sua chave da API, pois ela está definida como padrão e será usada a partir de agora

          let response = await axios.get('https://api.thecatapi.com/v1/breeds/')
          this.breeds = response.data;

          // escolha um para exibir inicialmente
          this.selected_breed = this.breeds[10]
        } catch (err) {
          console.log(err)
        }

        console.log(this.breeds)
      },
      async getImages() {
        try {

          let query_params = {
            breed_ids: this.selected_breed.id,
            limit: 8
          }
          let response = await axios.get('https://api.thecatapi.com/v1/images/search' , {
            params: query_params
          })

          this.pagination_count = response.headers['pagination-count'];
          this.images = response.data
          this.current_image = this.images[0]

          console.log("-- (" + this.images.length + ") Images from TheCatAPI.com")
          console.log(this.pagination_count, 'images available for this query.')

        } catch (err) {
          console.log(err)
        }

                console.log(this.images)

      }
    },

    // mounted() {
    //   Dogs.listar().then(resposta => {
    //     console.log(resposta.data)
    //     this.items = resposta.data
    //   })
    // }

  }
</script>