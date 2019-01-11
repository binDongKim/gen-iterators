# Generators in Javascript(ES6)

---

## Iterables

- In order to be **iterable**, an object must implement the **@@iterator** method, which returns an *Iterator* object.
- All iterators created by generators are also iterables, as generators assign the **Symbol.iterator** method by default.
- Arrays, Strings, Maps, Sets

---

## Iterators in Javascript

- An iterator is any object which implements the *Iterator protocol* by having a `next()` method which returns an object with two properties: `value` and `done`.
- Once created, an iterator object can be iterated explicitly by repeatedly calling `next()`.

---

## More about Iterators and Iterables

In order to build our own iterable we need to follow the iteration protocol, which says:

- An object becomes an iterable if it implements a function whose key is `Symbol.iterator` and returns an `iterator`.
- The iterator itself is an object with a function called `next` inside it. next should return an object with two keys `value` and `done`. `value` contains the next element of the iteration and `done` a flag saying if the iteration has finished.

---

## Consuming Iterators

- for ..of loop
- Spread syntax 

---

## Create our own iterable

```javascript
const iterable = {
    [Symbol.iterator]() {
        let data = ['foo','bar']
        return { // Iterator
            next() {
                return {
                    done: data.length === 0,
                    value: data.pop()
                }
            }
        }
    }
}

for(let e of iterable) {
    console.log(e)
    // 'foo'
    // 'bar'
}
```

---

## Iterator as an Iterable

```javascript

const iterable = {
    data: ['foo','bar'],
    next() {
        return {
            done: this.data.length === 0,
            value: this.data.pop()
        }
    },
    [Symbol.iterator]() {
        // Return the iterable itself.
        return this
    }
}
```
This is the pattern followed by ES6 built-in iterators.

---

## Generators in Javascript

```javascript
function* generator() {
	// statement
}

const genObj = generator(); // Generator object is created
```

- `function*` defines a *generator* function.
- **Calling a generator function does not execute its body immediately**; an `iterator` object for the function is return instead.
- When the iterator's `next()` method is called, the generator function's body is executed until the first `yield` expression.
- The `yield` expression specifies the value to be returned from the iterator.

---

## Generators in Javascript

- The `next()` method returns an object with a `value` property containing the yielded value and a `done` property which indicates whether the generator has yielded its last value.
- Calling the `next()` method with an argument will resume the generator function execution, replacing the `yield` expression where execution was paused with the argument from `next()`.
  ```javascript
  function* logGenerator() {
    console.log(0);
    console.log(1, yield);
  }

  const gen = logGenerator();

  gen.next();             // 0
  gen.next('pretzel');    // 1 pretzel
  ```
  
---

## Generators in Javascript

- A `return` statement in a generator, when executed, will make the generator finished(i.e. the `done` property of the generator object will be set to `true`). If a value is `returned`, it will be set as the `value` property of the object returned by the generator.
- When a generator is finished, subsequent `next` calls will just return an object of this form: `{ value: undefined, done: true }`.

	```javascript
    function* yieldAndReturn() {
      yield "Y";
      return "R";
      yield "unreachable";
    }

    const gen = yieldAndReturn()
    
    console.log(gen.next()); // { value: "Y", done: false }
    console.log(gen.next()); // { value: "R", done: true }
    console.log(gen.next()); // { value: undefined, done: true }
    ```
    
---

## Iterators and Generators

- When generator functions(with the `function*` syntax) are called initially, they don't execute any of their code, instead returning a type of iterator called a Generator.
- When a value is consumed by calling the generator's **next** method, the generator function executes until it encounters the **yield** keyword.

---

## Function and Generator

**Functions** feature a behaviour called run-to-completion, which means that once a function is executed, it runs till it finishes its task.
Whereas, **Generators** are functions which can be exited and later re-entered. Their context(variable bindings) will be saved across re-entrances. The purpose of a generator is to implement a function without the run-to-completion behavior. Instead, it returns an iterator object which holds the control flow. A generator is a function that produces a sequence of results instead of a single value, i.e you generate a series of values.

---

## An simple analogy for generator functions

Imagine you are reading a book. A pizza delivery guy arrive and you get up to open the door. However, before doing that, you set a **bookmark** at the last page you read. You mentally save the events of the plot. Then, you go and get your pizza. Once you return back to your room, you begin the book from the page that you set the bookmark on. You don't begin it from the first page again. In a sense, you acted as a generator function.

---

## Function and Generator

<img src="https://cdn-images-1.medium.com/max/800/1*7X8rtWOiz5RKENZ_vugmKg.png" style="width: 900px; height: 550px">

---

## Implementing Iterables

```javascript
const iterableObj = {
  [Symbol.iterator]() {
    let step = 0;
    return {
      next() {
        step++;
        if (step === 1) {
          return { value: 'This', done: false};
        } else if (step === 2) {
          return { value: 'is', done: false};
        } else if (step === 3) {
          return { value: 'iterable.', done: false};
        }
        return { value: '', done: true };
      }
    }
  },
}
for (const val of iterableObj) {
  console.log(val);
}
// This
// is 
// iterable.
```

---

## Implementing Iterables using generators

```javascript
function* iterableObj() {
  yield 'This';
  yield 'is';
  yield 'iterable.'
}

for (const val of iterableObj()) {
  console.log(val);
}
// This
// is 
// iterable.
```

---

## References

- [function* description in MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)
- [Understanding Generators in ES6 Javascript](https://codeburst.io/understanding-generators-in-es6-javascript-with-examples-6728834016d5)
- [A Simple Guide to Understanding Javascript Generators](https://medium.com/dailyjs/a-simple-guide-to-understanding-javascript-es6-generators-d1c350551950)
- [Introduction to javascript iterables, iterators and generators](https://medium.com/dubizzletechblog/introduction-to-javascript-iterables-iterators-and-generators-a26be413dfd9)
- [Demystifying ES6 Iterables & Iterators](https://medium.freecodecamp.org/demystifying-es6-iterables-iterators-4bdd0b084082)
- [Difference between Symbol.iterator and @@iterator](https://stackoverflow.com/questions/29670320/es-6-difference-between-symbol-iterator-and-iterator)
- [Why do you access Symbol.iterator via brackets?](https://stackoverflow.com/questions/51489239/why-do-you-access-symbol-iterator-via-brackets)