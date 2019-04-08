---
title: Test
date: 2019-04-08 12:56:03
tags:
---

```cpp
#include <iostream>
#include <string>

using namespace std;

const string rem[] = {"**********新一局**********",
                      "  1.出剪子",
                      "  2.出石头",
                      "  3.出布",
                      "**************************"};
int main()
{
    //x、y分别记录甲乙的猜拳代码
    int x, y;
    cout << "2018110639_杨宇豪" << endl;
    //输出提示界面
    for (int i = 0; i < 5; ++i)
        cout << rem[i] << endl;
    cout << "请输入甲的猜拳代码：";
    cin >> x;
    cout << "请输入乙的猜拳代码：";
    cin >> y;
    //先判断甲乙平局的情况
    if (x == y)
        cout << "甲乙平局！" << endl;
    //通过规则判断甲乙的胜负
    else
        switch (x)
        {
        case 1:
            if (y == 2)
                cout << "乙获胜！" << endl;
            if (y == 3)
                cout << "甲获胜！" << endl;
            break;
        case 2:
            if (y == 1)
                cout << "甲获胜！" << endl;
            if (y == 3)
                cout << "乙获胜！" << endl;
            break;
        case 3:
            if (y == 2)
                cout << "甲获胜！" << endl;
            if (y == 1)
                cout << "乙获胜！" << endl;
            break;
        default:
            cerr << "您输入的猜拳代码有误，请重新输入！" << endl;
        }
    return 0;
}
```