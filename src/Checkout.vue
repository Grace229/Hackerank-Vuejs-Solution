<template>
  <div class="layout-row">
    <ProductList class="flex-70" :products="products" @add-to-cart="addToCart" @remove-from-cart="removeFromCart" />
    <Cart class="flex-30" :cart="cart" @get-total="getTotal" />
  </div>
</template>

<script>
import ProductList from "@/components/ProductList";
import Cart from "@/components/Cart";

export default {
  name: "Checkout",
  components: {Cart, ProductList},
  data() {
    return {
      cart: {
        items: [],
        subTotal: 0,
        totalPrice: 0,
        discount: 0,
        selectedCoupon: 0
      },
      products: []
    }
  },
  created() {
    this.products = PRODUCTS.map((product, index) => {
      product.id = index + 1;
      product.image = `/images/items/${product.name.toLocaleLowerCase()}.png`;
      product.cartQuantity = 0;
      return product;
    });
  },
  methods: {
    addToCart(product) {
      const index = this.products.findIndex(p => p.id === product.id);
      this.products[index].cartQuantity = 1;
      this.products[index].addedToCart = true;
      this.cart.subTotal = this.products[index].price + this.cart.subTotal
      if(this.selectedCoupon === 0){
 this.cart.totalPrice = this.cart.subTotal
      }else{
       this.cart.discount = this.cart.selectedCoupon / 100 * this.cart.subTotal;
       this.cart.totalPrice = this.cart.subTotal - this.cart.discount;
      }
      this.cart.items.push({
        id: this.products[index].id,
        item: this.products[index].heading,
        price: this.products[index].price,
        quantity: 1,
      });
    },
    removeFromCart(product) {
      const index = this.products.findIndex(p => p.id === product.id);
      this.products[index].cartQuantity = 0;
      this.products[index].addedToCart = false;
       this.cart.subTotal = this.cart.subTotal- this.products[index].price 
          if(this.selectedCoupon === 0){
 this.cart.totalPrice = this.cart.subTotal
      }else{
       this.cart.discount = this.cart.selectedCoupon / 100 * this.cart.subTotal;
       this.cart.totalPrice = this.cart.subTotal - this.cart.discount;
      }
      const cartIndex = this.cart.items.findIndex(c => c.id === product.id);
      this.cart.items.splice(cartIndex, 1);
    },
    getTotal(coupon) {
this.cart.selectedCoupon = coupon;
this.cart.discount = coupon / 100 * this.cart.subTotal;
this.cart.totalPrice = this.cart.subTotal - this.cart.discount;
    }
  }
}
export const PRODUCTS = [
  {
    heading: "Cap - $10",
    name: "Cap",
    price: 10,
    addedToCart: false
  },
  {
    heading: "Hand Bag - $30",
    name: "HandBag",
    price: 30,
    addedToCart: false
  },
  {
    heading: "Shirt - $30",
    name: "Shirt",
    price: 30,
    addedToCart: false
  },
  {
    heading: "Shoes - $50",
    name: "Shoe",
    price: 50,
    addedToCart: false
  },
  {
    heading: "Pant - $40",
    name: "Pant",
    price: 40,
    addedToCart: false
  },
  {
    heading: "Slipper - $20",
    name: "Slipper",
    price: 20,
    addedToCart: false
  }
]

</script>

<style scoped>

</style>
