---
layout: post
title: Json Schema
subtitle: Json Schema
author: oliver
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - Json Schema
---
##### 参考文献链接
[JSON Schema](http://imweb.io/topic/56b1b4bb5c49f9d377ed8ee9)


```
{
      "name": "George Washington",
      "birthday": "February 22, 1732",
      "address": "Mount Vernon, Virginia, United States"
  }
```
如何描述上面 JSON 对象呢？

首先，它是一个 object；

其次，它拥有 name、birthday、address 这三个字段

并且，name 、address 的字段值是一个字符串 String，birthday 的值是一个日期。

最后，将上面的信息如何用 JSON 来表示？如下：


```
{
      "type": "object",
      "properties": {
           "name": { "type": "string" },
           "birthday": { "type": "string", "format": "date" },
           "address": { "type": "string" }
      }
  }
```
## 二、JSON Schema 的举例
1. 空 schema，所有都合法。
2. type指定JSON数据类型

```
{ "type": "string" }
```
3. type可以包含多个类型

```
{ "type": ["number", "string"] }

"I'm a string" // 合法

42  // 合法

["Life", "the universe", "and everything"] // 不合法
```

4. string限定长度

```
{
    "type": "string",
    "minLength": 2,
    "maxLength": 3
}


"AA" // 合法
"AAA" // 合法
"A"  // 不合法
"AAAA" // 不合法
```

5. string模式匹配

```
{
  "type": "string",
  "pattern": "^(\\([0-9]{3}\\))?[0-9]{3}-[0-9]{4}$"
}

"555-1212" // ok

"(888)555-1212" // ok

"(888)555-1212 ext. 532" // not ok

"(800)FLOWERS" // not ok
```
6.string值的枚举


```
{
    "type": "string",
    "enum": ["red", "amber", "green"]
}

"red" // ok

"blue" // not ok: blue 没有在 enum 枚举项中
```

7. Integer
integer 一定是整数类型的 number

```
{ "type": "integer" }

42 // ok
1024 // ok
```
8. multipleOf 数字倍数

```
{ "type": "number", "multipleOf": 2.0 }  //2的倍数的number

42 // ok
21 // not ok
```
9. number限定范围

```
{
    "type": "number",
    "minimum": 0,
    "maximum": 100,
    "exclusiveMaximum": true
}
//exclusiveMaximum 为 true 表示包含边界值 maximum，类似的还有 exclusiveMinimum 字段.
```
10. object 不允许有额外的字段
```
{
    "type": "object",
    "properties": {
        "number": { "type": "number" },
        "street_name": { "type": "string" },
        "street_type": { 
             "type": "string",
             "enum": ["Street", "Avenue", "Boulevard"]
        }
    },
    "additionalProperties": false
}

{ "number": 1600, "street_name": "Pennsylvania", "street_type": "Avenue" } // ok
{ "number": 1600, "street_name": "Pennsylvania", "street_type": "Avenue","direction": "NW" } // not ok

//因为包含了额外的字段 direction，而 schema 规定了不允许额外的字段 "additionalProperties": false
```
11. object 允许有额外的字段，并限定类型
```
{
    "type": "object",
    "properties": {
    "number": { "type": "number" },
    "street_name": { "type": "string" },
    "street_type": { 
        "type": "string",
        "enum": ["Street", "Avenue", "Boulevard"]
    }
    },
    "additionalProperties": { "type": "string" }
}


{ "number": 1600, "street_name": "Pennsylvania", "street_type": "Avenue","direction": "NW" } // ok
{ "number": 1600, "street_name": "Pennsylvania", "street_type": "Avenue", "office_number": 201 } // not ok

//额外字段 `"office_number": 201` 是 number 类型，不符合 schema
```
12. object 必填字段

```
{
     "type": "object",
     "properties": {
          "name": { "type": "string" },
          "email": { "type": "string" },
          "address": { "type": "string" },
          "telephone": { "type": "string" }
     },
     "required": ["name", "email"]
}


// ok
{
  "name": "William Shakespeare",
  "email": "bill@stratford-upon-avon.co.uk"
}

多出字段也是 ok 的
// ok
{
    "name": "William Shakespeare",
    "email": "bill@stratford-upon-avon.co.uk",
    "address": "Henley Street, Stratford-upon-Avon, Warwickshire, England",
    "authorship": "in question"
}

少了字段，就是不行
// not ok
{
    "name": "William Shakespeare",
    "address": "Henley Street, Stratford-upon-Avon, Warwickshire, England",
}
```
13. object 指定属性个数

```
{
    "type": "object",
    "minProperties": 2,
    "maxProperties": 3
}


{ "a": 0, "b": 1 } // ok
{ "a": 0, "b": 1, "c": 2, "d": 3 } // not ok
```

15. Object 属性的模式匹配

```
{
    "type": "object",
    "patternProperties": {
         "^S_": { "type": "string" },
         "^I_": { "type": "integer" }
    },
    "additionalProperties": false
}

{ "S_25": "This is a string" } // ok
{ "I_0": 42 } // ok

// not ok
{ "I_42": "This is a string" }
{ "keyword": "value" }
```

16. array 数组

```
// ok
{ "type": "array" }
[1, 2, 3, 4, 5]
[3, "different", { "types" : "of values" }]

// not ok:
{"Not": "an array"}
```

17. array 指定数组成员类型

```
{
    "type": "array",
    "items": {
        "type": "number"
    }
}
[1, 2, 3, 4, 5] // ok
[1, 2, "3", 4, 5] // not ok
```

18. array 指定数组成员类型，逐个指定

```
{
"type": "array",
     "items": [{
          "type": "number"
          },{
          "type": "string"
          },{
          "type": "string",
          "enum": ["Street", "Avenue", "Boulevard"]
          },{
          "type": "string",
          "enum": ["NW", "NE", "SW", "SE"]
     }]
}
// ok
[1600, "Pennsylvania", "Avenue", "NW"]

[10, "Downing", "Street"] // 缺失一个也是可以的

[1600, "Pennsylvania", "Avenue", "NW", "Washington"] // 多出一个也是可以的
// not ok
[24, "Sussex", "Drive"]
["Palais de l'Élysée"]
```

19. array 指定数组成员类型，逐个指定，严格限定

```
{
    "type": "array",
    "items": [{
        "type": "number"
        },
        {
        "type": "string"
        },
        {
        "type": "string",
        "enum": ["Street", "Avenue", "Boulevard"]
        },
        {
        "type": "string",
        "enum": ["NW", "NE", "SW", "SE"]
        }
    ],
    "additionalItems": false
}
[1600, "Pennsylvania", "Avenue", "NW"] // ok

[1600, "Pennsylvania", "Avenue"] // ok

[1600, "Pennsylvania", "Avenue", "NW", "Washington"] // not ok 多出了字段就是不行
```

20. array 数组长度限制

```
{
   "type": "array",
   "minItems": 2,
   "maxItems": 3
}
[1, 2] // ok

[1, 2, 3, 4] // not ok
```

21. array element uniqueness 数组元素的唯一性

```
{
    "type": "array",
    "uniqueItems": true
}
[1, 2, 3, 4, 5] // ok
[1, 2, 3, 3, 4] // not ok:出现了重复的元素 3
```

22. boolean

```
{ "type": "boolean" }
true // ok
0 // not ok
```

23. null

```
{ "type": "null" }
null // ok

"" // not ok
```

24. schema 的合并

```
string 类型，最大长度为 5 ；或 number 类型，最小值为 0

{
    "anyOf": [
       { "type": "string", "maxLength": 5 },
       { "type": "number", "minimum": 0 }
    ]
}

`anyOf` 包含了两条规则，符合任意一条即可
"short"  // ok
42 // ok
"too long" // not ok 长度超过 5 
-5 // not ok 小于了 0
```

25. allOf、oneOf
 
```
`anyOf` 是满足任意一个 Schema 即可，而 `allOf` 是要满足所有 Schema
  `oneOf` 是满足且只满足一个
```

26. oneOf

```
{
    "oneOf": [
        { "type": "number", "multipleOf": 5 },
        { "type": "number", "multipleOf": 3 }
    ]
}
10 // ok
15 // not ok 因为它既是 3 又是 5 的倍数
上面的 schema 也可以写为：

{
    "type": "number",
    "oneOf": [
        { "multipleOf": 5 },
        { "multipleOf": 3 }
    ]
}
```

27. not

```
{ "not": { "type": "string" } }
只要是非 string 类型即可

42 // ok
{"key" : "value"} // ok
"This is a string" // not ok
```
