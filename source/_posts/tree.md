---
title: 树形数据的方法论
date: 2019-05-23 14:40:31
tags: js vue json
index_img: /img/tree.png
categories: js vue json
---

数据结构

```
var  data2= [
         {
          id: 1,
          label: '一级 1',
          children: [{
            id: 4,
            label: '二级 1-1',
            children: [{
              id: 9,
              label: '三级 1-1-1'
            }, {
              id: 10,
              label: '三级 1-1-2'
            }]
          }]
        }, 
        {
          id: 2,
          label: '一级 2',
          children: [{
            id: 5,
            label: '二级 2-1'
          }, {
            id: 6,
            label: '二级 2-2'
          }]
        }, 
        {
          id: 3,
          label: '一级 3',
          children: [{
            id: 7,
            label: '二级 3-1'
          }, {
            id: 8,
            label: '二级 3-2'
          }]
        }];
    


```

#### 根据ID获取该节点的所有父节点的对象

```
  function getParentId(list,id) {
        for (let i in list) {
            if(list[i].id==id){
            return [list[i]]
          }
          if(list[i].children){
            let node=getParentId(list[i].children,id);
            if(node!==undefined){
                return node.concat(list[i])
               }
          }
        }        
    }
           getParentId(data2,10)//打印出来就是想要的数据

```

![1635303387290](1635303387290.png)

#### 根据ID获取该节点的对象

```js
function getId(list,id) {
        for (let i in list) {
            if(list[i].id==id){
            return [list[i]]
          }
          if(list[i].children){
            let node=getParentId(list[i].children,id);
            if(node!==undefined){
                return node;
               }
          }
        }    
    }

      getId(data2,4)//打印出来就是想要的数据

```

![1635303459540](1635303459540.png)

#### 根据ID获取所有子节点的对象，首先把该节点的对象找出来，上面getId（）这个方法

```js
  function getNodeId(list,newNodeId=[]) {
        for (let i in list) {
            newNodeId.push(list[i])
          if(list[i].children){
                  getNodeId(list[i].children,newNodeId);
            }
        } 
        return newNodeId;      
    }

      //查找id=4的所有子级节点
    let objId=getId(data2,4);
    let childId=getNodeId(objId);//打印出来就是想要的数据 

```

![1635303550956](1635303550956.png)

**平行结构转树形菜单**

```js
var data = [
			  {"id":2,"name":"第一级1","pid":0},
			  {"id":3,"name":"第二级1","pid":2},
			  {"id":5,"name":"第三级1","pid":4},
			  {"id":100,"name":"第三级2","pid":3},
			  {"id":6,"name":"第三级2","pid":3},
			  {"id":601,"name":"第三级2","pid":6},
			  {"id":602,"name":"第三级2","pid":6},
			  {"id":603,"name":"第三级2","pid":6}
			];
// 数组转树形结构数据（原理即为通过设置id为key值，再通过pid去找这个key是否一样，一样则为这数据的子级数据）
			function arrayToJson(treeArray){
			    var r = [];
			    var tmpMap ={};
			    for (var i=0, l=treeArray.length; i<l; i++) {
			     // 以每条数据的id作为obj的key值，数据作为value值存入到一个临时对象里面
			      tmpMap[treeArray[i]["id"]]= treeArray[i]; 
			    } 
			    console.log('tmpMap',tmpMap)
			    for (i=0, l=treeArray.length; i<l; i++) {
			      var key=tmpMap[treeArray[i]["pid"]];
			      console.log('key',key)
			      //循环每一条数据的pid，假如这个临时对象有这个key值，就代表这个key对应的数据有children，需要Push进去
			      //如果这一项数据属于哪个数据的子级
			      if (key) {
			      	// 如果这个数据没有children
			        if (!key["children"]){
			            key["children"] = [];
			            key["children"].push(treeArray[i]);
			        // 如果这个数据有children
			        }else{
			          key["children"].push(treeArray[i]);
			        }       
			      } else {
			        //如果没有这个Key值，就代表找不到属于哪个数据，那就代表没有父级,直接放在最外层
			        r.push(treeArray[i]);
			      }
			    }
			    return r
			   }
```

 树形结构数据转单层数组形式数据： 

```js
var jsonarr = [{
			    id: 1,
			    name: '北京市',
			    ProSort: 1,
			    remark: '直辖市',
			    pid: '',
			    isEdit: false,
			    children: [{
			      id: 35,
			      name: '朝阳区',
			      pid: 1,
			      remark: '',
			      isEdit: false,
			      children: []
			    }, {
			      id: 36,
			      name: '海淀区',
			      pid: 1,
			      remark: '',
			      isEdit: false,
			      children: []
			    }, {
			      id: 37,
			      name: '房山区',
			      pid: 1,
			      remark: '',
			      isEdit: false,
			      children: []
			    }, {
			      id: 38,
			      name: '丰台区',
			      pid: 1,
			      remark: '',
			      isEdit: false,
			      children: []
			    }]
			  }, {
			    id: 2,
			    name: '天津市',
			    ProSort: 2,
			    remark: '直辖市',
			    pid: '',
			    isEdit: false,
			    children: [{
			      id: 41,
			      name: '北辰区',
			      pid: 2,
			      remark: '',
			      isEdit: false,
			      children: [{
			      id: 113,
			      name: '天津大道',
			      ProSort: 2,
			      remark: '道路',
			      pid: '',
			      isEdit: false,
			      children:[]}]
			    }, {
			      id: 42,
			      name: '河北区',
			      pid: 2,
			      remark: '',
			      isEdit: false,
			      children: []
			    }, {
			      id: 43,
			      name: '西青区',
			      pid: 2,
			      remark: '',
			      isEdit: false,
			      children: []
			    }]
			  }, {
			    id: 3,
			    name: '河北省',
			    ProSort: 5,
			    remark: '省份',
			    pid: '',
			    isEdit: false,
			    children: [{
			      id: 45,
			      name: '衡水市',
			      pid: 3,
			      remark: '',
			      isEdit: false,
			      children: []
			    }, {
			      id: 46,
			      name: '张家口市',
			      pid: 3,
			      remark: '',
			      isEdit: false,
			      children: []
			    }]
			  }]
// 树形结构数据转单层数组形式
			function jsonToArray(nodes) {
		      var r=[];
		      if (Array.isArray(nodes)) {
		        for (var i=0, l=nodes.length; i<l; i++) {
		          r.push(nodes[i]); // 取每项数据放入一个新数组
		          if (Array.isArray(nodes[i]["children"])&&nodes[i]["children"].length>0)
		           // 若存在children则递归调用，把数据拼接到新数组中，并且删除该children
		            r = r.concat(jsonToArray(nodes[i]["children"]));
		              delete nodes[i]["children"]
		        }
		      } 
		      return r;
		    }
```

