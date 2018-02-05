<template>
  <div class="hello">
    <h2>Comics Test</h2>

    <ul>
      <li v-for="comic in comics">
        <h2>{{ comic.title }}</h2>
        <p>{{ comic.description }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import md5 from 'md5';
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data () {
    return {
      comics: [],
      reversed: [],
      collection: {
        public_key: 'bf08c092e21f974cac255c16051f406c',
        private_key: '8185f9edd39c6adeb4a9dde724e3915d85f82b8b'
      }
    }
  },
  computed: {
        hashed () {
          let concat = this.unixtime + this.collection.public_key + this.collection.private_key
          return md5(concat)

    },
    unixtime () {
          let datenow = Date.now()
          var ts = Math.floor(datenow / 1000)
          return ts
        }

     },
  created () {
      this.getComic()
    },
  methods: {
      getComic () {
        axios.get('https://gateway.marvel.com/v1/public/comics', {
          params: {
            apikey: this.collection.public_key,
            hash: this.hashed,
            dateDescriptor: 'thisWeek'
          }
        })
        .then(response => {
          this.reversed = response.data.data.results
          this.comics = this.reversed.reverse()
        })
        .catch(e => {
          console.log(e)
        })
      },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
