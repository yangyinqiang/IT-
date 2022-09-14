# [官网](http://mockjs.com/examples.html#Name)



# **步骤**

1. npm install mockjs -D  //依赖下载
2. ![image-20220905163658259](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220905163658259.png)



3.![image-20220905163757360](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220905163757360.png)



4.如果用阿贾克斯进行请求的话，导入阿贾克斯依赖

- npm install axios 
- npm install vue-axios

![image-20220905165044100](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220905165044100.png)

5.![image-20220905164350870](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220905164350870.png)





![image-20220905164452776](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220905164452776.png)



6.封装

![image-20220906102654092](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220906102654092.png)

![image-20220906102804933](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220906102804933.png)

![image-20220906102827358](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220906102827358.png)



![image-20220906102858242](https://gitee.com/gong-kai2/note/raw/develop/imgs/image-20220906102858242.png)

# 1、生成数据

```js
import Mock from 'mockjs'
//生成文本
// const data = Mock.mock({
//     "string|1-10": "★"
//   })
//生成文本
//   const data=Mock.mock({
//     string: '@cword(3,10)'
//   })
//生成标题和句子
//   const data=Mock.mock({
//     title:'@ctitle',
//     sentence:'@csentence'
//   })
// 生成增量
//   const data=Mock.mock({
//     id:'@increment(1)'
//   })
//生成数值
//   const data=Mock.mock({
//     'number|1-100':10
//   })
//生成段落
//   const data=Mock.mock({
//     content:'@cparagraph(5)'
//   })
//生成名字，身份证号，地址
//   const data=Mock.mock({
//     name:'@cname',
//     idCard:'@id()',
//     address:'@city(true)'
//   })
//生成图片
//   const data=Mock.mock({
//     img_url:"@image('250x250",'#FFA07A','#FFBBFF','png','坤坤')"
//   })
//生成时间
//   const data=Mock.mock({
//     date:'@date(yyyy-MM-dd hh:mm:ss)'
//   })
//生成数组
//   const data=Mock.mock({
//     'newsList|8-20':[{
//     name:'@cname',
//     address:'@city(true)',
//     id:'@increment(1)'
// }]
//   })
// console.log(data)
```

# 2、拦截请求

写在js中

```js
Mock.mock('/api/news','get',{
    status:200,
    msg:'get successly'
})
// Mock.mock('/api/post/news','post',{
//     status:200,
//     msg:'post successly'
// })第一种写法
Mock.mock('/api/post/news','post',()=>{
    return{
        status:200,
        msg:'post successly'
    }
})

```

```js
export default {
    data() {
    return{

    }
  },
  created(){
    axios.get('/api/news').then(res=>{
      console.log(res)
    })
    axios.post('/api/post/news').then(res=>{
      console.log(res)
    })
  },
  name: 'App',
  components: {
    HelloWorld
  }
}
```

# 3、增删改查

```vue
//定义获取数据列表
Mock.mock('/api/get/news', 'get', () => {
    return {
        status: 200,
        message: '获取数据成功',
        list: newList,
        total: newList.length
    }
})
//增
Mock.mock('/api/get/add', 'post', (options) => {
    const body=JSON.parse(options.body)
    console.log(body);
    
    newList.push({
        "project_name": body.project_name,
        "project_type": body.project_type,
        "person_charge": body.person_charge,
        "Developers": body.Developers,
        "scheduled_start_time": body.scheduled_start_time,
        "planned_completion_time": body.planned_completion_time,
        "actual_start_time": body.actual_start_time,
        "actual_completion_time": body.actual_completion_time,
        "remarks": body.remarks   
    })
    return {
        status: 200,
        message: '获取数据成功',
        list: newList,
        total: newList.length
    }
})
//删
Mock.mock('/api/delete/news', 'post', (options) => {
    const body=JSON.parse(options.body)
    const index=newList.findIndex(item=>{
        return item.project_name===body.row.project_name
    })
    console.log(newList)
    newList.splice(index,1)
    console.log(newList)
    return {
        status: 200,
        message: '删除成功',
        list: newList,
        total: newList.length
    }
})

//改
Mock.mock('/api/get/change', 'post', (options) => {
    const body=JSON.parse(options.body)
    const index=newList.findIndex(item=>{
        // console.log(item)
        return item.id===body.id
    })
    newList[index]=body
    return {
        status: 200,
        message: '修改成功',
        list: newList,
        total: newList.length
    }
})

//查
Mock.mock('/api/get/query', 'post', (options) => {
    const body = JSON.parse(options.body)
    // console.log("**********");
    // console.log(body);
    const index = newList.findIndex(item => item.id == body)
    const onelist = newList[index]
    console.log(onelist);   
    return {
        status: 200,
        message: '查询成功',
        list: onelist,
        total:onelist.length
    }
})

```

