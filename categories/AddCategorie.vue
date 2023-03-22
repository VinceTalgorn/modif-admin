<template>
    <div>
        <div class="main">
            <header>
                <h2>Créer un une catégorie</h2>
            </header>
            <div class="form-group">
                <label for="categorie_name">Nom de la categorie</label>
                <input
                    type="text"
                    class="form-control"
                    id="categorie_name"
                    v-model="categorie_name"
                    name="categorie_name"
                />
            </div>
            <footer>
                <button class="btn" @click="postCategorie">Créer</button>
            </footer>
            <div v-show="error" class="error">{{ this.errorMsg }}</div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "AddCategorie",
    components: {},
    data() {
        return {
            categorie_name: "",
            username: "",
            userId: "",
            role: "",
            //Gestion affichage des erreurs
            error: null,
            errorMsg: "",
        };
    },

    mounted() {
        this.username = sessionStorage.getItem("username");
        this.userId = sessionStorage.getItem("userId");
        this.role = sessionStorage.getItem("role");
    },

    methods: {
        //CREATE Categorie
        postCategorie() {
            const userToken = sessionStorage.getItem("token");
            const data = {
                UserId: this.userId.toString(),
                categorie_name: this.categorie_name.toString(),
            };
            if (this.userId) {
                console.log(this.userId);
                axios
                    .post(
                        "http://localhost:3000/api/categories",
                        JSON.stringify(data),
                        {
                            headers: {
                                Authorization: "Bearer " + userToken,
                                "Content-Type": "application/json",
                            },
                        }
                    )
                    .then((response) => {
                        console.log("response to postCategories");
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
    justify-content: space-around;
}
</style>
