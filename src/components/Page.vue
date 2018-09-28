<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="judge" class="pagebtn" @click="changeBtn">.....</button>
    <button v-for="item in pagelist" :class='[{currentPage:item===currentPage},"pagebtn"]' 
    @click="changeBtn(item)">
      {{item}}
    </button>
    <button class="pagebtn" @click="changeBtn">......</button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
  import $ from 'jquery'

export default {
  name: 'Page',
  data:function(){
    return{
      pagelist:[1,2,3,4,5],
      currentPage:1,//当前选中页码
      judge:false
    }
  },
  methods:{
    changeBtn:function(item){
      if(item>4){
        this.judge=true
      }else{
        this.judge=false
      }
      if(typeof item !='number'){
        switch(item.target.innerText){
          case '上一页':
            $('button.currentPage').prev().click();
          break;
          case '下一页':
            $('button.currentPage').next().click();
          break;
          case '首页':
            this.pagelist=[1,2,3,4,5]
            this.changeBtn(1);
          break;
          case '.....':
            $('button.currentPage').prev().click()
          break;
          case '......':
            $('button.currentPage').next().click()
          break;
          default:
          break;
        }
        return;
      }
      if(item===this.pagelist[4]){
        this.pagelist.shift()//移除第一个
        this.pagelist.splice(4,0,this.pagelist[3]+1)//在后边加一个
      }else if(item===this.pagelist[0]&&item!=1){
        this.pagelist.unshift(this.pagelist[0]-1)//在前边减一个，但是不能小于1
        this.pagelist.splice(5,1)//在后边移除一个
      }
      this.currentPage=item
      this.$emit('pageChange',this.currentPage)
    }
  }
}
</script>

<style scoped>
  .pagination {
    margin-top: 5px;
    margin-bottom: 20px;
    background-color: white;
    padding: 6px 20px;
    border-radius: 5px;
    /*box-shadow: 0px 2px 9px #888888;*/
    border: 1px solid #888888;
  }

  button {
    background-color: #fff;
    border: 1px solid #ddd;
    color: #778087;
    border-radius: 3px;
    outline: none;
    height: 21px;
    cursor: pointer;
    padding: 0 2px;
    width: 55px;
    height: 29px;
  }

  .pagebtn {
    position: relative;
    bottom: 1px;
    width: 40px;
    margin: 0 4px;
  }

  .currentPage {
    color: white;
    background-color: #1f1b1b;

  }
</style>