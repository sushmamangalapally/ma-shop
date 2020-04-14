<template>
  <div :class="showShoppingCart ? 'right' : 'noright'">
    <div class="wrapper">
      <header class="header">
        <div>
          <h1>{{ msg }}</h1>
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
        <div class="products-cart">
          <h1>Shopping Cart</h1>
          <p v-show="shoppingCart.length == 0">
            You have no items in your cart :(
          </p>

          <table v-show="shoppingCart.length !== 0">
            <caption>Your Shopping Cart</caption>
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
          </table>
        </div>
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
      <div class="products-cart">
        <h1>Shopping Cart</h1>

        <p v-show="shoppingCart.length == 0">
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
        </table>
      </div>
    </aside>

  </div>
</template>

<script>
import products from "../assets/products.json";
import {
  faCartPlus,
  faShoppingCart,
  faCheckCircle,
} from "@fortawesome/free-solid-svg-icons";

export default {
  name: "ShoppingCart",
  props: {
    msg: String,
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
    };
  },
  beforeMount() {
    let newObj = {};
    console.log(this.items);
    for (let j of this.items) {
      newObj[j.prodId] = j;
    }
    console.log(newObj);
    this.shoppingCartObj = newObj;
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
        let itemIndex = this.shoppingCart.indexOf(productID);
        this.shoppingCart.splice(itemIndex, 1);
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

a {
  color: #3e2c72;
  text-decoration-line: none;
  font-weight: 500;
}
.products {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
}
.item-info {
  background: #FFF;
  padding: 10px;
  transition: transform .2s;
}
.item-info:hover {
  box-shadow: 5px 5px #78788d5c;
  transform: scale(1.01);
  transition: transform .2s;
}
.card {
  display: grid;
  grid-template-rows: auto;
  grid-row-gap: 10px;
  overflow: hidden;
}
img {
  height: 215px;
}
div.product-image {
  margin: 0 auto;
  overflow: hidden;
}

.priceCart {
  display: grid;
  grid-template-columns: 1fr auto;
}

.add-cart,
.shop-cart > button {
  padding: 10px;
  border: 0;
  border-radius: 80%;
  font-size: 20px;
  cursor: pointer;
  border: 3px solid #d5d5cc;
}

.header,
.products-header {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 1fr;
  grid-column-gap: 0;
  grid-row-gap: 0;
}
.header {
  padding: 18px;
  border-bottom: 5px solid #e4e4e4;
}
.shop-cart,
.sort-products {
  justify-self: end;
}
.sort-products > select {
  border: 0;
  font-size: 18px;
  background: #FFF;
  width: 100%;
}
.hide-right {
  display: none;
}
.show-right {
  display: block;
  border-left: 5px solid #e4e4e4;
  padding: 0 5px;
}
.hide-right-mobile {
  display: none;
}
.show-right-mobile {
  display: none;
  border-left: 5px solid #e4e4e4;
  padding: 0 5px;
}
.right {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}
.noright {
  display: block;
  grid-template-columns: none;
}
table {
  width: 100%;
}
table,
th,
td {
  border: 1px solid #bfbaba8f;
  border-collapse: collapse;
  background: #FFF;
}
th,
td {
  padding: 15px;
  text-align: left;
}

img {
  transition: transform .2s;
}

img:hover {
  transform: scale(1.5);
  transition: transform .2s;
}

.content {
  margin: 60px;
}

footer {
  text-align: center;
  background: #3e2c72;
  padding: 10px;
  color: #FFF;
  font-size: 30px;
  font-weight: 500;
}

.mobile-responsive-cart {
  display: none;
}

.products-header {
  margin-bottom: 10px;
}
.show-right-img {
  height: 85px;
}

@media (max-width: 600px) {
  body {
    background-color: teal;
  }
  .products {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 10px;
  }
  .right {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    grid-gap: 10px;
  }
  .noright {
    display: block;
    grid-template-columns: none;
  }
  .show-right {
    display: none;
    padding: 0 5px;
  }
  .show-right-mobile {
    display: block;
    padding: 0 5px;
    border-left: none;
    background: #f1f1f1;
    border-bottom: 5px solid #e4e4e4;
  }
  img {
    height: 85px;
  }
}

@media (max-width: 400px) {
  body {
    background-color: teal;
  }
  .products {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    grid-gap: 10px;
  }
  .right {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    grid-gap: 10px;
  }
  .noright {
    display: block;
    grid-template-columns: none;
  }
  .show-right {
    display: none;
    padding: 0 5px;
  }
  .show-right-mobile {
    display: block;
    padding: 0 5px;
    border-left: none;
    background: #f1f1f1;
    border-bottom: 5px solid #e4e4e4;
  }

  .products-header {
    margin-bottom: 10px;
    display: grid;
    grid-template-columns: repeat(1, 1fr);
  }

  .content {
    margin: 5px;
  }
}
</style>
