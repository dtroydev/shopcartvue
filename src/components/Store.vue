<template>
  <div class="wrapper">
    <div class="cart">Cart
      <div class="cartItems">Items:
        <span class="cartItemsNumber">{{ cart.length }}</span>
      </div>
    </div>
    <div class="container">
      <h1 class="heading">Products</h1>
      <ul>
        <li v-for="(product, indexProduct) in products" :key="product.product" class="productgrid">
          <div class="image">
            <ImageSource :product="product" :baseUrl="baseUrl" @loader="loader" />
          </div>
          <div class="column">
            <div>
              <div class="name">{{ title[indexProduct] }}
                <div class="sale" v-if="product.onSale && product.inventory">
                  <span>On Sale!</span>
                </div>
              </div>
              <div class="description">{{product.description}}</div>
            </div>
            <div class="spreadevengrid">
              <div class="inStock" v-if="product.inventory">In Stock
                <div class="inStockQty">({{product.inventory}} units)</div>
              </div>
              <span class="outOfStock" v-else>Sold Out!</span>
              <div v-if="product.varieties">
                <div class="variety" :class="{ varietySelected: indexVariety === product.selectedVariety }" @mouseover="varietyMouseover(indexProduct, indexVariety)" v-for="(variety, indexVariety) in product.varieties" :key="variety.color" :style="{backgroundColor: variety.color}"></div>
              </div>
            </div>
            <div class="spreadeven">
              <button :disabled="!product.inventory" class="cartButton" @click="cartButtonAdd(indexProduct, product.selectedVariety)">Add to Cart</button>
              <button :disabled="!product.inventory" class="cartButton" @click="cartButtonRemove(indexProduct, product.selectedVariety)">Remove from Cart</button>
            </div>
          </div>
        </li>
      </ul>
      <StoreReviews class="reviews" @reviewSubmitted="reviewSubmitted" :reviews="reviews" />
    </div>
  </div>
</template>

<script>
import ImageSource from './ImageSource.vue';
import StoreReviews from './StoreReviews.vue';

export default {
  name: 'Store',
  props: {},
  data() {
    return {
      baseUrl: 'https://loremflickr.com/200/200/',
      cart: [],
      reviews: [],
      products: [
        {
          brand: 'GreatFit',
          product: 'Socks',
          description:
            'Very Comfy Socks. If you wear these socks you will feel all kinds of delight',
          inventory: 100,
          onSale: true,
          selectedVariety: 0,
          varieties: [
            { color: 'green' },
          ],
        },
        {
          brand: 'SuperSole',
          product: 'Shoes',
          description:
            'Cool Shoes. Only the cool kids in your neighbourhood wear these, so if you want to be cool, you know what to do',
          inventory: 0,
          onSale: true,
          selectedVariety: 0,
          varieties: [
            { color: 'blue' },
            { color: 'white' },
            { color: 'black' },
          ],
        },
        {
          brand: 'ComfortStyle',
          product: 'Shirts',
          description:
            'Great Shirt. When you want to rise to the occasion, you put on the shirt that lets you shine. This shirt does that',
          inventory: 50,
          onSale: false,
          selectedVariety: 0,
          varieties: [
            { color: 'green' },
            { color: 'white' },
            { color: 'black' },
            { color: 'red' },
          ],
        },
      ],
    };
  },
  methods: {
    cartButtonAdd(productIndex, varietyIndex) {
      this.cart.push({ productIndex, varietyIndex });
    },
    cartButtonRemove(productIndex, varietyIndex) {
      const finder = e => e.productIndex === productIndex && e.varietyIndex === varietyIndex;
      const removeIndex = this.cart.findIndex(finder);
      if (removeIndex !== -1) this.cart.splice(removeIndex, 1);
    },
    varietyMouseover(indexProduct, indexVariety) {
      this.products[indexProduct].selectedVariety = indexVariety;
    },
    reviewSubmitted(review) {
      this.reviews.push(review);
    },
    loader() {
      // nop function testing event reception from image component
    },
  },
  computed: {
    title() {
      return this.products.map(e => `${e.brand} ${e.product}`);
    },
  },
  components: {
    ImageSource,
    StoreReviews,
  },
};
</script>

<style scoped>
.wrapper {
  position: relative;
}

.container {
  margin-top: 100px;
  text-align: center;
  overflow: auto;
}

.reviews {
  margin: 2em;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  border: 1px solid black;
  min-width: 400px;
}

.cart {
  box-sizing: border-box;
  --cart-width: 7em;
  text-align: center;
  width: var(--cart-width);
  padding: 1em;
  padding-bottom: 2em;
  border: 1px solid lightgray;
  font-weight: bold;
  position: absolute;
  top: -5em;
}

.cartItems {
  position: absolute;
  left: 15%;
  font-weight: normal;
}
.cartItemsNumber {
  width: 2em;
  text-align: right;
}
h1 {
  font-size: 1.5em;
}

ul {
  list-style-type: none;
  padding: 0;
}

/* hr */
li:nth-child(n + 2):before {
  content: '';
  border: 1px solid;
  border-image: linear-gradient(
      to right,
      white,
      lightgrey,
      darkgrey,
      lightgrey,
      white
    )
    1;
  position: absolute;
  top: 0.5em;
  width: 100%;
}

.cartButton {
  border-radius: 5px;
  font-family: Quicksand, sans-serif;
  font-size: 1em;
  font-weight: bold;
  color: white;
  background-color: #00a2fe;
  border: 0;
  margin: 0.5em 1em;
  padding-top: 0.6em;
  padding-bottom: 0.6em;
  min-width: 8em;
  max-height: 2.45em;
  white-space: nowrap;
  /* max-height = 1.0em * 1.25 (default line height) + 0.6em * 2 */
}

.cartButton:hover {
  background-color: dodgerblue;
}

.cartButton:active {
  background-color: orange;
}

.cartButton:focus {
  outline: 0;
}

.cartButton:disabled {
  background-color: #ccc;
}

a {
  position: relative;
  text-decoration: none;
  padding: 0.7em 1em;
}

span {
  display: inline-block;
}

.heading {
  font-size: 1.8em;
  text-decoration: underline;
}

.productgrid {
  display: grid;
  margin-left: auto;
  margin-right: auto;
  padding-top: 1em; /* this is space for the hr */
  grid-template-columns: 1fr 1.5fr;
  grid-template-rows: 200px;
  grid-column-gap: 1em;
  position: relative; /* for hr */
}

.name {
  position: relative;
  font-size: 1.6em;
  font-weight: bold;
  margin-bottom: 0.2em;
}

.sale {
  right: 0.75em;
  bottom: 0.15em;
  position: absolute;
  font-size: 0.6em;
  font-weight: bold;
  color: rgb(255, 0, 0, 1);
  /* transform: rotate(5deg); */
}

.description {
  position: relative;
  font-size: 1.1em;
  font-style: italic;
  font-size: 300;
  font-family: 'Raleway', sans-serif;
  max-height: 4.84em;
  overflow: hidden;
  padding-right: 1em;
  text-align: justify;
}

/* ellipsis */
.description:before {
  font-weight: bold;
  content: '...';
  position: absolute;
  right: 0;
  bottom: 0;
}

.description:after {
  content: '';
  position: absolute;
  right: 0;
  width: 1em;
  height: 1em;
  background: white;
}

.image {
  display: grid;
  justify-items: center;
}

.inStock {
  font-weight: bold;
  color: green;
  position: relative;
  left: -0.4em;
}

.inStockQty {
  font-weight: normal;
  white-space: nowrap;
}

.outOfStock {
  font-size: 1.1em;
  font-weight: bold;
  color: red;
  white-space: nowrap;
  position: relative;
  left: -0.4em;
  top: 0.5em;
}

.strikeThrough {
  text-decoration: line-through;
}

.column {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.spreadeven {
  display: flex;
  justify-content: space-around;
  align-items: center;
  position: relative;
}

.spreadevengrid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  position: relative;
}

.variety {
  box-sizing: border-box;
  border: 1px solid black;
  margin: 0 0.1em;
  display: inline-block;
  width: 2em;
  height: 2em;
  position: relative;
  left: -0.4em;
  top: 0.2em;
}

.variety:after {
  box-sizing: border-box;
  content: '';
  position: absolute;
  border: 1px solid lightgray;
  left: 0;
  bottom: -0.5em;
  width: 100%;
  opacity: 0;
  transition: opacity 3s, border-color 1s;
}

.variety.varietySelected:after {
  border: 1px solid orange;
  opacity: 1;
  transition: opacity 1s;
}

@media (min-width: 0px) and (max-width: 767px) {
  .productgrid {
    width: 90%;
  }
  .reviews {
    width: 85%;
  }
  .description {
    font-size: 0.9em;
  }

  .cart {
    left: 50%;
    margin-left: calc(var(--cart-width) / -2);
  }

  img {
    transform: scale(0.8);
  }
}

@media (min-width: 768px) {
  .productgrid {
    width: 70%;
  }
  .reviews {
    width: 65%;
  }

  .description {
    font-size: 0.95em;
  }

  img {
    transform: scale(0.85);
  }

  .cart {
    right: 20%;
  }
}

@media (min-width: 992px) {
  .productgrid {
    width: 50%;
  }
  .reviews {
    width: 45%;
  }

  .description {
    font-size: 1em;
  }

  img {
    transform: scale(0.9);
  }

  .cart {
    right: 25%;
  }
}

@media (min-width: 1200px) {
  .productgrid {
    width: 45%;
  }
  .reviews {
    width: 40%;
  }

  img {
    transform: scale(1);
  }

  .cart {
    right: 30%;
  }
}
</style>
