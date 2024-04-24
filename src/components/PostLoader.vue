<template>
    <article class="post">
            <div class="p-header">
                <vue-article-skeleton></vue-article-skeleton>
            </div>
    </article>
</template>

<script>
export default {    
  name:'PostLoader',
}
</script>

<style scoped>
    .skeleton {
        box-shadow: none;
        padding: 20px 5px;
    }
    .skeleton .vs_article-title {
        margin-left: 0;
    }
    .skeleton > :nth-child(2) > :nth-child(1){
        display: none;
    }
    .skeleton > :nth-child(1) > :nth-child(1)  {
        width: 100%;
        border-radius: 5px;
    }
    .skeleton > :nth-child(1) > :nth-child(2)  {
        width: 100%;
        border-radius: 5px;
    }
    .skeleton > :nth-child(2) > :nth-child(1) > :nth-child(2){
        display: none;
    }
    .skeleton > :nth-child(2) > :nth-child(2)  {
        flex-basis: 100%;
    }
    .skeleton > :nth-child(2) > :nth-child(2) > :nth-child(2){
        display: none;
    }
    .skeleton > :nth-child(3){
        display: none;
    }
</style>