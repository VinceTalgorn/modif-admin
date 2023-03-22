<template>
    <div class="main-content">
        <header>
            <h2>Liste des articles</h2>
        </header>
        <div class="block-content">
            <ul>
                <li
                    class="liste-articles"
                    :class="{ active: index == currentIndex }"
                    v-for="(article, index) in articles"
                    :key="index"
                    @click="setActiveArticle(article, index)"
                >
                    {{ article.article_name }}
                </li>
            </ul>
            <div class="data-content" v-if="currentArticle">
                <div class="data-articles">
                    <strong>Nom de l'article :</strong>
                    {{ currentArticle.article_name }}
                </div>
                <div class="data-articles">
                    <strong>Titre 1 de l'article :</strong>
                    {{ currentArticle.article_title_1 }}
                </div>
                <div class="data-articles">
                    <strong>Description de l'article 1 :</strong>
                    {{ currentArticle.article_description_1 }}
                </div>
                <div class="data-articles">
                    <strong>Titre 2 de l'article :</strong>
                    {{ currentArticle.article_title_2 }}
                </div>
                <div class="data-articles">
                    <strong>Description de l'article 2 :</strong>
                    {{ currentArticle.article_description_2 }}
                </div>
                <div class="data-articles">
                    <strong>Titre 3 de l'article :</strong>
                    {{ currentArticle.article_title_3 }}
                </div>
                <div class="data-articles">
                    <strong>Description de l'article 3 :</strong>
                    {{ currentArticle.article_description_3 }}
                </div>

                <div class="data-articles">
                    <strong>Photo de l'article :</strong>
                    {{ currentArticle.article_src }}
                </div>

                <div class="data-articles">
                    <strong>Admin ID de l'article :</strong>
                    {{ currentArticle.UserId }}
                </div>

                <div v-if="userId == currentArticle.UserId && role == 'true'">
                    <a
                        class="btn"
                        :href="
                            '/admin/articles/listarticles/' + currentArticle.id
                        "
                        ><span class="card-icon">Modifier</span></a
                    >
                </div>
            </div>
            <div v-else>
                <br />
                <p class="choose">Choisissez un article...</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "ListArticle",
    components: {},
    data() {
        return {
            articles: [],
            role: "",
            currentArticle: null,
            currentIndex: -1,
            UserId: "",
            article_name: "",
            article_title_1: "",
            article_title_2: "",
            article_title_3: "",
            article_description_1: "",
            article_description_2: "",
            article_description_3: "",
            article_src: "",
        };
    },

    methods: {
        retrieveArticle() {
            const userToken = sessionStorage.getItem("token");
            axios
                .get("http://localhost:3000/api/articles", {
                    headers: { Authorization: "Bearer " + userToken },
                })
                .then((response) => {
                    this.articles = response.data.ListeArticles;
                    this.role = sessionStorage.getItem("role");
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        refreshList() {
            this.retrieveArticle();
            this.currentArticle = null;
            this.currentIndex = -1;
        },

        setActiveArticle(article, index) {
            this.currentArticle = article;
            this.currentIndex = article ? index : -1;
        },
    },
    mounted() {
        this.retrieveArticle();
        this.username = sessionStorage.getItem("username");
        this.userId = sessionStorage.getItem("userId");
    },
};
</script>

<style>
.main-content {
    padding: 20px 30px;
}
.block-content {
    display: flex;
}
header h2 {
    font-size: 24px;
    font-weight: bold;
    text-align: left;
}
.liste-articles {
    font-size: 14px;
    color: #3a5199;
    font-weight: bold;
    padding: 5px;
    cursor: pointer;
}

.liste-articles:hover {
    transition: all 0.3s ease-in-out;
    transform: scale(1.1);
}

strong {
    font-size: 14px;
    color: #3a5199;
    margin-right: 5px;
}

.choose {
    font-size: 14px;
}

.data-content {
    margin-left: 30px;
}

.data-articles {
    font-size: 14px;
    margin-bottom: 10px;
}
</style>
