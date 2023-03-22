<template>
    <div class="main-content">
        <header>
            <h2>Liste des catégories</h2>
        </header>
        <div class="block-content">
            <ul>
                <li
                    class="liste-categories"
                    :class="{ active: index == currentIndex }"
                    v-for="(categorie, index) in categories"
                    :key="index"
                    @click="setActiveCategorie(categorie, index)"
                >
                    {{ categorie.categorie_name }}
                </li>
            </ul>
            <div class="data-content" v-if="currentCategorie">
                <div class="data-categories">
                    <strong>Nom de la catégorie :</strong>
                    {{ currentCategorie.categorie_name }}
                </div>

                <div class="data-categories">
                    <strong>Admin ID de la catégorie :</strong>
                    {{ currentCategorie.UserId }}
                </div>
                <div v-if="userId == currentCategorie.UserId && role == 'true'">
                    <a
                        class="btn"
                        :href="
                            '/admin/categories/listcategories/' +
                            currentCategorie.id
                        "
                        ><span class="card-icon">Modifier</span></a
                    >
                </div>
            </div>
            <div v-else>
                <br />
                <p class="choose">Choisissez une catégorie...</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "ListCategorie",
    components: {},
    data() {
        return {
            categories: [],
            role: "",
            currentCategorie: null,
            currentIndex: -1,
            categorie_name: "",
            UserId: "",
        };
    },

    methods: {
        retrieveCategorie() {
            const userToken = sessionStorage.getItem("token");
            axios
                .get("http://localhost:3000/api/categories", {
                    headers: { Authorization: "Bearer " + userToken },
                })
                .then((response) => {
                    this.categories = response.data.ListeCategories;
                    this.role = sessionStorage.getItem("role");
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        refreshList() {
            this.retrieveCategorie();
            this.currentCategorie = null;
            this.currentIndex = -1;
        },

        setActiveCategorie(categorie, index) {
            this.currentCategorie = categorie;
            this.currentIndex = categorie ? index : -1;
        },
    },
    mounted() {
        this.retrieveCategorie();
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
.liste-categories {
    font-size: 14px;
    color: #3a5199;
    font-weight: bold;
    padding: 5px;
    cursor: pointer;
}

.liste-pins:hover {
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

.data-categories {
    font-size: 14px;
    margin-bottom: 10px;
}
</style>
