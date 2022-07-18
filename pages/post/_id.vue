<template>
  <section class="container">
    <div>
      <h1 class="title">
        {{post.title.rendered}}
      </h1>
      <span v-html="post.content.rendered"></span>
    </div>
  </section>
</template>
<script>
import axios from 'axios'
export default {
  head () {
    return {
      title: this.post._yoast_wpseo_title,
      meta: [
        { hid: 'description', id: 'description', name: 'description', content: this.post._yoast_wpseo_metadesc }
      ]
    }
  },
  asyncData ({ params }) {
    return axios.get(`http://seocom.agency/wp-json/wp/v2/posts/${params.id}`)
      .then(response => {
        console.log(response.data._yoast_wpseo_metadesc)
        return { post: response.data }
      })
      .catch((error) => {
        return { error: error }
      })
  },
  data () {
    return {
      post: {},
      error: []
    }
  }
}
</script>
<style>
.container {
  margin: 0 auto 20px;
  min-height: 100vh;
  padding: 20px;
}
.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
  margin-bottom: 20px;
}
h1, h2, h3, h4 {
    font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    color: #35495e;
    margin: 20px 0 10px;
}
a {
  color: #35495e;
}
p, ul, li {
  color: #444;
} 
img {
    margin: 10px auto;
    display: block;
}
</style>