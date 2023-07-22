# Java 中的三种 for 循环区别


## 1. for (int i = 0; i < userList.size(); i++)

## 2. for (User user : userList)

## 3. userList.forEach(user -> {})

## 总结：

别人总结了：

1. 1w 以内数据用方式 1，for
2. 10w 以内数据用集合的迭代器，forEach
