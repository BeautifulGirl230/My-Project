<template>
  <div class="box">
    <!-- <Loading v-model="isLoadings"></Loading> -->
    <!--  顶部Header 区域    -->
<!--    <mt-header style="z-index: 2" fixed title="潮流搭配">-->
<!--        <mt-button icon="back" @click="text" slot="left">返回</mt-button>-->
<!--    </mt-header>-->
<!-- 
    <van-pull-refresh class="van-pull-refresh"
      v-model="isLoading"
      loosing-text="释放即可刷新...."
      animation-duration=500
      head-height= 0
      @refresh="onRefresh"
      </van-pull-refresh>
    > -->
    <Header/>
     <mt-navbar fixed class="mt-nacber-title" v-model="selected">
      <mt-tab-item id="1">小清新</mt-tab-item>
      <mt-tab-item id="2">明星范</mt-tab-item>
    </mt-navbar>
    <Scroll class="content">
      <mt-tab-container class="mint-tab-container" v-model="selected">
        <mt-tab-container-item id="1">
          <div class="mint-tab-container-img" v-for="item in list" :key="item.id">
            <router-link :to="'/Detalis/ClothingDetails/' + item.id">
              <img v-lazy="item.img_url" alt="">
              <div class="mint-tab-container-title"></div>
              <div class="mint-tab-container-size">{{ item.price_item }}</div>
            </router-link>
          </div>
        </mt-tab-container-item>
        <mt-tab-container-item id="2">
          <div class="mint-tab-container-img" v-for="item in souper" :key="item.id">
            <router-link :to="'/Detalis/ClothingDetails/' + item.id">
              <img v-lazy="item.img_url" alt="">
              <div class="mint-tab-container-title"></div>
              <div class="mint-tab-container-size">{{ item.price_item }}</div>
            </router-link>
          </div>
        </mt-tab-container-item>
      </mt-tab-container>
    </Scroll>
  </div>
</template>

<script>
  import Header from "@/components/commion/content/regionTopBottom/Header";
  import Scroll from "@/components/commion/scroll/Scroll"; //=> 引入BetterScroll组件
  import { getSnake } from "@/api/request"; //=> axios请求
  import { Loading } from 'vux'
  import { mapState } from 'vuex'
  import { Toast } from 'mint-ui'
    export default {
        components:{
          Header,
          Scroll,
          // Loading,
        },
        data(){
            return{
                // isLoading:false,
                selected: '1', //设置默认显示页1
                list:[],
                souper:[],
            }
        },
        created() {
            this.souperjson();
        },
        computed:{
            ...mapState({
                isLoadings: state => state.vux.isLoading
            })
        },
        methods:{
            text(){
                this.$router.go(-1);           //=>返回上一给页面
            },
            // onRefresh() {
            //     setTimeout(() => {
            //         this.isLoading = false;
            //         Toast("刷新成功")
            //     }, 2000);
            // },
            souperjson(){
                getSnake().then((item) => {
                    if (item.data.status === 0){
                        this.list = item.data.souper;
                        this.souper = item.data.souper2;
                        console.log(this.list);
                        console.log(this.souper);
                    }else{
                        Toast('获取失败')
                    }
                })
            }
        },
    }
</script>

<style lang="scss" scoped>
.box{
  position: relative;
  height: 100vh;
  .mt-nacber-title{
        width: 100%;
        position: absolute;
        margin-top: 40px;
        // margin-bottom: 45px;
        z-index: 2;
      }
  .content{
    overflow: hidden;
    position: absolute;
    top: 90px;
    bottom: 50px;
    left: 0;
    right: 0;
     //给下拉刷新添加样式
      .van-pull-refresh{
        text-align: center;
        color: #999999;
      }
      .mint-tab-container{
        margin-top: 45px;
        background-color: #EEE;
      }
      .mint-tab-container-img{
        width: 360px;
        height: 360px;
        margin: 8px 7px;
        position: relative;
        overflow: hidden;
        img{
          width: 100%;
        }
        .mint-tab-container-title{
          position: absolute;
          width: 100%;
          height: 100px;
          background-color: #999;
          opacity: 0.6;
          bottom: 0;
        }
        .mint-tab-container-size{
          width: 90%;
          height: 60px;
          line-height: inherit;
          font-weight: 500;
          overflow: hidden;
          color: #ffffff;
          position: absolute;
          z-index: 1;
          bottom: 30px;
          left: 20px;
        }
    }
  }
}
</style>
