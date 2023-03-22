<template>
    <div class="main">
        <header>
            <h2>Modifier l'article</h2>
        </header>
        <div class="form-group">
            <label for="article_name">Modifier le nom de l'article</label>
            <input
                type="text"
                class="form-control"
                id="article_name"
                v-model="article_name"
                name="article_name"
            />
        </div>
        <div class="form-group">
            <label for="article_title_1"
                >Modifier lz titre 1 de l'article</label
            >
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
                >Modifier la description 1 de l'article</label
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
            <label for="article_title_2"
                >Modifier le titre 2 de l'article</label
            >
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
                >Modifier la description 2 de l'article</label
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
            <label for="article_title_3"
                >Modifier le titre 3 de l'article</label
            >
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
                >Modifier la description 3 de l'article</label
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
            <label for="article_src">Modifier l'image de l'article</label>
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
            <button class="btn" @click="deleteArticle">Delete</button>
            <button type="submit" class="btn" @click="updateArticle">
                Update
            </button>
            <router-link to="/admin/articles/listarticles" class="return">
                <i class="fa-sharp fa-solid fa-arrow-left"></i>Retour à la liste
                des articles</router-link
            >
        </footer>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "ModifyArticle",
    components: {},
    data() {
        return {
            message: "",
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
        this.getArticle();
        this.message = "";
        this.userId = sessionStorage.getItem("userId");
        this.role = sessionStorage.getItem("role");
    },
    methods: {
        selectFile() {
            this.file = this.$refs.file.files[0];
            this.article_src = URL.createObjectURL(this.file);
        },
        //GET THE DATA OF THE MESSAGE:id
        getArticle() {
            axios
                .get(
                    "http://localhost:3000/api/articles/" +
                        this.$route.params.id,
                    {
                        headers: {
                            Authorization:
                                "Bearer " + sessionStorage.getItem("token"),
                        },
                    }
                )
                .then((res) => {
                    this.article_name = res.data.article_name;
                    this.article_title_1 = res.data.article_title_1;
                    this.article_title_2 = res.data.article_title_2;
                    this.article_title_3 = res.data.article_title_3;
                    this.article_description_1 = res.data.article_description_1;
                    this.article_description_2 = res.data.article_description_2;
                    this.article_description_3 = res.data.article_description_3;
                    this.article_src = res.data.article_src;

                    console.log("this is res from getData");
                    console.log(res);
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        updateArticle() {
            const userToken = sessionStorage.getItem("token");
            const formData = new FormData();
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
            console.log("what is this.file");
            console.log(this.file);
            axios
                .put(
                    "http://localhost:3000/api/articles/" +
                        this.$route.params.id,
                    formData,
                    {
                        headers: { Authorization: "Bearer " + userToken },
                    }
                )
                .then((response) => {
                    console.log(response.data.message);
                    this.$router.push({ name: "ListArticle" });
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        deleteArticle() {
            axios
                .delete(
                    "http://localhost:3000/api/articles/" +
                        this.$route.params.id,
                    {
                        headers: {
                            Authorization:
                                "Bearer " + sessionStorage.getItem("token"),
                        },
                    }
                )
                .then((response) => {
                    console.log(response);
                    this.$router.push({ name: "ListArticle" });
                })
                .catch((err) => {
                    console.log("this is error from deletePublication");
                    console.log(err);
                });
        },
    },
};
</script>

<style>
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
    margin-top: 10px;
    display: flex;
    justify-content: space-around;
}

.return {
    color: #3a5199;
    position: absolute;
    left: 35px;
    transition: all 0.5s ease;
    font-size: 16px;
}
.return:hover {
    transform: scale(1.1);
}
</style>
