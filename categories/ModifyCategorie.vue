<template>
    <div class="main">
        <header>
            <h2>Modifier la Categorie</h2>
        </header>
        <div class="form-group">
            <label for="categorie_name">Modifier le nom de la catégorie</label>
            <input
                type="text"
                class="form-control"
                id="categorie_name"
                v-model="categorie_name"
                name="categorie_name"
            />
        </div>

        <footer>
            <button class="btn" @click="deleteCategorie">Delete</button>
            <button type="submit" class="btn" @click="updateCategorie">
                Update
            </button>
            <router-link to="/admin/categories/listcategories" class="return">
                <i class="fa-sharp fa-solid fa-arrow-left"></i>Retour à la liste
                des categories</router-link
            >
        </footer>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "ModifyCategorie",
    components: {},
    data() {
        return {
            message: "",
            categorie_name: "",
        };
    },
    mounted() {
        this.getCategorie();
        this.message = "";
        this.userId = sessionStorage.getItem("userId");
        this.role = sessionStorage.getItem("role");
    },
    methods: {
        //GET THE DATA OF THE CATEGORIE:id
        getCategorie() {
            axios
                .get(
                    "http://localhost:3000/api/categories/" +
                        this.$route.params.id,
                    {
                        headers: {
                            Authorization:
                                "Bearer " + sessionStorage.getItem("token"),
                        },
                    }
                )
                .then((res) => {
                    this.categorie_name = res.data.categorie_name;
                    console.log("this is res from getData");

                    console.log(res);
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        updateCategorie() {
            const userToken = sessionStorage.getItem("token");
            const formData = {
                categorie_name: this.categorie_name.toString(),
            };
            axios
                .put(
                    "http://localhost:3000/api/categories/" +
                        this.$route.params.id,
                    JSON.stringify(formData),
                    {
                        headers: {
                            Authorization: "Bearer " + userToken,
                            "Content-Type": "application/json",
                        },
                    }
                )
                .then((response) => {
                    console.log(response.data.message);
                    this.$router.push({ name: "ListCategorie" });
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        deleteCategorie() {
            axios
                .delete(
                    "http://localhost:3000/api/categories/" +
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
                    this.$router.push({ name: "ListCategorie" });
                })
                .catch((err) => {
                    console.log("this is error from deleteCategorie");
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
