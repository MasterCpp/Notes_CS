# 1001-1010

## 1001 害死人不偿命的(3n+1)猜想

卡拉兹(Callatz)猜想：

对任何一个正整数 *n*，如果它是偶数，那么把它砍掉一半；如果它是奇数，那么把 (3*n*+1) 砍掉一半。这样一直反复砍下去，最后一定在某一步得到 *n*=1。卡拉兹在 1950 年的世界数学家大会上公布了这个猜想，传说当时耶鲁大学师生齐动员，拼命想证明这个貌似很傻很天真的命题，结果闹得学生们无心学业，一心只证 (3*n*+1)，以至于有人说这是一个阴谋，卡拉兹是在蓄意延缓美国数学界教学与科研的进展……

我们今天的题目不是证明卡拉兹猜想，而是对给定的任一不超过 1000 的正整数 *n*，简单地数一下，需要多少步（砍几下）才能得到 *n*=1？

```c++
#include <iostream>
using namespace std;
int main()
{
    int n,step;
    cin >> n;
    while(n!=1)
    {
        if(n%2==0)
            n=n/2;
         else
             n=(3*n+1)/2;
             step++;
    }
    cout << step;
    system("Pause");
    return 0;
}
```

## 1002 写出这个数

读入一个正整数 *n*，计算其各位数字之和，用汉语拼音写出和的每一位数字。

输出样例：

在一行内输出 *n* 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。

```C++
//输入：1234567890987654321123456789
//输出：yi san wuu
#include<iostream>
#include<string>
using namespace std;
int main()
{
	long long sum = 0;
	string str;
	cin >> str;
	int res = 0;
	for (int i = 0; i < str.length(); i++)
	{
		res += str[i] - '0';
	}
	//关键就是如何用拼音来表示？
	string ch[10] = { "ling","yi","er","san","si","wu","liu","qi","ba","jiu" };
	string t = to_string(res);
	cout << t << endl;
	for (int i = 0; i < t.length(); i++)
	{
		if (i != 0)
			cout << " ";
		cout << ch[t[i] - '0'];
	}
	system("Pause");
	return 0;
}
```

## 1003 我要通过！

只要读入的字符串满足下列条件，系统就输出“**答案正确**”，否则输出“**答案错误**”。

得到“**答案正确**”的条件是：

1. 字符串中必须仅有 `P`、 `A`、 `T`这三种字符，不可以包含其它字符；
2. 任意形如 `xPATx` 的字符串都可以获得“**答案正确**”，其中 `x` 或者是空字符串，或者是仅由字母 `A` 组成的字符串；
3. 如果 `aPbTc` 是正确的，那么 `aPbATca` 也是正确的，其中 `a`、 `b`、 `c` 均或者是空字符串，或者是仅由字母 `A` 组成的字符串。

```c++
//输入输出：每个测试输入包含一个测试用例。第一行给出一个正整数n(≤10)，是需要检测的字符串个数。接下来每个字符串占一行，字符串长度不超过 100，且不包含空格。
//10、PAT、PAAT、AAPATAA、AAPAATAAAA、       xPATx、PT、Whatever、APAAATAA、APT、APATTAA
//输出格式：YES、YES、YES、YES、        NO、NO NO NO NO NO
```

根据上述可以看出规律：

- 只能有P、A、T三个字母，不能有以外的字母
- 如果P和T之间有一个A，那么P左边和T右边的A个数相等
- 经过递归过程，因为APATA是正确的的，所以APAATAA是正确的
- 由于APAATAA是正确的，所以APAAATAAA是正确的

规律得出：P左边的A的个数乘P和T中间的A的个数，等于T右边A的个数

```C++
#include <iostream>
#include<string>
#include<map>
using namespace std;
int main()
{
    int n;
    int p=0,t=0;
    string s;
    cin >> n;
    while(n--)
    {
        map<char,int>m;
        cin>>s;
        for(int i = 0;i<s.size();i++)
        {
            m[s[i]]++;
            if(s[i]=='P')p=i;
            if(s[i]=='T')t=i;
        }
        if(m['P']==1&&m['T']==1&&m['A']!=0&&m.size()==3&&((t-p) != 1)&&(p*(t-p-1)==s.length()-t-1))
            cout << "YES" << endl;
        else
            cout << "NO" << endl;
    }
    system("Pause");
    return 0;
}
```

## 1004 成绩排名

读入 *n*（>0）名学生的姓名、学号、成绩，分别输出成绩最高和成绩最低学生的姓名和学号

每个测试输入包含 1 个测试用例，格式为

```
第 1 行：正整数 n
第 2 行：第 1 个学生的姓名 学号 成绩
第 3 行：第 2 个学生的姓名 学号 成绩
  ... ... ...
第 n+1 行：第 n 个学生的姓名 学号 成绩
其中姓名和学号均为不超过 10 个字符的字符串，成绩为 0 到 100 之间的一个整数，这里保证在一组测试用例中没有两个学生的成绩是相同的。
```

输出格式：

```
对每个测试用例输出 2 行，第 1 行是成绩最高学生的姓名和学号，第 2 行是成绩最低学生的姓名和学号，字符串间有 1 空格。
```

```C++
#include<iostream>
#include<algorithm>
#include<string>
#include<map>
#include<vector>
using namespace std;
struct Stud {
	string name;
	string id;
	int score;
};
bool compare(Stud a,Stud b)
{
    return a.score>b.score;
}
Stud stu[10000];
int main() {
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
		cin >> stu[i].name >> stu[i].id >> stu[i].score;
	sort(stu, stu + n,compare);
	cout << stu[0].name << " " << stu[0].id << endl;
	cout << stu[n-1].name << " " << stu[n - 1].id << endl;
	return 0;
}
```

## 1005  继续3n+1猜想

卡拉兹(Callatz)猜想已经在1001中给出了描述。在这个题目里，情况稍微有些复杂。

当我们验证卡拉兹猜想的时候，为了避免重复计算，可以记录下递推过程中遇到的每一个数。例如对 *n*=3 进行验证的时候，我们需要计算 3、5、8、4、2、1，则当我们对 *n*=5、8、4、2 进行验证的时候，就可以直接判定卡拉兹猜想的真伪，而不需要重复计算，因为这 4 个数已经在验证3的时候遇到过了，我们称 5、8、4、2 是被 3“覆盖”的数。我们称一个数列中的某个数 *n* 为“关键数”，如果 *n* 不能被数列中的其他数字所覆盖。

现在给定一系列待验证的数字，我们只需要验证其中的几个关键数，就可以不必再重复验证余下的数字。你的任务就是找出这些关键数字，并按从大到小的顺序输出它们。

输入格式：

每个测试输入包含 1 个测试用例，第 1 行给出一个正整数 *K* (<100)，第 2 行给出 *K* 个互不相同的待验证的正整数 *n* (1<*n*≤100)的值，数字间用空格隔开。

输出格式：

每个测试用例的输出占一行，按从大到小的顺序输出关键数字。数字间用 1 个空格隔开，但一行中最后一个数字后没有空格。

```c++
#include<iostream>
#include<algorithm>
#include<string>
#include<map>
#include<vector>
using namespace std;
int a[10000];
bool compare(int a,int b){
    return a>b;
}
int main(){
    int k,n,t=0;
    cin>>k;
    vector<int> v(k);
    for(int i=0;i<k;i++){
        cin>>n;
        v[i]=n;
        while (n!=1){
            if(n%2 != 0)
            n=3*n+1;
            n=n/2;
            if(a[n]==1)break;
            a[n]=1;
        }
    }
    sort(v.begin(),v.end(),compare);
    for(int i=0;i<v.size();i++){
        if(a[v[i]]==0){
            if(t==1)cout<<" ";
            cout<<v[i];
            t=1;
        }
    }
	system("Pause");
    return 0;
}
```

## 1006  换个格式输出整数

让我们用字母 `B` 来表示“百”、字母 `S` 表示“十”，用 `12...n` 来表示不为零的个位数字 `n`（<10），换个格式来输出任一个不超过 3 位的正整数。例如 `234` 应该被输出为 `BBSSS1234`，因为它有 2 个“百”、3 个“十”、以及个位的 4。

输入格式：

每个测试输入包含 1 个测试用例，给出正整数 *n*（<1000）。

输出格式：

每个测试用例的输出占一行，用规定的格式输出 *n*。

```c++
#include<iostream>
using namespace std;
int main()
{
    int num;
    cin>>num;
    for(int i=0;i<num/100;i++)cout<<"B";
    for(int i=0;i<num/10%10;i++)cout<<"S";
    for(int i=0;i<num%10;i++)cout<<i+1;
	system("Pause");
    return 0;
}
```

1007  素数对猜想