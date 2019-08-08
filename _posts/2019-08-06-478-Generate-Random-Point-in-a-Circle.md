---
layout: post
title:  "478. Generate Random Point in a Circle"
date: 2019-08-06 19:50:00 -0400
categories: articles
---
Given the radius and x-y positions of the center of a circle, write a function randPoint which generates a uniform random point in the circle.

Note:

input and output values are in floating-point.
radius and x-y position of the center of the circle is passed into the class constructor.
a point on the circumference of the circle is considered to be in the circle.
randPoint returns a size 2 array containing x-position and y-position of the random point, in that order.
Example 1:
```
Input: 
["Solution","randPoint","randPoint","randPoint"]
[[1,0,0],[],[],[]]
Output: [null,[-0.72939,-0.65505],[-0.78502,-0.28626],[-0.83119,-0.19803]]
```
Example 2:
```
Input: 
["Solution","randPoint","randPoint","randPoint"]
[[10,5,-7.5],[],[],[]]
Output: [null,[11.52438,-8.33273],[2.46992,-16.21705],[11.13430,-12.42337]]
```
Explanation of Input Syntax:
```
The input is two lists: the subroutines called and their arguments. Solution's constructor has three arguments, the radius, x-position of the center, and y-position of the center of the circle. randPoint has no arguments. Arguments are always wrapped with a list, even if there aren't any.
```
```c++
class Solution {
public:
    Solution(double radius, double x_center, double y_center) {
        r = radius; centerX = x_center; centerY = y_center;
    }
    
    vector<double> randPoint() {
        double theta = 2 * M_PI * ((double)rand() / RAND_MAX);
        double len = sqrt((double)rand() / RAND_MAX) * r;
        return {centerX + len * cos(theta), centerY + len * sin(theta)};
    }

private:
    double r, centerX, centerY;
};

```
```c++
class Solution {
public:
    double rad, xc, yc;
    //c++11 random floating point number generation
    mt19937 rng{random_device{}()};
    uniform_real_distribution<double> uni{0, 1};

    Solution(double radius, double x_center, double y_center) {
        rad = radius, xc = x_center, yc = y_center;
    }

    vector<double> randPoint() {
        double x0 = xc - rad;
        double y0 = yc - rad;

        while(true) {
            double xg = x0 + uni(rng) * 2 * rad;
            double yg = y0 + uni(rng) * 2 * rad;
            if (sqrt(pow((xg - xc), 2) + pow((yg - yc), 2)) <= rad)
                return {xg, yg};
        }
    }
};
```
```c++
class Solution {
public:
    double radius;
    double x_center;
    double y_center;
    
    Solution(double radius, double x_center, double y_center) {
        this->radius = radius;
        this->x_center = x_center;
        this->y_center = y_center;
    }
    
    vector<double> randPoint() {
        double x;
        double y;
        
        do {
            x = (2 * ((double)rand() / RAND_MAX) - 1.0) * radius;
            y = (2 * ((double)rand() / RAND_MAX) - 1.0) * radius;
        } while (x * x + y * y > radius * radius);
        
        return { x_center + x, y_center + y };
    }
};
```