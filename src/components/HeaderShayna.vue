<template>
<div>
    <!-- Header Section Begin -->
    <header class="header-section">
        <div class="header-top">
            <div class="container">
                <div class="ht-left">
                    <div class="mail-service">
                        <i class=" fa fa-envelope"></i> akmal.fauzi19@gmail.com
                    </div>
                    <div class="phone-service">
                        <i class=" fa fa-phone"></i> +628 5878051187
                    </div>
                </div>
            </div>
        </div>
        <div class="container">
            <div class="inner-header">
                <div class="row">
                    <div class="col-lg-2 col-md-2">
                        <div class="logo">
                            <router-link to="/">
                                <img src="img/logo_website_shayna.png" alt="" />
                            </router-link>
                        </div>
                    </div>
                    <div class="col-lg-7 col-md-7"></div>
                    <div class="col-lg-3 text-right col-md-3">
                        <ul class="nav-right">
                            <li class="cart-icon">
                                Keranjang Belanja &nbsp;
                                <a href="#">
                                    <i class="icon_bag_alt"></i>
                                    <span>{{ keranjangUser.length }}</span>
                                </a>
                                <div class="cart-hover">
                                    <div class="select-items">
                                        <table>
                                            <tbody v-if="keranjangUser.length > 0">

                                                <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                                                    <td class="si-pic">
                                                        <img class="photo-item" :src="keranjang.photo" alt="" />
                                                    </td>
                                                    <td class="si-text">
                                                        <div class="product-selected">
                                                            <p>{{ keranjang.price | currency }} x 1</p>
                                                            <h6>{{ keranjang.name }}</h6>
                                                        </div>
                                                    </td>
                                                    <td class="si-close" @click="removeItem(keranjang.id)">
                                                        <i class="ti-close"></i>
                                                    </td>
                                                </tr>

                                            </tbody>

                                            <tbody v-else>
                                                <tr>
                                                    <center>
                                                        <td>Keranjang Kosong</td>
                                                    </center>
                                                </tr>
                                            </tbody>

                                        </table>
                                    </div>
                                    <div class="select-total">
                                        <span>Total Item:</span>
                                        <h5>{{ resultCount }}</h5>
                                    </div>
                                    <div class="select-total">
                                        <span>Total Harga:</span>
                                        <h5>{{ totalHarga | currency }}</h5>
                                    </div>
                                    <div class="select-button">
                                        <a href="#" class="primary-btn view-card">
                                            <router-link to="/cart" style="color: #FFF">
                                                VIEW CARD
                                            </router-link>
                                        </a>
                                        <!-- <a href="#" class="primary-btn checkout-btn">CHECK OUT</a> -->
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <!-- Header End -->
</div>
</template>

<script>
export default {
    name: 'HeaderShayna',
    data() {
        return {
            keranjangUser: [],
        };
    },
    methods: {
        removeItem(xx) {
            // this.keranjangUser.splice(index, 1);
            // const parsed = JSON.stringify(this.keranjangUser);
            // localStorage.setItem("keranjangUser", parsed);
            // window.location.reload();
            let faveGifs = JSON.parse(localStorage.getItem("keranjangUser"));
            let faveGif = faveGifs.map(faveGif => faveGif.id);
            let index = faveGif.findIndex(id => id == xx);
            this.keranjangUser.splice(index, 1);
            const parsed = JSON.stringify(this.keranjangUser);
            localStorage.setItem("keranjangUser", parsed);
            window.location.reload();
            // eslint-disable-next-line no-console
            console.log(index);
        },
    },

    mounted() {
        if (localStorage.getItem("keranjangUser")) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));

            } catch (e) {
                localStorage.removeItem("keranjangUser");
            }
        }
    },
    computed: {
        totalHarga() {
            return this.keranjangUser.reduce(function (items, data) {
                return items + data.price;
            }, 0);
        },
        resultCount() {
            return this.keranjangUser && this.keranjangUser.length
        }
    },
}
</script>

<style scoped>
.photo-item {
    width: 50px;
    height: 50px;
}
</style>
