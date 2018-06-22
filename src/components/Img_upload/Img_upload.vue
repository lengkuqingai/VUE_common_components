<template>
  <div class="clearfix wrap">
    <div class="image-item" flex="cross:center main:center" v-for="(item,index) in imgsList" :key="index"
         v-dragging="{ item: item, list: imgsList, group: 'index',key: item }"
         :class="{'mr-0':(index+1)%4 == 0}"
    >
      <div><img :src="item" class="img" @click="$emit('itemClick',index)"></div>
      <img src="./img/close@2x.png" width="18" class="close" @click="remove(index)">
    </div>
    <div class="image-item bg-eff1f0 mr-0" flex="cross:center main:center" @click="checkImg()" v-show="imgsList.length<maxNumber">
      <img src="../../assets/add@2x.png" width="20">
    </div>
    <input type="file" accept="image/*" class="file" ref="file" @change="imgSuccess()">
    <popup v-model="showPropup" position="right" width="100%" style="overflow:hidden;background: #303030">
      <mine-cropper @hide="hidePopup" :url="url" :radio="radio"/>
    </popup>
  </div>
</template>

<script>
  import { Popup } from 'vux'
  import MineCropper from './MineCropper'

  export default {
    data() {
      return {
        showPropup: false,
        url: ''
      }
    },
    methods: {
      checkImg() {
        this.$refs['file'].click()
      },
      imgSuccess() {
        this.url = URL.createObjectURL(this.$refs['file'].files[0])
        this.showPropup = true
      },
      hidePopup(url) {
        this.showPropup = false
        if (url) {
          let list = this.imgsList
          list.push(url)
          this.$emit('changeList', list)
        }
      },
      remove(index) {
        let list = this.imgsList
        list.splice(index, 1)
        this.$emit('changeList', list)
      }
    },
    components: {
      Popup,
      MineCropper
    },
    props: ['maxNumber', 'imgsList', 'radio']
  }
</script>


<style scoped>
  .wrap {
    box-sizing: content-box;
    padding: 0 2.3%;
    margin-top: 10px;
  }
  .image-item {
    background: #eff1f0;
    width: 22vw;
    height: 22vw;
    float: left;
    margin-right: 2vw;
    margin-bottom: 10px;
    position: relative;
  }
  .image-item .img {
    width: 100%;
    height: 100%;
    position: absolute;
    right: 0;
    bottom: 0;
  }
  .close {
    position: absolute;
    right: -3px;
    top: -3px;
    z-index: 10;
  }
  .file {
    display: none;
  }
</style>
