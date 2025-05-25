# OOPS-In-JavaScript

### 🔍 What is OOPs (Object-Oriented Programming)?

**OOPs** is a programming paradigm based on the concept of **"objects"**, which contain **data** (properties) and **methods** (functions).

---

### 🔑 4 Core Principles of OOP:

| Principle         | Description                                              | Example in JS                     |
| ----------------- | -------------------------------------------------------- | --------------------------------- |
| **Encapsulation** | Bundling data and methods, hiding internal state         | Using private fields: `#balance`  |
| **Abstraction**   | Showing only essential features, hiding complexity       | `getBalance()` hides logic inside |
| **Inheritance**   | One class inherits properties/methods from another       | `class Dog extends Animal`        |
| **Polymorphism**  | Same method behaves differently depending on object type | `speak()` in `Animal` vs `Dog`    |

---

### 💡 Example:

```js
class Animal {
  speak() { console.log("Animal speaks"); }
}
class Dog extends Animal {
  speak() { console.log("Dog barks"); }
}
const a = new Animal();
const d = new Dog();
a.speak(); // Animal speaks
d.speak(); // Dog barks
```

---

**OOP makes code reusable, modular, and easier to manage.**

Here's a **deep dive into Object-Oriented Programming (OOPs) concepts in JavaScript**, covering both ES5 and modern ES6+ syntax:

---

### 🔹 1. **Class & Object**

**Class** is a blueprint; **Object** is an instance of it.

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
}
const p1 = new Person("Hari");
p1.greet(); // Hello, I'm Hari
```

---

### 🔹 2. **Encapsulation**

Binding data and methods into a single unit; restrict direct access using closures or `#` private fields (ES2022+).

```js
class Counter {
  #count = 0;
  increment() { this.#count++; }
  getCount() { return this.#count; }
}
const c = new Counter();
c.increment();
console.log(c.getCount()); // 1
```

---

### 🔹 3. **Abstraction**

Hide implementation details. Use methods to expose necessary features only.

```js
class BankAccount {
  #balance = 0;
  deposit(amount) { if (amount > 0) this.#balance += amount; }
  getBalance() { return this.#balance; }
}
```

---

### 🔹 4. **Inheritance**

Use `extends` to inherit from a parent class.

```js
class Animal {
  speak() { console.log("Animal speaks"); }
}
class Dog extends Animal {
  speak() { console.log("Dog barks"); }
}
const d = new Dog();
d.speak(); // Dog barks
```

---

### 🔹 5. **Polymorphism**

Same method name behaves differently in different classes.

```js
class Shape {
  area() { return 0; }
}
class Circle extends Shape {
  constructor(radius) { super(); this.radius = radius; }
  area() { return Math.PI * this.radius * this.radius; }
}
```

---

### 🔹 6. **Static Methods & Properties**

Belong to class, not instance.

```js
class MathUtil {
  static add(a, b) { return a + b; }
}
console.log(MathUtil.add(2, 3)); // 5
```

---

### 🔹 7. **Composition over Inheritance** (Advanced)

Prefer composing behaviors using functions or mixins.

```js
const canFly = (obj) => ({ fly: () => console.log(`${obj.name} flies`) });
const canSwim = (obj) => ({ swim: () => console.log(`${obj.name} swims`) });

function createBird(name) {
  const bird = { name };
  return { ...bird, ...canFly(bird), ...canSwim(bird) };
}
const duck = createBird("Duck");
duck.fly(); duck.swim();
```

Here’s an **OOP mini project in JavaScript (ES6)** that demonstrates all core OOP principles:

---

### 📘 **Project: Library Management System**

#### ✅ Features:

* Add books
* Borrow/return books
* Track borrowed status

---

### 🧱 **Code:**

```js
// Base Class
class Book {
  constructor(title, author) {
    this.title = title;
    this.author = author;
    this.isBorrowed = false;
  }

  borrow() {
    if (!this.isBorrowed) {
      this.isBorrowed = true;
      console.log(`${this.title} borrowed.`);
    } else {
      console.log(`${this.title} is already borrowed.`);
    }
  }

  returnBook() {
    this.isBorrowed = false;
    console.log(`${this.title} returned.`);
  }
}

// Subclass (Inheritance)
class EBook extends Book {
  constructor(title, author, fileSizeMB) {
    super(title, author);
    this.fileSizeMB = fileSizeMB;
  }

  download() {
    console.log(`Downloading ${this.title} (${this.fileSizeMB}MB)...`);
  }
}

// Library Class (Encapsulation & Abstraction)
class Library {
  #books = [];

  addBook(book) {
    this.#books.push(book);
  }

  listBooks() {
    this.#books.forEach(b => console.log(`${b.title} by ${b.author} - Borrowed: ${b.isBorrowed}`));
  }

  findBook(title) {
    return this.#books.find(b => b.title === title);
  }
}

// Usage
const lib = new Library();
const book1 = new Book("Atomic Habits", "James Clear");
const ebook1 = new EBook("Deep Work", "Cal Newport", 5);

lib.addBook(book1);
lib.addBook(ebook1);

lib.listBooks();
book1.borrow();
ebook1.download();
ebook1.borrow();
lib.listBooks();
```

---

### 💡 OOP Concepts Applied:

* **Class/Object**: `Book`, `EBook`, `Library`
* **Encapsulation**: `#books` is private in `Library`
* **Inheritance**: `EBook` inherits `Book`
* **Polymorphism**: Overriding methods in `EBook`
* **Abstraction**: `Library` hides book list logic


Here’s the **same mini project in ES5 syntax** using constructor functions and prototypes:

---

### 📘 **Library Management System – ES5 Version**

```js
// Book constructor
function Book(title, author) {
  this.title = title;
  this.author = author;
  this.isBorrowed = false;
}

Book.prototype.borrow = function() {
  if (!this.isBorrowed) {
    this.isBorrowed = true;
    console.log(this.title + " borrowed.");
  } else {
    console.log(this.title + " is already borrowed.");
  }
};

Book.prototype.returnBook = function() {
  this.isBorrowed = false;
  console.log(this.title + " returned.");
};

// EBook constructor (Inheritance)
function EBook(title, author, fileSizeMB) {
  Book.call(this, title, author); // Inherit Book props
  this.fileSizeMB = fileSizeMB;
}
EBook.prototype = Object.create(Book.prototype); // Inherit methods
EBook.prototype.constructor = EBook;

EBook.prototype.download = function() {
  console.log("Downloading " + this.title + " (" + this.fileSizeMB + "MB)...");
};

// Library constructor (Encapsulation)
function Library() {
  var books = [];

  this.addBook = function(book) {
    books.push(book);
  };

  this.listBooks = function() {
    books.forEach(function(b) {
      console.log(b.title + " by " + b.author + " - Borrowed: " + b.isBorrowed);
    });
  };

  this.findBook = function(title) {
    return books.find(function(b) {
      return b.title === title;
    });
  };
}

// Usage
var lib = new Library();
var book1 = new Book("Atomic Habits", "James Clear");
var ebook1 = new EBook("Deep Work", "Cal Newport", 5);

lib.addBook(book1);
lib.addBook(ebook1);

lib.listBooks();
book1.borrow();
ebook1.download();
ebook1.borrow();
lib.listBooks();
```

---

