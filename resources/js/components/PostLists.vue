<template>

     <div class="row">
         <form action="#" @submit.prevent="createPost()">
            <div class="input-group">
                <input v-model="post.title" type="text" name="title" class="form-control" autofocus>
                <textarea v-model="post.body" name="body" class="form-control">
                </textarea>
                <span class="input-group-btn">
                    <button type="submit" class="btn btn-primary">New Post</button>
                </span>
            </div>
        </form>

         <button id="show-modal" @click="showModal = true">Show Modal</button>
  <!-- use the modal component, pass in the prop -->
  <modal v-if="showModal" @close="showModal = false">
    <!--
      you can use custom content here to overwrite
      default content
    -->
    <h3 slot="header">custom header</h3>
  </modal>
      <button @click="showCreatePost(post.id)" class="btn btn-default btn-xs pull-right">Create Post</button>
      <li v-if='list.length === 0'>There are no posts yet!
      
      </li>
      <!-- Post Content Column -->
      <div class="col-lg-8" v-for="(post, index) in list">
        <!-- Title -->
        <h1 class="mt-4">{{ post.title }}</h1>
        <!-- Post Content -->    
        <hr>
        <p>{{ post.body }}</p>
        <button @click="deletePost(post.id)" class="btn btn-danger btn-xs pull-right">Delete</button>
      </div>
    </div>

</template>
<script>
    export default {
        data() {
            return {
                list: [],
                post: {
                    id: '',
                    title: '',
                    body: ''
                }
            };
        },
        
        created() {
            this.fetchPostList();
        },
        
        methods: {
            fetchPostList() {
                axios.get('api/posts').then((res) => {
                    this.list = res.data;
                });
            },
 
            createPost() {
                axios.post('api/posts', this.post)
                    .then((res) => {
                        this.post.title = '';
                        this.post.body = '';
                        this.edit = false;
                        this.fetchPostList();
                    })
                    .catch((err) => console.error(err));
            },
 
            deletePost(id) {
                axios.delete('api/posts/' + id)
                    .then((res) => {
                        this.fetchPostList()
                    })
                    .catch((err) => console.error(err));
            },
        }
    }
</script>
