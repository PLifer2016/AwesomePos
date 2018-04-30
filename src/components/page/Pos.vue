<template>
    <div class="pos">
      <el-row>
        <el-col :span="7" class="orders" id="order-list" style="overflow: auto">
          <el-tabs type="card">
            <el-tab-pane label="点餐">
              <el-table :data="tableData"  border style="width: 100%">
                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                <el-table-column prop="count" label="数量" width="100"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="div-total">
                <small>数量：</small>{{totalCount}}, <small>金额：</small>{{totalMoney}}元
              </div>
              <div class="biv-btn">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="delAllGoods()">删除</el-button>
                <el-button type="success" @click="checkout()">结账</el-button>
              </div>
            </el-tab-pane>

            <el-tab-pane label="挂单">
              456
            </el-tab-pane>

            <el-tab-pane label="外卖">
              789
            </el-tab-pane>

          </el-tabs>
        </el-col>

        <el-col :span="17" class="products" id="product-list">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li v-for="item in oftenGoods" @click="addOrderList(item)">
                  <span>{{ item.goodsName }}</span>
                  <span class="o-price">￥{{ item.price }}元</span>
                </li>
              </ul>
            </div>
          </div>

          <div class="goods-type">
            <el-tabs type="card">
              <el-tab-pane label="汉堡">
                <ul class='cookList'>
                  <li v-for="item in type0Goods"  @click="addOrderList(item)">
                    <span class="foodImg"><img :src=item.goodsImg width="100%"></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>

              <el-tab-pane label="小食">
                <ul class='cookList'>
                  <li v-for="item in type1Goods"  @click="addOrderList(item)">
                    <span class="foodImg"><img :src=item.goodsImg width="100%"></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>

              <el-tab-pane label="饮料">
                <ul class='cookList'>
                  <li v-for="item in type2Goods"  @click="addOrderList(item)">
                    <span class="foodImg"><img :src=item.goodsImg width="100%"></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>

              <el-tab-pane label="套餐">
                <ul class='cookList'>
                  <li v-for="item in type3Goods"  @click="addOrderList(item)">
                    <span class="foodImg"><img :src=item.goodsImg width="100%"></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>

        </el-col>
      </el-row>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "pos",
      data () {
        return {
          tableData: [

          ],
          oftenGoods:[
            {
              goodsId:1,
              goodsName:'香辣鸡腿堡',
              price:18
            }, {
              goodsId:2,
              goodsName:'田园鸡腿堡',
              price:15
            }, {
              goodsId:3,
              goodsName:'和风汉堡',
              price:15
            }, {
              goodsId:4,
              goodsName:'快乐全家桶',
              price:80
            }, {
              goodsId:5,
              goodsName:'脆皮炸鸡腿',
              price:10
            }, {
              goodsId:6,
              goodsName:'魔法鸡块',
              price:20
            }, {
              goodsId:7,
              goodsName:'可乐大杯',
              price:10
            }, {
              goodsId:8,
              goodsName:'雪顶咖啡',
              price:18
            }, {
              goodsId:9,
              goodsName:'大块鸡米花',
              price:15
            }, {
              goodsId:20,
              goodsName:'香脆鸡柳',
              price:17
            }
          ],
          type0Goods:[


          ],
          type1Goods:[


          ],
          type2Goods:[


          ],
          type3Goods:[


          ],
          totalMoney: 0,
          totalCount: 0,
        }
      },

      created: function () {
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
          .then(response=> {
            console.log(response);
            //this.oftenGoods=response.data;
            this.type0Goods = response.data[0];
            this.type1Goods = response.data[1];
            this.type2Goods = response.data[2];
            this.type3Goods = response.data[3];
          })
          .catch(error=>{
            console.log(error);
            alert('网络错误，不能访问');
          })
      },
        mounted () {
          var orderHeight=document.body.clientHeight;
          document.getElementById("order-list").style.height=orderHeight+'px';
          document.getElementById("product-list").style.height=orderHeight+'px';
        },

      methods: {
          addOrderList (goods) {
            //判断商品是否在订单列表中
            let isHave = false;
            for(let i=0; i<this.tableData.length; i++) {
              if(this.tableData[i].goodsId == goods.goodsId) {
                isHave = true;
              }
            }

            //根据判断写业务逻辑
            if(isHave) {
              //改变列表中商品数量
              let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
              arr[0].count++;
            }else {
              let newGoods = {
                goodsId: goods.goodsId,
                goodsName: goods.goodsName,
                price: goods.price,
                count: 1
              };
              this.tableData.push(newGoods);
            }

            this.getAllMoney ();
          },

        //删除单个商品
        delSingleGoods (goods) {
            this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
            this.getAllMoney ();
        },

        delAllGoods () {
          this.tableData = [];
          this.totalCount = 0;
          this.totalMoney = 0;
        },

        //汇总数量和金额
        getAllMoney () {
            this.totalCount = 0;
            this.totalMoney = 0;
            if(this.tableData){
              this.tableData.forEach((element)=>{
                this.totalMoney += (element.price*element.count);
                this.totalCount += element.count;
              })
            }
        },
        checkout() {
          if (this.totalCount!=0) {
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;
            this.$message({
              message: '结账成功，感谢你又为店里出了一份力!',
              type: 'success'
            });

          }else{
            this.$message.error('不能空结。老板了解你急切的心情！');
          }

        }
      }

    }
</script>

<style scoped>
  .pos {
    height: 100%;
  }
  .orders {
    background-color: aliceblue;
    height: 100%;
  }
  .products {
    background-color: #c8dec9;
    height: 100%;
  }
  .el-tabs__item {
    padding-left: 20px !important;
  }
  .biv-btn {
    margin-top: 10px;
  }

  .title{
    height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
  }
  .often-goods-list ul li{
    list-style: none;
    float:left;
    border:1px solid #E5E9F2;
    padding:10px;
    margin:5px;
    background-color:#fff;
    cursor: pointer;
  }
  .o-price{
    color:#58B7FF;
  }

  .goods-type {
    clear: both;
  }

  .cookList li{
    list-style: none;
    width:23%;
    border:1px solid #E5E9F2;
    height: auot;
    overflow: hidden;
    background-color:#fff;
    padding: 2px;
    float:left;
    margin: 2px;
    cursor: pointer;
  }
  .cookList li span{

    display: block;
    float:left;
  }
  .foodImg{
    width: 40%;
  }
  .foodName{
    font-size: 18px;
    padding-left: 10px;
    color:brown;

  }
  .foodPrice{
    font-size: 16px;
    padding-left: 10px;
    padding-top:10px;
  }
  .div-total {
    background-color: white;
    border: 1px solid #ebeef5;
    padding: 10px 0;
  }
</style>
