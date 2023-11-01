<script setup>
    import {ref} from 'vue'
    import AV from '../LeanClooud/db'
    const fileEl = ref(null)
    const file = ref(null)
    const res = ref({})
    const fileid = ref("")
    const imgsrc = ref('')
    function changeFile(e){
        const fileitem = e.target.files[0]
        file.value = new AV.File(fileitem.name,fileitem)
        console.log(file)
    }
    function upload(){
        file.value.save().then(res=>{
            console.log(res.toJSON())
            res.value = res
        })
    }
   async function getUrl(){
        file.value = await new AV.Query('_File').get(fileid.value)
        imgsrc.value = file.value.get('url')
        
        
    }
    function getsm (){
       const src =  new AV.File.withURL('test',imgsrc.value)
       console.log(src.toJSON())
       imgsrc.value=src.thumbnailURL(100,200)
        
    }
    function delimg(){
        const file = AV.File.createWithoutData(fileid.value)
        console.log(file.toJSON())
        file.destroy()
    }
    function click (){
        const file = new AV.File.withURL('kuaishou.png',"https://w2.eckwai.com/udata/pkg/ks-merchant/kwaishop-seller-login/banner.png")
        
        file.save().then(res=>{
            console.log(res)
        })
    
    }   
</script>
<template>
    <h1>文件上传</h1>
    <div>
        <input type="file" name="" id="" @change="changeFile" ref="fileEl">
        <button @click="upload">上传</button>
        <input type="text" v-model="fileid">
        <button @click="getUrl">获取图片</button>
        <button @click="getsm">获取缩略图</button>
        <button @click="delimg">删除</button>
        <div>
            <img v-if="imgsrc" :src="imgsrc" alt="">
        </div>
        <div>
            <button @click="click">click</button>
        </div>
    </div>

</template>