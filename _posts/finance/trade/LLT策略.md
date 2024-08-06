---
title: 
author: since2014
date: 2023-10-28 18:02:00 +0800
categories: [Finance, Trade]
tags: [金融, 交易, 偏离率]
render_with_liquid: false
---


# 使用技巧



# TV脚本

## 第一版
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

## 常用版

```js
//@version=5
indicator("LLT Indicator", overlay=true)

// 可调整的参数a
a = input.float(0.05, title="Alpha")

// 初始化LLT
var float LLT = na

// 计算LLT
price = close
LLT := na(LLT[1]) ? price : na(LLT[2]) ? price : (a - math.pow(a, 2) / 4) * price + (math.pow(a, 2) / 2) * nz(price[1]) - (a - (3 / 4) * math.pow(a, 2)) * nz(price[2]) + 2 * (1 - a) * nz(LLT[1]) - math.pow(1 - a, 2) * nz(LLT[2])

// 计算LLT的变化率
deltaLLT = LLT - LLT[1]

// 生成交易信号
longCondition = deltaLLT > 0 and deltaLLT[1] <= 0
shortCondition = deltaLLT < 0 and deltaLLT[1] >= 0

// 绘制LLT
plot(LLT, color=color.blue, title="LLT Line")

// 标记买入和卖出信号
plotshape(series=longCondition, location=location.belowbar, color=color.green, style=shape.arrowup, title="Buy", text="Buy")
plotshape(series=shortCondition, location=location.abovebar, color=color.red, style=shape.arrowdown, title="Sell", text="Sell")

// 设置通知
alertcondition(longCondition, title="Buy Alert", message="LLT Buy Signal at {{close}}")
alertcondition(shortCondition, title="Sell Alert", message="LLT Sell Signal at {{close}}")

// 显示交易信号通知
if (longCondition) 
	alert("LLT Buy Signal at " + str.tostring(close), alert.freq_once_per_bar)
if (shortCondition)
	alert("LLT Sell Signal at " + str.tostring(close), alert.freq_once_per_bar)
```

## 这个是tradingview买入信号通知的代码
```js
//@version=5
indicator("Dragonfly Signal", overlay=true)

// 定义各个移动平均线
ma5 = ta.sma(close, 5)
ma10 = ta.sma(close, 10)
ma30 = ta.sma(close, 30)
ma90 = ta.sma(close, 90)
ma120 = ta.sma(close, 120)
ma256 = ta.sma(close, 256)

// 定义检查MA5和MA10拐点的函数
checkInflection(ma, len) =>
    found = false
    for i = 1 to len
        if (ma[i] < ma[i+1] and ma[i-1] > ma[i])
            found := true
    found

// 检查过去5根K线的MA5和MA10拐点
ma5Inflection = checkInflection(ma5, 5)
ma10Inflection = checkInflection(ma10, 5)

// 定义开仓条件
longCondition1 = close < ma30 and close < ma90 and close < ma120 and close < ma256
longCondition2 = close > ma10 and open < ma10
longCondition3 = close > ma5
longCondition4 = ma5 > ta.sma(close[1], 5) and ma10 > ta.sma(close[1], 10)
longCondition5 = ma5Inflection
longCondition6 = ma10Inflection

// 检查是否满足所有开仓条件
longEntry = longCondition1 and longCondition2 and longCondition3 and longCondition4 and longCondition5 and longCondition6

// 创建警报条件
alertcondition(longEntry, title="Buy Signal", message="Buy Signal at {{close}}")

// 标记买入信号
if longEntry
    label.new(bar_index, low, "Buy", color=color.rgb(33,243,243), textcolor=color.white, style=label.style_label_up, size=size.small)
    
```
## *参考链接*

