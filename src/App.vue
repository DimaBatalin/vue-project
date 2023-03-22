<template>
  <div id="app">

    <post-form
      @add="addPost"
    ></post-form>

    <div class="posts">
      <h2>Список постов</h2> 

      <input 
        v-model="searchString"
        type="text" 
        placeholder="Поиск постов" 
        style="margin-bottom: 20px;"
      >

      <Post-list
        @delete-post="deletePost"
        :posts="searchedPost"
      ></Post-list>  
    </div>

    
  
    <!-- <div>
      Количество лайков: {{ likes }}
    </div>
    <div>
      Количество дизлайков: {{ dislikes }}
    </div>
    <div>
      <button class="like" @click="addLike">Лайк</button>
      <button class="dislike" @click="addDislike">Дизлайк</button>
    </div> -->
  </div>
</template>

<script>
import PostList from "./components/PostList.vue"
import PostForm from "./components/PostForm.vue"
import axios from "axios"

export default {
  name: "App",
  components: { 
      PostList, PostForm,
  },
  data() {
    return {
      title: "",
      text: "",
      posts: [],
      searchString: "",
    };
  },
  computed: {
    searchedPost() {
      const sortedPosts = []
      for (const post of this.posts){
        if (post.title.includes(this.searchString)){
          sortedPosts.push(post)
        }
      }
      return sortedPosts
    },
  },
  methods: {
    // addLike() {
    //     this.likes += 1;
    // },
    // addDislike() {
    //     this.dislikes += 1;
    //   },
    addPost(post) {
      this.posts.push({
        ...post
      });
    },
    deletePost(index, text) {
      // console.log(index, text)
      console.log(text);
      this.posts.splice(index, 1);
    }, 
    async getPosts() {
      const url = "https://jsonplaceholder.typicode.com/posts"
      try {
        const response = await axios.get(url)
        console.log(response.data);
        this.posts = response.data
      } catch (error) {
        console.log("ОШИБКА");
        console.log(error);
      }
    },
  },
  async created() {
    await this.getPosts()
  },
    
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 60px 20px 0 20px;
}

.like {
  background: green;
  color: white;
  cursor: pointer;
}

.dislike {
  background: red;
  color: white;
  cursor: pointer;
}

.form {
  margin-bottom: 30px;
}

h1 {
  margin-bottom: 10px;
}

h2 {
  margin-bottom: 10px;
}

h4 {
  margin-bottom: 8px;
}

input {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid teal;
}

button {
  padding: 10px;
  background: none;
  border: 1px solid teal;
  color: teal;
  cursor: pointer;
}

.post {
  width: 100%;
  padding: 15px;
  border: 2px solid teal;
  margin-bottom: 10px;
}
</style>
