<template>
    <article class="post">
        <section>
            <div class="p-header">
                <h3 class="p-title">{{post?.title.at(0).toUpperCase()}}{{post?.title.slice(1)}}</h3>
            </div>
            <div  class="p-userInfo">
                <p>{{post?.userName}}</p>
                <p>{{post?.userPhone}}</p>
            </div>
        </section>
        <section class="p-image">
            <img :src="post?.imgSrc" alt="post-image" />
        </section>
    </article>
</template>

<script>
export default {    
  name:'PostView',
  props:{
    post : {}
  }
}
</script>

<style scoped>
    .p-title{
        text-align: start;
        font-weight: 700;
        margin-block:15px;
    }
    .p-userInfo{
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 13px;
        height: 25px;
    }
    .p-userInfo p{
        margin:0;
        color: #6b6b6b;
    }
    .p-image{
        margin-block: 15px 20px;
        & img{
            border-radius: 5px;
            max-width: 100%;
        }
    }
</style>