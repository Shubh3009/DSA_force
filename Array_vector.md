<img width="1239" height="681" alt="image" src="https://github.com/user-attachments/assets/66ccdc1d-c051-4459-92a5-6495544f0962" />

<img width="994" height="599" alt="image" src="https://github.com/user-attachments/assets/6af2b7df-8f71-459e-99ba-9ae71ebf4fc3" />

<img width="1335" height="694" alt="image" src="https://github.com/user-attachments/assets/f19111f9-03f5-4484-9876-e4d41e7f7b5b" />

<img width="1208" height="712" alt="image" src="https://github.com/user-attachments/assets/9475ce69-e5b8-4317-bd7d-a48caf0a4023" />

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
        

} 

```
