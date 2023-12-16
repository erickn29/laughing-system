<script>
import axios from "axios"
import 'highlight.js/lib/common';
import hljs from 'highlight.js';
import 'highlight.js/styles/atom-one-dark.css';

export default {
    data() {
        return {
            posts: [],
            isLoaded: false,
        }
    },
    components: {
    },
    methods: {
        highLight() {
            document.querySelectorAll('pre code').forEach((el) => {
                el.innerHTML = el.innerHTML.
                    replace(/&/g, "&amp;").
                    replace('<code>', '').
                    replace('</code>', '').
                    replace(/</g, "&lt;").
                    replace(/>/g, "&gt;").
                    replace(/"/g, "&quot;").
                    replace(/'/g, "&#039;")
                hljs.highlightElement(el);
            });
        },
        async fetchPosts() {
            try {
                const response = await axios.get('http://192.168.100.18:8000/api/v1/posts/post/')
                if (response.status == 200) {
                    this.posts = response.data
                    this.isLoaded = true
                }
            } catch (error) {
                console.log(error)
            }
        },
    },
    async mounted() {
        await this.fetchPosts()
        this.highLight()
    }
}
</script>

<template>
    <div v-if="isLoaded">
        <h1>{{ posts[1].title }}</h1>
        <p v-html="posts[1].text"></p>
    </div>
</template>

<style></style>
