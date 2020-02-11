<style scoped>
img{
  width: 50px
}
.d-flex{
  display: flex;
}
.flex-cus{
  justify-content: space-between;
  margin: 0 auto;
}
.container{
  background-color: #d5d4d4;
  width: 40%;
  position: fixed;
  height: 100vh;
  overflow-y: hidden;
  right: 25px;
  top: 65px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: all ease .5s    
}
.container.hide{
    top: 100vh;
}
.container .head-cart{
  padding: 10px
}
.container .head-cart i{
  position: absolute;
  right: 5px;
  top: 6px;
}
.container .item{
  width: 90%;
}
.container .content-cart{
    background-color: #d5d4d4;
}
.head-cart{
  padding: 15px;
  background-color: #d71249;
  color: #fff
}
.content-cart{
  padding-top: 10px;
  padding-bottom: 65px;
  max-height: 100vh;
  overflow-y: auto;
  background-color: #ece8e8;
}
.item{
  width: 50%;
  margin: 0 auto;
}
.head-item{
  background-color: #fafafa;
  padding: 10px 25px;
  color: #676262;  
  border-bottom: 1px solid #ece8e8;  
}
.head-item h4{
  line-height: 1.52rem;
    padding-left: 10px;
}
.body-item{
  background-color: #fff;  
  padding: 15px 25px;
  justify-content: space-between;
}
.footer-item{
  background-color: #fff;  
  justify-content: space-between;
  padding: 10px 25px;
  margin: 0 auto 25px;
}
.body-item-except{
  margin-top: -25px;
  border-top: 1px solid #ece8e8;  
}
.text-item{
  text-align: left;
  width: 80%;
}
.body-item img{
  width: 13%;
  border: 1px solid #ded5d5;
}
.cost{
  font-weight: 700;
}
.footer-cart{
  padding: 15px;
  position: relative;
  bottom: 61px;
  background-color: #fff;
  border: 1px solid #ece8e8;  
}
.container-fix{
  box-sizing: border-box;
  bottom: -80px;
  right: 25px;
  position: fixed;
  width: 40%;
  padding: 15px;
  background-color: #fff;
  border: 1px solid #ece8e8;
  transition: all ease .9s
}
.container-fix.show{
  bottom: 0;
}
.container-fix i{
  position: absolute;
  right: -15px;
  top: -28px;
  color: #d71249;
  font-size: 1.5rem;
  background-color: #fff;
  border-radius: 50%;
}
.head-item .material-icons{
  color: #676262;
}
.material-icons{
  color: #ece8e8;
}
.material-icons.checked{
  color: #d71249
}
.material-icons.checked:hover{
  color: #d71249
}
.material-icons:hover{
  cursor: pointer;
  color: #d5d4d4
}
.btn{
  background-color: #d71249;
  color: #fff;
  border: none;
  padding: 10px 20px;
}
.val-buy span{
  cursor: pointer;
  background-color: #fafafa;
  padding: 5px 10px;
  margin: 0 5px;
  border: 1px solid #ece8e8;
}
.val-buy input{
  width: 60px;
  padding: 8px 6px 6px 6px;
  background-color: #ece8e8;
  border: none;
  text-align: center;  
}
.spacing{
  height: 120px;
  width: 100%;
}
.total{
  text-align: left
}
.hidden{
  display: none;
}
.contain-shop .item{
  margin: 0 10px;
}
.contain-shop .content-cart{
  max-height: none;
}
.p-relative{
  position: relative;
}
</style>

<template>
  <div class="main">
    <div class="contain-shop">

      <div class="head-cart">
        <h4>MY SHOP </h4>
      </div>
      <div class="content-cart">

        <div class="item" v-for="(item,key) in listProd" :key="key">
          <div v-if="item.store.id != cond" class="head-item d-flex">
            <i class="material-icons">local_grocery_store</i> <h4>{{item.store.name}}</h4>
          </div>
          <div v-if="item.store.id == cond" class="body-item d-flex body-item-except">
            <div class="text-item">
              <p>{{listProd[key].name}}</p>
              <label class="cost">Rp. {{formatRupiah(item.price)}}</label>
            </div>
            <img :src="item.stuff.image_url">
          </div>
          <div v-else class="body-item d-flex">
            <div class="text-item">
              <p>{{item.name}}</p>
              <label class="cost">Rp. {{formatRupiah(item.price)}}</label>
            </div>
            <img :src="item.stuff.image_url">
          </div>
          <div class="footer-item d-flex">
            <div class="val-buy">
              Available : <input type="text" disabled :value="item.stuff.stock">
            </div>
            <i v-if="item.stuff._select == true " class="material-icons checked">check_box</i>
            <i v-else class="material-icons" @click="addCart(item.id)">library_add</i>
          </div>        
          <span class="hidden">{{cond = item.store.id}}</span>
        </div>
      </div>

    </div>

    <div class="container-fix" :class="{  show : hideCart }">
      <div class="d-flex flex-cus p-relative">
        <div class="total">TOTAL <br><strong>Rp. {{formatRupiah(totalCost)}}</strong></div>
        <button class="btn">Bayar</button>
        <i class="material-icons" @click="maxi">keyboard_arrow_up</i>
      </div>      
    </div>
    
    <div class="container" :class="{ hide : hideCart}">
      <div class="head-cart p-relative">
        <h4>Keranjang Belanja </h4>
        <i class="material-icons" @click="minimize">keyboard_arrow_down</i>
      </div>
      <div class="content-cart">

        <div class="item" v-for="(buy,keys) in buyList" :key="keys">
          <div v-if="buy.storeId != condBuy && keys != 0" class="head-item d-flex hmm">
            <i class="material-icons">local_grocery_store</i> <h4>{{buy.storeName}}</h4>
          </div>
          <div v-if="keys == 0" class="head-item d-flex">
            <i class="material-icons">local_grocery_store</i> <h4>{{buy.storeName}}</h4>
          </div>          
          <div v-if="buy.storeId == condBuy && keys != 0" class="body-item d-flex body-item-except">
            <div class="text-item">
              <p>{{buy.name}}</p>
              <label class="cost">Rp. {{formatRupiah(buy.price)}}</label>
            </div>
            <img :src="buy.image_url">
          </div>
          <div v-else class="body-item d-flex">
            <div class="text-item">
              <p>{{buy.name}}</p>
              <label class="cost">Rp. {{formatRupiah(buy.price)}}</label>
            </div>
            <img :src="buy.image_url">
          </div>
          <div class="footer-item d-flex">
            <div class="val-buy">
              <span v-if="buy.stock > 1" @click="min(keys)">-</span>
              <span v-else>-</span>
              <input type="text" :value="buy.stock">
              <span v-if="buy.stock == buy._stock">+</span>
              <span v-else @click="plus(keys)">+</span>
            </div>
            <i class="material-icons" @click="trashCart(buy.id)">delete</i>
          </div>        
          <span class="hidden">{{condBuy = buy.storeId}}</span>
        </div>

      </div>
      <div class="footer-cart">
        <div class="d-flex flex-cus">
          <div class="total">TOTAL <br><strong>Rp. {{formatRupiah(totalCost)}}</strong></div>
          <button class="btn">Bayar</button>
        </div>
      </div>
    </div>

  </div>  
</template>

<script>

  export default {
    name: 'Cart',
    data() {
      return {
        cond: null,
        condBuy: null,
        buyList: [],
        listProd: null,
        totalCost: 0,
        hideCart: true
      }
    },
    computed: {
    },
    watch: {
      buyList: {
        handler() {
          this.getTotal()
        },
        deep: true
      }
    },
    methods: {
      minimize(){
        this.hideCart = true
      },
      maxi(){
        this.hideCart = false
      },
      min(key){
      console.log(key)
        this.buyList[key].stock = this.buyList[key].stock - 1
        this.listProd[key].stuff.stock = this.listProd[key].stuff.stock + 1
      },
      plus(key){
        this.buyList[key].stock = this.buyList[key].stock + 1
        this.listProd[key].stuff.stock = this.listProd[key].stuff.stock - 1
      },      
      trashCart(itemId){
        this.buyList.splice(id,1)
        let id = this.listProd.findIndex((obj => obj.id == itemId));
        this.listProd[id].stuff.stock = this.listProd[id].stuff._stock
        this.listProd[id].stuff._select = false

      },
      addCart(itemId){
        let id = this.listProd.findIndex((obj => obj.id == itemId));
        this.listProd[id].stuff._stock = this.listProd[id].stuff.stock
        this.listProd[id].stuff._select = true
        this.buyList.push({
          id: this.listProd[id].id,
          name: this.listProd[id].name,
          price: this.listProd[id].price,
          storeId: this.listProd[id].store.id,
          storeName: this.listProd[id].store.name,
          image_url: this.listProd[id].stuff.image_url,
          stock: this.listProd[id].stuff.stock,
          _stock: this.listProd[id].stuff._stock,
        })
        this.listProd[id].stuff.stock = 0
      },
      formatRupiah(number, prefix){
        let number_string = number.toString().replace(/[^,\d]/g, "").toString(),
          split = number_string.split(","),
          sisa = split[0].length % 3,
          rupiah = split[0].substr(0, sisa),
          ribuan = split[0].substr(sisa).match(/\d{3}/gi);

        if (ribuan) {
          let separator = sisa ? "." : "";
          rupiah += separator + ribuan.join(".");
        }

        rupiah = split[1] != undefined ? rupiah + "," + split[1] : rupiah;
        return prefix == undefined ? rupiah : rupiah ? "Rp. " + rupiah : "";
      },
      getTotal(){
        let total = 0
        for (let index = 0; index < this.buyList.length; index++) {
          let val = parseInt(this.buyList[index].price) * parseInt(this.buyList[index].stock, 10);
          total += val
        }
        return this.totalCost = total
      }
    },
    created(){
      fetch("data.json")
        .then( res =>
          res.json()
        )
        .then((data) => {
          return this.listProd = data.items.sort((a, b) => (a.store.id === b.store.id) ? 1 : +1)
        }).then(()=>{
          this.getTotal()
        });
    }
  }
</script>
