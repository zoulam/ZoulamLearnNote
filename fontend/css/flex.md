# \[css\]flex布局详细介绍

```text
flex有两类元素：父元素和子元素、两者关系是相对的
    父元素可设置属性【display、fd、fw、ff、jc、ai、ac】 
    start表示轴的起点、end表示轴的终点，第一个属性为默认值，即flex默认是行排列
        display: flex | inline-flex【行元素】;
        flex-direction: row【左到右】 | row-reverse | column | column-reverse;
        flex-wrap: nowrap | wrap【正常换行】 | wrap-reverse【向上换行】;
        flex-flow: <flex-direction> || <flex-wrap>;
        justify-content【主轴】: flex-start | flex-end | center | space-between【两端对齐】 | space-around【均匀对齐，两侧留有空隙】;
        align-items【辅轴】: flex-start | flex-end | center | baseline【基线】 | stretch【拉伸】;
        align-content【多轴线、即出现换行】: flex-start | flex-end | center | space-between | space-around | stretch;
    子元素可设置属性：【优先级、放大、缩小、】
         order: <integer>;
         flex-grow:0【默认值，即如果存在剩余空间，也不放大】, <number>;
         flex-shrink:0【默认值，即空间不足，也不缩小】 <number>;
         flex-basis【基础、成分】: <length> | auto;【配合上面两个属性生效】
         flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
             auto (1 1 auto) 和 none (0 0 auto)
         align-self: auto | flex-start | flex-end | center | baseline | stretch;// 超脱父元素的控制，单独设置排列方式
```

