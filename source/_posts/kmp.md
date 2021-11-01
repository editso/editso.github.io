---
title: 字符串匹配算法-KMP
date: 2019-11-27 15:50:04
tags: 
    - Algorithm
    - 编程
    - Code
---

# 字符串匹配算法-KMP

## 说明  
kmp的一些概述不做解释了, 请参考:  [kmp算法](https://baike.baidu.com/item/kmp%E7%AE%97%E6%B3%95/10951804?fr=aladdin) (百度百科)  
参考了 `阮一峰`的: [字符串匹配的KMP算法](http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)  
使用 `C` 语言实现的算法  

## 部分匹配表
指在一串字符串中, 前缀与后缀中所共有的字符  
- 前缀: 不包含字符串最后一个字符  
- 后缀: 不包含字符串第一个字符  

### 例子:  
字符串 (`AHABAD`)**  
由此得出他们的部分匹配表为  

**`A -> 0`**  
    前后都没有所有共有字符数所以为 0  

**`AH -> 0`**  
    前缀: `A`  
    后缀: `H`  
    它们没有功能字符所有为 `0`  

**`AHA -> 1`**  
    前缀: `A, AH`  
    后缀: `A, HA`  
    它们有共有的字符 共有数为 `1`  

**`AHAB -> 0`**  
    前缀: `A, AH, AHA`  
    后缀: `B, AB, HAB`  
    它们没有共有字符所有为 `0`  

**`AHABA -> 1`**  
    前缀: `A, AH, AHA, AHAB`  
    后缀: `A, BA, ABA, HABA`  
    它们有共有字符 共有数为 `1`  

> 由此得出 `AHABAD` 的前缀表为

| | | | | | |
|  :----:  | :----:  | :----: | :----: | :----: | :----: |
|0|1|2|3|4|5|
|A|H|A|B|A|D|
|-1|0|0|1|0|1|

> 部分匹配表算法实现过程 `C 语言`  

```
int* prefix(char* s){
    /*
        接收一个字符串,
        返回一个部分匹配表  
    */

    int len = strlen(s),
        i = 0,
        j = -1, 

        *p = malloc(sizeof(int) * len);

    p[0] = -1;

    while(i < len - 1){
        if(j == -1 || s[i] == s[j]){
            p[++i] = ++j;
            continue;
        }
        j = p[j];
    }

    return p;
}
```

**变量 `i` && `j` && `p` 的作用**  
    在这里 `p` 可以看作是一次回溯,
    应为每当 if 条件不满足时 则进行一次回溯  
    那么此时 j 的位置就需要重置到 p[j]

> 部分匹配表生成过程 以字符串 `AHABAD` 为例

|变量                |A            |H              |A               |B           |A           |D  
|:----:              |:----:       | :----:       |  :----:          | :----:     | :----:     | :----:  
|s = AHABAD          |\            |A             |A                |H           |A           |\  
|j = -1              |-1, 0        |0,-1, 0       |0, 1             |1, 0, -1, 0 |0, 1        | \  
|i = 0               |0, 1         |1, 2          |2, 3             |3, 4        |4, 5        | \  
|p[0]=-1        |p[1]=0  |p[2]= 0  |p[3]=1       |p[4]=0  |p[5]=1 | \

> **kmp**  
```
//kmp匹配算法
int kmpSearch(char *t, char *s){
    int t_len = strlen(t), 
        s_len = strlen(s);

    int *p = prefix(s), 
        m = 0, //代表母串匹配到的位置
        j = 0; // 代表字符匹配到的位置

    while(t_len > m && s_len > j){
        //j == -1 那么代表不和任何匹配直接向后移动以为
        if(j == -1 ||  t[m] == s[j]){
            m++;
            j++;
            continue;
        }
        //当不相等时, 就去查询部分匹配表
        j = p[j];
    }

    if (j == s_len)
        //返回匹配到的索引位置
        return m - j;
    return -1;
}
```

### 完整代码
所需头文件:  
> `string.h stdlib.h`  
```
//部分匹配表算法
int* prefix(char* s){
    //prefix table
    int len = strlen(s),
        i = 0,
        j = -1,
        *p = malloc(sizeof(int) * len);
    p[0] = -1;

    while(i < len - 1){
        if(j == -1 || s[i] == s[j]){
            p[++i] = ++j;
            continue;
        }
        j = p[j];
    }

    return p;
}

//kmp
int kmpSearch(char *t, char *s){
    int t_len = strlen(t), 
        s_len = strlen(s);

    int *p = prefix(s), 
        m = 0,
        j = 0;

    while(t_len > m && s_len > j){
        if(j == -1 ||  t[m] == s[j]){
            m++;
            j++;
            continue;
        }
        j = p[j];
    }

    if (j == s_len)
        return m - j;
    return -1;

}
```