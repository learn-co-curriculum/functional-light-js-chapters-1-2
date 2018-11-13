# Functional-Light JS
#### _Kyle Simpson_

## Foreword

Tenents of "Functional-Light Programming"
- practical & pragmatic approach over top-down academic approach
- understanding over terminology and jargon
- humans over computers
- apply where useful over prescriptive absolutism

>"I believe that programming is fundamentally about humans, not about code. I believe that code is first and foremost a means of human communication, and only as a side effect (hear my self-referential chuckle) does it instruct the computer."

> "I think that absolutism is bogus"

## Chapter 1: Why Functional Programming
### Confidence
"Trust": Trusting code is more than just running the tests. You trust what you understand; you understand what you trust

> I mean that you can verify, by reading and reasoning, not just executing, that you understand what a piece of code will do; you aren’t just relying on what it _should_ do.

### Communication

> For example, once you learn what map(..) does, you’ll be able to almost instantly spot and understand it when you see it in any program. But every time you see a for loop, you’re going to have to read the whole loop to understand it. The syntax of the for loop may be familiar, but the substance of what it’s doing is not; that has to be read, every time.

:points up: good point!

### Readability

**Imperative:** _How_ something is done. To understand you must step through at a low level.

**Declarative:** Describe _what_ outcome you want at a high level.

Declarative over Imperative

Explicit over Implicit

### Resources
Books.
Some FP/JavaScript books that you should definitely read:

- [Professor Frisby’s Mostly Adequate Guide to Functional Programming by Brian Lonsdorf](https://drboolean.gitbooks.io/mostly-adequate-guide/content/ch1.html)
- [JavaScript Allongé by Reg Braithwaite](https://leanpub.com/javascriptallongesix)
- [Functional JavaScript by Michael Fogus](http://shop.oreilly.com/product/0636920028857.do)

Blogs/sites.
Some other authors and content you should check out:

- [Fun Fun Function Videos by Mattias P Johansson](https://www.youtube.com/watch?v=BMUiFMZr7vk)
- [Awesome FP JS](https://github.com/stoeffel/awesome-fp-js)
- [Kris Jenkins](http://blog.jenkster.com/2015/12/what-is-functional-programming.html)
- [Eric Elliott](https://medium.com/@_ericelliott)
- [James A Forbes](https://james-forbes.com/)
- [James Longster](https://github.com/jlongster)
- [André Staltz](http://staltz.com/)
- [Functional Programming Jargon](https://github.com/hemanth/functional-programming-jargon#functional-programming-jargon)
- [Functional Programming Exercises](https://github.com/InceptionCode/Functional-Programming-Exercises)

## Chapter 2: The Nature of Functions
### Functions vs Procedures
Basic definition of a function:

> "a function is a collection of code that can be executed one or more times."

Procedure:

> A procedure is an arbitrary collection of functionality. It may have inputs, it may not. It may have an output (return value), it may not.

To use a function _as a function_ and not just a procedure

> "But let’s be clear what a function is. It’s not just a collection of statements/operations. Specifically, a function needs one or more inputs (ideally, just one!) and an output.

![letmebeclear.jpeg](http://www.quickmeme.com/img/27/27da477625e9460e59ec78eecf65f07d4a9a241c226fe3c64df6b62c2e025a4b.jpg)

One set of values map to another set of values (like a graph in high school math class)

### Function Inputs
Arguments: values you pass in
Parameters: named variables that receive the passed in values

ES6!
- default parameters
  - `function sayHi(name = 'alex')`
  - careful against overusing
- ...spread operator (or 'rest' or 'gather')
  - `function (foo, ...bar) {}`
  - `const array = [x,y,z]; myFunction(...arr)`
- parameter destructuring
  - `function foo([first, second]) {}`
  - `function bar({ event, payload }) {}`
  - benefits of parameter destructuring with an object: **Named Arguments**; **Unordered Arguments**
  - Manual variable assignment === imperative. You have to mentally execute each step to understand desired outcome.
  - Destructuring approach === declarative. Becomes self-explanatory!

### Function Outputs
- One output
  - ES6 destrucuring of an array or object is a good way to get around 1 output
  - but.. are u sure u need more than one output
- `return` is also control flow, but gets confusing real quick
- Things we want to avoid (in the name of being explicit over implicit)
  - un`return`ed values
  - side effects
  - good example of sneaky side effect:

```js
function sum(list) {
  var total = 0;
  for (let i = 0; i < list.length; i++) { if (!list[i]) list[i] = 0;
          total = total + list[i];
      }
  return total;
}
var nums = [ 1, 3, 9, 27, , 84 ];
sum( nums );  
```

### Higher Order Functions
Functions that return functions, functions that receive other functions.

- :thumbs up: Closures
  - >Closure is when a function remembers and accesses variables from outside of its own scope, even when that function is executed in a different scope. currying

```js
function expect(actual) {
  return {
    to_eq: function(expected) {
      return actual === expected
    }
  }
}

expect(sum(1,2)).to_eq(3)
// when `to_eq` is invoked it has the value of expected inside the closure
```

- :thumbs down: Anonymous Functions
  - he _really_ doesn't like anonymous functions
  - "named functions are always more preferable to anonymous functions"
  -  the name shows up in the stack trace!
- :thumbs down: `this`-aware functions
  - `this` is only an implicit arg
  - we wanna be explicit `f(context)`, pass it thru!

## Discussion Questions

1 - What's the role of JavaScript here? The introduction makes it clear that this is about the underlying concepts and there's still value in this book even if you apply to other languages, but then we immediately dive into the low-level details of ES6 syntax. Not exactly a criticism, it's important to cover these fundamentals, just opening the question is this a JS book or a FP book?

2 - Definitely agree with the benefits of parameter destructuring, but where do we draw the line?
This seems fine:

```js
const MyComponent = ({ currentUser, loading, lesson }) => {
  // render stuff
}
```

But what do we think about:

```js
const MyComponent = ({ currentUser: { learnUsername: username, id, onEnterpriseServer }, loading, lesson: { title, type, remote, unit: { iteration: title }}}) => {
  // render more specific stuff
}
```

3 - One place I'm apt to agree with him around avoiding all of the ES6 arrow function syntax differentiations is in this scenario:

```js
somePromise.then((payload) => processPayload(payload))
```

then ... a bug appears ... you want to console log in the callback. A multi-line change is required:

```js
somePromise.then((payload) => {
  console.log('what is this thing even???', payload)
  return processPayload(payload)
}
```

And then you have to put it back to the way it was when finished.
A silly example but, ya, how do we feel about the tradeoffs of the shorthand syntax? Has anyone been burned by having anonymous functions show up in stack trace?

4 - Here's an example of some very non-functional, side-effecty code from Ironboard. it's not easy to follow:

```ruby
class TrackSwitcherInfoBuilder < BaseService

  def self.info_hash_for_user(user)
    new(user).info_hash_for_user
  end

  def initialize(user)
    @user = user
    @removed_track_ids = []
  end

  def info_hash_for_user
    if user.admin? || user.teaching_assistant?
      query = batch_track_query_for_admin
    else
      filter_student_track_ids
      query = batch_track_query_for_student
    end

    unless removed_track_ids.empty?
      query = query.
        where('batch_tracks.track_id NOT IN (?)', removed_track_ids)
    end

    query.
      as_json.map(&:with_indifferent_access)
  end

  private

  attr_reader :user, :removed_track_ids

  def filter_student_track_ids
    filter_cpb_tracks if user.has_role?(Batch.community_powered_bootcamp, 'student')
    filter_fswd_tracks if user.learn_verified_enrollment && !user.learn_verified_enrollment.graduated?
  end

  def batch_track_query_for_admin
    user.batch_tracks.
      joins(:batch).
      joins(:track).
      select('
        batch_tracks.id,
        batch_tracks.batch_id,
        batches.iteration AS batch_iteration,
        batch_tracks.track_id,
        curriculums.title AS track_title
      ').
      order('LOWER(curriculums.title), LOWER(batches.iteration)')
  end

  def batch_track_query_for_student
    user.batch_tracks.
      joins(:batch).
      joins(:track).
      select('
        DISTINCT ON (curriculums.title)
        batch_tracks.id,
        batch_tracks.batch_id,
        batches.iteration AS batch_iteration,
        batch_tracks.track_id,
        curriculums.title AS track_title
      ').
      order('curriculums.title, batches.created_at')
  end

  def filter_fswd_tracks
    removed_track_ids.concat(Track.fswd_track_ids - [Track.verified_web_development_track(user).id])
    if $rollout.active?(:curriculum_swap_v6, user)
      remove_v6_from_filter
    end
  end

  def filter_cpb_tracks
    if !user.has_completed_cpb_onboarding_track?
      # Hide CPB full track if student has not completed CPB welcome track
      removed_track_ids.concat([Track.cpb_full_track_id, Track.cpb_full_track_v5_id])
    end


  end

  def remove_v6_from_filter
    removed_track_ids.delete(Track.verified_web_development_v6_track_id)
  end
end
```

Anyone got other examples/stories of things that could be refactored to use FP principles? Places where using FP ideas made life easier?

5 - This is the first meeting! What do people want to get out of this book?
