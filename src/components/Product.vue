<template>
    <div class="products-cart">
        <h1>Shopping Cart ${{ total.toFixed(2) }}</h1>

        <!-- <p v-show="shoppingCart.length == 0">
            You have no items in your cart :(
        </p>

        <table v-show="shoppingCart.length !== 0">
            <caption>
            Your Shopping Cart
            </caption>
            <tr>
            <th scope="col">Item</th>
            <th scope="col">Price</th>
            </tr>
            <tr class="item-cart" v-for="item in shoppingCart" :key="item">
            <td scope="row">{{ shoppingCartObj[item]["caption"] }}</td>
            <td>${{ shoppingCartObj[item]["price"].toFixed(2) }}</td>
            </tr>
            <tr>
            <th>Total</th>
            <th>${{ total.toFixed(2) }}</th>
            </tr>
        </table> -->
    </div>
</template>

<script>

export default {
  name: "Product",
  props: {
    total: {
            type: Number,
            default: function() {
                return []
            }
    },   
    shoppingCart: {
            type: Array,
            default: function() {
                return []
            }
    },           
    shoppingCartObj: {
            type: Object,
            default: function () {
                return {
                    "listID": 0,
                    "prodId": 0,
                    "displayOrder": 0,
                    "quantity": 0,
                    "productLink": {
                        "rel": "",
                        "href": "",
                        "type": ""
                    },
                    "caption": "",
                    "imageURL": "",
                    "price": 0,
                    "currency": "$",
                    "description": "",
                    "hasFullDescription": false,
                    "brand": "",
                    "productUrl": "",
                    "isAvailable": false,
                    "maxOrderQuantity": 0,
                    "mediumImageURL": ""
                }
            }
        }
  }
}