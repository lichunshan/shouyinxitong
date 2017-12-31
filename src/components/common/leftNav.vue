<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="icon iconfont icon-zhufang"></i>
                <div>首页</div>
            </li>
            <li>
                <i class="icon iconfont icon-caifub"></i>
                <div>财富</div>
            </li>
            <li>
                <i class="icon iconfont icon-wode"></i>
                <div>我的</div>
            </li>
            <li>
                <i class="icon iconfont icon-shoujichongzhi"></i>
                <div>首冲</div>
            </li>
            <li>
                <i class="icon iconfont icon-xinyongqiahuankuan"></i>
                <div>欠款</div>
            </li>
        </ul>
    </div>
</template>
<script>
    export default {
        name:'leftNav',
        data(){
            return {}
        }
    }
</script>
<style scoped>
    .left-nav{
       color:#fff;
       font-size:10px;
       height:100%;
       background-color: #1D8ce0;
       float:left;
       width:5%;
    }
    .iconfont{
       font-size:24px;
    }
    .left-nav ul{
        padding:0px;
        margin: 0px;
       
    }
    .left-nav li{
        list-style: none;
        text-align: center;
        border-bottom:1px solid #20a0ff;
        padding:10px;
    }
</style>