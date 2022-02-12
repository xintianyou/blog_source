---
title: element-ui Calendar 日历渲染自定义文案
date: 2021-06-18 15:29:09
tags: 
    - CSS
    - VUE
    - ELEMENT-UI
categories: 
    - web前端
---


[element-ui Calendar 官网链接](https://element.eleme.cn/#/zh-CN/component/calendar)

官网的只有一个基础的日历展示，但是提供了插槽，使得我们可以自定义日历单元格
[dateCell scoped slot 参数](https://element.eleme.cn/#/zh-CN/component/calendar#datecell-scoped-slot-can-shu)
| 参数 | 说明 | 类型 |
| :--- | :--- | :--- |
| date | 单元格代表的日期 | Date |
| data | { type, isSelected, day}，type 表示该日期的所属月份，可选值有 prev-month，current-month，next-month；isSelected 标明该日期是否被选中；day 是格式化的日期，格式为 yyyy-MM-dd | Object |

#### 看效果![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618151000205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70)
[codepen在线预览调试](https://codepen.io/luyouyuanfang/pen/LYWayqX?editors=1011)

```xml
<el-calendar v-model="value">
  <template slot="dateCell" slot-scope="{date, data}">
    <div @click="handleClickDay(date, data)">
      <div>{{data.day.split('-').slice(2).join('')}}</div>
      <div v-for="item in events">
        <div v-if="item.days.indexOf(data.day.split('-').slice(2).join('')) > -1 && item.months.indexOf(data.day.split('-')[1]) > -1">
          <div v-for="i in item.event">
            {{i}}
          </div>
        </div>
      </div>
    </div>
  </template>
</el-calendar>
```

```javascript
data() {
      return {
        value: new Date(),
        events: [
          {months: ['06', '07'], days: ['09'], event: ['打球']},
          {months: ['06'], days: ['15'], event: ['看电影', '玩游戏']},
          {months: ['06'], days: ['18'], event: ['吃饭']},
        ]
      }
    }
```

#### 点击单元格获取数据

```javascript
methods: {
	handleClickDay(date, data) {
		// "2021-06-18T00:00:00.000Z" // [Object, Object]
		// { 
			// type: 'current-month',  // 表示当前月份， 可选值有 prev-month，current-month，next-month
			// isSelected: 'false', // 表示是否被选中
			// day: '2021-06-18' // 当前日期
		// }
	}
}
```

ps:
calendar组件插槽的data参数会返回当前每个单元格的日期**字符串**yyyy-MM-dd格式，数据源中也可以直接是完整的日期格式，避免转化。