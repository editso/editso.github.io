---
title: acm-士兵队列训练问题
date: 2020-11-08 15:44:38
tags:
    - Algorithm
    - Acm
    - C
---
# 士兵队列训练问题
某部队进行新兵队列训练，将新兵从一开始按顺序依次编号，并排成一行横队，训练的规则如下：从头开始一至二报数，凡报到二的出列，剩下的向小序号方向靠拢，再从头开始进行一至三报数，凡报到三的出列，剩下的向小序号方向靠拢，继续从头开始进行一至二报数。。。，以后从头开始轮流进行一至二报数、一至三报数直到剩下的人数不超过三人为止。 

## 分析
该题意思是,队列编号第一轮编号`1~2`叫到 `2` 的出列, 第二轮编号`1~3`叫到`3`的出列,反复循环直到剩余人数不超过`3`人

## 实现代码 `c`
```
#include <stdio.h>
#include <stdlib.h>


void full(int num, int *soldier){
    for (int i = 0; i < num; i++)
    {
        soldier[i] = i+1;
    }
}


int* line_up(int num){
    int* soldier = malloc(sizeof(int) * num);
    //初始化士兵
    full(num, soldier);
    /**
     * i: 当前士兵
     * j: 报数
     * s: 最高编号
    */
    for (int i = 0, j = 1, s = 2, m = num; m >= 3; i++)
    {
        if(i > num && s == 2){
            /**
             * 第一轮报数完成, 进行第二轮报数
            */
            s = 3; i = 0; j = 1;
        }else if(i > num && s == 3){
            /**
             * 第二轮报数完成,重新开始到第一轮报数
            */
            s = 2,  i = 0, j = 1;
        }
        /**
         * 当前士兵已经出列, 继续报数
        */
        if(soldier[i] == -1) continue;
        if(j == s){
            /**
             * 出列
            */
            soldier[i] = -1;
            j = 1; m--;
        }else{
            j++; // 编号
        }
    }
    return soldier;
}


int out_soldier(int num){
    int *soldier = line_up(num);
    for (int i = 0; i < num; i++)
    {   
        if(soldier[i] == -1) continue;
        printf("%d ", soldier[i]);
    }
    printf("\n");
    free(soldier);
}


int main(){
    int row, col;
    scanf("%d", &row);
    int array[row], i = 0;
    while (i < row)
    {
        scanf("%d", array + i++);
    }
    i = 0;
    while (i < row)
    {
        out_soldier(array[i++]);
    }
    return 0;
}
```