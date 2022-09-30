<template>
  <div>
    <h1>App跟组件</h1>
    <es-header title="购物车案例"></es-header>
    <es-goods       v-for="item in goodslist"
    :key="item.id"
    :id="item.id"
    :thumb="item.goods_img"
    :title="item.goods_name"
    :price="item.goods_price"
    :count="item.goods_count"
    :checked="item.goods_state"
    @stateChange="onGoodsStateChange"
    @countChange="onGoodsCountChange"></es-goods>
    <es-footer :amount="amount" :total= "total"   @fullChange="onFullStateChange"></es-footer>
  </div>
</template>

<script>
  import EsHeader from './components/EsHeader.vue'
  import EsFooter from './components/EsFooter.vue'
  import EsGoods from './components/EsGoods.vue'
  import EsCounter from './components/EsCounter.vue'
  export default{
    name: 'MyApp',
    components:
    {
      EsHeader,
      EsFooter,
      EsGoods,
      EsCounter,
    },
    created()
    {
      this.getGoodsList();
    },
    data(){
      return {
        goodslist:[],
      }
    },
    computed:
    {
      amount(){
        let a= 0;
        this.goodslist.filter(x => x.goods_state).forEach(x =>{ a += x.goods_price*x.goods_count});
        return a;
      },
      total(){
        let t= 0;
        this.goodslist.filter(x => x.goods_state).forEach(x =>{ t += x.goods_count});
        return t;
      },
    },
    methods: {
      async getGoodsList(){
        const {data: res } = await this.$http.get('api/cart');
        if(res.status!=200)
          return alert("失败");
        this.goodslist = res.list;
      },
      onFullStateChange(isFull)
      {
        this.goodslist.forEach(x =>{ x.goods_state = isFull});
      },
      onGoodsStateChange(e)
      {
        const findResult = this.goodslist.find(x => x.id === e.id);
        if (findResult) {
       findResult.goods_state = e.value
        }
      },
      onGoodsCountChange(e) {
        // 根据 id 进行查找
        const findResult = this.goodslist.find(x => x.id === e.id)
        // 找到了对应的商品，则更新其数量
        if (findResult) {
        findResult.goods_count = e.value
        }
        }
    },
  }
</script>
