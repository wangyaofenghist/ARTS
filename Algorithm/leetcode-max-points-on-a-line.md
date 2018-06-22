Leetcode:max-points-on-a-line

##题目描述

Given *n* points on a 2D plane, find the maximum number of points that lie on the same straight line.

提供在2D平面上的n个点，查找在同一条直线上的最多的点的数量

分析

散布在一整纸上的n个点，两两之间都能确定一条直线，但是如何判定第三个点是否在这个直线上？

换一个角度，将两个点的直线作为一个直角三角形的斜边，以两个点任意一点作为起点，延伸直线，上面的点都可以形成相似的直角三角形，也就是说 A，B构成一个直线，如果 (A.x-B.x)/(A.y-B.y) 的值与 (A.x-C.x)/(A.y-C.y) 的值相同，那么C一定在AB所构成的直线上，同时如果 A == D 则D点一定在所有A所在的直线上

### 考察点

细节转换、穷举

### 代码

```
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class Solution {
public:
    /*
    需要两重循环，第一重循环遍历起始点a，第二重循环遍历剩余点b。
    a和b如果不重合，就可以确定一条直线。
    对于每个点a，构建 斜率->点数 的map。
    (1)b与a重合，以a起始的所有直线点数+1 (用dup统一相加)
    (2)b与a不重合，a与b确定的直线点数+1
    */
    int maxPoints(vector<Point> &points) {
        int size = points.size();
        if(size==0){
            return 0;
        }
        int ret = 0;
        for(int i=0;i<size;i++){
            int curNum = 1;
            int vcnt = 0;
            int dup = 0;
            map<double,int> mp;
            for(int j=0;j<size;j++){
                if(i==j)
                    continue;
                double x1 = points[i].x - points[j].x;
                double y1 = points[i].y - points[j].y;
                if(x1 ==0 && y1==0){
                    dup++;
                }else if(x1==0) {
                    if(vcnt==0){
                        vcnt = 2;
                    }else{
                        vcnt++;
                    }
                    curNum = max(curNum,vcnt);
                }else{
                    double tmp = y1/x1;
                    if(mp[tmp]==0){
                        mp[tmp] = 2;
                    }else{
                        mp[tmp] ++;
                    }
                    curNum = max(curNum,mp[tmp]);
                }
            }
            ret = max(ret,curNum+dup);
        }
        return ret;
    }
};
```

