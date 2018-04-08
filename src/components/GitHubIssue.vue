<template>
    <div class="container">
        <div v-if="loader.getIssue"><img src="/static/loading.svg" alt=""></div>

        <div v-if="!loader.getIssue && issue.number">
            <h1>Issue #{{ issue.number }}</h1>
            <h2>{{ issue.title }}</h2>
            <div>{{ issue.body }}</div>
            <a href="javascript:history.go(-1)" class="btn btn-primary">Voltar</a>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'GitHubIssue',

        created() {
            this.getIssue();
        },

        data() {
            return {
                issue: {},
                loader: {
                    getIssue: false,
                },
            };
        },

        methods: {
            getIssue() {
                this.loader.getIssue = true;
                const url = `https://api.github.com/repos/${this.$route.params.name}/${this.$route.params.repo}/issues/${this.$route.params.issue}`;
                axios.get(url).then((response) => {
                    this.issue = response.data;
                }).finally(() => {
                    this.loader.getIssue = false;
                });
            },
        },
    };
</script>
