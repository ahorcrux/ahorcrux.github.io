---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Finance, Trade]
tags: [金融, 交易, 偏离率]
render_with_liquid: false
---

## TV脚本
```js
//@version=5
strategy("LLT Strategy", overlay=true)

// 可调整的参数a
a = input.float(0.05, title="Alpha")

// 初始化LLT
var float LLT = na

// 计算LLT，如果是第一根K线，则LLT设为该K线的收盘价
price = close
LLT := na(LLT[1]) ? price : (a - math.pow(a, 2) / 4) * price + (math.pow(a, 2) / 2) * nz(price[1]) - (a - (3 / 4) * math.pow(a, 2)) * nz(price[2]) + 2 * (1 - a) * nz(LLT[1]) - math.pow(1 - a, 2) * nz(LLT[2])

// 计算LLT的变化率
deltaLLT = LLT - LLT[1]

// 生成交易信号
longCondition = deltaLLT > 0 and deltaLLT[1] <= 0
shortCondition = deltaLLT < 0 and deltaLLT[1] >= 0

// 执行交易逻辑
if (longCondition)
	if (strategy.position_size < 0)
		strategy.close("Close Short", comment="Close Short")
	strategy.entry("Long", strategy.long, comment="Enter Long")

if (shortCondition)
	if (strategy.position_size > 0)
		strategy.close("Close Long", comment="Close Long")
	strategy.entry("Short", strategy.short, comment="Enter Short")

// 绘制LLT
plot(LLT, color=color.blue, title="LLT")

```
## *参考链接*

