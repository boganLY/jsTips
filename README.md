# 1. % 的用法

```javascript
  const users = [
  '付小小',
  '曲丽丽',
  '林东东',
  '周星星',
  '吴加好',
  ];
  for(var i = 0; i < 50; i++){
    let user = users[i % users.length];
    console.log(user); // 付小小 曲丽丽 林东东 周星星 吴加好 付小小....
  }
```
- `x % y` 得到的数字最大不会超过 `y`,可以循环输出数组里的内容。
