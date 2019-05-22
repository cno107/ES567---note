# JS 配列メソッド

## join( )

`**join()**` メソッドは、配列 (または[配列風オブジェクト](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Indexed_collections#Working_with_array-like_objects)) の全要素を順に連結した文字列を新たに作成して返します。区切り文字はカンマ、または指定された文字列です

```javascript
var elements = ['Fire', 'Air', 'Water'];
console.log(elements.join('-'));
// expected output: "Fire-Air-Water"         String
```

## concat( )

contcat( ) 数组拼窜 返回Arrary

```javascript
var new_array = old_array.concat([value1[, value2[, ...[, valueN]]]])
```

## push( )    pop( )

`**push()**` メソッドは、配列の**末尾**に **1 つ以上**の要素を**追加**することができます。また戻り値として新しい配列の要素数を返します

`**pop()**` メソッドは、配列から**最後**の要素を**取り除き**、その要素を返します。このメソッドは配列の長さを変化させます

## shift( )     unshift( )

`**shift()**` メソッドは、配列から**最初**の要素を**取り除き**、その要素を返します。このメソッドは配列の長さを変えます

```javascript
var array1 = [1, 2, 3];
var firstElement = array1.shift();
console.log(array1);
// expected output: Array [2, 3]
```

`**unshift()**` メソッドは、配列の**最初**に **1 つ以上**の要素を**追加**し、新しい配列の長さを返します。

```javascript
var array1 = [1, 2, 3];
console.log(array1.unshift(4, 5));
// expected output: 5
console.log(array1);
// expected output: Array [4, 5, 1, 2, 3]
```

## sort( )

```typedarray.sort([compareFunction])``` 函数里面有两个参  arr里的第n和n+1位

```javascript
let arr = [1,4,7,2,1,4,3,5];

arr.sort(function (a,b) {

    return a > b;  // a - b   升
  //return a < b ;    b - a   降
})
console.log(arr);
// a > b 如果为true 则换位置 if (a - b) > 0 为true 换位置
```

## reverse( )

反转数组

## slice( )

`**slice()**` メソッドは `begin` から `end` まで選択された配列の一部をシャローコピーして、新しい配列オブジェクトを返します (`end` は含まれません)。元の配列は変更されません

```javascript
let arr = ['a','b','c','d','e','f','g'];

let newArr = arr.slice(1,-2);

console.log(newArr);  //[ 'b', 'c', 'd', 'e' ]   包括第一个 不包括倒数第二个 且index从0开始
```

## splice( )

**splice()** メソッドは、古い要素を取り除きつつ新しい要素を追加することで、配列の内容を変更します

```javascript
//三个以上参数   开始位置 个数  替换为 ……
let arr = ['a','b','c','d','e','f','g'];

arr.splice(1,4,'?','&');   // [ 'a', '?', '&', 'f', 'g' ]   负数无效 从0开始
```

## indexOf   lastindexOf( )

+ Array.prototype.indexOf(value) : 得到值在数组中的第一个下标
+ Array.prototype.lastIndexOf(value) : 得到值在数组中的最后一个下标

```javascript
var arr = [1,2,3,6,4,5,6,7,8,9,6];
console.log(arr.indexOf(6));
console.log(arr.lastIndexOf(6));
```

## forEach( )

Array.prototype.forEach(function(item, index){}) : 遍历数组

```javascript
console.log(arr.forEach(function (item,index) {
  console.log(item+'~'+index)
}));
```

## map ( )

Array.prototype.map(function(item, index){}) : 遍历数组返回一个新的数组，返回加工之后的值

```javascript
var cno = arr.map(function (item,index) {
  return   item + 10 ;
})
```

## filter( )

## every( )

**every()** メソッドは、与えられた関数によって実行されるテストに、配列のすべての要素が通るかどうかをテストします

```javascript
let arr = [1,2,3,4,5,999];

let tf = arr.every(function (value) { //三个参  value index arr
    return value < 100
});

console.log(tf);  //false
```

## some( )

## reduce( )     reduceRight( )

# Array extend

- Array.from(v) : 将伪数组对象或可遍历对象转换为真数组
- Array.of(v1, v2, v3) : 将一系列值转换成数组
- find(function(value, index, arr){return true}) : 找出第一个满足条件返回true的元素

​      可以定义一个变量来接受这个return

- findIndex(function(value, index, arr){return true}) : 找出第一个满足条件返回true的元素下标

