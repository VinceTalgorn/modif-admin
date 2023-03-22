<template>
    <div class="main-content">
        <header>
            <h2>Liste des shops</h2>
        </header>
        <div class="block-content">
            <ul>
                <li
                    class="liste-shops"
                    :class="{ active: index == currentIndex }"
                    v-for="(shop, index) in shops"
                    :key="index"
                    @click="setActiveShop(shop, index)"
                >
                    {{ shop.shop_name }}
                </li>
            </ul>
            <div class="data-content" v-if="currentShop">
                <div class="data-shops">
                    <strong>Nom du shop :</strong>
                    {{ currentShop.shop_name }}
                </div>
                <div class="data-shops">
                    <strong>Description du shop :</strong>
                    {{ currentShop.shop_description }}
                </div>
                <div class="data-shops">
                    <strong>Produits du shop :</strong>
                    {{ currentShop.shop_products }}
                </div>
                <div class="data-shops">
                    <strong>Location Parc du shop :</strong>
                    {{ currentShop.shop_location_park }}
                </div>
                <div class="data-shops">
                    <strong>Location land du shop :</strong>
                    {{ currentShop.shop_location_area }}
                </div>
                <div class="data-shops">
                    <strong>Photo du shop :</strong>
                    {{ currentShop.shop_src }}
                </div>
                <!-- <div>
                         <strong>Photo de la map du shop :</strong> 
                        {{ currentShop.shop_map }}
                    </div> -->
                <div class="data-shops">
                    <strong>Admin ID du shop :</strong>
                    {{ currentShop.UserId }}
                </div>

                <div v-if="userId == currentShop.UserId && role == 'true'">
                    <a
                        class="btn"
                        :href="'/admin/shops/listshops/' + currentShop.id"
                        ><span class="card-icon">Modifier</span></a
                    >
                </div>
            </div>
            <div v-else>
                <br />
                <p class="choose">Choisissez un shop...</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "ListShop",
    components: {},
    data() {
        return {
            shops: [],
            role: "",
            currentShop: null,
            currentIndex: -1,
            UserId: "",
            shop_name: "",
            shop_description: "",
            shop_products: "",
            shop_location_park: "",
            shop_location_area: "",
            shop_src: "",
            // shop_map: "",
        };
    },

    methods: {
        retrieveShop() {
            const userToken = sessionStorage.getItem("token");
            axios
                .get("http://localhost:3000/api/shops", {
                    headers: { Authorization: "Bearer " + userToken },
                })
                .then((response) => {
                    this.shops = response.data.ListeShops;
                    this.role = sessionStorage.getItem("role");
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        refreshList() {
            this.retrieveShop();
            this.currentShop = null;
            this.currentIndex = -1;
        },

        setActiveShop(shop, index) {
            this.currentShop = shop;
            this.currentIndex = shop ? index : -1;
        },
    },
    mounted() {
        this.retrieveShop();
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
.liste-shops {
    font-size: 14px;
    color: #3a5199;
    font-weight: bold;
    padding: 5px;
    cursor: pointer;
}

.liste-shops:hover {
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

.data-shops {
    font-size: 14px;
    margin-bottom: 10px;
}
</style>
