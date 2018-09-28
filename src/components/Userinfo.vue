<template>
  <div class="UserInfo">
    <!-- 正在加载中 -->
    <div class="loading" v-if="loading">
      <img src="../assets/loading.gif">
    </div>
    <!-- 加载完毕 --> 
    <div class="userInfomation" v-else>
      <section>
        <img :src="user.avatar_url">
        <span>{{user.loginname}}</span>
        <p>{{user.score}}积分</p>
        <p>注册时间 {{user.create_at | formatDate}}</p>
      </section>
      <div class="replies">
        <p>最近回复的主题</p>
        <ul>
          <li v-for="item in user.recent_replies">
            <router-link :to="{
              name:'article',
              params:{
                id:item.id
              }
            }">
              <p>{{item.title}}</p>
            </router-link>
          </li>
        </ul>
      </div>
      <div class="topics">
        <p>最近创建的话题</p>
        <ul>
          <li v-for="item in user.recent_topics">
            <router-link :to="{
              name:'article',
              params:{
                id:item.id
              }
            }">
              <p>{{item.title}}</p>
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Userinfo',
  data:function(){
    return{
      loading:true,
      user:[]
    }
  },
  methods:{
    getUser:function(){
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res=>{
          this.loading=false//获取数据成功后取消加载动画
          this.user=res.data.data
        }).catch(function(err){
          console.log(err)
        })
    }
  },
  beforeMount:function(){
    this.loading=true//获取数据时加载动画
    this.getUser()//在页面加载之前获取数据    
  }
}
</script>

<style scoped>
  .userInfomation {
    background: white;
    width: 75%;
    margin: 10px auto;
  }
  .userInfomation section {
    padding: 12px;
  }
  .userInfomation img {
    width: 30px;
  }
  .userInfomation li {
    list-style:none;
  }
  .userInfomation .replies,
  .userInfomation .topics {
    font-size: 0.72rem;
    border-top: 10px #DDDDDD solid;
  }
  .userInfomation > div > p {
    padding: 12px 0 12px 12px;
    background-color: rgba(212, 205, 205, 0.17);
    font-size: 0.75rem;
    margin: 0;
  }
  .userInfomation > div >ul > li {
    padding: 4px 0 4px 12px;
    white-space: nowrap;
    font-size: 0.72rem;
    text-overflow: ellipsis;
    overflow: hidden;
    line-height: 30px;
    vertical-align: middle;
  }
  .userInfomation > div >ul > li > a {
    color: #094E99;
    text-decoration: none;
  }
  .loading {
    text-align: center;
    padding-top: 300px;
  }

</style>