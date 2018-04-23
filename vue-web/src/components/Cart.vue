<template>
  <div class="main">
    <div class="container">
      <div class="cart">
        <div class="checkout-title">
          <span>购物车</span>
        </div>
        <transition></transition>
        <!--table-->
        <div class="item-list-wrap">
          <div class="cart-item">
            <ul>
            <div class="cart-item-head">
                <li>商品信息</li>
                <li>商品金额</li>
                <li>商品数量</li>
                <li>总金额</li>
                <li>编辑</li>
              </ul>
            </div>
            <ul class="cart-item-list">
              <li v-for="(item,index) in productList">
                <div class="cart-tab-1">
                  <div class="cart-item-check">
                    <a href="javascript:;" class="item-check-btn" v-bind:class="{'check': item.checked}"
                       @click="selectProduct(item)">

                    </a>
                  </div>
                  <div class="cart-item-pic">
                    <img :src="item.productImage" alt="烟">
                  </div>
                  <div class="cart-item-title">
                    <div class="item-name">{{ item.productName }}</div>
                  </div>
                  <div class="item-include">
                    <dl>
                      <dt>赠送:</dt>
                      <dd v-for="part in item.parts" v-text="part.partsName"></dd>
                    </dl>
                  </div>
                </div>
                <div class="cart-tab-2">
                  <div class="item-price">{{ item.productPrice | formatMoney }}</div>
                </div>
                <div class="cart-tab-3">
                  <div class="item-quantity">
                    <div class="select-self select-self-open">
                      <div class="quantity">
                        <a href="javascript:;" v-on:click="changeMoney(item,-1)">-</a>
                        <input type="text" value="0" v-model="item.productQuantity">
                        <a href="javascript:;" @click="changeMoney(item,1)">+</a>
                      </div>
                    </div>
                    <div class="item-stock">有货</div>
                  </div>
                </div>
                <div class="cart-tab-4">
                  <div class="item-price-total">{{item.productPrice * item.productQuantity | money('元')}}</div>
                </div>
                <div class="cart-tab-5">
                  <div class="cart-item-operation">
                    <a href="javascript:void 0" class="item-edit-btn" @click="itemConfirm(item)">
                    x
                    </a>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
        <hello-world></hello-world>
        <carousel></carousel>


        <!-- footer -->
        <div class="cart-foot-wrap">
          <div class="cart-foot-l">
            <div class="item-all-check">
              <a href="javascript:void 0">
                    <span class="item-check-btn" :class="{'check': checkedAllFlag}" @click="ckecdedAll(true)">
                      <svg class="icon icon-ok"><use xlink:href="#icon-ok"></use></svg>
                    </span>
                <span>全选</span>
              </a>
            </div>
            <div class="item-all-del">
              <a href="javascript:void 0" class="item-del-btn" @click="ckecdedAll(false)">
                <span>取消全选</span>
              </a>
            </div>
          </div>
          <div class="cart-foot-r">
            <div class="item-total">
              Item total: <span class="total-price">{{ totalMoney }}</span>
            </div>
            <div class="next-btn-wrap">
              <!--<a router-link="/address" class="btn btn&#45;&#45;red" style="width: 200px">结账</a>-->
              <router-link class="btn btn--red" tag="a" to="/address">aaaa</router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--tab-->
    <div class="md-modal modal-msg md-modal-transition" id="showModal" :class="{'md-show': modelFlag}">
      <div class="md-modal-inner">
        <div class="md-top">
          <button class="md-close" @click="modelFlag = false">关闭</button>
        </div>
        <div class="md-content">
          <div class="confirm-tips">
            <p id="cusLanInfo">你确认删除此订单信息吗?</p>
          </div>
          <div class="btn-wrap col-2">
            <button class="btn btn--m" id="btnModalConfirm" @click="itemDelte()">Yes</button>
            <button class="btn btn--m btn--red" id="btnModalCancel" @click="modelFlag = false">No</button>
          </div>
        </div>
      </div>
    </div>
    <!--遮罩层-->
    <div class="md-overlay" v-if="modelFlag"></div>
  </div>
</template>

<script>
import HellowWorld from './HelloWorld';
import Carousel from './Carousel';
import Transition from './transition';

  export default {
    name: 'Cart',
    data() {
      return {
        totalMoney: 0,
        productList: [],
        checkedAllFlag: false,
        modelFlag: false,
        curProduct: ''
      }
    },
    filters: {
      formatMoney(value) {
        return '￥' + value.toFixed(2);
      }

    },

    mounted() {
      this.$nextTick(() => {
        this.cartView()
      })

    },
    //  定义vue中的方法
    methods: {
      cartView() {
        this.$http.get('static/data/cartData.json', {'id': 123}).then(res => {
          this.productList = res.data.result.list;
          console.log(res.data.result.list)
        }).catch(error => {
          console.log(1111)
        })
      },
      changeMoney(product, way) {
        console.log(product, way);
        product.productQuantity += way;
        if (product.productQuantity < 1) {
          product.productQuantity = 1;
        }
        this.countTotal();
      },
      selectProduct(item) {
        if (typeof item.checked == "undefined") {
          this.$set(item, 'checked', true);
        } else {
          //  变成false之后再点击无法切换，所以此处要取反
          item.checked = !item.checked;
          console.log(item)
        }
        this.countTotal();
      },
      ckecdedAll(flag) {
        this.checkedAllFlag = flag;
        this.productList.forEach((item, index) => {
          if (typeof item.checked == "undefined") {
            this.$set(item, 'checked', flag)
          } else {
            //  变成false之后再点击无法切换，所以此处要取反
            item.checked = this.checkedAllFlag
          }
        });
        this.countTotal();
      },
      //  计算总数的方法
      countTotal() {
        this.totalMoney = 0;
        this.productList.forEach((item, index) => {
          if (item.checked) {
            this.totalMoney += item.productPrice * item.productQuantity
          }
        })
      },
      itemConfirm(item){
        this.modelFlag = true;
        this.curProduct = item;
      },
      itemDelte(){
        let index = this.productList.indexOf(this.curProduct);
        this.productList.splice(index,1);
        this.modelFlag = false;
      }
    },
    components:{
      'hello-world':HellowWorld,
      'carousel': Carousel,
      'transition': Transition
    }
  }
</script>

<style>
  @import '../assets/css/base.css';
  @import "../assets/css/checkout.css";
  @import '../assets/css/modal.css';
  @import '../assets/css/reset.css';

  .quantity a {
    padding: 0 10px;
    background: #333;
    color: #fff;
  }
</style>
