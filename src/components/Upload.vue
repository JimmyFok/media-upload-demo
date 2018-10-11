<template>
  <div class="upload-box">
    <div class="upload">      
      <input @change="add" id="file" type="file" accept="image/*" />
      <div class="colline"></div>
      <div class="rowline"></div>      
    </div> 
    <div class="upfile">
      <div class="upitem" v-for="(item,index) in medias" :key="index" v-show="item.onOff" >
        <div class="del" @click="del(index)">
          <div class="colline"></div>
          <div class="rowline"></div>      
        </div>
        <img :src="item.base64" />   
      </div>
      
    </div>     
    
    <!-- <img src="./assets/logo.png"> -->
  </div>
</template>

<script>
import lrz from 'lrz/dist/lrz.bundle.js'
export default {
  name: 'Upload',
  data () {
    return {
      medias: [],
    }
  },
  methods:{
    del(index){
      // 索引
      console.log(index)
      //this.medias[index].onOff = !this.medias[index].onOff  // 这样只是隐藏了数据，所以应该删掉
      //delete this.medias[index];// 错误操作,因为delete是删除了，但是length不会改变。所以要用splice(index,1)来删除当前开始的第一个
      this.medias.splice(index,1);
      console.log(this.medias)
    },
    add(){ 
      let that =this;    
      // console.log(that)
      lrz(event.currentTarget.files[0])
              .then(function (rst) {
                  // 处理成功会执行     
                  console.log(typeof rst)   
                  // that.$set(rst,'onOff',true)          
                  that.medias.push({
                    'onOff':true,
                    ...rst
                    }
                  ); 
                  let len = that.medias.length;                 
                  console.log(that.medias);
              })
              .catch(function (err) {
                  // 处理失败会执行
              })
              .always(function () {
                  // 不管是成功失败，都会执行
              });
    }
  },
  // mounted:function(){
  //   // 调用该方法
  //   console.log(this.medias) ;
  //   const arr = this.medias;
  //   document.querySelector('#file').addEventListener('change', function (arr) {        
  //       lrz(this.files[0])
  //             .then(function (rst) {
  //                 // 处理成功会执行
  //                 console.log(typeof rst);
  //                 console.log(rst.base64);
  //                 arr.push(rst);
  //                 console.log(arr.base64);
  //             })
  //             .catch(function (err) {
  //                 // 处理失败会执行
  //             })
  //             .always(function () {
  //                 // 不管是成功失败，都会执行
  //             });
  //     });
  // },  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .upload, .upfile img{
    width: 80px;
    height: 80px;
    border: 1px solid #18A656;
    border-radius: 4px;
    position: relative;
    float: left;
    overflow: hidden;
  }
  .upload input{
    opacity: 0;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    cursor: pointer;
  }
  .colline{
    position: absolute;
    width: 1px;
    height: 50%;
    left: 50%;
    top: 50%;
    margin-left: -0.5px;
    margin-top: -25%;
    background-color: #18A656;
  }
  .rowline{
    position: absolute;
    height: 1px;
    width: 50%;
    left: 50%;
    top: 50%;
    margin-top: -0.5px;
    margin-left: -25%;
    background-color: #18A656;
  }
  .upitem{
    float:left;
    margin-left: 12px;
    position: relative;
    /* padding-left: 12px; */
  }
  .del{
    position: absolute;
    border-radius: 50%;
    width: 20px;
    height: 20px;
    background-color:gray;
    right: -6px;
    top: -6px;
    z-index:1;
  }
  .del .colline,.del .rowline{
    transform:rotate(45deg);
    background-color: white;
  }
  
  
</style>
