# Clean Code TypeScript for Little Developers! [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Clean%20Code%20Typescript&url=https://github.com/labs42io/clean-code-typescript)

Hey there, little coding superstar! 🌟 This is your friendly guide to writing super clean TypeScript code!

Just like how we keep our toys organized in neat little boxes, we want our code to be nice and tidy too! This guide will show you how to write TypeScript code that's easy to read, easy to understand, and easy to fix when things go wrong.

We got these super cool ideas from the [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript) guide and made them extra special for TypeScript! 🎉

## Table of Contents

  1. [Introduction](#introduction)
  2. [Variables](#variables)
  3. [Functions](#functions)
  4. [Objects and Data Structures](#objects-and-data-structures)
  5. [Classes](#classes)
  6. [SOLID](#solid)
  7. [Testing](#testing)
  8. [Concurrency](#concurrency)
  9. [Error Handling](#error-handling)
  10. [Formatting](#formatting)
  11. [Comments](#comments)
  12. [Translations](#translations)

## 🎯 Introduction

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

Hey little buddy! 👋 Have you ever tried to read someone else's messy handwriting? It's super hard, right? Well, messy code is just like that - it makes everyone confused and grumpy! 😤

This guide is all about learning how to write TypeScript code that's super clean and easy to understand - just like writing with your best handwriting! ✍️

These awesome ideas come from a super smart person named Robert C. Martin who wrote a book called [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882). We took his brilliant ideas and made them extra special for TypeScript!

Think of this guide as your friendly helper to make code that's:
- 📖 **Easy to read** - like a good storybook
- 🔄 **Easy to reuse** - like your favorite LEGO blocks
- 🛠️ **Easy to fix** - like when you need to change something in your drawing

You know what's really cool? These ideas aren't strict rules like "you must eat your vegetables" (though you should! 🥕). They're more like helpful suggestions that lots of smart grown-up developers have figured out work really well!

Coding is still pretty new - it's only been around for about 50 years! That's younger than your grandparents! 👴👵 So we're all still learning new tricks, just like when you learn to ride a bike or tie your shoes.

Here's a super important thing to remember: even the best developers make mistakes sometimes! That's totally okay! 😊 Every piece of code starts like a rough drawing that we keep making better and better. Don't worry if your first try isn't perfect - just keep practicing and having fun! 🎨

**[⬆ back to top](#table-of-contents)**

## 📦 Variables - Your Code's Little Labels!

Think of variables like little name tags for your toys! 🏷️ Just like you wouldn't want to call your teddy bear "Thingy" or "Stuff," we want to give our variables names that actually tell us what they are!

### 🏷️ Use meaningful variable names

Imagine you have two boxes - one has your crayons and one has your markers. If you labeled them "Box A" and "Box B," you'd have to open them to see what's inside every time! But if you label them "Crayons" and "Markers," you know exactly what's in each box right away! 🎨

**Bad:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}

```

**Good:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ back to top](#table-of-contents)**

### 🗣️ Use pronounceable variable names

Have you ever tried to say a really hard word and it came out all funny? 😅 Well, if you can't say a variable name out loud, it's really hard to talk about it with your friends!

It's like having a pet with a name you can't pronounce - you'd end up calling it "the thing with the long name" instead of its real name! 🐕

**Bad:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
}
```

**Good:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
}
```

**[⬆ back to top](#table-of-contents)**

### 🎯 Use the same vocabulary for the same type of variable

Imagine if your teacher sometimes called you by your name, sometimes called you "student," and sometimes called you "kid." That would be really confusing! 😵‍💫

It's the same with code - if we're talking about the same thing, we should always call it the same name!

**Bad:**

```ts
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**Good:**

```ts
function getUser(): User;
```

**[⬆ back to top](#table-of-contents)**

### 🔍 Use searchable names

Have you ever tried to find your favorite toy in a messy room? It's so much easier when everything has a proper place! 🧸

When we write code, other people (and even us later!) will read it way more times than we write it. So it's super important to use names that are easy to find when we're looking for something specific!

Think of it like having a toy box with labels - you can quickly find your blocks or your dolls without having to dig through everything! 📦

**Bad:**

```ts
// What the heck is 86400000 for?
setTimeout(restart, 86400000);
```

**Good:**

```ts
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 24 * 60 * 60 * 1000; // 86400000

setTimeout(restart, MILLISECONDS_PER_DAY);
```

**[⬆ back to top](#table-of-contents)**

### 📝 Use explanatory variables

Sometimes when we're reading code, we see something that doesn't make sense right away. It's like reading a sentence with a big, confusing word in it! 🤔

We can make things clearer by breaking them into smaller, easier-to-understand pieces - just like how we break big words into syllables!

**Bad:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
  // iterate through users map
}
```

**Good:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // iterate through users map
}
```

**[⬆ back to top](#table-of-contents)**

### 🧠 Avoid Mental Mapping

Have you ever played a game where you had to remember what each symbol meant? It's so much easier when the symbols actually look like what they represent! 🎮

In code, we want everything to be super clear and obvious - no guessing games! When someone reads our code, they should understand it right away, just like reading a picture book! 📚

**Bad:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Good:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ back to top](#table-of-contents)**

### Don't add unneeded context

If your class/type/object name tells you something, don't repeat that in your variable name.

**Bad:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Good:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

**[⬆ back to top](#table-of-contents)**

### Use default arguments instead of short circuiting or conditionals

Default arguments are often cleaner than short circuiting.

**Bad:**

```ts
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Good:**

```ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Use enum to document the intent

Enums can help you document the intent of the code. For example when we are concerned about values being
different rather than the exact value of those.

**Bad:**

```ts
const GENRE = {
  ROMANTIC: 'romantic',
  DRAMA: 'drama',
  COMEDY: 'comedy',
  DOCUMENTARY: 'documentary',
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed
    }
  }
}
```

**Good:**

```ts
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed
    }
  }
}
```

**[⬆ back to top](#table-of-contents)**

## 🎪 Functions - Your Code's Little Helpers!

Think of functions like little helpers that do specific jobs for you! 🧚‍♀️ Just like how you might have one helper to clean your room, another to help you with homework, and another to make your snack - each function should do just one thing really well!

### 🎯 Right-sized function arguments (2 or fewer ideally)

Imagine if you asked your friend to help you build a LEGO castle, but you gave them 10 different boxes at once! They'd get super confused and probably mix everything up! 😵

Functions work the same way - if we give them too many things to work with, they get confused and make mistakes! It's much better to give them just one or two things at a time.

**Why is this important?**
- 🧪 **Testing is easier** - like checking if your toy works with just one battery instead of trying every combination
- 🧠 **Less confusing** - easier to understand what the function needs
- 🛠️ **Easier to fix** - when something goes wrong, you know exactly what to check

If you find yourself needing lots of things, you can put them all in a special box (called an object) and give that box to your function instead!

**Bad:**

```ts
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Good:**

```ts
function createMenu(options: { title: string, body: string, buttonText: string, cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

You can further improve readability by using [type aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases):

```ts

type MenuOptions = { title: string, body: string, buttonText: string, cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ back to top](#table-of-contents)**

### 🎯 Functions should do one thing

This is like the golden rule of coding! 🌟 Just like how you wouldn't ask your toothbrush to also cook dinner (that would be silly! 😄), each function should have just one job to do.

**Why is this so important?**
- 🧩 **Easier to put together** - like building with LEGO blocks
- 🧪 **Easier to test** - like checking if your flashlight works
- 🧠 **Easier to understand** - like reading a simple story instead of a confusing one
- 🛠️ **Easier to fix** - when something breaks, you know exactly which helper to check

Think of it like having different toys for different games - you wouldn't try to play soccer with a tennis racket! 🏈

**Bad:**

```ts
function emailActiveClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```ts
function emailActiveClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ back to top](#table-of-contents)**

### 📢 Function names should say what they do

Imagine if your friend said "Hey, can you do the thing with the stuff?" You'd be super confused! 😕 But if they said "Hey, can you help me wash the dishes?" you'd know exactly what to do!

Function names work the same way - they should tell you exactly what the function is going to do, just like a recipe title tells you what you're going to cook! 👨‍🍳

**Bad:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);
```

**[⬆ back to top](#table-of-contents)**

### Functions should only be one level of abstraction

When you have more than one level of abstraction your function is usually doing too much. Splitting up functions leads to reusability and easier testing.

**Bad:**

```ts
function parseCode(code: string) {
  const REGEXES = [ /* ... */ ];
  const statements = code.split(' ');
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good:**

```ts
const REGEXES = [ /* ... */ ];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(' ');
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push( /* ... */ );
  });

  return syntaxTree;
}
```

**[⬆ back to top](#table-of-contents)**

### Remove duplicate code

Do your absolute best to avoid duplicate code.
Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.

Imagine if you run a restaurant and you keep track of your inventory: all your tomatoes, onions, garlic, spices, etc.
If you have multiple lists that you keep this on, then all have to be updated when you serve a dish with tomatoes in them.
If you only have one list, there's only one place to update!

Oftentimes you have duplicate code because you have two or more slightly different things, that share a lot in common, but their differences force you to have two or more separate functions that do much of the same things. Removing duplicate code means creating an abstraction that can handle this set of different things with just one function/module/class.

Getting the abstraction right is critical, that's why you should follow the [SOLID](#solid) principles. Bad abstractions can be worse than duplicate code, so be careful! Having said this, if you can make a good abstraction, do it! Don't repeat yourself, otherwise, you'll find yourself updating multiple places anytime you want to change one thing.

**Bad:**

```ts
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: (Developer | Manager)[]) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

You may also consider adding a union type, or common parent class if it suits your abstraction.
```ts
class Developer {
  // ...
}

class Manager {
  // ...
}

type Employee = Developer | Manager

function showEmployeeList(employee: Employee[]) {
  // ...
  });
}

```

You should be critical about code duplication. Sometimes there is a tradeoff between duplicated code and increased complexity by introducing unnecessary abstraction. When two implementations from two different modules look similar but live in different domains, duplication might be acceptable and preferred over extracting the common code. The extracted common code, in this case, introduces an indirect dependency between the two modules.

**[⬆ back to top](#table-of-contents)**

### Set default objects with Object.assign or destructuring

**Bad:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**Good:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // ...
}

createMenu({ body: 'Bar' });
```

Or, you could use the spread operator:

```ts
function createMenu(config: MenuConfig) {
  const menuConfig = {
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true,
    ...config,
  };

  // ...
}
```
The spread operator and `Object.assign()` are very similar.
The main difference is that spreading defines new properties, while `Object.assign()` sets them. More detailed, the difference is explained in [this](https://stackoverflow.com/questions/32925460/object-spread-vs-object-assign) thread.

Alternatively, you can use destructuring with default values:

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) {
  // ...
}

createMenu({ body: 'Bar' });
```

To avoid any side effects and unexpected behavior by passing in explicitly the `undefined` or `null` value, you can tell the TypeScript compiler to not allow it.
See [`--strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks) option in TypeScript.

**[⬆ back to top](#table-of-contents)**

### Don't use flags as function parameters

Flags tell your user that this function does more than one thing.
Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

**Bad:**

```ts
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**

```ts
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

**[⬆ back to top](#table-of-contents)**

### 🚫 Avoid Side Effects (part 1)

Imagine you have a magic box that's supposed to just give you back whatever you put in it, but instead it also changes the color of your room and moves your toys around! That would be super confusing and scary! 😱

Functions should work like simple, predictable magic boxes - you put something in, and you get something back. That's it! No surprises!

**What are side effects?**
- 📝 Writing to files when you didn't ask it to
- 🌍 Changing things that other parts of your program are using
- 💰 Accidentally sending all your allowance to a stranger (okay, that's an extreme example! 😅)

Sometimes you DO need to do these things (like saving your drawing to a file), but it's important to have just one special helper that's in charge of doing these important tasks. Don't let every function mess with important things!

**Bad:**

```ts
// Global variable referenced by following function.
let name = 'Robert C. Martin';

function toBase64() {
  name = btoa(name);
}

toBase64();
// If we had another function that used this name, now it'd be a Base64 value

console.log(name); // expected to print 'Robert C. Martin' but instead 'Um9iZXJ0IEMuIE1hcnRpbg=='
```

**Good:**

```ts
const name = 'Robert C. Martin';

function toBase64(text: string): string {
  return btoa(text);
}

const encodedName = toBase64(name);
console.log(name);
```

**[⬆ back to top](#table-of-contents)**

### Avoid Side Effects (part 2)

Browsers and Node.js process only JavaScript, therefore any TypeScript code has to be compiled before running or debugging.  In JavaScript, some values are unchangeable (immutable) and some are changeable (mutable). Objects and arrays are two kinds of mutable values so it's important to handle them carefully when they're passed as parameters to a function. A JavaScript function can change an object's properties or alter the contents of an array which could easily cause bugs elsewhere.

Suppose there's a function that accepts an array parameter representing a shopping cart. If the function makes a change in that shopping cart array - by adding an item to purchase, for example - then any other function that uses that same `cart` array will be affected by this addition. That may be great, however it could also be bad. Let's imagine a bad situation:

The user clicks the "Purchase" button which calls a `purchase` function that spawns a network request and sends the `cart` array to the server. Because of a bad network connection, the `purchase` function has to keep retrying the request. Now, what if in the meantime the user accidentally clicks an "Add to Cart" button on an item they don't actually want before the network request begins? If that happens and the network request begins, then that purchase function will send the accidentally added item because the `cart` array was modified.

A great solution would be for the `addItemToCart` function to always clone the `cart`, edit it, and return the clone. This would ensure that functions that are still using the old shopping cart wouldn't be affected by the changes.

Two caveats to mention to this approach:

1. There might be cases where you actually want to modify the input object, but when you adopt this programming practice you will find that those cases are pretty rare. Most things can be refactored to have no side effects! (see [pure function](https://en.wikipedia.org/wiki/Pure_function))

2. Cloning big objects can be very expensive in terms of performance. Luckily, this isn't a big issue in practice because there are [great libraries](https://github.com/immutable-js/immutable-js) that allow this kind of programming approach to be fast and not as memory intensive as it would be for you to manually clone objects and arrays.

**Bad:**

```ts
function addItemToCart(cart: CartItem[], item: Item): void {
  cart.push({ item, date: Date.now() });
};
```

**Good:**

```ts
function addItemToCart(cart: CartItem[], item: Item): CartItem[] {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ back to top](#table-of-contents)**

### Don't write to global functions

Polluting globals is a bad practice in JavaScript because you could clash with another library and the user of your API would be none-the-wiser until they get an exception in production. Let's think about an example: what if you wanted to extend JavaScript's native Array method to have a `diff` method that could show the difference between two arrays? You could write your new function to the `Array.prototype`, but it could clash with another library that tried to do the same thing. What if that other library was just using `diff` to find the difference between the first and last elements of an array? This is why it would be much better to just use classes and simply extend the `Array` global.

**Bad:**

```ts
declare global {
  interface Array<T> {
    diff(other: T[]): Array<T>;
  }
}

if (!Array.prototype.diff) {
  Array.prototype.diff = function <T>(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**Good:**

```ts
class MyArray<T> extends Array<T> {
  diff(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**[⬆ back to top](#table-of-contents)**

### Favor functional programming over imperative programming

Favor this style of programming when you can.

**Bad:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < contributions.length; i++) {
  totalOutput += contributions[i].linesOfCode;
}
```

**Good:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = contributions
  .reduce((totalLines, output) => totalLines + output.linesOfCode, 0);
```

**[⬆ back to top](#table-of-contents)**

### Encapsulate conditionals

**Bad:**

```ts
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**Good:**

```ts
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid negative conditionals

**Bad:**

```ts
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**Good:**

```ts
function isEmailUsed(email: string): boolean {
  // ...
}

if (!isEmailUsed(email)) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid conditionals

This seems like an impossible task. Upon first hearing this, most people say, "how am I supposed to do anything without an `if` statement?" The answer is that you can use polymorphism to achieve the same task in many cases. The second question is usually, "well that's great but why would I want to do that?" The answer is a previous clean code concept we learned: a function should only do one thing. When you have classes and functions that have `if` statements, you are telling your user that your function does more than one thing. Remember, just do one thing.

**Bad:**

```ts
class Airplane {
  private type: string;
  // ...

  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
      default:
        throw new Error('Unknown airplane type.');
    }
  }

  private getMaxAltitude(): number {
    // ...
  }
}
```

**Good:**

```ts
abstract class Airplane {
  protected getMaxAltitude(): number {
    // shared logic with subclasses ...
  }

  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid type checking

TypeScript is a strict syntactical superset of JavaScript and adds optional static type checking to the language.
Always prefer to specify types of variables, parameters and return values to leverage the full power of TypeScript features.
It makes refactoring more easier.

**Bad:**

```ts
function travelToTexas(vehicle: Bicycle | Car) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(currentLocation, new Location('texas'));
  }
}
```

**Good:**

```ts
type Vehicle = Bicycle | Car;

function travelToTexas(vehicle: Vehicle) {
  vehicle.move(currentLocation, new Location('texas'));
}
```

**[⬆ back to top](#table-of-contents)**

### Don't over-optimize

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of times, if you are optimizing then you are just wasting your time. There are good [resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) for seeing where optimization is lacking. Target those in the meantime, until they are fixed if they can be.

**Bad:**

```ts
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:**

```ts
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Remove dead code

Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase.
If it's not being called, get rid of it! It will still be saved in your version history if you still need it.

**Bad:**

```ts
function oldRequestModule(url: string) {
  // ...
}

function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Good:**

```ts
function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**[⬆ back to top](#table-of-contents)**

### Use iterators and generators

Use generators and iterables when working with collections of data used like a stream.
There are some good reasons:

- decouples the callee from the generator implementation in a sense that callee decides how many
items to access
- lazy execution, items are streamed on-demand
- built-in support for iterating items using the `for-of` syntax
- iterables allow implementing optimized iterator patterns

**Bad:**

```ts
function fibonacci(n: number): number[] {
  if (n === 1) return [0];
  if (n === 2) return [0, 1];

  const items: number[] = [0, 1];
  while (items.length < n) {
    items.push(items[items.length - 2] + items[items.length - 1]);
  }

  return items;
}

function print(n: number) {
  fibonacci(n).forEach(fib => console.log(fib));
}

// Print first 10 Fibonacci numbers.
print(10);
```

**Good:**

```ts
// Generates an infinite stream of Fibonacci numbers.
// The generator doesn't keep the array of all numbers.
function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function print(n: number) {
  let i = 0;
  for (const fib of fibonacci()) {
    if (i++ === n) break;
    console.log(fib);
  }
}

// Print first 10 Fibonacci numbers.
print(10);
```

There are libraries that allow working with iterables in a similar way as with native arrays, by
chaining methods like `map`, `slice`, `forEach` etc. See [itiriri](https://www.npmjs.com/package/itiriri) for
an example of advanced manipulation with iterables (or [itiriri-async](https://www.npmjs.com/package/itiriri-async) for manipulation of async iterables).

```ts
import itiriri from 'itiriri';

function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

itiriri(fibonacci())
  .take(10)
  .forEach(fib => console.log(fib));
```

**[⬆ back to top](#table-of-contents)**

## 🎁 Objects and Data Structures - Your Code's Treasure Boxes!

Think of objects like special treasure boxes that hold your most important things! 🏴‍☠️ Just like how you might have a jewelry box for your special accessories, objects help us organize and protect our data in neat, organized ways!

### 🔒 Use getters and setters

Imagine you have a piggy bank, but instead of just being able to reach in and grab coins whenever you want, you have special rules! 🐷

**Why are getters and setters so cool?**
- 🛡️ **Protection** - Like having a guard for your treasure box
- ✅ **Validation** - Making sure only good things go in (like not putting a sandwich in your piggy bank!)
- 📝 **Logging** - Keeping track of what happens (like writing in a diary)
- 🎯 **Control** - You decide exactly how things get in and out
- ⚡ **Smart loading** - Only getting things when you actually need them

It's like having a magical piggy bank that can check if you're putting in real coins, keep track of how much money you have, and even get money from the bank when you need it!

**Bad:**

```ts
type BankAccount = {
  balance: number;
  // ...
}

const value = 100;
const account: BankAccount = {
  balance: 0,
  // ...
};

if (value < 0) {
  throw new Error('Cannot set negative balance.');
}

account.balance = value;
```

**Good:**

```ts
class BankAccount {
  private accountBalance: number = 0;

  get balance(): number {
    return this.accountBalance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error('Cannot set negative balance.');
    }

    this.accountBalance = value;
  }

  // ...
}

// Now `BankAccount` encapsulates the validation logic.
// If one day the specifications change, and we need extra validation rule,
// we would have to alter only the `setter` implementation,
// leaving all dependent code unchanged.
const account = new BankAccount();
account.balance = 100;
```

**[⬆ back to top](#table-of-contents)**

### Make objects have private/protected members

TypeScript supports `public` *(default)*, `protected` and `private` accessors on class members.

**Bad:**

```ts
class Circle {
  radius: number;

  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**Good:**

```ts
class Circle {
  constructor(private readonly radius: number) {
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Prefer immutability

TypeScript's type system allows you to mark individual properties on an interface/class as *readonly*. This allows you to work in a functional way (an unexpected mutation is bad).
For more advanced scenarios there is a built-in type `Readonly` that takes a type `T` and marks all of its properties as readonly using mapped types (see [mapped types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)).

**Bad:**

```ts
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**Good:**

```ts
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```

For arrays, you can create a read-only array by using `ReadonlyArray<T>`.
It doesn't allow changes such as `push()` and `fill()`, but can use features such as `concat()` and `slice()` that do not change the array's value.

**Bad:**

```ts
const array: number[] = [ 1, 3, 5 ];
array = []; // error
array.push(100); // array will be updated
```

**Good:**

```ts
const array: ReadonlyArray<number> = [ 1, 3, 5 ];
array = []; // error
array.push(100); // error
```

Declaring read-only arguments in [TypeScript 3.4 is a bit easier](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#improvements-for-readonlyarray-and-readonly-tuples).

```ts
function hoge(args: readonly string[]) {
  args.push(1); // error
}
```

Prefer [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions) for literal values.

**Bad:**

```ts
const config = {
  hello: 'world'
};
config.hello = 'world'; // value is changed

const array  = [ 1, 3, 5 ];
array[0] = 10; // value is changed

// writable objects is returned
function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // value is changed
```

**Good:**

```ts
// read-only object
const config = {
  hello: 'world'
} as const;
config.hello = 'world'; // error

// read-only array
const array  = [ 1, 3, 5 ] as const;
array[0] = 10; // error

// You can return read-only objects
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // error
```

**[⬆ back to top](#table-of-contents)**

### type vs. interface

Use type when you might need a union or intersection. Use an interface when you want `extends` or `implements`. There is no strict rule, however, use the one that works for you.
For a more detailed explanation refer to this [answer](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/54101543#54101543) about the differences between `type` and `interface` in TypeScript.

**Bad:**

```ts
interface EmailConfig {
  // ...
}

interface DbConfig {
  // ...
}

interface Config {
  // ...
}

//...

type Shape = {
  // ...
}
```

**Good:**

```ts

type EmailConfig = {
  // ...
}

type DbConfig = {
  // ...
}

type Config  = EmailConfig | DbConfig;

// ...

interface Shape {
  // ...
}

class Circle implements Shape {
  // ...
}

class Square implements Shape {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

## 🏗️ Classes - Building Blocks of Your Code!

Think of classes like different types of LEGO sets! 🧱 Each class should be like a specific LEGO set that builds one particular thing - like a castle set, or a spaceship set, or a house set. You wouldn't want one giant LEGO set that tries to build everything at once - that would be super confusing! 😵

### 📏 Classes should be small

Just like how you wouldn't want one toy that tries to be a doll, a car, a puzzle, and a board game all at the same time, classes should focus on doing just one thing really well! 🎯

The size of a class isn't measured by how many lines it has, but by how many different jobs it's trying to do. A good class is like a specialist - it's really good at one specific thing!

**Bad:**

```ts
class Dashboard {
  getLanguage(): string { /* ... */ }
  setLanguage(language: string): void { /* ... */ }
  showProgress(): void { /* ... */ }
  hideProgress(): void { /* ... */ }
  isDirty(): boolean { /* ... */ }
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  addSubscription(subscription: Subscription): void { /* ... */ }
  removeSubscription(subscription: Subscription): void { /* ... */ }
  addUser(user: User): void { /* ... */ }
  removeUser(user: User): void { /* ... */ }
  goToHomePage(): void { /* ... */ }
  updateProfile(details: UserDetails): void { /* ... */ }
  getVersion(): string { /* ... */ }
  // ...
}

```

**Good:**

```ts
class Dashboard {
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  getVersion(): string { /* ... */ }
}

// split the responsibilities by moving the remaining methods to other classes
// ...
```

**[⬆ back to top](#table-of-contents)**

### High cohesion and low coupling

Cohesion defines the degree to which class members are related to each other. Ideally, all fields within a class should be used by each method.
We then say that the class is *maximally cohesive*. In practice, this, however, is not always possible, nor even advisable. You should however prefer cohesion to be high.

Coupling refers to how related or dependent are two classes toward each other. Classes are said to be low coupled if changes in one of them don't affect the other one.

Good software design has **high cohesion** and **low coupling**.

**Bad:**

```ts
class UserManager {
  // Bad: each private variable is used by one or another group of methods.
  // It makes clear evidence that the class is holding more than a single responsibility.
  // If I need only to create the service to get the transactions for a user,
  // I'm still forced to pass and instance of `emailSender`.
  constructor(
    private readonly db: Database,
    private readonly emailSender: EmailSender) {
  }

  async getUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await db.transactions.find({ userId });
  }

  async sendGreeting(): Promise<void> {
    await emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**Good:**

```ts
class UserService {
  constructor(private readonly db: Database) {
  }

  async getUser(id: number): Promise<User> {
    return await this.db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await this.db.transactions.find({ userId });
  }
}

class UserNotifier {
  constructor(private readonly emailSender: EmailSender) {
  }

  async sendGreeting(): Promise<void> {
    await this.emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await this.emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Prefer composition over inheritance

As stated famously in [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four, you should *prefer composition over inheritance* where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition. The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.

You might be wondering then, "when should I use inheritance?" It depends on your problem at hand, but this is a decent list of when inheritance makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a" relationship (Human->Animal vs. User->UserDetails).

2. You can reuse code from the base classes (Humans can move like all animals).

3. You want to make global changes to derived classes by changing a base class. (Change the caloric expenditure of all animals when they move).

**Bad:**

```ts
class Employee {
  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(
    name: string,
    email: string,
    private readonly ssn: string,
    private readonly salary: number) {
    super(name, email);
  }

  // ...
}
```

**Good:**

```ts
class Employee {
  private taxData: EmployeeTaxData;

  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  setTaxData(ssn: string, salary: number): Employee {
    this.taxData = new EmployeeTaxData(ssn, salary);
    return this;
  }

  // ...
}

class EmployeeTaxData {
  constructor(
    public readonly ssn: string,
    public readonly salary: number) {
  }

  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Use method chaining

This pattern is very useful and commonly used in many libraries. It allows your code to be expressive, and less verbose. For that reason, use method chaining and take a look at how clean your code will be.

**Bad:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): void {
    this.collection = collection;
  }

  page(number: number, itemsPerPage: number = 100): void {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
  }

  orderBy(...fields: string[]): void {
    this.orderByFields = fields;
  }

  build(): Query {
    // ...
  }
}

// ...

const queryBuilder = new QueryBuilder();
queryBuilder.from('users');
queryBuilder.page(1, 100);
queryBuilder.orderBy('firstName', 'lastName');

const query = queryBuilder.build();
```

**Good:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): this {
    this.collection = collection;
    return this;
  }

  page(number: number, itemsPerPage: number = 100): this {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
    return this;
  }

  orderBy(...fields: string[]): this {
    this.orderByFields = fields;
    return this;
  }

  build(): Query {
    // ...
  }
}

// ...

const query = new QueryBuilder()
  .from('users')
  .page(1, 100)
  .orderBy('firstName', 'lastName')
  .build();
```

**[⬆ back to top](#table-of-contents)**

## 🌟 SOLID - The Super Powers of Good Code!

SOLID is like having super powers for your code! 💪 These are five special rules that help us write code that's strong, flexible, and easy to work with - just like having the perfect set of superpowers! 🦸‍♀️

### 🎯 Single Responsibility Principle (SRP) - The Focused Superhero!

Imagine if Superman tried to be a doctor, a chef, a teacher, and a firefighter all at the same time! He'd be super stressed and probably not very good at any of them! 😰

The Single Responsibility Principle is like saying "each superhero should have just one main superpower." In code, each class should have just one main job to do!

**Why is this so important?**
- 🎯 **Focused** - Like a superhero who's really good at one thing
- 🛠️ **Easier to fix** - When something breaks, you know exactly which superhero to call
- 🧩 **Easier to understand** - Like reading a simple story about one character
- 🔄 **Easier to change** - If you need to update something, you only need to change one thing

Think of it like having different toys for different games - you wouldn't try to play soccer with a tennis racket! ⚽

**Bad:**

```ts
class UserSettings {
  constructor(private readonly user: User) {
  }

  changeSettings(settings: UserSettings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Good:**

```ts
class UserAuth {
  constructor(private readonly user: User) {
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  private readonly auth: UserAuth;

  constructor(private readonly user: User) {
    this.auth = new UserAuth(user);
  }

  changeSettings(settings: UserSettings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification." What does that mean though? This principle basically states that you should allow users to add new functionalities without changing existing code.

**Bad:**

```ts
class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    if (this.adapter instanceof AjaxAdapter) {
      const response = await makeAjaxCall<T>(url);
      // transform response and return
    } else if (this.adapter instanceof NodeAdapter) {
      const response = await makeHttpCall<T>(url);
      // transform response and return
    }
  }
}

function makeAjaxCall<T>(url: string): Promise<T> {
  // request and return promise
}

function makeHttpCall<T>(url: string): Promise<T> {
  // request and return promise
}
```

**Good:**

```ts
abstract class Adapter {
  abstract async request<T>(url: string): Promise<T>;

  // code shared to subclasses ...
}

class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    const response = await this.adapter.request<T>(url);
    // transform response and return
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S is a subtype of T, then objects of type T may be replaced with objects of type S (i.e., objects of type S may substitute objects of type T) without altering any of the desirable properties of that program (correctness, task performed, etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class, then the parent class and child class can be used interchangeably without getting incorrect results. This might still be confusing, so let's take a look at the classic Square-Rectangle example. Mathematically, a square is a rectangle, but if you model it using the "is-a" relationship via inheritance, you quickly get into trouble.

**Bad:**

```ts
class Rectangle {
  constructor(
    protected width: number = 0,
    protected height: number = 0) {

  }

  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  setWidth(width: number): this {
    this.width = width;
    return this;
  }

  setHeight(height: number): this {
    this.height = height;
    return this;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): this {
    this.width = width;
    this.height = width;
    return this;
  }

  setHeight(height: number): this {
    this.width = height;
    this.height = height;
    return this;
  }
}

function renderLargeRectangles(rectangles: Rectangle[]) {
  rectangles.forEach((rectangle) => {
    const area = rectangle
      .setWidth(4)
      .setHeight(5)
      .getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Good:**

```ts
abstract class Shape {
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  abstract getArea(): number;
}

class Rectangle extends Shape {
  constructor(
    private readonly width = 0,
    private readonly height = 0) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private readonly length: number) {
    super();
  }

  getArea(): number {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes: Shape[]) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ back to top](#table-of-contents)**

### Interface Segregation Principle (ISP)

ISP states that "Clients should not be forced to depend upon interfaces that they do not use.". This principle is very much related to the Single Responsibility Principle.
What it really means is that you should always design your abstractions in a way that the clients that are using the exposed methods do not get the whole pie instead. That also include imposing the clients with the burden of implementing methods that they don’t actually need.

**Bad:**

```ts
interface SmartPrinter {
  print();
  fax();
  scan();
}

class AllInOnePrinter implements SmartPrinter {
  print() {
    // ...
  }

  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements SmartPrinter {
  print() {
    // ...
  }

  fax() {
    throw new Error('Fax not supported.');
  }

  scan() {
    throw new Error('Scan not supported.');
  }
}
```

**Good:**

```ts
interface Printer {
  print();
}

interface Fax {
  fax();
}

interface Scanner {
  scan();
}

class AllInOnePrinter implements Printer, Fax, Scanner {
  print() {
    // ...
  }

  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements Printer {
  print() {
    // ...
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.

2. Abstractions should not depend upon details. Details should depend on abstractions.

This can be hard to understand at first, but if you've worked with Angular, you've seen an implementation of this principle in the form of Dependency Injection (DI). While they are not identical concepts, DIP keeps high-level modules from knowing the details of its low-level modules and setting them up. It can accomplish this through DI. A huge benefit of this is that it reduces the coupling between modules. Coupling is a very bad development pattern because it makes your code hard to refactor.

DIP is usually achieved by a using an inversion of control (IoC) container. An example of a powerful IoC container for TypeScript is [InversifyJs](https://www.npmjs.com/package/inversify)

**Bad:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

class XmlFormatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}

class ReportReader {

  // BAD: We have created a dependency on a specific request implementation.
  // We should just have ReportReader depend on a parse method: `parse`
  private readonly formatter = new XmlFormatter();

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader();
const report = await reader.read('report.xml');
```

**Good:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

interface Formatter {
  parse<T>(content: string): T;
}

class XmlFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}


class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts a JSON string to an object T
  }
}

class ReportReader {
  constructor(private readonly formatter: Formatter) {
  }

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader(new XmlFormatter());
const report = await reader.read('report.xml');

// or if we had to read a json report
const reader = new ReportReader(new JsonFormatter());
const report = await reader.read('report.json');
```

**[⬆ back to top](#table-of-contents)**

## 🧪 Testing - Making Sure Your Code Works!

Testing is like checking if your toy actually works before you give it to your friend! 🎁 You wouldn't want to give someone a broken toy, right? Well, we don't want to give our users broken code either!

**Why is testing so super important?**
- 🛡️ **Safety net** - Like having training wheels on your bike
- 🎯 **Confidence** - You know your code works before you share it
- 🐛 **Bug catcher** - Like a superhero that catches problems before they become big issues
- 😌 **Peace of mind** - You can sleep well knowing your code is working! 😴

Think of tests like having a checklist before you go on a trip - you want to make sure you have everything you need and that everything works! ✈️

There are lots of super cool testing tools available for TypeScript, so find one that your team likes! The important thing is to always test your code - no excuses! Testing is more important than even finishing the code, because broken code is worse than no code at all! 🚫

### 🎯 The three laws of TDD (Test-Driven Development)

TDD is like building with LEGO blocks by following the instructions step by step! 🧱 You test first, then build, then test again!

**The three golden rules:**
1. 🚫 **No building without testing** - You can't write code unless you have a test that's failing first (like having instructions before you build!)

2. 🎯 **Just enough test** - Write only enough test to make it fail, then stop (like reading just one step of the instructions!)

3. ✅ **Just enough code** - Write only enough code to make the test pass, then stop (like building just one piece at a time!)

It's like the scientific method - you make a guess (write a test), then try to prove it (write code), then check if you were right (run the test)! 🔬

**[⬆ back to top](#table-of-contents)**

### F.I.R.S.T. rules

Clean tests should follow the rules:

- **Fast** tests should be fast because we want to run them frequently.

- **Independent** tests should not depend on each other. They should provide same output whether run independently or all together in any order.

- **Repeatable** tests should be repeatable in any environment and there should be no excuse for why they fail.

- **Self-Validating** a test should answer with either *Passed* or *Failed*. You don't need to compare log files to answer if a test passed.

- **Timely** unit tests should be written before the production code. If you write tests after the production code, you might find writing tests too hard.

**[⬆ back to top](#table-of-contents)**

### Single concept per test

Tests should also follow the *Single Responsibility Principle*. Make only one assert per unit test.

**Bad:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles date boundaries', () => {
    let date: AwesomeDate;

    date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));

    date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));

    date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**Good:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles 30-day months', () => {
    const date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));
  });

  it('handles leap year', () => {
    const date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));
  });

  it('handles non-leap year', () => {
    const date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**[⬆ back to top](#table-of-contents)**

### The name of the test should reveal its intention

When a test fails, its name is the first indication of what may have gone wrong.

**Bad:**

```ts
describe('Calendar', () => {
  it('2/29/2020', () => {
    // ...
  });

  it('throws', () => {
    // ...
  });
});
```

**Good:**

```ts
describe('Calendar', () => {
  it('should handle leap year', () => {
    // ...
  });

  it('should throw when format is invalid', () => {
    // ...
  });
});
```

**[⬆ back to top](#table-of-contents)**

## Concurrency

### Prefer promises vs callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting *(the callback hell)*.
There are utilities that transform existing functions using the callback style to a version that returns promises
(for Node.js see [`util.promisify`](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original), for general purpose see [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify))

**Bad:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';

function downloadPage(url: string, saveTo: string, callback: (error: Error, content?: string) => void) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html', (error, content) => {
  if (error) {
    console.error(error);
  } else {
    console.log(content);
  }
});
```

**Good:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url)
    .then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));
```

Promises supports a few helper methods that help make code more concise:

| Pattern                  | Description                                |
| ------------------------ | -----------------------------------------  |
| `Promise.resolve(value)` | Convert a value into a resolved promise.   |
| `Promise.reject(error)`  | Convert an error into a rejected promise.  |
| `Promise.all(promises)`  | Returns a new promise which is fulfilled with an array of fulfillment values for the passed promises or rejects with the reason of the first promise that rejects. |
| `Promise.race(promises)`| Returns a new promise which is fulfilled/rejected with the result/error of the first settled promise from the array of passed promises. |

`Promise.all` is especially useful when there is a need to run tasks in parallel. `Promise.race` makes it easier to implement things like timeouts for promises.

**[⬆ back to top](#table-of-contents)**

### Async/Await are even cleaner than Promises

With `async`/`await` syntax you can write code that is far cleaner and more understandable than chained promises. Within a function prefixed with `async` keyword, you have a way to tell the JavaScript runtime to pause the execution of code on the `await` keyword (when used on a promise).

**Bad:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = util.promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));
```

**Good:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

async function downloadPage(url: string): Promise<string> {
  const response = await get(url);
  return response;
}

// somewhere in an async function
try {
  const content = await downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
  await write('article.html', content);
  console.log(content);
} catch (error) {
  console.error(error);
}
```

**[⬆ back to top](#table-of-contents)**

## 🚨 Error Handling - When Things Go Wrong!

Uh oh! Sometimes things don't go as planned in our code, just like when you're building with blocks and they fall down! 😅 But that's totally okay - errors are actually our friends because they tell us when something needs fixing!

Think of errors like your toy's way of saying "Hey, something's not working right!" - they're like little warning lights that help us figure out what went wrong! 💡

### Always use Error for throwing or rejecting

JavaScript as well as TypeScript allow you to `throw` any object. A Promise can also be rejected with any reason object.
It is advisable to use the `throw` syntax with an `Error` type. This is because your error might be caught in higher level code with a `catch` syntax.
It would be very confusing to catch a string message there and would make
[debugging more painful](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).
For the same reason you should reject promises with `Error` types.

**Bad:**

```ts
function calculateTotal(items: Item[]): number {
  throw 'Not implemented.';
}

function get(): Promise<Item[]> {
  return Promise.reject('Not implemented.');
}
```

**Good:**

```ts
function calculateTotal(items: Item[]): number {
  throw new Error('Not implemented.');
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error('Not implemented.'));
}

// or equivalent to:

async function get(): Promise<Item[]> {
  throw new Error('Not implemented.');
}
```

The benefit of using `Error` types is that it is supported by the syntax `try/catch/finally` and implicitly all errors have the `stack` property which
is very powerful for debugging.
There are also other alternatives, not to use the `throw` syntax and instead always return custom error objects. TypeScript makes this even easier.
Consider the following example:

```ts
type Result<R> = { isError: false, value: R };
type Failure<E> = { isError: true, error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, 'empty'> {
  if (items.length === 0) {
    return { isError: true, error: 'empty' };
  }

  // ...
  return { isError: false, value: 42 };
}
```

For the detailed explanation of this idea refer to the [original post](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).

**[⬆ back to top](#table-of-contents)**

### Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix or react to said error. Logging the error to the console (`console.log`) isn't much better as often it can get lost in a sea of things printed to the console. If you wrap any bit of code in a `try/catch` it means you think an error may occur there and therefore you should have a plan, or create a code path, for when it occurs.

**Bad:**

```ts
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}

// or even worse

try {
  functionThatMightThrow();
} catch (error) {
  // ignore error
}
```

**Good:**

```ts
import { logger } from './logging'

try {
  functionThatMightThrow();
} catch (error) {
  logger.log(error);
}
```

**[⬆ back to top](#table-of-contents)**

### Don't ignore rejected promises

For the same reason you shouldn't ignore caught errors from `try/catch`.

**Bad:**

```ts
getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    console.log(error);
  });
```

**Good:**

```ts
import { logger } from './logging'

getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    logger.log(error);
  });

// or using the async/await syntax:

try {
  const user = await getUser();
  await sendEmail(user.email, 'Welcome!');
} catch (error) {
  logger.log(error);
}
```

**[⬆ back to top](#table-of-contents)**

## Formatting

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is *DO NOT ARGUE* over formatting. There are tons of tools to automate this. Use one! It's a waste of time and money for engineers to argue over formatting. The general rule to follow is *keep consistent formatting rules*.

For TypeScript there is a powerful tool called [ESLint](https://typescript-eslint.io/). It's a static analysis tool that can help you improve dramatically the readability and maintainability of your code. There are ready to use ESLint configurations that you can reference in your projects:

- [ESLint Config Airbnb](https://www.npmjs.com/package/eslint-config-airbnb-typescript) - Airbnb style guide

- [ESLint Base Style Config](https://www.npmjs.com/package/eslint-plugin-base-style-config) - a Set of Essential ESLint rules for JS, TS and React

- [ESLint + Prettier](https://www.npmjs.com/package/eslint-config-prettier) - lint rules for [Prettier](https://github.com/prettier/prettier) code formatter

Refer also to this great [TypeScript StyleGuide and Coding Conventions](https://basarat.gitbook.io/typescript/styleguide) source.

### Migrating from TSLint to ESLint

If you are looking for help in migrating from TSLint to ESLint, you can check out this project: <https://github.com/typescript-eslint/tslint-to-eslint-config>

### Use consistent capitalization

Capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just *be consistent*.

**Bad:**

```ts
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

type animal = { /* ... */ }
type Container = { /* ... */ }
```

**Good:**

```ts
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

const discography = getArtistDiscography('ACDC');
const beatlesSongs = SONGS.filter((song) => isBeatlesSong(song));

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = { /* ... */ }
type Container = { /* ... */ }
```

Prefer using `PascalCase` for class, interface, type and namespace names.
Prefer using `camelCase` for variables, functions and class members.
Prefer using capitalized `SNAKE_CASE` for constants.

**[⬆ back to top](#table-of-contents)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee.
We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Bad:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**Good:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**[⬆ back to top](#table-of-contents)**

### Organize imports

With clean and easy to read import statements you can quickly see the dependencies of current code. Make sure you apply following good practices for `import` statements:

- Import statements should be alphabetized and grouped.
- Unused imports should be removed.
- Named imports must be alphabetized (i.e. `import {A, B, C} from 'foo';`)
- Import sources must be alphabetized within groups, i.e.: `import * as foo from 'a'; import * as bar from 'b';`
- Prefer using `import type` instead of `import` when importing only types from a file to avoid dependency cycles, as these imports are erased at runtime
- Groups of imports are delineated by blank lines.
- Groups must respect following order:
  - Polyfills (i.e. `import 'reflect-metadata';`)
  - Node builtin modules (i.e. `import fs from 'fs';`)
  - external modules (i.e. `import { query } from 'itiriri';`)
  - internal modules (i.e `import { UserService } from 'src/services/userService';`)
  - modules from a parent directory (i.e. `import foo from '../foo'; import qux from '../../foo/qux';`)
  - modules from the same or a sibling's directory (i.e. `import bar from './bar'; import baz from './bar/baz';`)

**Bad:**

```ts
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { Customer, Credentials } from '../model/types';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Good:**

```ts
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';
import type { Customer, Credentials } from '../model/types';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```

**[⬆ back to top](#table-of-contents)**

### Use typescript aliases

Create prettier imports by defining the paths and baseUrl properties in the compilerOptions section in the `tsconfig.json`

This will avoid long relative paths when doing imports.

**Bad:**

```ts
import { UserService } from '../../../services/UserService';
```

**Good:**

```ts
import { UserService } from '@services/UserService';
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@services": ["services/*"]
    }
    ...
  }
...
```

**[⬆ back to top](#table-of-contents)**

## 💬 Comments - When Words Help Explain!

Comments are like little sticky notes you put on your code to help explain what's happening! 📝 But here's a secret - the best code doesn't need many comments because it explains itself!

Think of it like this: if you draw a really good picture of a cat, you don't need to write "This is a cat" underneath it - people can already tell it's a cat! 🐱

> Don't comment bad code—rewrite it.
> — *Brian W. Kernighan and P. J. Plaugher*

The smartest developers say that if your code needs lots of comments to explain what it does, maybe the code itself should be clearer! It's like having a really messy room and putting up signs everywhere saying "clothes go here" and "toys go there" - it's better to just keep the room organized! 🧹

### Prefer self explanatory code instead of comments

Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Bad:**

```ts
// Check if subscription is active.
if (subscription.endDate > Date.now) {  }
```

**Good:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) { /* ... */ }
```

**[⬆ back to top](#table-of-contents)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad:**

```ts
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
}
```

**Good:**

```ts
type User = {
  name: string;
  email: string;
}
```

**[⬆ back to top](#table-of-contents)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use `git log` to get history!

**Bad:**

```ts
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**Good:**

```ts
function combine(a: number, b: number): number {
  return a + b;
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the proper indentation and formatting give the visual structure to your code.
Most IDE support code folding feature that allows you to collapse/expand blocks of code (see Visual Studio Code [folding regions](https://code.visualstudio.com/updates/v1_17#_folding-regions)).

**Bad:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Client class
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public methods
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private methods
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**Good:**

```ts
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**[⬆ back to top](#table-of-contents)**

### TODO comments

When you find yourself that you need to leave notes in the code for some later improvements,
do that using `// TODO` comments. Most IDE have special support for those kinds of comments so that
you can quickly go over the entire list of todos.

Keep in mind however that a *TODO* comment is not an excuse for bad code.

**Bad:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Good:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ back to top](#table-of-contents)**

## Translations

This is also available in other languages:
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [vitorfreitas/clean-code-typescript](https://github.com/vitorfreitas/clean-code-typescript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese**:
  - [beginor/clean-code-typescript](https://github.com/beginor/clean-code-typescript)
  - [pipiliang/clean-code-typescript](https://github.com/pipiliang/clean-code-typescript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [ralflorent/clean-code-typescript](https://github.com/ralflorent/clean-code-typescript)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [mheob/clean-code-typescript](https://github.com/mheob/clean-code-typescript)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [Kornil/clean-code-typescript](https://github.com/kornil/clean-code-typescript)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [MSakamaki/clean-code-typescript](https://github.com/MSakamaki/clean-code-typescript)
- ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [738/clean-code-typescript](https://github.com/738/clean-code-typescript)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Real001/clean-code-typescript](https://github.com/Real001/clean-code-typescript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [3xp1o1t/clean-code-typescript](https://github.com/3xp1o1t/clean-code-typescript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [ozanhonamlioglu/clean-code-typescript](https://github.com/ozanhonamlioglu/clean-code-typescript)
- ![uk](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [KirillPd/clean-code-typescript](https://github.com/KirillPd/clean-code-typescript)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hoangsetup/clean-code-typescript](https://github.com/hoangsetup/clean-code-typescript)

References will be added once translations are completed.
Check this [discussion](https://github.com/labs42io/clean-code-typescript/issues/15) for more details and progress.
You can make an indispensable contribution to *Clean Code* community by translating this to your language.

**[⬆ back to top](#table-of-contents)**