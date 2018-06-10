<template>
  <div class="scroll-page">
    这是一个滚动组件
    <div class="container">
      <div class="content" ref="container" @scroll.stop="contentScroll">
        <ul class="list" ref="list">
          <li class="list-item" v-for="index in 100" :key="index">这里是我的{{index}}个列表项</li>
        </ul>
      </div>
      <div class="scroll" ref="scroll"
           @click.stop="scrollClick"
           @mousemove.stop="scrollSlide"
      >
        <div class="bar" ref="bar"
          @mousedown.stop="scrollBtnDown"
          @mouseup.stop="scrollBtnUp"
          @mouseleave.stop="scrollBtnUp"
        ></div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      scrollFlag:false,//滚动条开关
    }
  },
  mounted(){
    this.scrollInit();
  },
  methods:{
    //滚动条初始化
    scrollInit(){
      let bar =this.$refs.bar;
      let list =this.$refs.list;
      let container=this.$refs.container;
      let listH=list.offsetHeight;
      let containerH=container.offsetHeight;
      if(listH<containerH){
        return
      }
      let barPercent=containerH/listH;
      bar.style.height=`${barPercent*100}%`;
    },
  //  监听滚动事件
    contentScroll(event){
      let target=event.target;
      let bar =this.$refs.bar;
      let container=this.$refs.container;
      let containerH=container.offsetHeight;
      let scrollH=target.scrollTop;
      let scrollP=scrollH/containerH;
      bar.style.transform=`translateY(${scrollP*100}%)`;
    },
  //  点击滚动条
    scrollClick(event){
      let target=event.target;
      if(target.className.indexOf('scroll')<0){
        return
      }
      let list =this.$refs.list;//列表内容
      let container=this.$refs.container;//滚动容器
      let listH=list.offsetHeight;
      let containerH=container.offsetHeight;//
      let clickY=event.offsetY;//点击的Y坐标
      let scrollP=clickY/containerH;
      let scrollH=listH*scrollP-containerH;
      container.scrollTop=scrollH;
    },
  //  滚动条按钮的鼠标按下事件
    scrollBtnDown(event){
      this.scrollFlag=true;
      console.log('down');
      return false
    },
    scrollBtnUp(event){
      this.scrollFlag=false;
      console.log('up');
      return false
    },
  //  拖动滚动条
    scrollSlide(event){
      let target=event.target;
      if(this.scrollFlag){
        let bar =this.$refs.bar;
        let list =this.$refs.list;//列表内容
        let container=this.$refs.container;//滚动容器
        let listH=list.offsetHeight;
        let containerH=container.offsetHeight;//
        let moveY=event.offsetY;//点击的Y坐标
        let scrollP=moveY/containerH;
        let scrollH=listH*scrollP;
        // let scrollH=moveY;
        container.scrollTop=scrollH;
        // container.scrollTop=scrollP*listH;
        let transP=scrollH/containerH;
        // bar.style.transform=`translateY(${moveY/(bar.offsetHeight)*100}%)`;
        console.log(moveY);
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .container{
    border: 1px solid red;
    height: 600px;
    overflow: hidden;
    position: relative;
  }
  .content{
    border: 1px solid blue;
    height: 600px;
    overflow-x: hidden;
    overflow-y: scroll;
    margin-right: -18px;
  }
  .scroll{
    position: absolute;
    right: 2px;
    bottom: 0;
    top: 0;
    width: 26px;
    border: 1px solid red;
    z-index: 1;
  }
  .bar{
    position: relative;
    display: block;
    width: 100%;
    height:0;
    cursor: pointer;
    border-radius: 4px;
    background-color: hsla(220,4%,58%,.3);
    transition: background-color .3s;
  }
</style>
