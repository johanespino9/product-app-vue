<template>
    <div id="product-list">
        <h1>
            Product List
        </h1>
          <v-container class="">
            <v-row no-gutters style="spacing">
                <v-col
                v-for="product in products"
                :key="product.id"
                cols="12"
                sm="4"
                style="height: 200px;"
                >
                <ProductItem  v-on:getProducts="getProducts" :id="product.id" :title="product.title" :description="product.description " :price="product.price"/>
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script>
import ProductItem from './ProductItem.vue'
import { db } from '../main'

    export default {
        name: 'ProductList',
        data: () => ({
            products: []
        }),
        components: {
            ProductItem
        },
        created() {
            this.getProducts()
        },
        methods: {
            async getProducts() {
                const snapshot = await db.collection('products').get()
                let products = []
                snapshot.forEach( product => {
                    let productToAdd = product.data()
                    productToAdd.id = product.id

                    products.push(productToAdd)
                });

                this.products = products
                console.log(products)
            }
        }
    }
</script>

<style>
    #product-list {
        color: red;
    }
</style>