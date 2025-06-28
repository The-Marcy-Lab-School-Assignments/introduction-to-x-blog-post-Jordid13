# Transition from JavaScript to TypeScript

As someone whose primary programming language has always been JavaScript, when I heard about TypeScript being a superset of JavaScript, I asked myself – What even is a superset? In the world of programming, a superset is a programming language that enhances another language by adding new features. In this case, TypeScript adds features like static typing and interfaces (just to mention a couple), which make it more powerful than JavaScript. If you are coming from a JavaScript background, you might not understand what these new terms mean, which is why I am here to walk you through the basics of TypeScript from the perspective of a JavaScript programmer!

Here's a brief outline of what I'll be covering in this article:

- [**Static Typing**](#static-typing)
- [**Interfaces and Type Aliases**](#interfaces-and-type-aliases)
- [**Type Annotations and Narrowing**](#type-annotations-and-narrowing)
- [**Wrapping up & tips**](#wrapping-up-and-tips)

## Static Typing

Static typing is one of the most important features of TypeScript that I didn't know I needed until I started using it. Static typing essentially means that the type of any variable is determined at compile time, not at runtime. But what are compile time and runtime, you might be wondering? Compile time occurs when the TypeScript compiler analyzes the code, checks for errors, and converts it into JavaScript, which can then be run in browsers or Node.js. On the other hand, runtime happens when our code is executed by the JavaScript engine. This means that TypeScript determines the types of variables before actually running the code. This is incredibly beneficial for developers because the compiler can catch errors early and prevent them from occurring in the code when it's actually run.

For example, let's say I want to store a list of my best friends in an array. That array should contain strings representing their names, not any other type of data. With static typing, I am able to let the compiler know that the array should only contain strings. See an example of this in the code below:

```ts
const friends: string[] = [];
```

If you are coming from a JavaScript background, you might be wondering why there is a colon after the variable name. This is because in TypeScript, the colon is used to specify the type of the variable, like this: `variable: type`. So in the code above, what we're saying is that the `friends` variable should be an array of strings (`string[]`) and nothing else. You might also be wondering what `string[]` means. The syntax `string[]` means 'an array of strings'. You might also see this written as `Array<string>`, which is just another way of saying the same thing. If I try to store any other type of data in this array, the compiler will report an error. See the example in the code below:

```ts
const friends: string[] = [];

friends[0] = 123; // Compiler is not happy - Error: Type 'number' is not assignable to type 'string'.
friends[0] = "Bacon"; // Compiler rocks with this! - "Bacon" is of type string
```

At first glance, this might seem like a small change, but it has a huge impact in terms of code quality and maintainability. Imagine that you are working on a large project with multiple developers. Variables are being created everywhere, and it's easy to make mistakes like assigning the wrong data type to a variable, especially when dealing with large amounts of code or if you lack familiarity with the codebase. In TypeScript, the compiler will catch these errors before the code is even run and prevent them from ever being executed, which accelerates the development process, makes the code more reliable, and improves code readability and maintainability for other developers.

> The compiler will become your best friend and worst enemy

## Interfaces and Type Aliases

The friends array example is helpful, but what if we want to declare more complex types? Let's delve deeper into the other core features of TypeScript: interfaces and type aliases, and how we can use them to define more complex data structures.
Before delving into interfaces and type aliases, let's start by understanding what a type is. A type in TypeScript is a way to define the structure of a variable. You are probably familiar with different primitive JavaScript types like `number`, `string`, `boolean`, etc. Generally, in JavaScript, we wouldn't usually define a type for a variable; instead, we let it be defined at runtime, as in the example below:

```js
let name = "Marco";
```

In the above example, the variable `name` is of type string even though it's not declared as such. This is because JavaScript infers the type of a variable from the value. JavaScript also doesn't enforce strict typing. This means a variable can hold any type of value, and you can reassign it to a different type (for example, from a string to a number) without any issues.

```js
let name = "Marco"; // value is a string

name = 10; // value is a number

name = true; // value is a boolean
```

The code above shows how JavaScript allows a variable to change its type freely, which can sometimes lead to unexpected behavior. TypeScript helps address this by letting us define specific types for our variables. For example, we can create a custom type called `Name` to ensure that variables meant to represent a name always follow certain rules. This is done using a feature of TypeScript called type aliases.

```ts
// Declares the type Name and assigns it the type string (Name is the alias of type string)
type Name = string;

// myName expects a type of Name which is a string
let myName: Name = "Marco";

// the Name type is also reusable!
let friendsName: Name = "Bryan";
```

In the above example, the variable `myName` is of type `Name`, which is a `string`, and it's declared as such. So if we try to assign any other type aside from `Name` to it, the compiler will report an error. This makes the code more reliable and predictable since now we know exactly what type the variable is. But what if we wanted to create a more robust type for `Name` that contains the first and last name? In that case, we can assign our type to an object with the properties `first` and `last`.

```ts
// Now type Name must be an object with first and last properties
type Name = { first: string; last: string };

// myName expects an object with first and last of type string
let myName: Name = { first: "Marco", last: "Polo" };
```

This is one way to add more properties to our type, but we can also use an `interface`. Interfaces are very similar to types; however, the core difference is that `interfaces` are much more flexible in situations when we are dealing with more complex data and want to define the shape of an object (ideal for this situation).

Let's look at an example of how we can use interfaces to define the type of `Name`.

```ts
interface Name {
  first: string;
  last: string;
}

let myName: Name = { first: "Marco", last: "Polo" };
```

At a glance, the code looks very similar to the type alias example. However, when defining an interface, we don't use the `=` symbol; instead, we simply use an object to define the shape of our interface. Now, what if we want to add more properties to our `Name` type? With a type alias, we would need to create a new type using intersection—a technique where we combine the original type with another type that includes the additional properties. In contrast, interfaces make this process easier by allowing us to add new properties by extending the interface with the `extends` keyword. Let's take a look at a few examples.

**Extending a type alias:**

```ts
// Declare a type alias that expects an object with the first and last properties as strings
type Name = { first: string; last: string };

// Adds the language property by creating a new type that intersects with the existing type
type NewName = Name & {
  language: string;
};
```

**Extending an interface:**

```ts
// Declare an interface (Name) that expects an object with the first and last properties as strings
interface Name {
  first: string;
  last: string;
}

// Extend the interface by creating a new interface
interface NameWithLanguage extends Name {
  language: string;
}
```

The above examples highlight the added flexibility that interfaces provide when it comes to extending types. Interfaces allow us to add properties by extending them using the `extends` keyword, whereas with type aliases, we have to redefine the type each time and intersect both types. Interfaces are generally more flexible when we're dealing with objects that may need to be extended or modified, while type aliases are useful for primitives, unions, intersections, or when you need to compose types in more complex ways.

## Type Annotations and Narrowing

Now that we've covered the basics of types and interfaces, it's a good time to explore the concept of type annotations and narrowing. I found type annotations to be very useful for clearly defining the input and output of a function. For example, if we have a function that takes an array of numbers and returns the sum of all the numbers, we can use type annotations to define the type of the input and output as follows:

```ts
// Telling the compiler that arr is an array of numbers and this function should return a number by using type annotations
const sumAll = (arr: number[]): number => {
  return arr.reduce((a, b) => a + b, 0);
};
```

In the above example, the `arr` parameter is declared as an array of numbers, and the function should return a number. To define the return type, we use the `:` symbol after the function parameter. This is helpful for developers to clearly understand the inputs and outputs of a function, and it also helps to catch potential errors in the code if it returns a different type than expected.

Not only can we use primitive types for type annotations, but we can also use type aliases and interfaces. Let's take a look at an example of how we can use type aliases and interfaces to define the type of the input for a function that logs the name of a person.

```ts
type Name = string;

// Function expects a name with the type alias "Name" and returns nothing (void)
const logName = (name: Name): void => {
  console.log(name);
};

const myName: Name = "Marco";

logName(myName);
```

In the case above, we use the `Name` type alias to define the type of the `name` input parameter for the `logName` function. If we use an interface, the type annotation does not change, but we must add a `name` property to our interface since interfaces describe the shape of objects. See an example of this below:

```ts
interface NameObj {
  name: string;
}

const logName = (name: NameObj): void => {
  console.log(name);
};

const myName: NameObj = { name: "Marco" };

logName(myName);
```

Now that we understand the basics of type annotations, let's delve deeper into narrowing types in TypeScript. Sometimes the TypeScript compiler will warn us when we try to use methods on types we're not supposed to. This is better explained with an example. Imagine that for some strange reason, we need to make a function that can take either a number or a string and make it uppercase. If you are familiar with JavaScript, you might know that you can't use the `.toUpperCase()` method on a number. Even though we know that this is true due to JavaScript's nature of determining variable types at runtime, it will try to run the code and run into an error. However, in TypeScript, if we try to do the same, the compiler will immediately report it as an error.

See the example below:

```ts
const makeUpperCase = (content: number | string): string => {
  content.toUpperCase(); // The compiler does not like that—it throws an error: Property 'toUpperCase' does not exist on type 'string | number'.
};
```

Before proceeding to explain why this error is happening, you might have noticed a new symbol here: `|`. This is called a union type, and it basically tells the compiler, "hey, this `content` parameter can be of type number or string." You can think of it as similar to using "or" (`||`) in logic, but for type annotations. (Note: `|` is not the same as `||` in code, but conceptually, it means "either type.")

Now back to the error. The above error happens because the compiler notices that `content` can be either a `number` or a `string`, and it also knows that trying to use the `toUpperCase()` method on a number is not possible. Thanks for the heads up, compiler! But how do we get around this? We can use type narrowing to narrow down the type of the `content` variable to either a string or a number. This can look like a simple if-else statement that checks the type of `content` and then performs the appropriate action.

```ts
const makeUpperCase = (content: number | string): string => {
  // Here our if statement is narrowing by checking if the content is a number and handles it appropriately
  if (typeof content === "number") {
    // Convert the number to a string and return it
    const convertedNumber = content.toString();
    return convertedNumber;
  } else {
    // If the type is not a number then it must be a string so we can just make it uppercase and return
    return content.toUpperCase();
  }
};
```

In the above example, the compiler will not throw any errors because we are properly handling our inputs. If the content is a number, we convert it to a string using the `toString()` method and then return it, because making a number string uppercase doesn't change it. Otherwise, if the content is a string, we can just make it uppercase directly and return it. This is a common pattern in TypeScript when we need to handle different types of inputs in a function and ensure that the correct method is called based on the type of the input. Although it requires a bit of code, it's incredibly useful to catch errors early on in the development process.

## Wrapping Up and Tips

I hope you enjoyed this article and found it helpful in your transition from JavaScript to TypeScript. My TypeScript journey so far has been great; I love the way TypeScript helps me write better, more maintainable code and how it alerts me when I make mistakes. I'm excited to continue developing my knowledge in TypeScript and sharing my experiences with others!
If I were to start from scratch, I would heavily rely on the official TypeScript documentation, as they also have a guide tailored for developers who are already familiar with JavaScript. Another tip is to always practice a new concept. Most of the time, I have the documentation and my code editor open. Whenever I learn a new concept, I try to implement it in my own way, coming up with my own examples. I highly recommend following this approach!

All of the information in this article is available in the official TypeScript documentation, which you can visit at https://www.typescriptlang.org/docs/.

Best of luck in your TypeScript journey!
