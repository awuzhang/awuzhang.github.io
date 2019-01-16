---
layout:     post
title:      MapReduce学习笔记
subtitle:   留存显示
date:       2018-12-12
author:     awuzhang
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - javascript
    - MapReduce
    
---

#### 学习笔记，用 Javascript 简单模拟 MapReduce 计算单词词频

假设下面分章分布在各服务器，在各服务器上 map & reduce 单服的词频， 然后再汇总到1台服务器上 reduce


``` javascript 
// 以句号切割出数据集
data = ['Consumer price index (CPI), a main gauge of inflation, rose 2.2 percent year-on-year in November, pulling back from the 2.5-percent gain in October, according to data from the National Bureau of Statistics (NBS).',
'That was the first slowdown of year-on-year CPI growth in the second half of 2018.',
'On a monthly basis, the CPI dipped 0.3 percent compared with a 0.2-percent gain in October.',
'The CPI growth edged down mainly due to the softening of food prices.',
'Food prices fell 1.2 percent month on month in November, dragging down the CPI growth by 0.25 percentage points, the NBS statistician Sheng Guoqing said.',
'Prices of fresh vegetables declined 12.3 percent due to abundant supply while the price of pork, the staple meat of the country, dipped 0.6 percent since pig supply accelerated in some regions for fear of the African swine fever, Sheng said.',
'On a yearly basis, the price of pork continued to slump in November, down 1.1 percent year-on-year, with the decline rate narrowing for a sixth month.',
'The unusual weather in the second and third quarters in parts of China might have enlarged the seasonal fluctuations of the prices of fresh vegetables and fruits, said Li Chao, researcher of the Huatai Securities.',
'Li forecast low inflation risk in the fourth quarter, saying food price fluctuations had limited influence on the overall consumer inflation in the medium term.',
'The CPI growth was also dragged down by the decline of oil prices. Prices of gasoline and diesel fell 4.9 percent and 5.2 percent month on month, respectively, pulling down the CPI growth by 0.12 percentage points.',
'The consumer inflation will continue to soften since domestic demand is weak and oil prices are hard to rebound in the short term, said Yang Yewei, an analyst with Southwest Securities.',
'For the coming year, Xiong Yuan, an analyst with Guosheng Securities, said China will not see much CPI inflation risk, which will offer much space for policymakers to maneuver.',
'China is aiming to keep annual CPI growth at around 3 percent this year, the same as the 2017 target.',
'The average year-on-year CPI growth for the first 11 months stood at 2.1 percent, unchanged from the first 10 months, according to the NBS.',
'Producer price index (PPI), which measures costs for goods at the factory gate, rose 2.7 percent year-on-year in November, with the growth declining for five consecutive months.']


// 单句 mapreduce 统计词频
fun1 = (str) => new Promise ((resolve)=> {
	// 正则匹配单词
	str = str.toLowerCase().replace(/[^a-zA-Z]/g, ' ').replace(/\s+/g, ' ').trim()

	// map 把单词分成 {"word" : 1}, reduce 把词频累加起来
	let res = str.split(' ')
		.map(n => JSON.parse(`{"${n}":1}`))
		.reduce((sum, n) => {
			sum[Object.keys(n)[0]] = (sum[Object.keys(n)] || 0) + n[Object.keys(n)] 
			return sum
		})
	resolve(res)
})


// Promise 去请求各服务器上的 reduce 数据；（这边是模拟，所以把分章的内容发个各服务器，一般情况下是分布的服务器统计自己的数据）
Promise.all( data.map(item => fun1(item)) ).then((datas) => {
	let sum = datas.reduce((sum, n) => {
		Object.keys(n).map(key => {
			sum[key] = (sum[key] || 0) + n[key] 
		})
		return sum
	})
	console.log(sum)
})

```

```
输出
{consumer: 3, price: 5, index: 2, cpi: 10, a: 5, …}
```

该文章主要是模拟 map 和 reduce 各做什么事情，分布式计算学习中，欢迎一起学习讨论
