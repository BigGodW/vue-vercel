<script setup>
import { ref,onMounted} from 'vue'
let todoId = ref('')
let todoData = ref({})
let todoList = ref([])
const item = ref({title:"",content:""})
import AV from 'leancloud-storage'
AV.init({
    appId:"msSlJB9iYabqoLXphS4uXZhb-gzGzoHsz",
	appKey:"rpJi7d91xiL0AkcSoeRz8Dv7",
	serverURL:"https://mssljb9i.lc-cn-n1-shared.com"
})
function getAll(){
    const query = new AV.Query('Todo')
    query.notEqualTo('title','')
    query.find().then(list=>{
        todoList.value = list
    })
}
onMounted(()=>{
    getAll()
})
const {Query,User} = AV
function addUser(){
    const Todo = AV.Object.extend('Todo')
    const todo = new Todo()
    todo.set('title',item.value.title)
    todo.set('content',item.value.content)
    todo.save().then(res=>{
        item.value.title = ''
        item.value.content = ''
        getAll()
    })
}
function searchId(){
    // const query = new AV.Query('Todo')
    // query.get(todoId.value).then(data=>{
    //     console.log(data)
    //     todoData.value = data
    // })
   // const todo = AV.Object.createWithoutData('Todo',todoId.value)
   const todo = AV.Object.createWithoutData("Todo", todoId.value);
   todoData.value = todo

}
function changeTodo(){
    console.log('change')
    todoData.value.remove('tags','html')
    todoData.value.save().then(res=>{
        console.log(res)
    })
    
}
function addPost(){
    // const post = new AV.Object('Post')
    // post.set('title','文章标题')
    // post.set('content','文章内容')
    const post = AV.Object.createWithoutData('Post','6541180bc4f6c57523e6eafa')
    const comment = new AV.Object('comment')
    comment.set('content','文章评论3222')
    comment.set('post',post)
    comment.save() // 评论保存 文章也会保存
}

</script>

<template>
    <h1>数据库</h1>
    <div>
        <p>{{item}}</p>
    <input type="text"  v-model="item.title"><br>
    <input type="text" v-model="item.content"><br>
    <button @click="addUser">添加</button>
    </div>

    <input type="text" v-model="todoId">
    <button @click="searchId">查询</button>
    
    <button @click="changeTodo">修改</button>
    <pre>
        {{ todoData }}
    </pre>

 
    <div>
        <ul>
            <li v-for="item in todoList">
                <pre>{{item.id}}-{{ item.get('title') }}</pre>
            </li>
        </ul>
    </div>
    <button @click="addPost">addPost</button>
</template>
