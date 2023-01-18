<template>
    <div class="row">
        <div class="col" v-for="(post, index) in posts" :key="index">
            <div>
                <img :src="`${store.imagBasePath}${post.cover_image}`" :alt="post.title">
                <div>
                    <h3>{{ post.title }}</h3>
                    <p>{{ truncateContent(post.content) }}</p>
                    <router-link class="btn btn-primary" :to="{name: 'single-post', params:{slug: post.slug}}">View more</router-link> 
                </div>
            </div>
        </div>
    </div>
    <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li class="page-item"><a class="page-link" href="#">Previous</a></li>
            <li class="page-item" v-for="n in lastPage"><a class="page-link" @click="getPosts(n)">{{n}}</a></li>    
        </ul>
    </nav>
</template>

<script>
    import axios from 'axios';
    import { store } from '../store';
    export default {
        name: 'CardComponent',
        data() {
            return {
                store,
                posts: [],
                currentPage: 1,
                lastPage: null,
                total: 0,
                contentMaxLen: 100
        };
        },
        methods: {
            getPosts(pagenum) {
                axios.get(`${this.store.apiBaseUrl}/posts`, { params: {
                    page: pagenum
                } }).then((response) => {
                //console.log(response.data.results);
                this.posts = response.data.results.data;
                this.currentPage = response.data.results.current_page;
                this.lastPage = response.data.results.last_page;
                this.total = response.data.results.total;
            });
        },
        truncateContent(text) {
            if (text.length > this.contentMaxLen) {
                return text.substr(0, this.contentMaxLen) + "...";
            }
            return text;
        }
        },
        mounted() {
            this.getPosts(1);
        }
    }
    
</script>

<style lang="scss" scoped>

</style>