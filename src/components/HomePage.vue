<template>

    <section class="title width-margin">
        <img src="@/assets/logos/trycar.png">
    </section>

    <!-- Start Loaders -->
    <div v-if="isLoadingFirstPosts && !serverError && !posts">
        <div v-for="(loader,index) in firstLoaders" :key="index" class="posts-container width-margin">
            <PostLoader />
        </div>
    </div>
    <!-- End Loaders -->

    <!-- Posts List start-->
    <div v-else-if="!isLoadingFirstPosts && posts.length > 0 && !serverError" >
        <div v-for="post in posts" :key="[post?.id]" class="posts-container width-margin">
            <PostView :post="post" />
        </div>
        <div v-if="loadingMorePosts">
            <div v-for="(loader,index) in firstLoaders" :key="index" class="posts-container width-margin">
                <PostLoader />
            </div>
        </div>
    </div>
    <!-- Posts List end  -->
    
    <!-- Start Error Handler -->
    <div v-else-if="!isLoadingFirstPosts && posts.length === 0 && serverError">
        <div class="posts-container width-margin">
            <div class="post p-error">
                Something wrong happened , Please try again later .
            </div>
        </div>
    </div>
    <!-- End Error Handler -->

    <!-- Start Empty Data -->
    <div v-else-if="!isLoadingFirstPosts && posts.length === 0 && !serverError">
        <div class="posts-container width-margin">
            <div class="post p-empty">
                Sorry , No posts found available now .
            </div>
        </div>
    </div>
    <!-- End Empty Data -->

    <button v-if="!lastResults" :disabled="loadingMorePosts" :class="{ 'disabled-btn': loadingMorePosts}" class="load-btn" @click="loadMore">Load More
    </button>

    <div  class="posts-container width-margin">
        <div  class="post paginaion-data">
           <span v-if="lastResults">No more posts ,</span> Showing {{ posts?.length }} posts , on Page {{ currentPage }} of Total {{ !serverError ? totalPostsNo : 0 }} . 
        </div>
    </div>
</template>
  
  <script>
    import axios from 'axios' ;
    import { toast } from 'vue3-toastify';
    import 'vue3-toastify/dist/index.css';
    import PostView from './PostView.vue';
    import PostLoader from './PostLoader.vue';
    export default {
    name: 'HomePage',
    data(){
        return {
            posts : [] ,
            isLoadingFirstPosts : false  ,
            firstLoaders : ['', ''] ,
            serverError : false ,
            currentPage : 1 ,
            lastPage : 1 ,
            totalPostsNo : 100 ,
            perPage : 10 ,
            lastResults : false ,
            loadingMorePosts : false 
        }
    },
    created(){
        this.getPosts();
    },
    components:{
        PostView,
        PostLoader
    },
    methods : {
        getPosts(){
            if(this.currentPage === 1){
                this.isLoadingFirstPosts = true ;
            }
            this.serverError = false ;
            axios.get(`https://jsonplaceholder.typicode.com/posts?_page=${this.currentPage}&_per_page=${10}`)
            .then((response)=>{
                const modifiedReponse = response?.data?.map((post)=>{
                    let phone = `+(20) 111 011 ${post?.id}`
                    phone = phone.padEnd(18 , 0)
                    return {
                        title : post?.title,
                        userName : `UserNo.${post?.userId}`,
                        userPhone : phone ,
                        imgSrc : `https://picsum.photos/600/300/?image=${post.id}`
                    }
                })
                this.posts = [...this.posts , ...modifiedReponse ] ;
                this.isLoadingFirstPosts = false ;
                this.serverError = false ;
                if(response?.data?.length === 10){
                    this.lastPage = this.currentPage + 1 ;
                }else{
                    this.lastPage === this.currentPage ;
                    this.lastResults = true ;
                }
                this.loadingMorePosts = false ;
            })
            .catch((error)=>{
               toast(error?.message , {
                type:'error',
                position : 	toast.POSITION.TOP_RIGHT ,
               })
                this.isLoadingFirstPosts = false ;
                this.serverError = true ;
                this.loadingMorePosts = false ;
            })
        },
        loadMore(){
            if(this.currentPage !== this.lastPage && this.currentPage < this.lastPage){
                this.currentPage++ ;
                this.loadingMorePosts = true ;
                this.getPosts();
            }else{
                this.lastResults = true ;
            }
        }
    }
  }
  </script>
  
  <style>
  .width-margin{
      width: 50%;
      margin: auto;
      margin-bottom: 30px;
  }
  .title{
    border-radius: 5px;
    padding-block: 1px;
    background-color: white;
    & img{
        width: 30%;
        padding-block: 15px;
    }
  }
  .posts-container{
    margin-block: 20px;
  }
  .post{
        border-radius: 5px;
        background-color: white;
        padding: 5px 25px;
    }
    .p-error{
        color: red;
        font-weight: 700;
        padding: 25px;
        font-size: 18px;
    }
    .p-empty{
        color: #6b6b6b;
        font-weight: 600;
        padding: 20px;
        font-size: 17px;
    }
    .load-btn{
        font-size: medium;
        color: white;
        border: none;
        background-color: #563293;
        padding: 10px;
        border-radius: 5px;
        font-weight: 700;
        cursor: pointer;
    }
    .disabled-btn{
        background-color: #8b8495;
        cursor:wait ;
    }
    .paginaion-data{
        color: #8d8080;
        font-weight: 700;
        font-size: medium;  
        padding-block: 15px;
    }
  @media screen and (max-width : 765px){
    .width-margin{
      width: 100%;
  }
    }
  </style>
  