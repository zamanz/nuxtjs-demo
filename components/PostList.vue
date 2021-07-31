<template>
    <div class="post__list">
        <div v-if="posts.length > 0" ref="postList">
            <div v-for="(post) in posts" :key="post.id" class="card h-100 border-0 mb-3 bg-transparent post">
                <div class="card-header d-flex justify-content-between p-0 mb-2 border-0 bg-transparent">
                    <a class="avatar d-flex align-items-center" href="#">
                        <img :src="`https://ui-avatars.com/api/?background=random&name=${post.user.name}`" class="img-circle" alt="Profile Image">
                        <span class="ms-3">{{ post.user.name }}</span>
                    </a>
                    <ul class="nav navbar">
                        <li class="px-1">
                            <a href="#" class="d-flex align-items-center">
                                <img src="@/assets/images/heart.svg" alt="">
                                <span class="ms-2">{{ post.comments.length }}</span>
                            </a>
                        </li>

                        <li class="px-1">
                            <a href="#" class="d-flex align-items-center">
                                <img src="@/assets/images/bookmark.svg" alt="">
                                <span class="ms-2">20</span>
                            </a>
                        </li>
                    </ul>
                </div>
                <div class="card-body p-0">
                    <NuxtLink :to="`/post/${post.slug}`">
                        <img v-if="post.image" :src="post.image" class="card-img-top img-rounded" alt="Blog Image" loading="lazy">
                        <v-skeleton-loader v-else type="image"></v-skeleton-loader>
                        <h4 class="title my-3">{{ post.title }}</h4>
                    </NuxtLink>
                    
                    <p>Time: {{ post.posted }}</p>
                    <ul class="nav mb-2">
                        <li v-for="(tag, index) in post.tags" :key="index" class="mx-1">
                            <NuxtLink :to="`/tags/${tag.slug}`">#{{ tag.name }}</NuxtLink>
                        </li>
                    </ul>

                </div>
            </div>

            <div class="d-flex justify-content-center align-items-center col-md-12">
                <div v-if="isLoding" class="spinner-border text-success" role="status">
                </div>
            </div>
        </div>
        <div v-else class="row">

            <div v-for="(item, index) in 6" :key="index" class="card h-100 border-0 mb-3 bg-transparent post">
                <div class="card-header d-flex justify-content-between p-0 mb-2 border-0 bg-transparent">
                    <div class="col-6 p-0 d-flex align-items-center">
                        <v-skeleton-loader type="avatar"></v-skeleton-loader>
                        <v-skeleton-loader type="sentences" width="150" class="ms-2"></v-skeleton-loader>
                    </div>
                    <div class="col-6 p-0 d-flex justify-content-end">
                        <v-skeleton-loader type="chip"></v-skeleton-loader>
                        <v-skeleton-loader type="chip" class="ms-2"></v-skeleton-loader>
                    </div>
                </div>
                <div class="card-body p-0">
                    <v-skeleton-loader type="image"></v-skeleton-loader>
                    <v-skeleton-loader type="paragraph" class="mt-2"></v-skeleton-loader>
                    <v-skeleton-loader type="text"></v-skeleton-loader>
                    <v-skeleton-loader type="text"></v-skeleton-loader>
                    <v-skeleton-loader type="text"></v-skeleton-loader>
                </div>
            </div>
        </div>

        <div class="d-flex justify-content-center align-items-center py-2">
            <a v-if="!isLoding" class="btn btn-success" href="#" @click.prevent="loadMore"><b>LOAD MORE</b></a>
        </div>
    </div>
</template>

<script>
export default {
    name:'PostList',
    data(){
        return {
            posts: [],
            next_page: 2,
            isLoding: false
        }
    },
    beforeMount(){
        this.getInitialPosts();
    },
    mounted(){
        window.addEventListener('scroll', this.getNextPosts)
    },
    beforeDestroy(){
        window.removeEventListener('scroll', this.getNextPosts)
    },
    methods:{
        getInitialPosts() {
            this.$axios.$get('/posts').then((response) => {
                this.posts = response.posts.data;                
            });
        },
        getNextPosts() {
            
            const bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
            if (bottomOfWindow) {
                this.isLoding = true;
                const page = this.next_page++;
                this.$axios.$get('/posts?page='+ page).then((response) => {
                    this.posts.push(...response.posts.data)
                    this.isLoding = false;
                });
            }
        },

        loadMore() {
            this.isLoding = true;
            const page = this.next_page++;
            this.$axios.$get('/posts?page='+ page).then((response) => {
                this.posts.push(...response.posts.data)
                this.isLoding = false;
            });
        }

    }
}
</script>
<style>
    .img-rounded{
        border-radius: 5px;
    }
    .img-circle{
        border-radius: 50%;
    }
    .post a{
        text-decoration: none;
        color: #000;
    }
    .icons div{
        margin-left: 10px
    }
</style>