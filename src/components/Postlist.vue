<template>
  <div class="PostList">
    <!-- 正在加载中 -->
  	<div class="loading" v-if="loading">
      <img src="../assets/loading.gif">
    </div>
    <!-- 加载完毕 -->
    <div class="posts" v-else>
      <ul>
        <li class="toobar">
          <span @click='getAll'>全部</span>
          <span @click='getGood'>精华</span>
          <span @click='getShare'>分享</span>
          <span @click='getAsk'>问答</span>
          <span @click='getJob'>招聘</span>
          <span>客户端测试</span>
        </li>
        <li v-for="item in postList">
          <!--头像-->
          <router-link :to="{
            name:'userinfo',
            params:{
              name:item.author.loginname
            }
          }">
            <img :src="item.author.avatar_url" alt="">
          </router-link>
          <!--回复/浏览-->
          <span class="allcount">
            <span class="reply_count">{{item.reply_count}}</span>/<span>{{item.visit_count}}</span>
          </span>
          <!--帖子的分类-->
          <span :class="[{put_good:(item.good===true)},{put_top:(item.top===true)},{'topiclist-tab':(item.top!=true&&item.good!=true)}]">
            {{item | formTitle}}
          </span>
          <!--标题-->
          <router-link :to="{
            name:'article',
            params:{
              id:item.id,
              name:item.author.loginname
            }
          }" class="stitle">
            
            {{item.title}}
            
          </router-link>
          <!--最終回复时间-->
          <span class="last_reply">
            {{item.last_reply_at | formatDate}}
          </span>
          <!--分页-->

        </li>
        <li>
          <Page @pageChange='polistPage'></Page>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import Page from './Page'
  import $ from 'jquery'

export default {
  name: 'PostList',
  data:function(){
    return{
      loading:true,
      postList:[],//存取的是获取到的数据
      postpage:1,
      tabs:''//自定义文章类型，初始为空
    }
  },
  components:{
    Page
  },
  methods:{
    getData:function(){
      this.$http.get("https://cnodejs.org/api/v1/topics",{
        params:{
          page:this.postpage,
          limit:20,
          tab:this.tabs
        }
      })
      .then(res=>{
        this.loading=false//获取数据成功后取消加载动画
        this.postList=res.data.data
      })
      .catch(err=>{
        console.log(err)
      })
    },
    polistPage:function(value){//更改页面的函数
      this.postpage=value
      this.getData()
    },
    getGood:function(){
      this.tabs="good"
      this.getData()
    },
    getShare:function(){
      this.tabs="share"
      this.getData()
    },
    getAsk:function(){
      this.tabs="ask"
      this.getData()
    },
    getJob:function(){
      this.tabs="job"
      this.getData()
    },
    getAll:function(){
      this.tabs=""
      this.getData()
    }
  },
  beforeMount:function(){
    this.loading=true//获取数据时加载动画
    this.getData()//在页面加载之前获取数据
  }
}
</script>

<style scoped>
.PostList{
  background-color: #e1e1e1;
}
.posts {
  margin-top: 10px;
}
.stitle{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  vertical-align: middle;
  max-width: 70%;
  display: inline-block;
} 
.PostList img {
  height: 30px;
  width: 30px;
  vertical-align: middle;
}

ul {
  list-style: none;
  width: 100%;
  max-width: 1344px;
  margin: 0 auto;
}

ul li:not(:first-child) {
  padding: 10px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
  font-weight: 400;
  background-color: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
}

li:not(:first-child):hover {
  background: #f5f5f5;;
}

li:last-child:hover {
  background: white;
}

li span {
  line-height: 30px;
}

.allcount {
  width: 70px;
  display: inline-block;
  text-align: center;
  font-size: 12px;
  word-break: break-word;
}

.reply_count {
  color: #9e78c0;
  font-size: 14px;
}

.put_good, .put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  color: #fff;
  font-size: 12px;
  margin-right: 10px;
}

.topiclist-tab {
  background-color: #e5e5e5;
  color: #999;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
}

.last_reply {
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}

.toobar {
  height: 40px;
  background-color: #f5f5f5;
}

.toobar span {
  font-size: 14px;
  color: #80bd01;
  line-height: 40px;
  margin: 0 10px;
  cursor: pointer;
}

.toobar span:hover {
  color: #9e78c0;
}

a {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: underline;
}

.loading {
  text-align: center;
  padding-top: 300px;
}
</style>