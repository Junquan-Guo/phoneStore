<template>
    <div>
        <pay-header title="结算"></pay-header>
        <div to="address" class="pay-address" >
             <p class="address-box">
                <span class="name">收货人：junquan</span>
                <span class="phone">15914967410</span>
            </p>
            <p class="address-details">
                收货地址：广东工贸职业技术学院 广州大道北1098号
            </p>
        </div>
        <div class="pay-shop" v-for="(list,index) in pay" :key="index">
            <div class="pay-shop-list">
                <p class="pay-shop-1">商品清单</p>
                <p class="pay-shop-2">
                    <img :src="list.homeImg">
                    <p class="pay-shop-2-box">
                        <span class="name">{{list.homeName}}<p>× {{$route.query.value}}</p></span>
                        <!-- <span>颜色：冰钻黑</span> -->
                        <span class="price">¥ {{list.homePrice}}</span>
                    </p>
                </p>
            </div>

            <div class="pay-shop-invoice">
                <p class="pay-invoice-1">发票信息</p>
                <div class="pay-invoice-2">
                    <div class="pay-invoice-2-2">
                        <div v-show="invoiceIndex===0">
                            <p>*请输入发票抬头:</p>
                            <input type="text" id="input" v-model="list.text" placeholder="请输入发票信息">
                            
                        </div>
                    </div>
                   
                </div>
            </div>

            <div class="pay-shop-fs">
                <div class="pay-fs-1">支付方式</div>
                <div class="pay-fs-2">
                    <!-- <div class="pay-fs-2-1" v-for="(item,index) in lists" :class="{active:index===ceshi}" @click="btn(index)" >
                        {{item.name}}
                    </div> -->
                    <div class="pay-fs-2-1" >
                        <div v-for="(list,index) in lists" :class="{active:index===listIndex}" @click="btn(list.name,index)">{{list.name}}</div>
                    </div>
                    <div class="pay-fs-2-2">
                       <div v-show="listIndex===0" class="pay-fs-2-2-1">支持支付宝支付、微信支付、银行卡支付、财付通等</div>
                       <div v-show="listIndex===1" class="pay-fs-2-2-2">花呗分期是花呗联合天猫淘宝推出的，面向互联网的赊购服务，通过支付宝轻松还款，0首付</div>
                       <div v-show="listIndex===2" class="pay-fs-2-2-3">货到再付款，支持现金交易</div>
                    </div>
                </div>
                
            </div>

            <div class="pay-shop-liuyan">
                <p class="pay-liuyan-1">订单留言</p>
                <div class="pay-liuyan-2">
                    <textarea v-model="list.ly" rows="5" placeholder="限300字（若有特殊需求，请联系商城在线客服)" maxlength="300"></textarea>
                    <p>商品总金额：¥{{$route.query.value*list.homePrice}}</p>
                    <p>运费：0.00</p>
                    <p>优惠：¥0.00</p>
                    <p>赠送积分：{{$route.query.value*list.homePrice}}</p>
                   
                </div>
            </div>
           
            <!-- <span>{{list.id}}</span>
            <span>{{list.homeName}}</span> -->

            <div class="pay-shop-footer">
                <p class="price">订单总金额：<span>¥{{$route.query.value*list.homePrice}}</span></p>
                <a class="order" @click="addOrder(list,index)">立即结算</a>
            </div>
        </div>
    </div>
</template>

<script>
import { Toast } from "mint-ui";
import { mapGetters, mapMutations } from "vuex"
import PayHeader from "../../components/header"
import axios from "axios"
export default {
  name: "pay",
  data() {
    return {
      listIndex: 0,
      invoiceIndex: 0,
      pay: [],
      lists: [
        {
          id: "1",
          name: "在线支付"
        },
        {
          id: "2",
          name: "花呗分期"
        },
        {
          id: "3",
          name: "货到付款"
        }
      ],
      text: "",
      ly: ""
    };
  },
  components: {
    PayHeader
  },
  //    computed: {
  //         address() {
  //         return this.$store.state.address;
  //         },
  //         ...mapGetters(
  //             ["this.$store.state.address"],
  //         )
  //     },
  methods: {
    btn(id, index) {
      this.listIndex = index;
    },
    invoiceClick(index) {
      this.invoiceIndex = index;
    },
    addOrder(id, index) {
      if (id.text == undefined) {
        Toast({
          message: "请输入发票抬头",
          duration: 950
        });
      } else {
        var data = {
          id: id.id,
          name: id.homeName,
          price: id.homePrice,
          text: id.text,
          ly: id.ly,
          img: id.homeImg,
          listname: this.lists[index].name,
          value: this.$route.query.value
        };
        this.$store.dispatch("setOrders", data);
        var _this = this;
        var time = setInterval(function() {
          _this.$router.push({
            path: "success"
          });
          clearInterval(time);
        }, 1000);
      }
    }
  },
  created() {
    var _this = this;
    var id = this.$route.query.id;
    var value = this.$route.query.value;
    axios.get("/ceshi.json").then(function(res) {
      for (var i = 0; i < res.data.data.set.length; i++) {
        if (res.data.data.set[i].id == id) {
          _this.pay.push(res.data.data.set[i]);
        }
      }
    });
    axios.get("/ceshi.json").then(function(res) {
      for (var i = 0; i < res.data.data.home.length; i++) {
        if (res.data.data.home[i].id == id) {
          _this.pay.push(res.data.data.home[i]);
        }
      }
    });
  }
};
</script>

<style lang="sass" scoped>
@import '../../assets/styles/common.scss'

.active 
    border: 1px solid #444
    color: red

.pay-address 
  width: 100%
  height: px2rem(280)
  background: url('https://shopstatic.vivo.com.cn/vivoshop/wap/dist/images/prod/bg-addr-box-line_d380baa.png') #fff left bottom repeat-x; // shopstatic.vivo.com.cn/vivoshop/wap/dist/images/prod/bg-addr-box-line_d380baa.png) #fff left bottom repeat-x;
  background-size: px2rem(104)
  padding-top: px2rem(96px)
  display: block

  .address-box 
    width: 87%
    margin: auto 
    font-size: px2rem(22px)
    padding-top: px2rem(20px)
    padding-bottom: px2rem(20px)

    .phone 
      float: right 

  .address-details 
    width: 87% 
    margin: auto 
    color: #666 
    font-size: px2rem(25px)


.pay-shop  
  width: 100% 
  margin-bottom: px2rem(96px) 

  .pay-shop-invoice  
    width: 100% 
    height: px2rem(280px)
    background: #fff 
    margin-bottom: 10px 
    margin-top: 10px 

  .pay-invoice-1  
    width: 100% 
    height: px2rem(96px) 
    line-height: px2rem(96px) 
    border-bottom: 1px solid #eaeaea 
    font-size: px2rem(22px) 
    padding-left:px2rem(46px)

  .pay-invoice-2  
    width: 100% 
    height: px2rem(260px) 

    .pay-invoice-2-1  
      width: 100% 
      height: 30% 

      div  
        display: block 
        width: px2rem(188px)
        height: px2rem(58px) 
        line-height: px2rem(58px)  
        border: 1px solid #d1d1d1 
        border-radius: 3px 
        margin: px2rem(6.5px) px2rem(20px)
        float: left 
        text-align: center 

    .pay-invoice-2-2  
      width: 92% 
      height: 70% 
      margin: auto 
      font-size px2rem(22px)

      p  
        margin: 10px 0 
        font-size: px2rem(22px)

      input  
        width: 100% 
        height: px2rem(77px)
        border: 1px solid #d1d1d1 
        border-radius: 3px 
        padding-left: px2rem(13px)

  .pay-shop-list  
    width: 100% 
    height: px2rem(292px)
    margin-top: px2rem(20px)
    background: #fff 

    .pay-shop-1  
      width: 100% 
      height: px2rem(96px)
      line-height: px2rem(96px) 
      border-bottom: 1px solid #eaeaea 
      font-size: px2rem(26px) 
      padding-left: px2rem(46px)

    .pay-shop-2  
      float: left 

      img  
        width: px2rem(162px)
        margin:px2rem(13px)

    .pay-shop-2-box  
      width: 70% 
      display: flex 
      flex-direction: column 

      .name  
        font-size: px2rem(26px) 
        margin-top: px2rem(18px) 
        height: px2rem(39px)

        p  
          float: right 
          margin-right: px2rem(32px) 

      .price  
        color: red 
        font-size: px2rem(22px)
        font-weight: 500 
        height: px2rem(39px) 

  .pay-shop-liuyan  
    width: 100% 
    height: px2rem(442px)
    background: #fff 
    margin-top: px2rem(20px)
    margin-bottom: px2rem(20px)

    .pay-liuyan-1  
      width: 100% 
      height: px2rem(96px)
      line-height: px2rem(96px)
      border-bottom: 1px solid #eaeaea 
      font-size: px2rem(26px)
      padding-left: px2rem(46px)

    .pay-liuyan-2  
      width: 90% 
      margin: auto 

      textarea  
        width: 100% 
        height: px2rem(130px) 
        border: 1px solid #d1d1d1 
        border-radius: 3px 
        padding: px2rem(10px) px2rem(13px) 
        margin: px2rem(20px) auto 
        display: block 

      p  
        color: #888 
        height: px2rem(31px)
        font-size: px2rem(22px)

  .pay-shop-fs  
    width: 100% 
    height: px2rem(325px) 
    background: #ffffff 

    .pay-fs-1  
      width: 100% 
      height: px2rem(96px)
      line-height: px2rem(96px)
      border-bottom: 1px solid #eaeaea 
      font-size: px2rem(26px) 
      padding-left: px2rem(46px)

    .pay-fs-2  
      width: 100% 
      height: px2rem(228px)
      background: #ffffff 

      .pay-fs-2-1  
        width: 100% 
        height: 40% 
        font-size: px2rem(22px)
        display: flex 
        justify-content: center 
        align-items: center 

        div  
          display: block 
          width: px2rem(187px)
          height: px2rem(58px) 
          line-height: px2rem(58px) 
          border: 1px solid #d1d1d1 
          border-radius: 3px 
          margin: px2rem(6.5px)
          float: left 
          text-align: center 

      .pay-fs-2-2  
        width: 100% 
        height: 60% 
        font-size: px2rem(22px) 

        div  
          width: 90% 
          height: px2rem(85px)
          border-radius: 3px 
          border: 1px solid #d1d1d1 
          margin: auto 
          padding: px2rem(20px)

        .pay-fs-2-2-2  
          height: px2rem(101px)

  .pay-shop-footer  
    width: 100% 
    height: px2rem(96px) 
    border-top: 1px solid #eaeaea 
    background: white 
    position: fixed 
    bottom: 0 

    .price  
        float: left 
        line-height: px2rem(96px) 
        font-size: px2rem(28px)
        color: #666 
        padding-left: px2rem(20px) 

        span 
          color: red 

    .order 
      width: px2rem(214px) 
      height: px2rem(58px)
      line-height: px2rem(58px)
      font-size: px2rem(22px)  
      margin-top: px2rem(20px)  
      margin-right: px2rem(20px) 
      border-radius: 30px 
      text-align: center 
      color: #fff 
      background: #f81200 
      float: right

</style>




