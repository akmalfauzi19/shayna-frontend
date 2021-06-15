<template>
<div class="product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/">
                            <i class="fa fa-home"></i> Home
                        </router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="pic-main" :src="gambar_default" alt />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" :dots="false" :nav="false">
                                    <div v-for="ss in productDetails.galleries" :key="ss.id" class="pt" @click="changeImage(ss.photo)" :class="ss.photo == gambar_default ? 'active' : '' ">
                                        <img class="product-pic" :src="ss.photo" alt />
                                    </div>

                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <strong>Deskripsi Product :</strong>
                                    <br>
                                    <p v-html="productDetails.description"></p>
                                    <h4>{{ productDetails.price | currency }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="#" class="primary-btn pd-cart">
                                        <a href="#" @click="saveKeranjang(productDetails.id, productDetails.name,
                                         productDetails.price, productDetails.galleries[0].photo)" class="primary-btn pd-cart">Add To Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->
    <RelatedShayna />
    <FooterShayna />
</div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from "@/components/HeaderShayna.vue";
import FooterShayna from "@/components/FooterShayna.vue";
import carousel from "vue-owl-carousel";
import RelatedShayna from "@/components/RelatedShayna.vue";
import axios from "axios";
import Swal from "sweetalert2";

export default {
    name: "product",
    components: {
        HeaderShayna,
        FooterShayna,
        carousel,
        RelatedShayna
    },
    data() {
        return {
            gambar_default: "",
            productDetails: [],
            keranjangUser: []

        };
    },
    methods: {
        changeImage(urlImage) {
            this.gambar_default = urlImage;
        },
        setDataPicture(data) {
            // replace object productDetails dengan data dari API
            this.productDetails = data;
            // replace object gambar default dengan data dari API
            this.gambar_default = data.galleries[0].photo;
        },
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {

            var productStored = {
                "id": idProduct,
                "name": nameProduct,
                "price": priceProduct,
                "photo": photoProduct
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
        if (localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch (e) {
                localStorage.removeItem('keranjangUser');
            }
        }
        axios
            .get("http://shayna-backend.belajarkoding.com/api/products", {
                params: {
                    id: this.$route.params.id
                }
            })
            .then(res => (this.setDataPicture(res.data.data)))
            // eslint-disable-next-line no-console
            .catch(err => console.log(err));
    }
};
</script>

<style scoped>
.pt {
    margin-right: 14px;
}

.pic-main {
    width: 300px;
    height: 500px;
}

.product-pic {
    width: 150px;
    height: 170px;
}
</style>
