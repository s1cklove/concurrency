# How to code in `exe`?

## (Be) General

I use C++20. As soon as C++26 will be supported well enough, I aim to switch to it (older versions problems are greatly shown in [visitor.hpp](source/exe/fiber/chan/detail/visitor.hpp)). No compiler-specific optimizations or attributes should be used. _Be general._

## Codestyle

First, the style must correspond to `clippy validate` formatting style, which includes, for example, mandatory `k` prefix for numeric constants.

### Imports

An import section should look like this _(excluding these comments, of course)_:
```
// 1. Subtree imports (including brothers)
#include "circular.hpp"
#include "select/clause.hpp"
#include "select/receive_clause.hpp"
#include "select/send_clause.hpp"
#include "waiter.hpp"

// 2. `exe` imports that are not in our subtree
#include <exe/fiber/chan/select.hpp>
#include <exe/thread/spinlock.hpp>
#include <exe/util/empty.hpp>

// 3. Non-standard dependencies
#include <twist/ed/std/lock_guard.hpp>
#include <vvv/list.hpp>

// 4. STL
#include <optional>
#include <utility>
```
Imports in each block should be sorted alphabetically.

### Names

PascalCase for any structs, classes, functions and methods. Pure virtual classes have to be marked with `I` prefix:
```cpp
class Unit;
class ITrampoline;
void DoSomething();
```

_Any_ class/struct fields must have trailing `_`: in concurrent code, you're not really intended to access fields directly, so you have to be aware of it.

### Namespaces

Namespaces should always be lowercased and logically separated: there's no strict rule for what should be a namespace.

However, if a file is pure implementation-driven (and does not provide any interface for users) -- put it into a `detail` subfolder and namespace. `exe` code has to be self-documenting.

You may also use sepapator lines (80 `/` characters) if you feel this way:

```cpp
// Thread-local URBG for poll ordering
// https://en.cppreference.com/cpp/algorithm/random_shuffle
inline std::mt19937& GetTwister() {
  ...
}

//////////////////////////////////////////////////////////////////////

// Definitions

using Index = uint32_t;

template <std::size_t I>
using Ordering = std::array<Index, I>;

template <typename... Clauses>
using Variant = std::variant<typename Clauses::ReturnType...>;

template <typename... Clauses>
using PollResult = std::optional<Variant<Clauses...>>;

template <typename... Clauses>
using Waiters = std::tuple<
    SelectWaiter<typename Clauses::DataType, Clauses::IsBuffered>...>;

template <std::size_t I, typename... Clauses>
using ClauseType = std::tuple_element_t<I, std::tuple<Clauses...>>;

//////////////////////////////////////////////////////////////////////

// Locking

template <typename... Clauses>
auto LockOrder(std::tuple<Clauses...>& clauses)
    -> Ordering<sizeof...(Clauses)> {
  INDEXED_AS(I)
  ...
```

### Guards & definitions

`#define` is allowed but not encouraged (example above).

`#pragma once` is used in each `.h` file.

### Indents

2 spaces. Microphone drops away.

### TODO

File size suffixes are not supported yet, same as custom error handlers, asserts, etc.

This might mean that no custom failure logic was implemented yet -- and that's right.

The most important feature to implement is, for sure, logging macros.

## Practice

### Integral types

Use unsigned types if possible ([committee](youtu.be/Puio5dly9N8?t=2558) stay away please).

Use `(u)int<size>_t`, like `int64_t`. Integral typenames must be verbose to avoid errors (mostly ABA) they might otherwise cause.

### Consider `auto`?

Sometimes, `auto` is fine and even encouraged:
```cpp
auto state = new ChannelState();
```
As a Haskell enjoyer, I usually use trailing return types:
```cpp
template <typename... Clauses>
auto LockOrder(std::tuple<Clauses...>& clauses)
    -> Ordering<sizeof...(Clauses)>
```
...But `auto` is not always the best solution:
```cpp
uint32_t winner = select_state.Winner();
```
So, only use it when the type is verbosely obvious from its context.

### Forward declarations

In case you don't want to show implementation details to user's autocompletion (in Fiber, for example: you don't really need to declare it), create a `fwd.hpp` in the directory and consider forward-declaration inside of it.
