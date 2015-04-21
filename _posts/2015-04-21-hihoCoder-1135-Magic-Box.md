---
layout: default
title: hihoCoder 1135 Magic Box（微软2016校园招聘在线笔试）
comments: true
---

##1135 : Magic Box
时间限制:10000ms
单点时限:1000ms
内存限制:256MB

###描述
The circus clown Sunny has a magic box. When the circus is performing, Sunny puts some balls into the box one by one. The balls are in three colors: red(R), yellow(Y) and blue(B). Let Cr, Cy, Cb denote the numbers of red, yellow, blue balls in the box. Whenever the differences among Cr, Cy, Cb happen to be x, y, z, all balls in the box vanish. Given x, y, z and the sequence in which Sunny put the balls, you are to find what is the maximum number of balls in the box ever.

For example, let's assume x=1, y=2, z=3 and the sequence is RRYBRBRYBRY. After Sunny puts the first 7 balls, RRYBRBR, into the box, Cr, Cy, Cb are 4, 1, 2 respectively. The differences are exactly 1, 2, 3. (|Cr-Cy|=3, |Cy-Cb|=1, |Cb-Cr|=2) Then all the 7 balls vanish. Finally there are 4 balls in the box, after Sunny puts the remaining balls. So the box contains 7 balls at most, after Sunny puts the first 7 balls and before they vanish.

###输入
Line 1: x y z

Line 2: the sequence consisting of only three characters 'R', 'Y' and 'B'.

For 30% data, the length of the sequence is no more than 200.

For 100% data, the length of the sequence is no more than 20,000, 0 <= x, y, z <= 20.

###输出
The maximum number of balls in the box ever.

###提示
Another Sample

| Sample Input  | Sample Output |
| ------------- |---------------|
| 0 0 0         |      4        |
| RBYRRBY       |               |

###样例输入
`1 2 3`
`RRYBRBRYBRY`
###样例输出
`7`

###Solution
实打实地硬算，应该没有更为巧妙的方法，能把复杂度从*O*(n)降到更低。

思想：每放入一个球，更新对当前局面各球的计数，判断是否满足消失的要求，更新全局最优解。

代码如下：

```cpp
  //#1135 : Magic Box
  #include<iostream>
  #include<string>
  #include<utility>
  #include<algorithm>
  using namespace std;
  
  int cnt[3], tmp[3];
  int diff[3];
  string str;
  int ret, pre;
  
  bool vanish(){
  	copy(cnt, cnt + 3, tmp);
  	sort(tmp, tmp + 3);
  	if(diff[2] == tmp[2] - tmp[0]){
  		int d21 = tmp[2] - tmp[1];
  		int d10 = tmp[1] - tmp[0];
  		if(d21 > d10){
  			return diff[1] == d21 && diff[0] == d10;
  		}else{
  			return diff[1] == d10 && diff[0] == d21;
  		}
  	}
  	return false;
  }
  
  int main(){
  	cin >> diff[0] >> diff[1] >> diff[2];
  	sort(diff, diff + 3);
  	cin >> str;
  	pre = -1;
  	ret = 0;
  
  	for(int i = 0; i < str.size(); i++){
  		switch(str[i]){
  		case 'R':
  			cnt[0]++;
  			break;
  		case 'B':
  			cnt[1]++;
  			break;
  		case 'Y':
  			cnt[2]++;
  			break;
  		default:
  			break;
  		}
  		ret = max(ret, i - pre);
  		if(vanish()){
  			pre = i;
  			cnt[0] = cnt[1] = cnt[2] = 0;
  		}
  	}
  	cout << ret << endl;
  	return 0;
  }
```
