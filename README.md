# Strings Lab

## Learning Goals

- Concatenate strings with the `+` operator
- Interpolate variables and other JavaScript expressions inside template literals
- Read the MDN documentation on string methods and practice using a few

## Introduction

For this lab, you've just been onboarded to the dev team working on Flatbook,
the world's premier social network. At the moment, the
view that our users see upon logging in is pretty generic. We'd like to improve
the user experience by adding some custom greeting capabilities.

## Work Through Failing Tests in a JavaScript Test Suite

To start off, let's install our dependencies with `npm i`. Once that's finished, run the test suite with the `npm test` command. Our code
is currently failing all of the tests, but we expected that because we haven't
done anything yet. Let's get to work!

### `currentUser`

The first test is telling us that `currentUser` is not defined. Let's go to
`index.js` and write the following code:

```js
const currentUser = 'Grace Hopper';
```

> **Note**: Generally, when the tests ask you to define something, you want to
> define it exactly as indicated in the test. But in this case, you don't have
> to write `'Grace Hopper'`, because the important part is the variable name:
> `currentUser`.  You can use your own name, your pet's name, your favorite
> programmer's name — whatever you'd like.

Rerun the tests and you should see that the first one is passing.

### `welcomeMessage`

The next failing test is similarly helpful, telling us exactly what we have to
fix: `welcomeMessage contains "Welcome to Flatbook, "`.

Let's return to `index.js` and define our second variable below where we
declared `currentUser`:

```js
const currentUser = 'Grace Hopper';

const welcomeMessage = 'Welcome to Flatbook, ';
```

Rerun the tests; you should see a second passing test.

The third test tells us that `welcomeMessage` should contain the value stored in
`currentUser`. This seems like it might contradict the second test a bit, but
let's try it out. Let's erase `'Welcome to Flatbook, '` and set `welcomeMessage`
equal to `currentUser` instead:

```js
const currentUser = 'Grace Hopper';

const welcomeMessage = currentUser;
```

When we rerun the tests, we still have two passing. But now the first and third
tests are passing instead of the first and second! That doesn't seem quite
right.

It turns out that the tests want `welcomeMessage` to include _both_ `'Welcome to
Flatbook, '` and the value stored in `currentUser`. Maybe we can include both of
them in a single string?

```js
const currentUser = 'Grace Hopper';

const welcomeMessage = 'Welcome to Flatbook, currentUser';
```

If we rerun the tests, we're once again passing the second test, but we're back
to failing the third test. The new error message for the third test gives us a
hint about what's happening:

```js
AssertionError: expected 'Welcome to Flatbook, currentUser' to contain 'Grace Hopper'
```

When JavaScript is expecting a variable to contain one thing, and it does not,
that is known as an `AssertionError`. The test suite looked at the value stored
in `welcomeMessage` and expected to find the string `'Grace Hopper'`, which is
the value stored in `currentUser`. Instead, `welcomeMessage` contains the
literal string `"currentUser"`. It's important to understand the distinction:

- `currentUser` is a _variable_ that contains a string (`'Grace Hopper'` in our examples).
- `'currentUser'` is a _string_, ***not a variable***.

The JavaScript engine sees a matching pair of single quotes (`' '`), creates a
new string, and assumes that _everything_ in between the matching punctuation
marks is part of that string. 

Since we want `welcomeMessage` to contain both `'Welcome to Flatbook, '` and the
_value_ stored in `currentUser`, we have two options: **concatenation** and
**interpolation**.

## Concatenate Strings with the `+` Operator

String concatenation is a way to take two strings and add one to the other,
creating a single, longer string. We just learned how to do so by using the `+` operator, like so:

```js
"High " + "five!";
//=> "High five!"

"We" + ' ' + `can` + " " + 'concat' + `enate` + " as many strings " + 'as our heart ' + `desires.`;
//=> "We can concatenate as many strings as our heart desires."
```

Since our `currentUser` variable contains a string, we can concatenate it to the
end of `'Welcome to Flatbook, '` to dynamically create a new string based on
whatever value `currentUser` contains at a given moment:

```js
const currentUser = 'Grace Hopper';

const welcomeMessage = 'Welcome to Flatbook, ' + currentUser;
```

If we run the test suite with our updated code, we'll see both the second and
third tests passing! However, before we move on, let's review interpolation.

## Interpolate Variables and Other JavaScript Expressions Inside Template Literals

String interpolation lets us dynamically insert values in the middle of a
string. To do this, use [template literals][template literals], which
are simply strings wrapped in backticks rather than single or double quotes.
Template literals enable us to interpolate the value of a variable into a string
by wrapping the variable in curly braces preceded by a dollar sign:
`${yourVariable}`. The `${}`, when inside backticks, tells the JavaScript engine
that it needs to *interpret the value* of `yourVariable` and insert that value
into the string. If you forget to use the backticks and use single or double
quotes instead, the dollar sign, curly braces and variable name will all be
inserted into the string instead of the variable's value.

Unlike string concatenation, template literals will also allow you to use
multi-line strings. Wrapping the string in backticks preserves any new lines
when the string is returned or output.

```js
const myString = 'template literal';

const myNumber = 10;

const myBoolean = false;

`Saying that interpolation with ${myString}s is better than concatenation ${90 + myNumber}% of the time is simply ${myBoolean}. But it is pretty cool!

Beware that new lines inside of a ${myString} will be preserved as new lines in the resulting ${typeof myString}!`;
//=> "Saying that interpolation with template literals is better than concatenation 100% of the time is simply false. But it is pretty cool!

// Beware that new lines inside of a template literal will be preserved as new lines in the resulting string!"
```

Note that, in the example above, one of the things we interpolated into our
string is an arithmetic expression: `${90 + myNumber}`. We aren't limited to
interpolating just variables — we can use *any expression* inside the
curly braces.

While, for most purposes, the choice of whether to use concatenation or string
interpolation is primarily a matter of personal preference, JavaScript
programmers tend to use string interpolation for all but the simplest of cases.

Let's rewrite our `welcomeMessage` to use a template literal:

```js
const currentUser = 'Grace Hopper';

const welcomeMessage = `Welcome to Flatbook, ${currentUser}`;
```

The first three tests are still passing, but the fourth wants our
`welcomeMessage` to end with an exclamation point. The fix is as simple as
adding a `!` as the last character in the template literal:

```js
const currentUser = 'Grace Hopper';

const welcomeMessage = `Welcome to Flatbook, ${currentUser}!`;
```

Four tests down, six to go!

## Read the MDN Documentation on String Methods and Practice One

### `excitedWelcomeMessage`

Sometimes we get so excited when someone logs into their Flatbook account that
we just want to shout out loud. We *could* copy over most of the code from
`welcomeMessage` and then change every character to its uppercase equivalent,
but as developers we try not to repeat ourselves. Instead, let's use the
`.toUpperCase()` string method:

```js
const currentUser = 'Grace Hopper';

const welcomeMessage = `Welcome to Flatbook, ${currentUser}!`;

const excitedWelcomeMessage = welcomeMessage.toUpperCase();
```

All strings in JavaScript have access to the same set of default methods, which
are common operations like changing a string and returning the new version,
searching through a string for specific character(s) and returning the match,
and so on. For example, we can use [`.toUpperCase()`][toUpperCase] and
[`.toLowerCase()`][toLowerCase] on a string to make the entire string uppercase
or lowercase. There are lots of other [string methods][string methods] that
you'll find useful at various points throughout your JavaScript programming
career.

Rerun the tests; you should see all seven tests passing. Woohoo! Great work!

## Submit Assignment 

Now that you've passed all tests, don't forget to submit your assignment. We'll 
walk through the steps once more, but in the future this should start to feel 
familiar enough that you no longer need these instructions! 

**Push your work to GitHub**
1. Track all the work you did for the lab using `git add -A`.
  - Again, make sure you are in the root directory of the cloned lab when running this command.
2. Save all the work you just tracked using `git commit -m "some message here"` 
  - Be sure your message is informative but succint, for example "complete lab".
3. Push all the work you tracked and save to your forked version of the lab on GitHub using `git push origin main` 

**Submit your work on Canvas**
1. Copy the link for your GitHub fork of the lab.
  - Check the link to make sure you see **your** username. If it's not there, you are _not_ copying your forked version.
2. Go back to the Canvas assignment for this lab and click the **Start Assignment** button at the top. 
3. Paste in the copied link into the Website URL box and hit submit.

## Resources

- [StackExchange – How to open the JavaScript console][open JS console]
- [MDN — Template literals][template literals]
- [DRY — Don't Repeat Yourself][DRY]
- [MDN — String — `.length`][length]
- [MDN — String — Character access][character access]
- [MDN — String — Methods][string methods]
  - [MDN — `.toUpperCase()`][toUpperCase]
  - [MDN — `.toLowerCase()`][toLowerCase]

[open JS console]: https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers/77337#77337
[template literals]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals
[DRY]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself
[string methods]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Methods_2
[length]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length
[character access]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Character_access
[toUpperCase]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase
[toLowerCase]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase