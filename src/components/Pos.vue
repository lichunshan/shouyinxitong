<template>
    <div class="pos">
        <div>
            <el-row>
                <el-col :span='7' class="pos-order" id="order-list">
                    <el-tabs>
                        <el-tab-pane label="结账">
                            <el-table border style="width:100%" :data="tableData">
                                <el-table-column label="商品名称" prop="goodsName" width="100"></el-table-column>
                                <el-table-column label="金额" prop="price" width="90"></el-table-column>
                                <el-table-column label="数量" prop="count" width="100"></el-table-column>
                                <el-table-column label="操作" prop="doing" width="100" fixed="right" >
                                    <template scope="scope">
                                        <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                                        <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
                            <div><span><small>数量：</small>{{totalCount}}</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span><small>总金额：</small>￥{{totalMoney}}元</span></div>
                            <div class="tab-btn">
                                <el-button type="warning">挂单</el-button>
                                <el-button type="danger" @click="delAllGoods">删除</el-button>
                                <el-button type="success" @click="checkout">结账</el-button>
                            </div>
                        </el-tab-pane>
                        <el-tab-pane label="挂单">
                            挂单
                        </el-tab-pane>
                        <el-tab-pane label="外卖">
                            外卖
                        </el-tab-pane>
                        
                    </el-tabs>
                    
                </el-col>
                <el-col :span='17' class="pos-order" id="order-list1"> 
                    <div class="often-goods">
                        <div class="title">常用商品</div>
                        <div class="often-goods-list">
                            <ul>
                                <li v-for="goods in oftenGoods" @click="addOrderList(goods)" class="changyong">
                                    <span>{{goods.goodsName}}</span>
                                    <span class="o-price">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </div>


                    <div class="goods-type">
                        <el-tabs>
                            <el-tab-pane label="汉堡">
                                <ul class="cookList">
                                    <li v-for="goods in type0Goods" @click="addOrderList(goods)" class="changyong">
                                        <span class="foodImg"><img :src="goods.goodsImg" alt="tupian" width="100%"></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="小食">
                                <ul class="cookList">
                                    <li v-for="goods in type1Goods" @click="addOrderList(goods)" class="changyong">
                                        <span class="foodImg"><img :src="goods.goodsImg" alt="tupian" width="100%"></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="饮料">
                                <ul class="cookList">
                                    <li v-for="goods in type2Goods" @click="addOrderList(goods)" class="changyong">
                                        <span class="foodImg"><img :src="goods.goodsImg" alt="tupian" width="100%"></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="套餐">
                                <ul class="cookList">
                                    <li v-for="goods in type3Goods" @click="addOrderList(goods)" class="changyong">
                                        <span class="foodImg"><img :src="goods.goodsImg" alt="tupian" width="100%"></span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                        </el-tabs>
                    </div>
                </el-col>
            </el-row>
        </div>
        
    </div>
</template>
<script>
    import axios from 'axios'
    export default {
        data(){
            return {
                tableData:[],
                oftenGoods:[],
                type0Goods:[],
                type1Goods:[],
                type2Goods:[],
                type3Goods:[],
                totalCount:0,
                totalMoney:0
            }
        },
        created:function(){
            axios.get("http://jspang.com/DemoApi/oftenGoods.php")
            .then(response=>{
                // console.log(response);
                this.oftenGoods=response.data;
            })
            .catch(error=>{
                alert("网络错误，请重试");
            });

            axios.get("http://jspang.com/DemoApi/typeGoods.php")
            .then(response=>{
                // console.log(response);
                this.type0Goods=response.data[0];
                this.type1Goods=response.data[1];
                this.type2Goods=response.data[2];
                this.type3Goods=response.data[3];
            })
            .catch(error=>{
                alert("网络错误，请重试");
            });
        },
        mounted:function(){
            var orderHeight=document.body.clientHeight;
            console.log(orderHeight);
            document.getElementById('order-list').style.height=orderHeight+'px';
            // document.getElementById('order-list1').style.height=orderHeight+'px';
        },
        methods:{
             addOrderList(goods){
                 
                 //判断是否这个商品已经存在于列表中
                 let isHave=false;
                 for(let i=0;i<this.tableData.length;i++){
                    if(this.tableData[i].goodsId==goods.goodsId){
                        isHave=true;
                    }
                 }   

                 //根据isHave的值判断列表中是否已经有数据
                    if(isHave){
                        //存在就进行数组增加
                        let arr=this.tableData.filter(o=>o.goodsId==goods.goodsId);
                        arr[0].count++;
                    }else {
                        //不存在就向数组中推入
                        let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
                        this.tableData.push(newGoods);
                    }
                //进行数量和价钱的汇总
                this.getAllMoney();
             },
             delSingleGoods(goods){
                this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId);
                this.getAllMoney();
             },//汇总数量和金额
             getAllMoney(){
                 this.totalCount=0;
                 this.totalMoney=0;
                 this.tableData.forEach((element)=>{
                    this.totalCount+=element.count;
                    this.totalMoney+=(element.price*element.count);
                })
             },
             delAllGoods(){
                 this.tableData=[];
                 this.totalCount=0;
                 this.totalMoney=0;
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
    .pos-order{
        background-color: #F9FAFC;
        border-right: 1px solid #C0CCDA;
        /* border-left: 1px solid #C0CCDA; */
    }
    .tab-btn{
        margin-top: 10px;
    }
    .title{
        height: 20px;
        border-bottom:1px solid #D3DCE6;
        text-align: left;
        color: #C1C1C1;
        padding: 10px;
    }
    .often-goods-list ul li{
        list-style:none;
        float: left;
        border: 1px solid #E5E9F2;
        padding: 10px;
        margin: 5px;
        background-color: #fff;
    }
    .o-price{
        color:#58B7FF; 
    }
    .goods-type{
        clear:both;
    }
    .cookList li{
        list-style:none;
        width: 23%;
        height: auot;
        overflow: hidden;
        background-color:#fff;
        padding: 2px;
        float:left;
        margin: 2px;
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
   .changyong{
       cursor:pointer;
   }
</style>