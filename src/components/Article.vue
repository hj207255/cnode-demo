<template>
  <div class="article">
  	<!-- 正在加载中 -->
  	<div class="loading" v-if="loading">
      <img src="../assets/loading.gif">
    </div>
    <!-- 加载完毕 -->
    <div v-else>
    	<div class="topic_header">
    		<div class="topic_title">{{article.title}}</div>
    		<ul>
    			<li>• 发布于 {{article.create_at | formatDate}}</li>
    			<li>• 作者 {{article.author.loginname}}</li>
    			<li>• {{article.visit_count}} 次浏览</li>
    			<li>• 最后一次编辑是 {{article.create_at | formatDate}}</li>
    			<li>• 来自 {{article | formTitle}}</li>
    		</ul>
        <div v-html="article.content" class="topic_content"></div>
    	</div>
    	<div id="reply">
	    	<div class="topbar">回复</div>
	    	<div v-for="(reply,index) in article.replies" class="replySec">
          <div class="replyUp">
            <router-link :to="{
              name:'userinfo',
              params:{
                name:reply.author.loginname
              }
            }">
              <img :src="reply.author.avatar_url">
              <span>{{reply.author.loginname}}</span>
            </router-link>
            <span>{{index+1}}楼</span>•<span>{{reply.create_at | formatDate}}</span>
            <span v-if="reply.ups.length>0">☝{{reply.ups.length}}</span>
            <span v-else></span>
            <p v-html="reply.content"></p>
          </div>
	    	</div>
    	</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Article',
  data:function(){
  	return{
  		loading:true,
  		article:[]//存取拿到的数据
  	}
  },
  methods:{
  	getArticle:function(){
  		this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
  			.then(res=>{
  				this.loading=false//获取数据成功后取消加载动画
  				this.article=res.data.data
  			}).catch(function(err){
  				console.log(err)
  			})
  	}
  },
  beforeMount:function(){
  	this.loading=true//获取数据时加载动画
  	this.getArticle()//在页面加载之前获取数据
  },
  watch:{
    $route:function(to,from){
      this.getArticle()
    }
  }
}
</script>

<style>
  @import url('../assets/markdown-github.css');
  .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    height: 16px;
    font-size: 12px;
    margin-top: 10px;
  }

  .article:not(:first-child) {
    margin-right: 340px;
    margin-top: 15px;
  }

  #reply, .topic_header {
    background-color: #fff;
  }

  #reply {
    margin-top: 15px;
  }

  #reply img {
    width: 30px;
    height: 30px;
    position: relative;
    bottom: -9px;
  }

  #reply a, #reply span {
    font-size: 13px;
    color: #666;
    text-decoration: none;
  }
  .replySec{
    border-bottom:1px solid #e5e5e5;
    padding:0 10px;
  }

  .loading {
    text-align: center;
    padding-top: 300px;
  }

  .replyUp a:nth-of-type(2) {
    margin-left: 0px;
    display: inline-block;
  }

  .topic_header {
    padding: 10px;
  }

  .topic_title {
    font-size: 20px;
    font-weight: bold;
    padding-top: 8px;
  }

  .topic_header ul {
    list-style: none;
    padding: 0px 0px;
    margin: 6px 0px;
  }

  .topic_header li {
    display: inline-block;
    font-size: 12px;
    color: #838383;
  }

  .topic_content {
    border-top: 1px solid #e5e5e5;
    padding: 0 10px;
  }

  .markdown-text p {
    overflow: auto;
  }
  .markdown-text img {
    height: auto;
    max-width: 100%;
    vertical-align: middle;
    border: 0;
  }
</style>