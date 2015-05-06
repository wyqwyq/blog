---
layout: default
title: hihoCoder 1154 Spring Outing（微软2016校园招聘在线笔试第二场 第三题）
comments: true
---

###1154 : Spring Outing
时间限制:20000ms
单点时限:1000ms
内存限制:256MB
###描述
You class are planning for a spring outing. N people are voting for a destination out of K candidate places.

The voting progress is below:

First the class vote for the first candidate place. If more than half of the class agreed on the place, the place is selected. The voting ends.

Otherwise they vote for the second candidate place. If more than half of the class agreed on the place, the place is selected. The voting ends.

Otherwise they vote for the third candidate place in the same way and go on.

If no place is selected at last there will be no spring outing and everybody stays at home.

Before the voting, the Chief Entertainment Officer did a survey, found out every one's preference which can be represented as a permutation of 0, 1, ... K. (0 is for staying at home.) For example, when K=3, preference "1, 0, 2, 3" means that the first place is his first choice, staying at home is the second choice, the second place is the third choice and the third place is the last choice.

The Chief Entertainment Officer sends the survey results to the class. So everybody knows the others' preferences. Everybody wants his more prefered place to be selected. And they are very smart, they always choose the optimal strategy in the voting progress to achieve his goal.

Can you predict which place will be selected?

###输入
The first line contains two integers, N and K, the number of people in your class and the number of candidate places.

The next N lines each contain a permutation of 0~K, representing someone's preference.

For 40% of the data, 1 <= N, K <= 10

For 100% of the data, 1 <= N, K <= 1000

###输出
Output the selected place. Or "otaku" without quotes if no place is selected.

####样例提示
In the sample case, if the second peoson vote against the first place, no place would be selected finally because the first person must vote against the second place for his own interest. Considering staying at home is a worse choice than the first place, the second person's optimal strategy is voting for the first place. So the first place will be selected.

###样例输入

```
2 2
1 0 2
2 1 0
```
###样例输出

```
1
```


###Solution
该问题逐轮投票，直至巧好选出地点或者所有投票失败。

问题关键在于，对某一轮投票，某人如何投票，Yes or No。

仔细想想，只可能以下两种情况：

1. 当前投票地点在他<font color="red">眼中</font>优先级最高，是最优选择，Yes；
2. 当前投票地点在他<font color="red">眼中</font>不是优先级最高的，No。

这里，“眼中”指的是如果他能预测到这轮没通过，后面最近的某一轮确定地点时（或者都不去）时的场景，从而做出比较。
这里要计算“眼中”，只能用倒推的方法做。

考虑最后地点K，假定前面的K-1个地点都失败了，那么很容易确定去不去最后一个地点K。
然后，再逐一倒推前面一轮投票。
每次对某人来说，可以知道后面是否有更好的选择。如果没有，就Yes，否则No。
这样就可以确定这轮投票地点是否能去。

代码如下：
***

```cpp
#include<iostream>
#include<stdio.h>
#include<algorithm>
#include<vector>
using namespace std;

const int MAXN = 1005;
int mat[MAXN][MAXN];
int pMat[MAXN][MAXN];
int cnt, ret;
int np[MAXN];
int N, K;

void solve(){
    cnt = 0;
    for(int i = 0; i < N; i++){
        if(mat[i][K] == 0){
            cnt++;
        }
    }
    if(cnt > N / 2){
        ret = K;
        for(int i = 0; i < N; i++){
            np[i] = pMat[i][K];
        }
    }else{
        ret = 0;
        for(int i = 0; i < N; i++){
            np[i] = pMat[i][0];
        }
    }

    for(int j = K - 1; j >= 1; j--){
        cnt = 0;
        for(int i = 0; i < N; i++){
            if(pMat[i][j] < np[i]){
                cnt++;
            }
        }
        if(cnt > N / 2){
            ret = j;
            for(int i = 0; i < N; i++){
                np[i] = pMat[i][j];
            }
        }
    }
    if(ret == 0){
        printf("otaku\n");
    }else
        printf("%d\n", ret);
}

int main(){
    scanf("%d %d", &N, &K);
    for(int i = 0; i < N; i++){
        for(int j = 0; j <= K; j++){
            scanf("%d", &mat[i][j]);
            pMat[i][mat[i][j]] = j;
        }
    }
    solve();
    return 0;
}

```
