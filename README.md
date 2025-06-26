# Transition from JavaScript to TypeScript

As someone whose primary programming language has always been JavaScript, when I heard about TypeScript being a superset of JavaScript, I asked myself – What even is a superset? In the world of programming, a superset is a programming language that enhances another language by adding new features. In this case, TypeScript adds features like static typing and interfaces (just to mention a few), which make it more powerful than JavaScript. If you are coming from a JavaScript background, you might not understand what these new terms mean, which is why I am here to walk you through the basics of TypeScript from the lens of a JavaScript programmer!

Here's a brief outline of what I'll be covering in this article:

- **Static Typing**
- **Interfaces and Type Aliases**
- **Explicit Types and Narrowing**
- **Wrapping up & tips**

## Static Typing

Static typing is one of the most important features of TypeScript that I didn't know I needed until I started using it. Static typing essentially means that the type of any variable is determined at compile time, not at runtime. But what are compile time and runtime, you might be wondering? Compile time occurs when the compiler executes and takes our high-level code and converts it into a language that our computers can work with. On the other hand, runtime happens when our code is executed through a program. This is all to say that TypeScript basically determines the types of variables before actually running the code. This is incredibly beneficial for developers because the compiler can catch errors early and prevent them from occurring in the code when it's actually run.

For example, let's say that I want to store a list of my best friends in an array. That array should contain strings representing their names, not any other type of data. With static typing, I am able to let the compiler know that the array should only contain strings. See an example of this in the code below:

```ts
const friends: string[] = [];
```

If you are coming from a JavaScript background, you might be wondering why there is a colon after the variable name. This is because in TypeScript, the colon is used to specify the type of the variable like this `variable: type of variable`. So in the code above, what we're saying is that the `friends` variable should be an array of strings `string[]` and nothing else. You might also be wondering what `string[]` means; this is just a way of saying an array where its elements are strings. You can think of it as a pattern: `<type><data structure>` where the type is string and the data structure is an array. If I try to store any other type of data in this array, the compiler will throw an error. See the example in the code below:

```ts
const friends: string[] = [];

friends[0] = 123; // Compiler is not happy - Throws error: Type 'number' is not assignable to type 'string'.
friends[0] = "Bacon"; // Compiler rocks with this!
```

At first glance, this might seem like a small change, but it has a huge impact in terms of code quality and maintainability. Imagine that you are working on a large project with multiple developers. Variables are being created everywhere, and it's easy to make mistakes like assigning the wrong data type to a variable, especially when dealing with large amounts of code and if you lack familiarity with the codebase. In TypeScript, the compiler will catch these errors before the code is even run and prevent them from ever being executed, which accelerates the development process, makes the code more reliable, and improves code readability and maintainability for other developers.

> The compiler will become your best friend and worst enemy

## Interfaces and Type Aliases

The friends array example is helpful, but what if we want to declare more complex types? Let's delve deeper into the other core features of TypeScript: interfaces along with type aliases; and how we can use interfaces and types to define more complex data structures.
Before delving into interfaces and type aliases, let's kick things off by understanding what a type is. A type in TypeScript is a way to define the structure of a variable. You are probably familiar with different primitive JavaScript types like `number`, `string`, `boolean`, etc. Generally, we wouldn't usually define a type for a variable in JavaScript, so we would let them be defined at runtime and do something like this:

```js
let name = "Marco";
```

In the above example, the variable `name` is of type string even though it's not declared as such. This is because JavaScript infers the type of a variable from the value. JavaScript also doesn't enforce strictness, which means that a variable can hold any type of value at any time and if we try to reassign the variable to a different type, like a `number`, the JavaScript engine will set the variable type to a new type with no problem.

```js
let name = "Marco"; // value is a string

name = 10; // value is a number

name = true; // value is a boolean
```

The above code highlights one of the inconsistencies in JavaScript which TypeScript addresses with its type system. In TypeScript, we have the ability to create a type `Name` which we can use later when we need to create a new name and enforce certain rules about what the variable can be. These are called type aliases.

```ts
// Declares the type Name and assigns it the type string (Name is the alias of type string)
type Name = string;

// myName expects a type of Name which is a string
let myName: Name = "Marco";

// the Name type is also reusable!
let friendsName: Name = "Bryan";
```

In the above example, the variable `myName` is of type `Name` which is a `string` and it's declared as such, so if we try to assign any other type aside from `Name` to it, the compiler will report an error. This makes the code more reliable and predictable since now we know exactly what type the variable is. Now, what if we wanted to create a more robust type for name that contains the first and last name? In that case, we can just assign our type to an object with the properties `first` and `last`.

```ts
// Now type Name must be an object with first and last properties
type Name = { first: string; last: string };

// myName expects an object with first and last of type string
let myName: Name = { first: "Marco", last: "Polo" };
```

This is one way to add more properties to our type, but we can also use an `interface`. Interfaces are very similar to types; however, the core differences are that `interfaces` are much more flexible in situations when we are dealing with more complex data and can only describe the shape of an object (ideal for this situation).

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

The above code highlights the dynamic nature of JavaScript's type system, which TypeScript addresses with static typing. Interfaces allow us to add properties by extending them using the `extends` keyword, whereas with type aliases we have to redefine the type each time and intersect both types. This makes interfaces much more flexible to work with when dealing with object data that may need to be extended or modified, whereas type aliases are more useful for representing types that are not objects, such as strings or numbers.

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

Now that we understand the basics of type annotations, let's delve deeper into narrowing types in TypeScript. Sometimes the TypeScript compiler will warn us when we try to use methods on types we're not supposed to. For example, let's say that we have a function that takes a number or a string and makes it uppercase. Sounds simple enough, but there's a catch! If we try to use the `.toUpperCase()` method on the input, we will get an error. See the example below:

```ts
const makeUpperCase = (content: number | string): string => {
  content.toUpperCase(); // The compiler does not like that—it throws an error: Property 'toUpperCase' does not exist on type 'string | number'.
};
```

Before proceeding to explain why this error is happening, you might have noticed a new symbol here: `|`. This is called a union type, and it basically tells the compiler, "hey, this `content` parameter can be of type number or string." You can think of it as similar to using "or" (`||`) in logic, but for type annotations. (Note: `|` is not the same as `||` in code, but conceptually, it means "either type.")

Now back to the error. The above error happens because TypeScript knows that `content` can be either a `number` or a `string`, and the compiler knows that trying to use the `toUpperCase()` method on a number is not possible. So, how do we get around this? We can use type narrowing to narrow down the type of the `content` variable to either a string or a number. This can look like a simple if-else statement that checks the type of `content` and then performs the appropriate action.

```ts
const makeUpperCase = (content: number | string): string => {
  // Here our if statement is narrowing by checking if the content is a number and handles it appropriately
  if (typeof content === "number") {
    // Convert the number to a string and return the uppercase version of it
    const convertedNumber = content.toString();
    return convertedNumber.toUpperCase();
  } else {
    // If the type is not a number then it must be a string so we can just make it uppercase and return
    return content.toUpperCase();
  }
};
```

In the above example, the compiler will not throw any errors because we are properly handling our inputs. If the content is a number, we first convert it to a string using the `toString()` method and then make it uppercase (though note that making a number string uppercase doesn't change it, e.g., `"123".toUpperCase()` is still `"123"`). Otherwise, if the content is a string, we can just make it uppercase directly and return it. This is a common pattern in TypeScript when we need to handle different types of inputs in a function and ensure that the correct method is called based on the type of the input. Although it requires a bit of code, it's incredibly useful to catch errors early on in the development process.

## Wrapping Up and Tips

I hope you enjoyed this article and found it helpful in your transition from JavaScript to TypeScript. My TypeScript journey so far has been great; I love the way TypeScript helps me write better, more maintainable code and how it alerts me when I make mistakes. I'm excited to continue developing my knowledge in TypeScript and sharing my experiences with others!
If I were to start from scratch, I would heavily rely on the official TypeScript documentation as they also have a guide tailored for developers who have familiarity with JavaScript. Another tip is to always practice a new concept. Most of the time I have the documentation and my code editor open. Whenever there is a new concept I learn, I try to implement it in my own way, coming up with my own examples. I highly recommend following this approach!
All of the information in this article is available in the official TypeScript documentation, which you can visit at https://www.typescriptlang.org/docs/.
Best of luck in your TypeScript journey!
