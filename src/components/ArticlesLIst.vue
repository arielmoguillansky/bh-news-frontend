<template>
  <div class="header">
    <h1>Stetic News</h1>
    <input v-model="searchValue" placeholder="Search">
  </div>
  <div v-if="resultQuery" >
  <div class="articleCard" v-for="article in resultQuery" :key="article.id">
    <div class="heading">
      <h3>
        {{article.title}}
      </h3>
      <h3>
        {{article.country}}
      </h3>
    </div>
    <div class="content">
      <p>
        {{article.text}}
      </p>
    </div>
    <div class="footer">
      <h3>
        {{article.publish_date}}
      </h3>
      <a :href="article.linked_article">Leer más en aqui!</a>
    </div>
  </div>
</div>
<div v-else>
  <h2>
    No se encontraron resultados
  </h2>
</div>
</template>

<script>
export default {
  data(){
    return{
      articles:[],
      searchValue: ''
    }
  },
  created: function(){
    this.getArticles()
  },
  methods: {
    getArticles(){
      fetch('https://bh-news-backend.herokuapp.com/api/getAllArticles.php')
      .then(res => res.json())
      .then((data) => {
        this.articles=[]
        if(data.length) {
          this.articles=data
        }
      })
      .catch(e => console.log(e))
    },
  },
    computed: {
    resultQuery() {
      if(this.searchValue.length > 0) {
        const result =  this.articles.filter(item => (item.title.toLowerCase().includes(this.searchValue.toLowerCase())));
        if(result.length > 0) return result
        else return null
      } else {
        return this.articles
      }
    }
  }
}
</script>