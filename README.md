# TypeScript NoteBook 

## 几个特殊类型
`never`--表示永不发生的值，函数报错时就可以返回never类型
`any`
`unknown`--未知类型，不能直接访问其属性或方法，需进行typeof类型检查
`void`--函数没有返回值


## 常见类型操作符

`in`--常用于**遍历**类型的属性，通常和映射类型一同使用 
```typeScript
type ReadOnly<T> = {
    readonly [P in keyof T]: T[P];
};
```

`keyof`--获取一个对象类型的所有键，作为**联合类型**返回  
```typeScript
const people = { a: 1, b: 2 };
type People = keyof typeof obj;// "a" | "b"
```

`typeof`--获取一个变量的类型  
```typeScript
const s = "abc"
type T = typeof s;// string
const people = { a: 1, b: 2 };
type People = typeof people;// { a: number; b: number; }
```

`infer` --类型推断操作符
```typeScript
type ReturnTypeOf<T> = T extends (...args: any[]) => infer R ? R:never;
type ReturnTypeOf<T> = T extends (...args: any[]) => R ? R : never;// 错！ R类型未知

type ElementType<T> = T extends (infer U)[] ? U : never;
type ElementType<T> = T[number] // OK

// infer 的最好例子： 实现TS内置的Parameters<T>类型
type myParameters<T extends (...args)=>any> 
= T extends (...any: S)=>any ? S : never
```


`extends`--表示子类型和父类型的继承关系，通常用在**泛型约束**以及条件**类型判断**上
```typeScript
type A = {
    a: string;
    b: number;
}
type B = {
    a: string;
}
type C = A extends B ? true : false;
```

`as`-- 在映射类型中用于重新映射键名称
```typescript
type MyOmit<T, K> = {
  [t in keyof T as t extends K ? never : t]: T[t]
}
```


## 常见内置工具类型

`Partial<T>` -- 将**对象类型**的所有属性设置为可选  `T|undefined`  

`Required<T>` -- 将**对象类型**的所有属性设置为必填  

`Readonly<T>` -- 将**对象类型**的所有属性设置为只读  

`Omit<T, K>` -- 从**对象类型**T中排除某些属性K后，返回新类型

`Pick<T, K>` -- 从**对象类型**T中挑选属性K，返回新类型

----------------


`Exclude<T, U>` -- 从**联合类型**T中移除U类型  

`Extract<T, U>` -- 从**联合类型**T中提取U类型  

`NonNullable<T>` -- 从**联合类型**T中移除null和undefined类型  

`ReturnType<T>` -- 获取**函数类型**T的返回值类型  

`Parameters<T>` -- 获取**函数类型**T的参数类型数组

-----------------

`Record<K, V>` -- 创建一个类型，其中所有的属性的key为K类型，value为V类型  



  

## 经验  

### 数组/对象的遍历访问
`访问对象的key`----[k in keyof K]
`访问数组的值` ----[val in A[number]]
```typescript
type Includes<T extends readonly any[], U> =
  { [P in T[number]]: true }[U] extends true ? true : false
```


### 数组类型的空值判定 extends、never
```typeScript
T[number] extends [] ?
T['length'] extends 0 ?
T extends [] ?
```

### 数组类型也可以用拓展运算符
```typeScript
type Concat<T extends any[], U extends any[]> = [...T, ...U]
```

### not a object
```typeScript
type NotObject<T> = keyof T extends never ? true : false;
```