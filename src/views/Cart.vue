<template>
  <div class="ccontainer">
    <div class="pcard" v-for="product in cart" :key="product.id">
        <div class="poravnanje">
            <img :src="product.slika"/>
            <h3 > {{product.ime}} </h3>
            <button  class="btn"><i class="fa-solid fa-plus" @click.prevent="addToCart(product)"></i></button>
            <h4> {{product.kolicina}} </h4>
            <button style="margin-left: 1rem" class="btn"><i class="fa-solid fa-minus" @click.prevent="removeFromCart(product)"></i></button>
            <h4 style="margin-right: 1rem">Ukupno: {{product.kolicina * product.cijena}}kn</h4>
        </div>
    </div>
    <div class="poravnanjeTwo">
        <h3>Ukupno: {{finalPrice}}kn</h3>
        <button class="btn btn-success" @click.prevent="finish">Završi kupnju</button>
    </div>
  </div>
</template>

<script>
import {mapGetters, mapMutations} from "vuex"
import {addDoc, collection, db} from "@/firebase"

export default {
/* ime, kolicina, cijena, dodati/maknuti kolicinu, ukupno, kupovina  */
name: "Kosarica",
computed: {
    ...mapGetters({cart: "getCart", finalPrice: "getCartPrice"})
},
methods: {
    ...mapMutations({addToCart: "addToCart", removeFromCart: "takeFromCart", clearCart: "clearCart"}),
    async finish() {
        try {
            await addDoc(collection(db, "narudzbe"), {
                narudzba: "Normalna narudzba",
                kosarica: this.cart
            })
            this.clearCart();
            this.$router.push({name: "Thanks"})
        } catch (error) {
            console.error(error)
        }
    }
}
}
</script>

<style scoped>

.ccontainer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 1rem;
}
.pcard {
    background: white;
    width: 60%;
}

.pcard img {
    max-height: 150px;
    min-width: 150px;
    min-height: 150px;
    max-width: 150px;
    object-fit: cover;
}

.poravnanje {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.poravnanjeTwo {
    display: flex;
    margin-left: auto;
    justify-content: space-around;
    gap: 1rem;
    margin-right: 20rem;
    margin-top: 1rem;
    align-items: center;
}
</style>