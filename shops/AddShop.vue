<template>
    <div class="main">
        <header>
            <h2>Créer un shop</h2>
        </header>
        <div class="form-group">
            <label for="shop_name">Nom du shop</label>
            <input
                type="text"
                class="form-control"
                id="shop_name"
                v-model="shop_name"
                name="shop_name"
            />
        </div>
        <div class="form-group">
            <label for="shop_description">Description du shop</label>
            <input
                type="text"
                class="form-control"
                id="shop_description"
                v-model="shop_description"
                name="shop_description"
            />
        </div>
        <div class="form-group">
            <label for="shop_products">Produits dans le shop</label>
            <input
                type="text"
                class="form-control"
                id="shop_products"
                v-model="shop_products"
                name="shop_products"
            />
        </div>
        <div class="form-group">
            <label for="shop_location_park">Localisation Parc du shop</label>
            <input
                type="text"
                class="form-control"
                id="shop_location_park"
                v-model="shop_location_park"
                name="shop_location_park"
            />
        </div>
        <div class="form-group">
            <label for="shop_location_area">Localisation land shop</label>
            <input
                type="text"
                class="form-control"
                id="shop_location_area"
                v-model="shop_location_area"
                name="shop_location_area"
            />
        </div>

        <div class="form-group">
            <label for="shop_src">Ajouter une image du shop</label><br />
            <input
                type="file"
                @change="selectFile"
                ref="file"
                id="file"
                name="shop_src"
                accept=".jpg, .jpeg, .gif, .png"
            />
            <img
                v-show="shop_src"
                class="publication-photo"
                :src="shop_src"
                alt="picture"
            />
        </div>
        <!-- <div class="form-group">
            <label for="shop_map">Ajouter une image du plan du shop</label>
            <input
                type="file"
                @change="selectFile"
                ref="file"
                id="file"
                name="shop_map"
                accept=".jpg, .jpeg, .gif, .png"
            />
            <img
                v-show="shop_map"
                class="publication-photo"
                :src="shop_map"
                alt="picture"
            />
        </div> -->

        <footer>
            <button class="btn" @click="postShop">Créer</button>
        </footer>
        <div v-show="error" class="error">{{ this.errorMsg }}</div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "AddShop",
    components: {},
    data() {
        return {
            username: "",
            userId: "",
            role: "",
            //Gestion affichage des erreurs
            error: null,
            errorMsg: "",
            shop_name: "",
            shop_description: "",
            shop_products: "",
            shop_location_park: "",
            shop_location_area: "",
            shop_src: "",
            shop_map: "",
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
            this.shop_src = URL.createObjectURL(this.file);
            this.shop_map = URL.createObjectURL(this.file);
        },
        //CREATE shop
        postShop() {
            const userToken = sessionStorage.getItem("token");
            const formData = new FormData();
            formData.append("UserId", this.userId.toString());
            formData.append("shop_name", this.shop_name.toString());
            formData.append(
                "shop_description",
                this.shop_description.toString()
            );
            formData.append("shop_products", this.shop_products.toString());
            formData.append(
                "shop_location_park",
                this.shop_location_park.toString()
            );
            formData.append(
                "shop_location_area",
                this.shop_location_area.toString()
            );
            formData.append("shop_src", this.file);
            formData.append("shop_map", this.file);
            if (this.userId) {
                axios
                    .post("http://localhost:3000/api/shops", formData, {
                        headers: {
                            Authorization: "Bearer " + userToken,
                            "Content-Type": "multipart/form-data",
                        },
                    })
                    .then((response) => {
                        console.log("response to postShop");
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
