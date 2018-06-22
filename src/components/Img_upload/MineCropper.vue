<template>
  <view-box ref="viewBox" :style="{top: platform == 'ios'?'20px':0}">
    <x-header slot="header" :left-options="{backText:'',preventGoBack:true}" @on-click-back="hide()">
      图片裁剪
      <div slot="right" @click="hide(true)">完成</div>
    </x-header>
    <vueCropper
      ref="cropper"
      :img="url"
      :outputSize="1"
      outputType="jpg"
      :info="true"
      :canScale="false"
      :autoCrop="true"
      :fixed="true"
      :fixedNumber="radio"
      :canMove="false"
    />
    <div slot="bottom" class="footer-button" flex="box:mean" v-if="$route.query.type =='edit' ">
      <div class="button left-button" flex="cross:center main:center">{{$route.query.isShow ? '上架':'下架'}}</div>
      <div class="button right-button" flex="cross:center main:center">删除</div>
    </div>
  </view-box>
</template>

<script>
  // vux 框架方法
import vueCropper from 'vue-cropper'
// 服务
import { UploadImgByBase64 } from '../servers/fileServer'

export default {
  props: ['url', 'radio'],
  data() {
    return {
      platform: window.APP_PLATFORM
    }
  },
  methods: {
    hide(state) {
      if (state) {
        this.$refs.cropper.getCropData(data => {
          this.$vux.loading.show()
          UploadImgByBase64(data, 'test.jpg')
            .then(sucData => {
              this.$vux.loading.hide()
              if (sucData.data.url) {
                this.$emit('hide', sucData.data.url)
              }
            })
            .catch(() => {
              this.$vux.loading.hide()
            })
        })
      } else {
        this.$emit('hide')
      }
    }
  },
  components: {
    vueCropper
  }
}
</script>
