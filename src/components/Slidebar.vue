<template>
  <div class="autherinfo">
    <!-- 正在加载中 -->
    <div class="loading" v-if="loading">
      <img src="../assets/loading.gif">
    </div>
    <!-- 加载完毕 -->
  	<div class="authersummay" v-else>
      <div class="topbar">作者</div>
      <router-link :to="{
        name:'userinfo',
        params:{
          name:userlist.loginname
        }
      }">
        <img :src="userlist.avatar_url">
        <span>{{userlist.loginname}}</span>
      </router-link>
      <p>积分：{{userlist.score}}</p>
    </div>
    <div class="recent_topics">
      <div class="topbar">作者最近主题</div>
      <ul>
        <li v-for="item in getTopics">
          <router-link :to="{
            name:'article',
            params:{
              id:item.id,
              name:item.author.loginname
            }
          }">
            {{item.title}}
          </router-link>   
        </li>
      </ul>
    </div>
    <div class="recent_replies">
      <div class="topbar">作者最近回复</div>
      <ul>
        <li v-for="item in getReplies">
          <router-link :to="{
            name:'article',
            params:{
              id:item.id,
              name:item.author.loginname
            }
          }">
            {{item.title}}
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Slidebar',
  data:function(){
    return{
      loading:true,
      userlist:[]
    }
  },
  methods:{
    getUser:function(){
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res=>{
          this.loading=false//获取数据成功后取消加载动画
          this.userlist=res.data.data
        }).catch(function(err){
          console.log(err)
        })
    }
  },
  computed:{
    getTopics:function(){
      if(this.userlist.recent_topics){
        return this.userlist.recent_topics.slice(0,5)
      }
    },
    getReplies:function(){
      if(this.userlist.recent_replies){
        return this.userlist.recent_replies.slice(0,5)
      }
    }
  },
  beforeMount:function(){
    this.loading=true//获取数据时加载动画
    this.getUser()//在页面加载之前获取数据    
  }
}
</script>

<style scoped>
  .authersummay, .recent_replies, .recent_topics {
    background-color: #fff;
  }
  .autherinfo {
    width: 328px;
    float: right;
    margin-top: 0;
  }
  li{
    padding: 3px 0 ;
  }
  .recent_replies ul, .recent_topics ul {
    margin-top: 0px;
    margin-bottom: 0px;
    list-style: none;
    padding-left: 14px;
  }

  ul a {
    font-size: 12px;
    text-decoration: none;
    color: #778087;
  }

  .authersummay a{
    font-size: 12px;
    text-decoration: none;
    color: #778087;
  }
  .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    height: 16px;
    font-size: 12px;
    margin-top: 10px;
  }

  img {
    height: 48px;
    width: 48px;
    border-radius: 3px;
    margin: 10px;
    vertical-align: middle;
  }

  .loginname {
    width: 100px;
    float: right;
    margin-top: 22px;
    margin-right: 159px;
    font-size: 14px;
  }

  .loginname a {
    text-decoration: none;
    color: #778087;
  }

  .authersummay .topbar {
    margin-top: 0px;
  }
  .authersummay p{
    padding-left: 14px;
    padding-bottom: 5px;
  }
</style>