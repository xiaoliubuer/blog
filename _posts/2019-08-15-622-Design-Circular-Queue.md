---
layout: post
title:  "622. Design Circular Queue"
date: 2019-08-15 08:18:00 -0400
categories: articles
---	
Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Your implementation should support following operations:

MyCircularQueue(k): Constructor, set the size of the queue to be k.
Front: Get the front item from the queue. If the queue is empty, return -1.
Rear: Get the last item from the queue. If the queue is empty, return -1.
enQueue(value): Insert an element into the circular queue. Return true if the operation is successful.
deQueue(): Delete an element from the circular queue. Return true if the operation is successful.
isEmpty(): Checks whether the circular queue is empty or not.
isFull(): Checks whether the circular queue is full or not.
 

Example:
```
MyCircularQueue circularQueue = new MyCircularQueue(3); // set the size to be 3
circularQueue.enQueue(1);  // return true
circularQueue.enQueue(2);  // return true
circularQueue.enQueue(3);  // return true
circularQueue.enQueue(4);  // return false, the queue is full
circularQueue.Rear();  // return 3
circularQueue.isFull();  // return true
circularQueue.deQueue();  // return true
circularQueue.enQueue(4);  // return true
circularQueue.Rear();  // return 4
```
Note:
```
All values will be in the range of [0, 1000].
The number of operations will be in the range of [1, 1000].
Please do not use the built-in Queue library.
```

```c++
class MyCircularQueue {
  private:
    // sotre elements
    std::vector<int> data;
    // pointers
    int head, tail;
    // size
    int size;

  public:
    /** Initialize your data structure here. Set the size of the queue to be k. */
    MyCircularQueue(int k) {
        size = k;
        data.reserve(size); // set the size of the queue
        head = tail = -1; // initial the value of pointers
    }
    
    /** Insert an element into the circular queue. Return true if the operation is successful. */
    bool enQueue(int value) {
        if (isFull()) return false;

        if (isEmpty()) head = (head + 1) % size;
        tail = (tail + 1) % size;

        data[tail] = value;

        return true;
    }
    
    /** Delete an element from the circular queue. Return true if the operation is successful. */
    bool deQueue() {
        if (isEmpty()) return false;

        if (head == tail) // deQueue the last element
            head = tail = -1; // running out of elements
        else
            head = (head + 1) % size;

        return true;
    }
    
    /** Get the front item from the queue. */
    int Front() {
        if (isEmpty()) return -1;
        else return data[head];
    }
    
    /** Get the last item from the queue. */
    int Rear() {
        if (isEmpty()) return -1;
        else return data[tail];
    }
    
    /** Checks whether the circular queue is empty or not. */
    bool isEmpty() {
        return (tail == -1 && head == -1) ? true : false;
    }
    
    /** Checks whether the circular queue is full or not. */
    bool isFull() {
        return (((tail + 1) % size) == head) ? true : false;
    }

    /** Debug. */
    void debug() {
        std::cout << "head: " << head << "; tail: " << tail << std::endl;
    }
};
```

```c++
class MyCircularQueue
{
private:
    vector<int> data;
    int maxSize;
    int front;
    int rear;

public:
    /** Initialize your data structure here. Set the size of the queue to be k. */
    MyCircularQueue(int k)
    {
        front = rear = -1;
        maxSize = k;
        //data.reserve(k);
        data.resize(k);
    }

    /** Insert an element into the circular queue. Return true if the operation is successful. */
    bool enQueue(int value)
    {
        if(isFull())
            return false;

        if(isEmpty())
        {
            front = rear = 0;
            data[rear] = value;
        }
        else if(rear == maxSize-1)
        {
            rear = 0;
            data[rear] = value;
        }
        else
        {
            rear++;
            data[rear] = value;
        }

        return true;
    }

    /** Delete an element from the circular queue. Return true if the operation is successful. */
    bool deQueue()
    {
        if(isEmpty())
            return false;

        if(front == rear)
        {
            front = rear = -1;
        }
        else if(front == maxSize-1)
        {
            front = 0;
        }
        else
        {
            front++;
        }

        return true;
    }

    /** Get the front item from the queue. */
    int Front()
    {
        if(isEmpty())
            return -1;

        return data[front];
    }

    /** Get the last item from the queue. */
    int Rear()
    {
        if(isEmpty())
            return -1;

        return data[rear];
    }

    /** Checks whether the circular queue is empty or not. */
    bool isEmpty()
    {
        return (front == -1) and (rear == -1);
    }

    /** Checks whether the circular queue is full or not. */
    bool isFull()
    {
        return ((rear+1) % maxSize == front);
    }
};
```

```c++
class MyCircularQueue {
public:
    MyCircularQueue(int k): size(0), maxSize(k), i(0), j(-1), q(vector<int>(k, 0)) {}
    
    bool enQueue(int value) {
        if(size == maxSize) return false;
        if(++j == maxSize) j = 0;
        q[j] = value;
        ++size;
        return true;
    }
    
    bool deQueue() {
        if(size == 0) return false;
        if(++i == maxSize) i = 0;
        --size;
        return true;
    }
    
    int Front() {
        return size == 0 ? -1 : q[i];
    }
    
    int Rear() {
        return size == 0 ? -1 : q[j];
    }
    
    bool isEmpty() {
        return size == 0;
    }
    
    bool isFull() {
        return size == maxSize;
    }
    
    int size;
    const int maxSize;
    int i;
    int j;
    vector<int> q;
};
```
```c++
class MyCircularQueue
{
	public: 
		MyCircularQueue(int Maxsize)
		{
			h_idx = 0;
			m_num = 0;
			
			if(Maxsize > 0)
			{
				array = new int[Maxsize];
				m_arraysize = Maxsize;
			}
		}
		
		bool deQueue()
		{
			if(m_num > 0)
			{
				h_idx = (h_idx + 1) % m_arraysize;
				m_num--;
				return true;
			}
			return false;
		}
		
		bool enQueue(int value)
		{
			if(m_num < m_arraysize)
			{
                //t_idx = (t_idx + 1) % m_arraysize;
				//array[t_idx] = value;
				//t_idx = (t_idx + 1) % m_arraysize;
				//m_num++;
                array[(h_idx+m_num)%m_arraysize] = value;
                m_num++;
                
                return true;
			}
			return false;
		}
		
		int Front()
		{
			if(m_num>0)
			{
				return array[h_idx];
			}
			else
			{
				return -1; // return -1 if Queue is empty
			}
		}
		
		int Rear()
		{
			if(m_num>0)
			{
				//return array[t_idx];
                return array[(h_idx+m_num-1)%m_arraysize];
			}
			else
			{
				return -1; // return -1 if Queue is empty
			}
		}
		
		/** Checks whether the circular queue is empty or not. */
		bool isEmpty() 
		{
			return (m_num == 0);	
		}
    
		/** Checks whether the circular queue is full or not. */
		bool isFull() 
		{
			return (m_num == m_arraysize);
		}
		
		/*int size()
		{
			return m_num;
		}*/
	
	
	
	private:
		int h_idx;	  //index to head. in [0, MAX-1]
		int m_num;    //num of valid elements in queue
		int m_arraysize;
        int* array;

};
```