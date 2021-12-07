任何一个人都无法达到你对他的全部预设，把美好的生活寄托在自己身上

今天的任务是实现列表过滤

Vue 使用两种方式实现列表过滤
下面让我们一步一步完成任务：

第一步：先编写html代码

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>列表过滤</title>
    <script src="../Vue.js/vue.js"></script>
</head>
<body>
    <div id="root">
        <h1>人员列表</h1>
        <input type="text" placeholder="请输入名字" v-model="keyword">
        <ul>
            <li v-for="(p,index) in filePerson" :key="index">
                {{p.name}}--{{p.age}}--{{p.sex}}
            </li>
        </ul>
    </div>
</body>
</html>
使用vue实现遍历需要使用v-for 遍历person中的内容。首先获取到原数据

<input type="text" placeholder="请输入名字" v-model="keyword">
此处使用v-model 可实现数据双向绑定，即可以获取用户输入的内容，也可以访问原数据内容，这样就可以实现通过用户输入在原数据中获取相关数据

第二步:在Vue中将原数据保存在一个数组中

const vm = new Vue({
            el:'#root',
            data:{
                keyword:'',
                person:[
                    {id:'001',name:'马冬梅',age:'10',sex:'女'},
                    {id:'002',name:'周冬梅',age:'40',sex:'女'},
                    {id:'003',name:'周杰伦',age:'30',sex:'男'},
                    {id:'004',name:'温兆伦',age:'60',sex:'男'}               
                ]
                         
            },
这里将原数据保存在名为person的数组中

keyword:''
设置keyword来获取data中数据，一开始没有内容我们用空串

第三步:编写计算属性实现过滤功能

<script>
	computed:{
       filePerson(){
           return this.person.filter((p)=>{
           return p.name.indexOf(this.keyword) !== -1
                 })
                }
            }
        })
    </script>
首先计算属性我们需要使用computed，在这里我们可看到新建了一个filePerson，这里是因为我们在实现过滤功能后会改变原界面，如果直接操作原数据会造成无法恢复原界面，所以我们直接保存一个数组，来访问新建的数组这样就不会破坏原数据。

 return this.person.filter((p)=>{
 return p.name.indexOf(this.keyword) !== -1
这里才是核心要素

filter（）函数，会创建一个新的数组，来检查指定数组中符合条件的元素，
filter()不会改变原数组，不会对空数组进行检测

indexOf() 方法会返回某个字符串首次出现的位置，此处检查keyword中是否含有name为指定字符串的数据。
注此方法对大小写比较敏感，如果检索的字符串没有出现则会返回-1.

当然我们也可以使用检监测属性完成列表过滤
代码如下：

new Vue({
            el:'#root',         
            data:{
                keyword:'',
                person:[
                    {id:'001',name:'马冬梅',age:'20',sex:'女'},
                    {id:'002',name:'周冬梅',age:'20',sex:'女'},
                    {id:'003',name:'周杰伦',age:'20',sex:'男'},
                    {id:'004',name:'温兆伦',age:'20',sex:'男'}               
                ],
                filePerson:[]
            },
            watch:{
                keyword:{
                    immediate:true,
                    handler(val){
                        this.filePerson = this.person.filter((p)=>{
                            return p.name.indexOf(val) !== -1
                        })
                    }
                }
            }
        })
这里我们同样是操作filePerson中的数据不改变原数据

以上是我学习的知识如有不对的地方还请指出，我们共同进步
