<template>
  <div>
    <h3 class="text-success mt-3"> Sort by popularity </h3>
    <!-- <button @click="searchName">busca</button>-->
    <div class="row">
      <div v-for="search in fullList" :key="search.id">
          <div class="card border border-0 shadow-lg p-2 m-4" style="width: 18rem;">
            <img v-bind:src="search.poster_path" class="card-img-top border" alt="Photo">
            <div class="card-body">
              <h5 class="card-title"> {{ search.title }} </h5>
              <p class="card-text"> {{ search.overview }} </p>
              <a href="#" class="btn btn-primary">Show more</a>
            </div>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'search',
  data () {
    return {
      title: 'titulo',
      description: 'desc',
      fullList: [],
      nResults: 0
    }
  },
  mounted () {
    let link = 'https://api.themoviedb.org/3/search/multi?api_key=e9d8b222a57983dac6baa7919533097e&language=en-EU&page=1&include_adult=false&query='
    var name = document.getElementById('inputSearch').value
    axios.get('' + link + name)
      .then(response => {
        var aux = [...response.data.results]
        for (let i = 0; i < aux.length; i++) {
          // problema cuando es una persona
          if (aux[i].title === undefined) {
            aux[i].title = aux[i].name
          }
          if (aux[i].overview !== undefined && aux[i].overview.length > 100) {
            aux[i].overview = aux[i].overview.slice(0, 100) + ' ...'
          }
          if (aux[i].poster_path === undefined || aux[i].poster_path === null) {
            aux[i]['poster_path'] = 'https://www.ilv.com.mx/static/images/product_image_not_found.gif'
          } else {
            aux[i].poster_path = 'https://image.tmdb.org/t/p/original' + aux[i].poster_path
          }
        }
        this.fullList = aux
      })
      .catch(new Error('Error de peticion'))
    document.getElementById('buttonSearch').onclick = () => {
      this.fullList = []
      let link = 'https://api.themoviedb.org/3/search/multi?api_key=e9d8b222a57983dac6baa7919533097e&language=en-EU&page=1&include_adult=false&query='
      var name = document.getElementById('inputSearch').value
      axios.get('' + link + name)
        .then(response => {
          var aux = [...response.data.results]
          for (let i = 0; i < aux.length; i++) {
            // problema cuando es una persona
            if (aux[i].title === undefined) {
              aux[i].title = aux[i].name
            }
            if (aux[i].overview !== undefined && aux[i].overview.length > 100) {
              aux[i].overview = aux[i].overview.slice(0, 100) + ' ...'
            }
            if (aux[i].poster_path === undefined || aux[i].poster_path === null) {
              aux[i]['poster_path'] = 'https://www.ilv.com.mx/static/images/product_image_not_found.gif'
            } else {
              aux[i].poster_path = 'https://image.tmdb.org/t/p/original' + aux[i].poster_path
            }
          }
          this.fullList = aux
        })
        .catch(new Error('Error de peticion'))
    }
  }
}
</script>
