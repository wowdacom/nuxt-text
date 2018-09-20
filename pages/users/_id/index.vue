<template>
  <h1>{{ title }}</h1>
</template>

<script>
import axios from 'axios'

export default {
  validate({ params }) {
    return !isNaN(+params.id)
  },
  asyncData ({ params, error }) {
    return axios.get(`http://localhost:3000/users/${params.id}`)
    .then((res) => {
        console.log(res)
      return { title: res.data.title }
    })
    .catch((e) => {
      error({ statusCode: 404, message: 'Post not found' })
    })
  },
  data () {
      return {
          name: "Who",
          username: 'Jim',
          email: "www@hotmail.com"
      }
  }
}
</script>

<style scoped>
.user {
  text-align: center;
  margin-top: 100px;
  font-family: sans-serif;
}
</style>