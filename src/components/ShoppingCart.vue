<template>
  <div :class="showShoppingCart ? 'right' : 'noright'">
    <div class="wrapper">
      <header class="header">
        <div>
          <h1>{{ shopName }}</h1>
        </div>

        <div
          class="shop-cart"
          aria-haspopup="true"
          aria-label="View Cart"
          aria-expanded="false"
        >
          <button type="button" title="Shopping cart" v-on:click="displayCart">
            <font-awesome-icon
              :icon="shoppingCartIcon"
              alt="Shopping Cart"
              title="Shopping Cart"
            />
            {{ numberOfItems }}
          </button>
        </div>
      </header>

      <aside
        :class="showShoppingCart ? 'show-right-mobile' : 'hide-right-mobile'"
        aria-hidden="true"
      >
        <shopping-cart-table :total="total" :shopping-cart="shoppingCart" :shopping-cart-obj="shoppingCartObj"></shopping-cart-table>
      </aside>

      <main class="content">
        <header class="products-header">
          <div>
            <h1>Products</h1>
          </div>
          <div class="sort-products" aria-label="Sort Products By">
            <label for="sort-choice">Sort By...</label>
            <select
              v-model="selected"
              @change="onChange"
              id="sort-choice"
              name="sort-choice"
            >
              <option value="priceHigh">Price - High to Low</option>
              <option value="priceLow">Price - Low to High</option>
              <option value="title">Title</option>
              <option value="brand">Brand</option>
              <option value="availability">Availability</option>
            </select>
          </div>
        </header>
        <section class="products">
          <div class="item-info" v-for="item in items" :key="item.prodId">
            <a :href="item.productUrl">
              <div class="card">
                <div class="product-image">
                  <img
                    :src="item.imageURL"
                    :alt="item.caption"
                    :class="showShoppingCart ? 'show-right-img' : ''"
                  />
                </div>
                <div>
                  <span>{{ item.caption }}</span>
                </div>
                <div>
                  <span>By {{ item.brand }}</span>
                </div>
                <div class="priceCart">
                  <div class="item-price">
                    <span>${{ item.price.toFixed(2) }}</span>
                  </div>

                  <div
                    class="add-cart"
                    v-on:click.prevent="changeCart(item.prodId)"
                  >
                    <font-awesome-icon
                      :icon="
                        shoppingCart.includes(item.prodId) ? checkIcon : myIcon
                      "
                    />
                  </div>
                </div>
              </div>
            </a>
          </div>
        </section>
      </main>

      <footer>
        eCommerce
      </footer>
    </div>

    <aside :class="showShoppingCart ? 'show-right' : 'hide-right'">
      <shopping-cart-table :total="total" :shopping-cart="shoppingCart" :shopping-cart-obj="shoppingCartObj"></shopping-cart-table>
    </aside>

  </div>
</template>

<script>
import products from "../assets/products.json";
import ShoppingCartTable from './ShoppingCartTable';

import {
  faCartPlus,
  faShoppingCart,
  faCheckCircle,
} from "@fortawesome/free-solid-svg-icons";

export default {
  name: "ShoppingCart",
  components: {
    ShoppingCartTable
  },
  props: {
    shopName: String,
  },
  data() {
    return {
      items: products["List"],
      myIcon: faCartPlus,
      shoppingCartIcon: faShoppingCart,
      checkIcon: faCheckCircle,
      numberOfItems: 0,
      shoppingCart: [],
      selected: "title",
      showShoppingCart: false,
      total: 0,
      shoppingCartObj: {}
    };
  },
  beforeMount() {
    for (let j of this.items) {
      this.shoppingCartObj[j.prodId] = j;
    }
  },
  mounted() {
    this.items.sort((a, b) => {
      return a.caption.localeCompare(b.caption);
    });
  },
  methods: {
    changeCart: function(productID) {

      if (this.shoppingCart.includes(productID)) {
        this.numberOfItems -= 1;
        this.shoppingCart.splice(this.shoppingCart.indexOf(productID), 1);
        this.total -= this.shoppingCartObj[productID]["price"];
      } else {
        this.numberOfItems += 1;
        this.shoppingCart.push(productID);
        this.total += this.shoppingCartObj[productID]["price"];
      }
    },
    onChange: function(event) {
      let topic = event.target.value;
      this.items = products["List"];

      if (topic == "priceHigh") {
        this.items.sort((a, b) => {
          return b.price - a.price;
        });
      } else if (topic == "priceLow") {
        this.items.sort((a, b) => {
          return a.price - b.price;
        });
      } else if (topic == "title") {
        this.items.sort((a, b) => {
          return a.caption.localeCompare(b.caption);
        });
      } else if (topic == "brand") {
        this.items.sort((a, b) => {
          return a.brand.localeCompare(b.brand);
        });
      } else if (topic == "availability") {
        this.items = this.items.filter((a) => a.isAvailable == true);
      }
    },
    displayCart: function() {
      this.showShoppingCart = !this.showShoppingCart;
    },
  },
};
</script>