<template>
  <div v-theme:column="'none'" id='show-blogs'>
    <h1> All Articles </h1>
    <input type='text' v-model='search' placeholder="search blogs" />
    <div v-for='blog in filterdBlogs' class='single-blog'> 
      <router-link v-bind:to="'/blog/' + blog.id"> <h2> {{ blog.title | to-uppercase }} </h2> </router-link>
      <article> {{ blog.content | snippet }} </article>

    </div>
   
  </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';


export default {
  data() {
    return {
      blogs: [],
      search: '',
    }
  },
  methods: {

  },
  created() {
    this.$http.get('https://vue-blog-47356.firebaseio.com/posts.json').then(function(data){
      return data.json()
    }).then(function(data){
      var blogsArray = [];
      for (let key in data){
        data[key].id = key
        blogsArray.push(data[key])
      }
      this.blogs = blogsArray;
    })
  },
  computed: {
    // filterdBlogs: function(){
    //   return this.blogs.filter((blog) => {
    //     return blog.title.match(this.search);
    //   });
  },
  filters: {
    'to-uppercase': function(value){
        return value.toUpperCase()

    },
    // other way of writing the function
    toUppercase(value){
      return value.toUpperCase();
    }
  },
  directives: {
    'rainbow': {
      bind(el, binding, vnode){
        el.style.color = '#' + Math.random().toString().slice(2,8);
      }
    }
  },
  mixins: [searchMixin]
}

</script>

<style>
#show-blogs{
  max-width: 800px;
  margin: 0px auto;
}
.single-blog{
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eee;
}
</style>
