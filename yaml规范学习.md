# yaml规范学习

> YAML是专门用来写配置文件的，跟JSON类似，但JSON方便

- [js-yaml 开源库](https://github.com/nodeca/js-yaml)
- [在线演示](http://nodeca.github.io/js-yaml/)
- [官方规范](http://yaml.org/spec/1.2/spec.html)

## 规则

- 大小写敏感
- 通过缩进控制，不能用tab，只能用空格，空格数量不重要，重要是对齐
- `#` 表示注释，JSON中没有注释

## 支持的数据格式

> 对象、数组、纯量（字符串、布尔、整数、浮点数、Null、时间、日期）

### 对象

```yml
# yaml 表示，时间采用iso8601
name: xiaoming
full name: test xiaoming
parent name: 'parent: xiaoming'
age: 10
isSet: true
boolStr: !!str true
boolNum: !!str 11
time: 2001-12-14t21:59:43.10-05:00
date: 2011-01-01
parent: ~
school: {name: myschool}

# js 对象
{
    name: 'xiaoming',
    'full name': 'test xiaoming',
    'parent name': 'parent: xiaoming',
    num: 10,
    isSet: true,
    boolStr: 'true',
    boolNum: '11',
    time: new Date('2001-12-14t21:59:43.10-05:00'),
    date: new Date('2011-01-01'),
    parent: null,
    school: {
        name: 'myschool'
    }
}
```

注意：
- 字符串中包含特殊字符时，需要加引号
- 

## 数组表示

- 示例一

```yml
# yaml 表示
- Cat
- Dog

# js 对象
['Cat', 'Dog']
```

- 示例二

```yml
# yaml 表示
animal: [Cat, Dog]

# js 对象
{animal: ['Cat', 'Dog']}
```

- 示例三

```yml
# yaml 表示
- 
  - Cat
  - Dog

# js 对象
[['Cat', 'Dog']]
```

## 复合结构

```yml
# yaml 表示
users:
 - xiaoming
 - liangliang
info:
 name: test

# js 对象
{
  users: ['xiaoming', 'liangliang'],
  info: {
    name: 'test'
  }
}
```

## 引用

锚点 `&` 和别名 `*` 可以用来引用

- 示例一

```yml
# yaml 表示
defaults: &defaults
  name: xiaoming
  age: 22
user:
  parent: abc
  <<: *defaults

# js 对象
{
    user: {
        name: 'xiaoming',
        age: 22
    }
}

```

注意：
1. `<<` 用来合并到当前数据
2.

- 示例二

```yml
# yaml 表示
- &common xiaoming
- lili
- *common

# js 对象
['lili', 'xiaoming']
```


