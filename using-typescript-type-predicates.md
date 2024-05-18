---
pubDate: 2023-07-17
updatedDate: 2023-07-17
title: "template"
description: "A template"
featured: false
draft: true
topics: []
---

Imagine this scenario. In Typescript, we have an array that contains some strings and some undefined values. 

```typescript
const rawFruitData: (string | undefined)[] = ["banana", undefined, "apple"]
```

Now I want to pull out just the fruit strings and remove the undefined from the raw data like so:

```typescript
const fruits: string[] = rawFruitData.filter((fruit) => fruit)
```

However, when I try to do this in Typescript I get this error:
```
Type '(string | undefined)[]' is not assignable to type 'string[]'.
    Type 'string | undefined' is not assignable to type 'string'.
    	Type 'undefined' is not assignable to type 'string'.(2322)
```

I ran into this problem recently and I used a [Type Predicate](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates) to help narrow the type. 

We can define the Type Predicate like so:

```typescript
function isFruit(fruit: string | undefined): fruit is string {
	return fruit !== undefined
}
```

`fruit is string` is the type predicate, and when `isFruit` is called on a variable, that variable type will be narrowed to `string`. `isFruit` is a user defined type guard. I can use the `isFruit` method in my filter and get any type errors:

```typescript
const fruits: string[] = rawFruitData.filter(isFruit);
console.log(fruits) // => ["banana", "apple"]
```