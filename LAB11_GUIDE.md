# Lab 11 Assignment Guide

**Due:** Tue Apr 7, 2026 4:30pm
**File to edit:** `js/stocks.js`

---

## 需要完成的 TODO 清单

### 1. 数据加载（第 20-23 行）
补全 `Promise.all` 里缺少的 4 个 stock：
```js
d3.csv("data/GOOG.csv").then(data => ({ name: "GOOG", values: data })),
d3.csv("data/AMZN.csv").then(data => ({ name: "AMZN", values: data })),
d3.csv("data/IBM.csv").then(data => ({ name: "IBM", values: data })),
d3.csv("data/MSFT.csv").then(data => ({ name: "MSFT", values: data })),
```

### 2. 数据处理（第 36-47 行）
在 `forEach` 里对每行数据做类型转换，并按日期排序：
```js
d.Date   = new Date(d.Date);
d.Close  = +d.Close;
d.Open   = +d.Open;
d.High   = +d.High;
d.Low    = +d.Low;
d.Volume = +d.Volume;
// 排序：
stock.values.sort((a, b) => a.Date - b.Date);
```

### 3. X 轴比例尺（第 89-91 行）
```js
const x = d3.scaleUtc()
    .domain(d3.extent(allValues, d => d.Date))
    .range([0, width]);
```

### 4. Y 轴比例尺（第 95-102 行）
```js
const y = d3.scaleLinear()
    .domain([
        d3.min(allValues, d => d.Close),
        d3.max(allValues, d => d.Close)
    ])
    .range([height, 0]);
```

### 5. 颜色比例尺（第 107 行）
```js
const color = d3.scaleOrdinal(d3.schemeCategory10)
    .domain(stocks.map(s => s.name));
```

### 6. 坐标轴（第 116、126 行）
```js
const xAxis = d3.axisBottom(x).ticks(d3.timeYear.every(1)).tickFormat(d3.timeFormat("%Y"));
const yAxis = d3.axisLeft(y).tickFormat(d => `$${d}`);
```

### 7. 折线生成器（第 142-143 行）
```js
const line = d3.line()
    .x(d => x(d.Date))
    .y(d => y(d.Close))
    .curve(d3.curveMonotoneX);
```

### 8. 绘制折线（第 147-155 行）
```js
stocks.forEach(stock => {
    svg.append('path')
        .datum(stock.values)
        .attr('fill', 'none')
        .attr('stroke', color(stock.name))
        .attr('stroke-width', 2)
        .attr('d', line);
});
```

### 9. 图表标题与坐标轴标签（第 164-192 行）
```js
// 标题
.attr('x', width / 2)
.attr('y', -20)
.text('Tech Stock Prices Over Time')

// X 轴标签
.attr('x', width / 2)
.attr('y', height + 45)
.text('Year')

// Y 轴标签（旋转后坐标系互换）
.attr('transform', 'rotate(-90)')
.attr('x', -height / 2)
.attr('y', -70)
.text('Closing Price (USD)')
```

### 10. 图例（第 210-224 行）
```js
legendRow.append('line')
    .attr('x1', 0).attr('y1', 0)
    .attr('x2', 20).attr('y2', 0)
    .attr('stroke', color(stock.name))
    .attr('stroke-width', 2);

legendRow.append('text')
    .attr('x', 25).attr('y', 4)
    .text(stock.name);
```

---

## 提交步骤
```bash
git add .
git commit -m 'complete stocks.js visualization'
git push
```
然后把 `https://github.com/qyniu/649-lab11` 提交到 Canvas。
