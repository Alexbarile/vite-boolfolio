<script>
import axios from 'axios';
import { store } from '../store';
import PostCard from '../components/PostCard.vue';

export default{
    name: "PostList",
    data(){
        return{
            store,
            posts: [],
            loading: true,

            // per il PAGINATE
            lastPage: null,
            currentPage: 1,
        }
    },
    components:{
        PostCard,
    },
    methods: {
        getPosts(post_page){
            this.loading = true;
            axios.get(`${this.store.baseUrl}/api/posts`, { params: { page: post_page }}).then((response) => {
                if(response.data.success) {
                    // se nel BACKEND abbiamo Post::all()
                    // this.posts = response.data.posts

                    // con PAGINATE
                    this.posts = response.data.posts.data;
                    this.currentPage = response.data.posts.current_page;
                    this.lastPage = response.data.posts.last_page;
                    this.store.loading = false
                }
                else {
                    alert('Non completo')
                }
            });
        }
    },
    mounted() {
        this.getPosts(this.currentPage);
    },
}

</script>

<template>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <h2 class="text-center">BoolPress</h2>
            </div>
        </div>
        <!-- <div v-if="loading" class="col-12 d-flex justify-content-center">
            <div class="lds-dual-ring"></div>
        </div> -->
        <div class="col-12 d-flex justify-content-center flex-wrap">
            <div class="row">
                <div class="col-12 col-md-6 col-lg-4" v-for="post in posts" :key="post.id">
                    <PostCard :post="post"></PostCard>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-12 d-flex justify-content-center">
                <nav>
                    <ul class="pagination">
                        <li :class="currentPage === 1 ? 'disabled' : 'page-item'">
                            <button class="page-link" @click="getPosts(currentPage - 1)">Prev</button>
                        </li>
                        <li :class="currentPage === i ? 'disabled' : 'page-item'" v-for="item in lastPage" :key="item.id">
                            <button class="page-link" @click="getPosts(item)">{{item}}</button>
                        </li>
                        <li :class="currentPage === lastPage ? 'disabled' : 'page-item'">
                            <button class="page-link" @click="getPosts(currentPage + 1)">Next</button>
                        </li>
                    </ul>
                </nav>
            </div>
        </div>
    </div>
</template>

<style lang="scss">
@use '../styles/app.scss' as *;

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid black;
  border-color: black transparent black transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

</style>
