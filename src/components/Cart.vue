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
  width: 50%;  
}
.container{
  background-color: #f5f5f5;
  position: relative;
  width: 100%;
  height: 100vh;
  overflow-y: hidden;
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
  position: fixed;
  bottom: 0;
  width: 100%;
  background-color: #fff;
  border: 1px solid #ece8e8;  
}
.head-item .material-icons{
  color: #676262;
}
.material-icons{
  color: #ece8e8;
}
.btn{
  background-color: #d71249;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin-right: 50px;
}
.val-buy span{
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
</style>

<template>
  <div class="container">
    <div class="head-cart">
      <h4>Keranjang Belanja </h4>
    </div>
    <div class="content-cart">

      <div class="item" v-for="(item,key) in buyList" :key="key">
        <div v-if="item.store.id != cond" class="head-item d-flex">
          <i class="material-icons">local_grocery_store</i> <h4>{{item.store.name}}</h4>
        </div>
        <div v-if="item.store.id == cond" class="body-item d-flex body-item-except">
          <div class="text-item">
            <p>{{item.name}}</p>
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
            <span v-if="item.stuff.stock > 0" @click="item.stuff.stock--">-</span>
            <span v-else>-</span>
            <input type="text" :value="item.stuff.stock">
            <span v-if="item.stuff.stock == arr[key]">+</span>
            <span v-else @click="item.stuff.stock++">+</span>
          </div>
          <i class="material-icons">delete</i>
        </div>        
        <span class="hidden">{{cond = item.store.id}}</span>
      </div>

      <div class="spacing"></div>
    </div>
    <div class="footer-cart">
      <div class="d-flex flex-cus">
        <div class="total">TOTAL <br><strong>Rp. {{formatRupiah(totalCost)}}</strong></div>
        <button class="btn">Bayar</button>
      </div>
    </div>
  </div>
</template>

<script>

  export default {
    name: 'Cart',
    props: {
      msg: String
    },
    data() {
      return {
        cond: null,
        buyList: null,
        totalCost: 0,
        arr : []
      }
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
      formatRupiah(number, prefix) {
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
          let val = parseInt(this.buyList[index].price) * parseInt(this.buyList[index].stuff.stock, 10);
          total += val
        }
        return this.totalCost = total
      }
    },
    mounted(){
      fetch("data.json")
        .then( res =>
          res.json()
        )
        .then((data) => {
          this.buyList = data.items.sort((a, b) => (a.store.id == b.store.id) ? 1 : -1)
          for (let index = 0; index < this.buyList.length; index++) {
            this.arr[index] = this.buyList[index].stuff.stock
          }
          return this.arr
        }).then(()=>{
          this.getTotal()
        });

    }
  }
</script>
