<template>
    <div class="main">
        <header>
            <h2>Modifier le Shop</h2>
        </header>
        <form>
            <div class="form-group">
                <label for="shop_name">Modifier le nom du shop</label>
                <input
                    type="text"
                    class="form-control"
                    id="shop_name"
                    v-model="shop_name"
                    name="shop_name"
                />
            </div>
            <div class="form-group">
                <label for="shop_description"
                    >Modifier la description du shop</label
                >
                <input
                    type="text"
                    class="form-control"
                    id="shop_description"
                    v-model="shop_description"
                    name="shop_description"
                />
            </div>
            <div class="form-group">
                <label for="shop_products">Modifier les produits du shop</label>
                <input
                    type="text"
                    class="form-control"
                    id="shop_products"
                    v-model="shop_products"
                    name="shop_products"
                />
            </div>
            <div class="form-group">
                <label for="shop_location_park"
                    >Modifier la localisation Parc du shop</label
                >
                <input
                    type="text"
                    class="form-control"
                    id="shop_location_park"
                    v-model="shop_location_park"
                    name="shop_location_park"
                />
            </div>
            <div class="form-group">
                <label for="shop_location_area"
                    >Modifier la localisation land shop</label
                >
                <input
                    type="text"
                    class="form-control"
                    id="shop_location_area"
                    v-model="shop_location_area"
                    name="shop_location_area"
                />
            </div>
            <div class="form-group">
                <label for="shop_src">Modifier l'image du shop</label>
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
        </form>
        <footer>
            <button class="btn" @click="deleteShop">Delete</button>
            <button type="submit" class="btn" @click="updateShop">
                Update
            </button>
            <router-link to="/admin/shops/listshops" class="return">
                <i class="fa-sharp fa-solid fa-arrow-left"></i>Retour à la liste
                des shops</router-link
            >
        </footer>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "ModifyShop",
    components: {},
    data() {
        return {
            message: "",
            shop_name: "",
            shop_description: "",
            shop_products: "",
            shop_location_park: "",
            shop_location_area: "",
            shop_src: "",
            // shop_map: "",
            file: null,
        };
    },

    mounted() {
        this.getShop();
        this.message = "";
        this.userId = sessionStorage.getItem("userId");
        this.role = sessionStorage.getItem("role");
    },
    methods: {
        selectFile() {
            this.file = this.$refs.file.files[0];
            this.shop_src = URL.createObjectURL(this.file);
            // this.shop_map = URL.createObjectURL(this.file);
        },
        //GET THE DATA OF THE MESSAGE:id
        getShop() {
            axios
                .get(
                    "http://localhost:3000/api/shops/" + this.$route.params.id,
                    {
                        headers: {
                            Authorization:
                                "Bearer " + sessionStorage.getItem("token"),
                        },
                    }
                )
                .then((res) => {
                    this.shop_name = res.data.shop_name;
                    this.shop_description = res.data.shop_description;
                    this.shop_products = res.data.shop_products;
                    this.shop_location_park = res.data.shop_location_park;
                    this.shop_location_area = res.data.shop_location_area;
                    this.shop_src = res.data.shop_src;
                    // this.shop_map = res.data.shop_map;

                    console.log("this is res from getData");
                    console.log(res);
                })
                .catch((err) => {
                    console.log(err);
                });
        },

        updateShop() {
            const userToken = sessionStorage.getItem("token");
            const formData = new FormData();
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
            // formData.append("shop_map", this.file);
            console.log("what is this.file");
            console.log(this.file);

            axios
                .put(
                    "http://localhost:3000/api/shops/" + this.$route.params.id,
                    formData,
                    {
                        headers: { Authorization: "Bearer " + userToken },
                    }
                )
                .then((response) => {
                    console.log(response.data.message);
                    this.$router.push({ name: "ListShop" });
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        deleteShop() {
            axios
                .delete(
                    "http://localhost:3000/api/shops/" + this.$route.params.id,
                    {
                        headers: {
                            Authorization:
                                "Bearer " + sessionStorage.getItem("token"),
                        },
                    }
                )
                .then((response) => {
                    console.log(response);
                    this.$router.push({ name: "ListShop" });
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
