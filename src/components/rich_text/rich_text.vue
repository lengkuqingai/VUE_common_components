<template>
    <div id="editor" class="bg-fff"></div>
</template>

<script>
  export default {
    name: 'rich_text',
    data () {
      return {
        editor:{}
      }
    },
    computed:{
    },
    mounted(){
      let E = require('wangeditor');
      let editor = new E('#editor');
      this.editor=editor;
      // editor.customConfig.uploadImgShowBase64 = true;
      editor.customConfig.showLinkImg = false;
      editor.customConfig.menus=[
        'head',  // 标题
        'bold',  // 粗体
        'italic',  // 斜体
        'underline',  // 下划线
        'emoticon',  // 表情
        'image',  // 插入图片
        'undo',  // 撤销
        'redo'  // 重复
      ];
      // 自定义图片上传，使用url地址，减缓服务器压力
      editor.customConfig.customUploadImg = (files, insert) => {
        let reader = new FileReader()
        reader.readAsDataURL(files[0])
        reader.onloadend = () => {
          let base64 = reader.result
          //UploadImgByBase64此方法是上传图片的一个方法，可调取本地图片上传接口，将base64位转成url地址来获取
          UploadImgByBase64(base64, files[0].name)
            .then(sucData => {
              insert(sucData.data.url)
            })
            .catch(() => {
            })
        }
      }
      editor.create();
      editor.txt.html(datas) // 获取接口传来的富文本添加至页面
    },
    methods: {
    },
    components:{
    }
  }
</script>


<style lang="less" >
  #editor{
    height:100%;
    padding-bottom:35px;
    div:nth-child(1){
      border:none !important;
    }
    div:nth-child(2){
      height:100% !important;
      border:none !important;
    }

  }
</style>

