<template>
    <div class="aaa">
        <!-- 使用Mint-UI提供的 Infinite scroll 实现上滑加载第2页数据 -->
        <!-- v-infinite-scroll="loadMore" 此属性用来设置AJAX请求数据的方法 -->
        <!-- infinite-scroll-disabled="loading" 此属性用来设置该组件是否还继续相应上滑请求的动作。该属性是一个bool类型值，true表示已经加载完成所有数据 -->
        <!-- infinite-scroll-distance="10" 此属性用来设置当组件距离底部多远的时候就触发loadMore事件 -->
        <ul
            v-infinite-scroll="loadMore"
            infinite-scroll-disabled="isLoading"
            infinite-scroll-distance="30">
            <li v-for="(item,index) in this.list" :key="index">
                {{ item.songlist_artic }}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return{
            list:[], //歌单列表
            isLoading:false, //默认是要响应上滑动作的
            pageindex:1,
            pagesize:21,
            requeTime:0
        }
    },
    mounted(){
    },
    methods:{
        loadMore: function(){
            if(!this.requeTime){
                clearInterval(this.requeTime)
            }
            var requeTime = setTimeout(() => {
                this.$http.get("http://10.10.100.115:8000/songlist/test01/api/?pageindex="+this.pageindex+"&pagesize="+this.pagesize)
                .then((res)=>{
                    console.log(res)
                    var data = res.data
                    if(data.Status==0){
                        this.list = this.list.concat(data.Data); //拼接两个数组，不能直接赋值
                        console.log(this.list.length)
                        console.log(this.list)
                        this.pageindex++;
                        if(data.Data.length < this.pagesize){ //此次请求的数据已经是最后一页
                            this.isLoading = true;
                        }
                    }
                })
                .catch((res)=>{
                    console.log("失败的回调",res)
                })
            }, 300);
        }
    },
}
</script>
<style >
.aaa ul li{
    height: 30px;
    list-style: none;
}
</style>