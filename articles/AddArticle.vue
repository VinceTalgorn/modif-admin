<template>
    <div class="main">
        <header>
            <h2>Créer un article</h2>
        </header>
        <div class="form-group">
            <label for="article_name">Nom de l'article</label>
            <input
                type="text"
                class="form-control"
                id="article_name"
                v-model="article_name"
                name="article_name"
            />
        </div>
        <div class="form-group">
            <label for="article_title_1">Titre 1 de l'article</label>
            <input
                type="text"
                class="form-control"
                id="article_title_1"
                v-model="article_title_1"
                name="article_title_1"
            />
        </div>

        <div class="form-group">
            <label for="article_description_1"
                >Description 1 de l'article</label
            >
            <input
                type="text"
                class="form-control"
                id="article_description_1"
                v-model="article_description_1"
                name="article_description_1"
            />
        </div>
        <div class="form-group">
            <label for="article_title_2">Titre 2 de l'article</label>
            <input
                type="text"
                class="form-control"
                id="article_title_2"
                v-model="article_title_2"
                name="article_title_2"
            />
        </div>
        <div class="form-group">
            <label for="article_description_2"
                >Description 2 de l'article</label
            >
            <input
                type="text"
                class="form-control"
                id="article_description_2"
                v-model="article_description_2"
                name="article_description_2"
            />
        </div>
        <div class="form-group">
            <label for="article_title_3">Titre 3 de l'article</label>
            <input
                type="text"
                class="form-control"
                id="article_title_3"
                v-model="article_title_3"
                name="article_title_3"
            />
        </div>
        <div class="form-group">
            <label for="article_description_3"
                >Description 3 de l'article</label
            >
            <input
                type="text"
                class="form-control"
                id="article_description_3"
                v-model="article_description_3"
                name="article_description_3"
            />
        </div>

        <div class="form-group">
            <label for="article_src">Ajouter une image à l'article</label><br />
            <input
                type="file"
                @change="selectFile"
                ref="file"
                id="file"
                name="image"
                accept=".jpg, .jpeg, .gif, .png"
            />
            <img
                v-show="article_src"
                class="publication-photo"
                :src="article_src"
                alt="picture"
            />
        </div>

        <footer>
            <button class="btn" @click="postArticle">Créer</button>
        </footer>
        <div v-show="error" class="error">{{ this.errorMsg }}</div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "AddArticle",
    components: {},
    data() {
        return {
            username: "",
            userId: "",
            role: "",
            //Gestion affichage des erreurs
            error: null,
            errorMsg: "",
            article_name: "",
            article_title_1: "",
            article_title_2: "",
            article_title_3: "",
            article_description_1: "",
            article_description_2: "",
            article_description_3: "",
            article_src: "",
            file: null,
        };
    },

    mounted() {
        this.username = sessionStorage.getItem("username");
        this.userId = sessionStorage.getItem("userId");
        this.role = sessionStorage.getItem("role");
    },

    methods: {
        selectFile() {
            this.file = this.$refs.file.files[0];
            this.article_src = URL.createObjectURL(this.file);
        },
        //CREATE shop
        postArticle() {
            const userToken = sessionStorage.getItem("token");
            const formData = new FormData();
            formData.append("UserId", this.userId.toString());
            formData.append("article_name", this.article_name.toString());
            formData.append("article_title_1", this.article_title_1.toString());
            formData.append("article_title_2", this.article_title_2.toString());
            formData.append("article_title_3", this.article_title_3.toString());
            formData.append(
                "article_description_1",
                this.article_description_1.toString()
            );
            formData.append(
                "article_description_2",
                this.article_description_2.toString()
            );
            formData.append(
                "article_description_3",
                this.article_description_3.toString()
            );
            formData.append("article_src", this.file);
            if (this.userId) {
                axios
                    .post("http://localhost:3000/api/articles", formData, {
                        headers: {
                            Authorization: "Bearer " + userToken,
                            "Content-Type": "multipart/form-data",
                        },
                    })
                    .then((response) => {
                        console.log("response to postArticle");
                        console.log(response.data.message);
                        window.location.reload(true);
                    });
            } else {
                this.error = true; // Si au moins 1 champ est vide on signal une erreur
                this.errorMsg = "Merci de remplir les champs"; //le message d'erreur
            }
        },
    },
};
</script>

<style scoped>
label {
    font-size: 14px;
}
.main {
    width: 400px;
    min-height: 500px;
    padding: 20px 0 0 30px;
}
.main a {
    position: absolute;
    top: 7px;
    right: 10px;
}
.main header h2 {
    font-size: 24px;
    font-weight: bold;
}
.publication-photo {
    border-radius: 10px;
    width: 350px;
    height: 300px;
}
footer {
    margin-top: 30px;
    display: flex;
}
</style>
