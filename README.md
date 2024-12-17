# TypeScript NoteBook

## 常见类型操作符
`in`--常用于**遍历**类型的属性，通常和映射类型一同使用 
```
type ReadOnly<T> = {
    readonly [P in keyof T]: T[P];
};
```
`keyof`--获取一个对象类型的所有键，作为**联合类型**返回  
```
const people = { a: 1, b: 2 };
type People = keyof typeof obj;// "a" | "b"
```

`typeof`--获取一个变量的类型  
```
const s = "abc"
type T = typeof s;// string
const people = { a: 1, b: 2 };
type People = typeof people;// { a: number; b: number; }
```


`extends`--表示子类型和父类型的继承关系，通常用在**泛型约束**以及条件**类型判断**上
```
type A = {
    a: string;
    b: number;
}
type B = {
    a: string;
}
type C = A extends B ? true : false;
```



## 常见内置工具类型
`Partial<T>` -- 将对象类型的所有属性设置为可选  `T|undefined`  

`Required<T>` -- 将对象类型的所有属性设置为必填  

`Readonly<T>` -- 将对象类型的所有属性设置为只读  

`Record<K, V>` -- 创建一个类型，其中所有的属性的key为K类型，value为V类型  
```
type PersonMap = Record<string, Person>;
const people: PersonMap = {
    tom: { name: 'Tom', age: 25 },
    jerry: { name: 'Jerry', age: 30 }
};
```
`Exclude<T, U>` -- 从T类型中移除U类型  

`Extract<T, U>` -- 从T类型中提取U类型  

`NonNullable<T>` -- 从T类型中移除null和undefined类型  

`ReturnType<T>` -- 获取函数类型T的返回值类型  
```
type Fn = (x: number) => string;
type Result = ReturnType<Fn>;  // string
```
`Parameters<T>` -- 获取函数类型T的参数类型**数组**



## 经验
`访问对象的key`----[k in keyof K]
`访问数组的值` ----[val in A[number]]