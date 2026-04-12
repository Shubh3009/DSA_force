<img width="1239" height="681" alt="image" src="https://github.com/user-attachments/assets/66ccdc1d-c051-4459-92a5-6495544f0962" />

<img width="994" height="599" alt="image" src="https://github.com/user-attachments/assets/6af2b7df-8f71-459e-99ba-9ae71ebf4fc3" />

<img width="1335" height="694" alt="image" src="https://github.com/user-attachments/assets/f19111f9-03f5-4484-9876-e4d41e7f7b5b" />

<img width="1208" height="712" alt="image" src="https://github.com/user-attachments/assets/9475ce69-e5b8-4317-bd7d-a48caf0a4023" />

### Basic of Vector

```cpp

#include<bits/stdc++.h>
using namespace std;

int main(){
    vector<int>arr={1,2,3,4,5}

    cout<<arr.size()<<endl;    // size of the vector (number of element) -5

    cout<<arr.capacity()<<endl;   // capacity of vector same as of initialisation - 5

    arr.push_back(6);   // put element at O(1) time


    cout<<arr.size()<<endl;    // size of the vector (number of element) - 6

    cout<<arr.capacity()<<endl;   // it doubles its size to accomodate more element - 10


    //print all element of array
    for (int i=0;i<arr.size();i++){
            cout<<arr[i]<<endl;
    }
        
    // fill constructor

    vector<int>arr(8,3)                 // op= {3,3,3,3,3,3,3,3}
    vector<int>vis(100,9)    
} 


https://leetcode.com/problems/two-sum/description/


___________________________________________________________________________________________________________________________________________________

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

 

Example 1:

Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
Example 2:

Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
Example 3:

Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
 

Constraints:

3 <= nums.length <= 3000
-105 <= nums[i] <= 105
 

```
