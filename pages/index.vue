<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">Seocom Headless WordPress</h1>
      <div class="posts" v-if="posts">
        <div class="post" v-for="post in posts" :key="post.id" >
          <nuxt-link :to="{name: 'post-id', params: { id:post.id } }">
            <h1 class="subtitle">
              {{post.title.rendered}}
            </h1>
            <p>{{post.excerpt.rendered | strippedContent}}</p>
            <p class="read-more">Leer más</p>
          </nuxt-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      posts: {},
      error: []
    }
  },
  created() {
    this.fetchData()
  },
  watch: {
    '$route': 'fetchData'
  },
  methods: {
    fetchData() {
      return axios.get('https://seocom.agency/wp-json/wp/v2/posts/')
      .then(response => {
        this.posts = response.data;
        return;
      })
      .catch((error) => {
        return { error: error }
      })
    },
  },
  filters: {
    strippedContent: function(string) {
           return string.replace(/<\/?[^>]+>/ig, " "); 
    }
  },
}
</script>
<style>
.container {
  margin: 0 auto 20px;
  min-height: 100vh;
  text-align: center;
}
.title {
  font-family:'Quicksand','Source Sans Pro',-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,'Helvetica Neue',Arial,sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
.subtitle {
  font-weight: 300;
  font-size: 30px;
  line-height: 33px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
.links {
  padding-top: 15px;
}
.posts {
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
  margin-top: 20px;
}
.post {
  max-width: 350px;
  box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  padding: 10px;
  margin: 15px 10px;
}
a {
  text-decoration: none;
}
p {
  color: #444;
}
.read-more {
  background: #1e8775;
  color: white;
  font-weight: bold;
  margin-top: 10px;
  padding: 5px;
}
.post a {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
}
</style>