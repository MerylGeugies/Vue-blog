<template>
  <div id='add-blog'>
    <h2> Add a new blog post </h2>
    <form v-if='!submitted'>
      <label> Blog Title: </label>
      <input type='text' v-model.lazy='blog.title' required />
      <label> Blog Content: </label>
      <textarea v-model.lazy='blog.content'> </textarea>
      <div id='checkboxes'>
        <label> Food </label>
        <input type='checkbox' value='food' v-model='blog.categories'/>
        <label> Travel </label>
        <input type='checkbox' value='travel' v-model='blog.categories'/>
        <label> Books </label>
        <input type='checkbox' value='books' v-model='blog.categories'/>
        <label> Favorites </label>
        <input type='checkbox' value='favorites' v-model='blog.categories'/>
      </div>
      <label> Author: </label>
      <select v-model='blog.author'>
        <option v-for='author in authors'> {{ author }}</option> 
      </select>
      <button v-on:click.prevent='post'> Add Blog </button>
    </form>

    <div v-if='submitted'> 
      <h3> You posted on the blog!</h3> 
    </div>

    <div id='preview'>
      <h3> Preview Blog </h3>
      <p> Blog Title: {{ blog.title }}</p>
      <p> Blog Content: </p>
      <p> {{ blog.content }} </p>
      <p> Blog Categories: </p>
      <ul>
        <li v-for='category in blog.categories'> {{ category }} </li>
      </ul>
      <p> Author: {{ blog.author }}</p>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      blog: {
        title: '',
        content: '',
        categories: [],
        author: ''
      },
      authors: ['Piet Hein', 'Jacob van Heemskerck', 'Michiel de Ruyter'],
      submitted: false,
    }
  },
  methods: {
    post: function(){
      this.$http.post('https://vue-blog-47356.firebaseio.com/posts.json',
        this.blog).then(function(data){
          console.log(data);
          this.submitted = true;
      });
    }
  }
}

</script>

<style>
#add-blog *{
    box-sizing: border-box;
}
#add-blog{
    margin: 20px auto;
    max-width: 500px;
}
label{
    display: block;
    margin: 20px 0 10px;
}
input[type="text"], textarea{
    display: block;
    width: 100%;
    padding: 8px;
}
#preview{
    padding: 10px 20px;
    border: 1px dotted #ccc;
    margin: 30px 0;
}
h3{
    margin-top: 10px;
}
#checkboxes input{
  display: inline-block;
  margin-right: 10px;
}
#checkboxes label{
  display: inline-block;
}
</style>
