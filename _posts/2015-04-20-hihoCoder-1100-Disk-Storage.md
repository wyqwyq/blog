---
layout: default
title: hihoCoder #1100 : Disk Storage（微软苏州校招笔试 1月3日）
comments: true
---


##1100 : Disk Storage
时间限制:10000ms 
单点时限:1000ms
内存限制:256MB

###描述
![](http://media.hihocoder.com//problem_images/20150103/14202824928356.png)

Little Hi and Little Ho have a disk storage. The storage's shape is a truncated cone of height H. R+H is radius of top circle and R is radius of base circle. 

Little Ho buys N disks today. Every disk is a cylinder of height 1. Little Ho wants to put these disk into the storage under below constraints:

1. Every disk is placed horizontally. Its axis must coincide with the axis of storage.
2. Every disk is either place on the bottom surface or on another disk.
3. Between two neighboring disks in the storage, the upper one's radius minus the lower one's radius must be less than or equal to M.

Little Ho wants to know how many disks he can put in the storage at most.

####输入
Input contains only one testcase.

The first line contains 4 integers: N(1 <= N <= 100000), M, H, R(1 <= M, R, H <= 100000000).

The second line contains N integers, each number prepresenting the radius of a disk. Each radius is no more than 100000000.

####输出
Output the maximum possible number of disks can be put into the storage.

####样例输入
```c++
5 1 10 3

1 3 4 5 10
```
####样例输出
`4`


###Solution
构造法。
关键是注意到，当最多能叠放N个disk时，一定可以重新调整成如下形式（这点可以用反证法证明）：
a<sub>0</sub>, a<sub>1</sub>, ... , a<sub>k</sub>, ..., a<sub>N-1</sub>自底向上叠放，其中N <= H，a<sub>0</sub> <= a<sub>1</sub> <= ...  *<=* **a<sub>k</sub>** *>=*  a<sub>k+1</sub> >= ... >= a<sub>N-1</sub>, 同时 a<sub>0</sub> <= R且对 0 <= i <= N-1，有 a<sub>i</sub> <= R + i。


根据以上限制条件，很容易写出如下代码：
```c++
	#include<iostream>
	#include<vector>
	#include<algorithm>
	#include<utility>
	using namespace std;
	
	int N, M, H, R;
	int tmp;
	vector<int> vec;
	int ret;
	
	void solve();
	
	int main(){
		cin >> N >> M >> H >> R;
		for(int i = 0; i < N; i++){
			cin >> tmp;
			if(tmp <= R + H)
				vec.push_back(tmp);
		}
		solve();
		cout << ret;
		return 0;
	}
	
	void solve(){
		sort(vec.begin(), vec.end());
		int i = 0;
		int k;
		ret = 0;
	
		for(; i < vec.size() && vec[i] <= R;){
			k = i + 1;
			while(k < vec.size() && vec[k] - vec[k-1] <= M && vec[k] <= R + k - i){
				k++;
			}
			ret = max(ret, k);
			i = k;
		}
		ret = min(ret, H);
	}
```

