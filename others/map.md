>map初始化

```c++
int num[] = {2,7,11,15};
vector<int> nums(num,num+sizeof(num)/sizeof(num[0]));
map<int,int>a;
for(int i = 0 ; i < nums.size() ;i++){
	// cout << nums[i] << " ";
	a.insert(map<int,int>::value_type(nums[i],i));
}
for(auto it:a){
	cout << it.first << " " << it.second << endl;
}
//a.count(值)返回值为1，0，1代表有这个数，0无
```

```c++

```