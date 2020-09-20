---
title: memorydependenceanalysis
date: 2020-09-09 23:36:54
tags: GVN
categories: LLVM
---

排版练习中

```
int a = 2;
int b = a * 2;
int c = a + b + 1;
```

简单描述，上述三行代码，a,b,c之间是有依赖关系的，b需要a的值，c需要b的值更新，这就是依赖，由于指令需要提高并发度，所以其实是必须等 a st(store)到一个register中，然后b才能调用这个register，后续一样，c必须等b st；这是最简单的例子。最近在分析一个bug中，不是以IR级别而是BB（basic blocks)级别；到目前为止，需要调整它的alignment还是删除掉这条ld（load），我尚无结论。

```
%1 = vload <4 * i16>  %2 align 4
```

从check而言这是不合理的，但是如果改成align 8，是正确的吗，为什么在第一次instr combine的过程不将align调整为8 而是 4。