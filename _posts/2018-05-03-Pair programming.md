---
layout: post
title: 'Pair programming'
date: 2018-05-03
author: heb1c
cover: ''
tags: test
---

> A software class work.

## 实验目的及要求
### 生成小学算术题
### 满足如下条件
* 题目避免重复
* 可定制(数量、打印方式)
* 可控制下面参数
    * 是否有乘除法
    * 整数范围
    * 除法有无余数
    * 是否支持小数
    * 有无括号
    * 加法有无负数
    * 是否支持分数
    * 打印间隔是否可调整

### 编程伙伴
* 任力 2015110435

### 代码实现

```java
#include <bits/stdc++.h>
#include<windows.h>
using namespace std;

string EXP[1000];
int expNumber = 0;
int numberRange = 0;
int numberType = 0;
int oprType = 0;
int colNumber = 0;
int rowHeight = 0;

//生成一定范围内的整数
int digitRand(int n){
    return rand()%n;
}

//生成一定范围内的小数
double doubleRand(int n){
    int i = digitRand(n);
    int d = digitRand(100);
    return i*1.0 + d/100.0;
}

//生成加减法
string oprLow(){
    string opr[] = {"+", "-"};
    return opr[digitRand(2)];

}

//生成加减乘除法
string oprAll(){
    string opr[] = {"+", "-", "*", "/"};
    return opr[digitRand(4)];

}

//生成分数
string fracRand(int n){
    ostringstream os;
    int i = digitRand(n);
    int d = digitRand(n);
    while(d == 0){
        d = digitRand(n);
    }
    os << i << '/' << d;
    return os.str();
}

//生成表达式
string createEXP(int numberRange, int numberType, int oprType){
    ostringstream os;
    int a[3] = {0, 0, 0};
    int b[2] = {0,0};
    if (numberType == 0) {}
    else if(numberType == 1) a[1] = 1;
    else a[2] = 2;

    if (oprType == 0){

    }else{
        b[1] = 1;
    }

    int numberIndex = a[digitRand(3)];
    int oprIndex = b[digitRand(2)];
    switch (numberIndex){
    case 0 :
        os << digitRand(numberRange);
        break;
    case 1:
        os << doubleRand(numberRange);
        break;
    case 2:
        os << fracRand(numberRange);
        break;
    default:
        os << 0;
        break;

    }
    switch (oprIndex){
    case 0:
        os << oprLow();
        break;
    case 1:
        os << oprAll();
        break;
    default:
        os << '+';
        break;

    }

    numberIndex = a[digitRand(3)];
    switch (numberIndex){
    case 0:
        os << digitRand(numberRange);
        break;
    case 1:
        os << doubleRand(numberRange);
        break;
    case 2:
        os << fracRand(numberRange);
        break;
    default:
        os << 0;
        break;
    }
    os << "=";
    return os.str();
}

void conT(){
    cout << "Please enter the number of EXP to be generated:";
    cin >> expNumber;
    cout << "Please enter the data range:";
    cin >> numberRange;
    cout << "Please enter the data type:(0.only integer, 1.integer&decimal, 2.integer&decimal&fraction)";
    cin >> numberType;
    cout << "Please enter the operation type:(0.only \"+,-\", 1.\"+,-,*,/\")";
    cin >> oprType;
    cout << "Please enter the number of EXP in each line:";
    cin >> colNumber;
    cout << "Please enter the number of blank line within two lines:";
    cin >> rowHeight;
}

void create(){
    for(int i = 0; i < expNumber; i++){
        EXP[i] = createEXP(numberRange, numberType, oprType);
    }
}

void print()
{
    int cnt = 0;
    for(int i = 0; i < expNumber; i++){
        cout << setw(15) << EXP[i] << "  ";
        cnt++;
        if(cnt == colNumber)
        {
            cnt = 0;
            for(int j = 0; j < rowHeight+1; j++){
                cout << "\n";
            }
        }
    }

}
int main(){
    srand((unsigned) time(NULL));
    conT();
    create();
    print();
    return 0;
}
```