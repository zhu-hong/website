---
title: "树形数据结构化"
date: 2021-11-11T19:41:59+08:00
draft: true
categories: [JavaScript]
---

## 数据树形结构化

> 一个扁平数据列表 => 描述树形结构的字段 PID => 树形结构列表
>
> PID => ID

1. 顶级与子级数据分开
2. 递归或其他操作

```javascript
const data = [
  {
    id: 2,
    pid: 0,
    path: '/course',
    name: 'Course',
    title: '课程管理',
  },
  {
    id: 3,
    pid: 2,
    path: 'operate',
    link: '/course/operate',
    name: 'CourseOperate',
    title: '课程操作',
  },
  {
    id: 4,
    pid: 3,
    path: 'info_data',
    link: '/course/operate/info_data',
    name: 'CourseInfoData',
    title: '课程数据',
  },
  {
    id: 5,
    pid: 2,
    path: 'add',
    link: '/course/add',
    name: 'CourseAdd',
    title: '课程增加',
  },
  {
    id: 6,
    pid: 0,
    path: '/student',
    name: 'Student',
    title: '学生管理',
  },
  {
    id: 7,
    pid: 6,
    path: 'operate',
    link: '/course/operate',
    name: 'StudentOperate',
    title: '学生操作',
  },
  {
    id: 8,
    pid: 6,
    path: 'add',
    link: '/student/add',
    name: 'Course',
    title: '学生增加',
  },
]

console.log(formatDataTree(data))

// 扁平化处理
// 第二种写法,有人说只做了第一层,为什么最后还是形成了树,是因为filter拿的是后面数组元素的引用,后面再给子元素添加它的子元素,已经添加到第一层的子元素也会跟着改
function formatDataTree(data: any[]) {
  let _data: any[] = JSON.parse(JSON.stringify(data));

  return _data.filter((p) => {
    const _arr = _data.filter((c) => c.pid === p.id)
    _arr.length && (p.children = _arr)
    return p.pid === 0
  })
}

// 递归写法
// function formatDataTree(data: any[]): any[] {
//   let [parents, childrens] = [data.filter(p => p.pid === 0), data.filter(p => p.pid !== 0)]

//   dataToTree(parents, childrens)

//   function dataToTree(parents: any[], childrens: any[]) {
//     parents.forEach((p, i) => {
//       childrens.forEach((c) => {
//         let _childrens: any[] = JSON.parse(JSON.stringify(childrens))
//         _childrens.splice(i, 1)
//         dataToTree([c], _childrens)

//         if (c.pid === p.id) {
//           if (!p.children) {
//             p.children = []
//           }
//           p.children.push(c)
//         }
//       })
//     })
//   }

//   return parents
// }
```