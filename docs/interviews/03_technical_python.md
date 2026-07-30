# 3️⃣ Technical Interview — Python & Software Engineering

> Part of the [Interview Handbook](README.md). Covers the Python and core CS knowledge tested in technical screens and onsite loops for AI/software engineering roles.

## 📑 Contents
- [How this round is graded](#how-this-round-is-graded)
- [Python Fundamentals](#python-fundamentals)
- [OOP & SOLID](#oop--solid)
- [Decorators, Generators, Iterators, Context Managers](#decorators-generators-iterators-context-managers)
- [Concurrency: Asyncio, Threading, Multiprocessing](#concurrency-asyncio-threading-multiprocessing)
- [Memory Management & Garbage Collection](#memory-management--garbage-collection)
- [Type Hinting](#type-hinting)
- [Design Patterns](#design-patterns)
- [Error Handling & Logging](#error-handling--logging)
- [Networking & File Handling](#networking--file-handling)
- [Practice Questions by Difficulty](#practice-questions-by-difficulty)
- [Cheat Sheet](#cheat-sheet)
- [Common Mistakes](#common-mistakes)

---

## How this round is graded
Interviewers are usually scoring four things, not just "did the code run":
1. **Correctness** — does it handle edge cases (empty input, None, huge input)?
2. **Idiom** — do you write Python the way a Python team writes Python (comprehensions, context managers, `enumerate`/`zip` instead of index juggling)?
3. **Communication** — do you narrate trade-offs out loud before/while coding?
4. **Depth on follow-up** — can you explain *why* under the hood (e.g., why a `list` append is amortized O(1))?

---

## Python Fundamentals

### Mutability
- Mutable: `list`, `dict`, `set`, custom objects by default.
- Immutable: `int`, `float`, `str`, `tuple`, `frozenset`.
- Classic trap: default mutable arguments.
```python
def add_item(item, bucket=[]):   # BUG: bucket is shared across calls
    bucket.append(item)
    return bucket

def add_item(item, bucket=None):  # fix
    if bucket is None:
        bucket = []
    bucket.append(item)
    return bucket
```

### `==` vs `is`
`==` compares value equality (calls `__eq__`); `is` compares object identity. Never use `is` to compare strings/ints except against singletons (`None`, `True`, `False`).

### Comprehensions
```python
squares = [x**2 for x in range(10) if x % 2 == 0]
lookup  = {k: v for k, v in pairs}
gen     = (x**2 for x in range(10**9))  # lazy, O(1) memory
```

### GIL (Global Interpreter Lock)
CPython's GIL allows only one thread to execute Python bytecode at a time. Consequence: threads help with I/O-bound work (network, disk) but not CPU-bound work — use `multiprocessing` or a C-extension/`numpy` for the latter.

---

## OOP & SOLID

| Principle | Meaning | Python example |
|---|---|---|
| **S**ingle Responsibility | A class should have one reason to change | Split `ReportGenerator` (builds data) from `ReportExporter` (writes PDF) |
| **O**pen/Closed | Open for extension, closed for modification | Use strategy pattern / plugins instead of editing a giant `if/elif` chain |
| **L**iskov Substitution | Subclasses must be usable wherever the base class is expected | A `Square(Rectangle)` that overrides `set_width` to also change height breaks LSP |
| **I**nterface Segregation | Prefer many small interfaces over one fat one | Use `Protocol` classes scoped to what a consumer actually needs |
| **D**ependency Inversion | Depend on abstractions, not concretions | Inject a `Notifier` interface instead of hardcoding `EmailSender` |

### Dunder methods interviewers probe
`__init__`, `__repr__` vs `__str__`, `__eq__`/`__hash__` (must define both or objects become unhashable inconsistently), `__len__`, `__iter__`, `__enter__`/`__exit__`, `__call__`.

---

## Decorators, Generators, Iterators, Context Managers

### Decorators
```python
import functools, time

def timed(func):
    @functools.wraps(func)   # preserves __name__/__doc__ — interviewers check for this
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.perf_counter() - start:.4f}s")
        return result
    return wrapper

@timed
def slow_add(a, b):
    return a + b
```
Parametrized decorator (decorator factory):
```python
def retry(times=3):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(times):
                try:
                    return func(*args, **kwargs)
                except Exception:
                    if attempt == times - 1:
                        raise
        return wrapper
    return decorator
```

### Generators & Iterators
- **Iterable**: implements `__iter__`. **Iterator**: implements `__iter__` and `__next__`.
- Generators are the easiest way to build an iterator — `yield` pauses/resumes state automatically.
- Use for streaming large files, infinite sequences, or memory-bound pipelines.
```python
def read_large_file(path):
    with open(path) as f:
        for line in f:
            yield line.strip()
```

### Context Managers
```python
class ManagedFile:
    def __init__(self, path):
        self.path = path
    def __enter__(self):
        self.file = open(self.path)
        return self.file
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
        return False  # don't suppress exceptions

# or, simpler:
from contextlib import contextmanager
@contextmanager
def managed_file(path):
    f = open(path)
    try:
        yield f
    finally:
        f.close()
```

---

## Concurrency: Asyncio, Threading, Multiprocessing

| Model | Best for | Why |
|---|---|---|
| `asyncio` | High-concurrency I/O (many network calls, API servers) | Single thread, cooperative event loop, no GIL contention, cheap "tasks" |
| `threading` | Moderate I/O concurrency, blocking libraries without async support | Bypasses GIL only during I/O waits |
| `multiprocessing` | CPU-bound work (parsing, ML preprocessing, image processing) | Separate processes, separate GIL each, true parallelism |

```python
import asyncio

async def fetch(url, session):
    async with session.get(url) as resp:
        return await resp.text()

async def main(urls):
    import aiohttp
    async with aiohttp.ClientSession() as session:
        return await asyncio.gather(*(fetch(u, session) for u in urls))
```
**Interview trap:** "Why not just use threads for everything?" → GIL means CPU-bound threads don't actually run in parallel; you're paying context-switch cost for no gain.

---

## Memory Management & Garbage Collection
- CPython uses **reference counting** as the primary mechanism — an object is freed the instant its refcount hits 0.
- A **generational garbage collector** additionally handles **reference cycles** (e.g., two objects referencing each other) that refcounting alone can't clean up.
- `gc.collect()` forces a cycle collection; `sys.getrefcount(obj)` inspects refcount (mind it's inflated by 1 from the call itself).
- Common memory leak sources: growing global caches, unclosed file/socket handles, closures holding large objects, circular refs with `__del__` (pre-3.4 issue, mostly resolved now).

## Type Hinting
```python
from typing import Optional, Union, Callable, TypeVar, Generic

T = TypeVar("T")

def first(items: list[T]) -> Optional[T]:
    return items[0] if items else None

Handler = Callable[[str, int], bool]
```
Static tools (`mypy`, `pyright`) catch type errors before runtime; hints are not enforced by the interpreter itself.

## Design Patterns
| Pattern | Use case | Note |
|---|---|---|
| Singleton | One shared config/connection pool | Often better replaced by dependency injection |
| Factory | Create objects without exposing instantiation logic | Common in model-provider selection (`OpenAI`, `Anthropic`, `Local`) |
| Strategy | Swap an algorithm at runtime | Used for pluggable retrieval strategies in RAG systems |
| Observer | Notify subscribers on state change | Event-driven systems, pub/sub |
| Decorator (structural) | Add behavior without subclassing | Python decorators are a language-level version of this pattern |

## Error Handling & Logging
```python
class InsufficientBalanceError(Exception):
    """Raised when withdrawal exceeds account balance."""

try:
    withdraw(amount)
except InsufficientBalanceError as e:
    logger.warning("Withdrawal rejected: %s", e)
except Exception:
    logger.exception("Unexpected error during withdrawal")  # logs traceback
    raise
finally:
    close_connection()
```
Prefer specific exceptions over bare `except:`; use `logging` (not `print`) with levels (`DEBUG`/`INFO`/`WARNING`/`ERROR`/`CRITICAL`) and structured context.

## Networking & File Handling
- Always use `with open(...)` — guarantees the file handle closes even on exception.
- For HTTP: `requests` (sync) or `httpx`/`aiohttp` (async); set timeouts explicitly — a request with no timeout can hang a service forever.
- Sockets: know the basic client/server flow (`socket()`, `bind()`, `listen()`, `accept()` vs `connect()`).

---

## Practice Questions by Difficulty

**Easy**
1. Reverse a string without using `[::-1]`.
2. Check if a string is a palindrome, ignoring case and spaces.
3. Flatten a nested list one level deep.

**Medium**
4. Implement an LRU cache without `functools.lru_cache`.
5. Write a decorator that caches results with a TTL (time-to-live).
6. Given a stream of log lines, write a generator that yields only lines matching a pattern, without loading the file into memory.

**Hard**
7. Implement a thread-safe rate limiter (token bucket).
8. Design a connection pool class using context managers.
9. Implement your own `asyncio`-style event loop with a minimal task queue (conceptual walkthrough is often enough).

**Expected answer shape for Q4 (LRU cache):**
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache: OrderedDict = OrderedDict()

    def get(self, key):
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)
        return self.cache[key]

    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)
```
Talking points interviewers want: O(1) get/put via `OrderedDict` (hash map + doubly linked list under the hood), what happens on capacity overflow, thread-safety follow-up (would need a lock).

---

## Cheat Sheet
- `list` append/pop-end: O(1) amortized. `list` insert/pop-front: O(n).
- `dict`/`set`: O(1) average lookup, O(n) worst case (hash collisions).
- `sorted()`: O(n log n), Timsort, stable.
- String concatenation in a loop: use `"".join(parts)`, not `+=` (avoids O(n²)).
- `is` for `None`/singleton checks; `==` for value checks.
- Prefer `pathlib.Path` over string path manipulation.

## Common Mistakes
- Mutable default arguments (see above).
- Catching `Exception` broadly and swallowing errors silently.
- Using threads for CPU-bound work and being surprised there's no speedup.
- Not closing resources (files, sockets, DB connections) — leaks under load.
- Comparing floats with `==` instead of `math.isclose`.
- Shadowing builtins (`list`, `id`, `type`) as variable names.

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md).*
