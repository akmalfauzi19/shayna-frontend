<template>
<div>
    <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :items="3" :autoplay="true" :nav="false" :dots="false">
                        <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
                            <div class="pi-pic">
                                <img class="product-pic" v-bind:src="itemProduct.galleries[0].photo" alt />
                                <ul>
                                    <li class="w-icon active" @click="saveKeranjang(itemProduct.id, itemProduct.name,
                                         itemProduct.price, itemProduct.galleries[0].photo)">
                                        <a href="#">
                                            <i class="icon_bag_alt"></i>
                                        </a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type}}</div>
                                <router-link to="/product">
                                    <h5>{{ itemProduct.name }}</h5>
                                </router-link>
                                <div class="product-price">
                                    {{ itemProduct.price | currency }}
                                </div>
                            </div>
                        </div>

                    </carousel>
                </div>

                <div class="col-lg-12" v-else>
                    <p>Produk terbaru belum tersedia saat ini</p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
</div>
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";
import Swal from "sweetalert2";



export default {
    name: "WomanShayna",
    components: {
        carousel
    },
    data() {
        return {
            products: [],
            keranjangUser: []
        };
    },
    methods: {
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {

            var productStored = {
                id: idProduct,
                name: nameProduct,
                price: priceProduct,
                photo: photoProduct
            }
            Swal.fire({
                position: 'center',
                icon: 'success',
                title: 'Barang berhasil masuk keranjang',
                showConfirmButton: true
            }).then((result) => {
                if (result.value) {
                this.keranjangUser.push(productStored);
                const parsed = JSON.stringify(this.keranjangUser);
                localStorage.setItem('keranjangUser', parsed);
                window.location.reload();
                }
            })
        }
    },
    mounted() {
        axios
            .get("http://shayna-backend.belajarkoding.com/api/products")
            .then(res => (this.products = res.data.data.data))
            // eslint-disable-next-line no-console
            .catch(err => console.log(err));

        if (localStorage.getItem("keranjangUser")) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
            } catch (e) {
                localStorage.removeItem("keranjangUser");
            }
        }
    }
};
</script>

<style scoped>
.product-item {
    margin-right: 25px;
}

.product-pic {
    width: 280px;
    height: 400px;
}
</style>
