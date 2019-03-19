---
layout: post
title:  "353. Design Snake Game"
date: 2019-03-17 20:55:23 -0400
categories: articles
---

# My try
```c++
class SnakeGame {
public:
    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    void render_map(){
        for (auto i : snake_body) {
            mymap[i.first][i.second] = 'S';
        }
    }
    
    void render_food(){
        pair<int,int> f = _food[foodid];
        mymap[f.first][f.second] = 'F';
    }
    SnakeGame(int width, int height, vector<pair<int, int>> food) {
        mymap(height, vector<int>(width, ''));
        snake_body.push_back(make_pair(0,0));
        _food = food;
        renderMap();
        
    }
    
    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body. */
    int move(string direction) {
        
    }
    
    vector<vector<char>> mymap;
    deque<pair<int, int>> snake_body;
    vector<pair<int, int>> _food;
    int foodidx = 0;
};
```
# Answer
```c++
class SnakeGame {
public:
    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    SnakeGame(int width, int height, vector<pair<int, int>> food) :
        m_width(width),
        m_height(height),
        m_food(food)
    {
        snake.push({ 0, 0 });
        hashedSnake.emplace(0);
    }

    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down
        @return The game's score after the move. Return -1 if game over.
        Game over when snake crosses the screen boundary or bites its body. */
    int move(string direction) {
        auto dir = dirs[direction];
        auto tail = snake.front();
        auto head = snake.back();
        auto newHead = pair<int, int>(head.first + dir.first, head.second + dir.second);
        if (newHead.first < 0 || newHead.first >= m_height || newHead.second < 0 || newHead.second >= m_width)
        {
            // out of bound
            return -1;
        }
        if (hashedSnake.count(newHead.first * m_width + newHead.second) && newHead != tail)
        {
            // collide
            // if newHead is tail, there is no collide
            return -1;
        }
        if (foodIndex < m_food.size() && newHead == m_food[foodIndex])
        {
            // if newHead is food, move to newHead, do not remove tail
            snake.push(newHead);

            // move to next food
            foodIndex++;
        }
        else
        {
            // no more food, or next point is not food, move to newHead and remove tail
            hashedSnake.erase(tail.first*m_width + tail.second);
            snake.pop();
            snake.push(newHead);
        }
        hashedSnake.emplace(newHead.first * m_width + newHead.second);
        return snake.size() - 1;
    }

private:
    vector<pair<int, int>> m_food;
    int foodIndex = 0;
    queue<pair<int, int>> snake; // front of queue is the tail (point to be removed), back of queue is the head (point to be added)
    unordered_set<int> hashedSnake;
    unordered_map<string, pair<int, int>> dirs
    {
        {"U", {-1, 0}},
        {"D", {1, 0}},
        {"L", {0, -1}},
        {"R", {0, 1}},
    };
    int m_width, m_height;
};
```