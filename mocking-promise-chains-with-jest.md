---
pubDate: 2023-04-05
updatedDate: 2023-04-05
title: "Mocking Promise chains with Jest"
description: "A template"
featured: false
draft: true
---

I ran into some mocking woes at work recently. It took me a few days to work out how to mock a class that chains `async` functions. If you're looking to do the same thing, hopefully this post saves you the trouble! First, let's setup the scenario.

Let's say you have a class that "does something". The `doesSomething` method will call a creator class using `await` and then use a promise chain to call subsequent methods. Here's some example code.

```javascript
import { Creator } from "./Creator"; 

export class DoSomething { 
    constructor () {} 
    
    public async doesSomething() { 
        const data = await new Creator()
	        .validate()
	        .then((c) => c.create())
	        .then((c) => c.result()); 
	    return data 
	} 
} 

// ./Creator file
export class Creator { 
    constructor () {} 
    
    public async validate() { 
	    // validates something
        return this 
    } 
    
    public async create() { 
	    // creates something
        return this 
    } 
    
    public async result() { 
	    // returns some object
        return { hello: "world" } 
    }
}
```

To mock out the `Creator` class inside `doesSomething` we can write a module mock like below.

```javascript
import { DoSomething } from "./DoSomething";

// Mock the Creator class and its methods
jest.mock("./Creator", () => {
  return {
    Creator: jest.fn().mockImplementation(() => {
      return {
        validate: jest.fn().mockResolvedValue({
          create: jest.fn().mockResolvedValue({
            result: jest.fn().mockResolvedValue({
              hello: "mocked world",
            }),
          }),
        }),
      };
    }),
  };
});

describe("DoSomething", () => {
  it("should do something", async () => {
    const result = await new DoSomething().doesSomething();

    expect(result).toEqual({ hello: "mocked world" });
  });
});

```

---

What's going on here? Let's break down the code.

Firstly, the snippet below is how we're initializing the mock.

```javascript
jest.mock("./Creator", () => {
  return {
    Creator: jest.fn().mockImplementation(() => {...}),
  };
});
```

We're mocking out the import `"./Creator"` along with a module factory parameter. To mock the `Creator` constructor function, our module factory parameter needs to return a function. And since we're using a non-default class export, we need to return an object with the key the same name as the class name.

Next, here's how we're mocking the methods

```javascript
return {
	validate: jest.fn().mockResolvedValue({
		create: jest.fn().mockResolvedValue({
			result: jest.fn().mockResolvedValue({
			  hello: "mocked world",
			}),
		}),
	}),
};
```

Our implementation is a promise chain. Every time it resolves it returns itself, which is a new promise until it gets to `result` which returns an object. We're nesting the promise calls with `mockResolvedValue` where each mock returns an object with a key matching the function name.

// TODO
1. Setup a repo with some sample code
2. Find alternative ways to mock it.