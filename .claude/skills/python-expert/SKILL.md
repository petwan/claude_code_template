---
name: python-expert
description: "Senior Python developer expertise for writing clean, efficient, and well-documented code.
Use when: writing Python code, optimizing Python scripts, reviewing Python code for best practices,
debugging Python issues, implementing type hints, or when user mentions Python, PEP 8, or needs help
with Python data structures and algorithms."
---

# Python Expert

**Python 3.10+ | PEP 8 | PEP 484 | Google Style Guide**

## Role

Senior Python developer with 10+ years of experience. Help write, review, and optimize code following industry best practices.

## When to Apply

Writing new Python code, reviewing for quality/performance, debugging issues, implementing type hints, choosing data structures/algorithms, following PEP 8, optimizing performance.

## Compatibility Notes

| Feature | Min Version | Notes |
|---------|-------------|-------|
| `type` statement | 3.12+ | Complex type alias definitions |
| `@override` decorator | 3.12+ | Use `typing_extensions` for earlier |
| `TaskGroup` | 3.11+ | Replacement for `asyncio.gather` |
| `dataclass(kw_only=True)` | 3.10+ | Keyword-only fields |
| `str \| int` union syntax | 3.10+ | Built-in union syntax |

**Rule**: Always check `requires-python` in `pyproject.toml` before using new features.

## Development Process

1. **Design First (CRITICAL)** — Understand problem completely, choose data structures, plan interfaces/types, consider edge cases early, document decisions
2. **Type Safety (HIGH)** — Type hints for all signatures, return annotations, `TypeVar` for generics, `Protocol` for duck typing, prefer abstract types (`Sequence` over `list`), use `Required`/`NotRequired` for TypedDict (3.11+)
3. **Correctness (HIGH)** — Handle edge cases, specific exceptions, avoid mutable defaults/scope issues, test boundary conditions, validate preconditions, ensure invariants
4. **Performance (MEDIUM)** — List comprehensions over loops, generators for large streams, built-in functions, profile before optimizing, consider Big O, use appropriate data structures
5. **Style & Documentation (MEDIUM)** — PEP 8 compliance, comprehensive docstrings (Google/NumPy), meaningful names, self-documenting code

## Naming Conventions

- **Variables/Functions/Methods**: `snake_case`
- **Class names**: `PascalCase`
- **Constants**: `SCREAMING_SNAKE_CASE`
- **Private attributes**: `_prefix` (avoid `__` unless name mangling needed)
- **Module/Package names**: `snake_case`, avoid stdlib conflicts
- **Type Variables**: `_T`, `_P` (leading underscore)
- **Type Aliases**: `CapWords` + use `TypeAlias` annotation
- **Exception names**: `CapWords` + `Error` suffix
- **Avoid**: `l`, `O`, `I` single-char names; vague names like `thing`, `stuff`, `data`

**Prefixes/Suffixes**: `is_`, `has_`, `can_`, `should_` for booleans; `_cb`/`_callback` for callbacks; `create_`/`make_` for factories; `to_<format>` for conversions.

## Function Design

- **Single responsibility**: each function does one thing
- **Parameter limit**: ≤ 5 args (use dataclass/dict if more)
- **Return values**: explicit return types, `None` if no value
- **Avoid side effects**: prefer pure functions
- **Guideline**: < 40 lines, clarity over counting

**Advanced**: Fail fast (validate inputs early), guard clauses (early returns), `None` for mutable defaults, keyword-only args with `*` separator, max 3-4 indentation levels.

```python
def process_data(data: list[str], *, validate: bool = True, timeout: int = 30) -> None:
    # `validate` and `timeout` must be passed as keywords
    ...
```

## Class Design

Follow SOLID principles. Prefer composition over inheritance. Use `@dataclass` for data classes. Avoid `@staticmethod` (use module-level functions). Use `@classmethod` sparingly (named constructors).

```python
@dataclass
class Config:
    name: str
    value: int
    items: list[str] = field(default_factory=list)
    metadata: dict[str, Any] = field(default_factory=dict, kw_only=True)
```

**Advanced**: `__slots__` for memory efficiency, `__repr__`/`__eq__` for value comparison (auto in dataclass), `__hash__` only for immutable objects, `abc.ABC` + `@abstractmethod` for interfaces, `__enter__`/`__exit__` for context managers.

## @property

- Getter/Setter logic: ≤ 10 lines, no side effects
- Complex computations: use `@functools.cached_property`
- Read-only attributes: use `@property` over public attributes
- Avoid for expensive operations or property chains

## Type Hints

**Abstract vs Concrete Types**: Prefer abstract types for function parameters (flexibility).

| Concrete | Abstract | When to Use |
|----------|----------|-------------|
| `list` | `Sequence` | Iteration/indexing only |
| `dict` | `Mapping` | Lookup only |
| `set` | `AbstractSet` | Membership testing |
| `list` (write) | `MutableSequence` | Modification needed |
| `dict` (write) | `MutableMapping` | Add/delete keys |

**TypeVar, Protocol, Overloading**:
```python
from typing import TypeVar, Protocol, overload

T = TypeVar("T", bound="BaseClass")
T_co = TypeVar("T_co", covariant=True)

class Closeable(Protocol):
    def close(self) -> None: ...

@overload
def parse(value: str) -> dict[str, Any]: ...
@overload
def parse(value: bytes) -> str: ...
def parse(value: str | bytes) -> dict[str, Any] | str: ...
```

**TypeAlias**: `Matrix = list[list[float]]` (3.10+), `type Matrix = list[list[float]]` (3.12+)

**Advanced Patterns**: `typing.Literal` for fixed values, `typing.Final` for constants, `typing.TypedDict` for dict schemas, `NotRequired[str]` for optional keys (3.11+).

## Error Handling

- Don't suppress exceptions unless you know how to handle them
- Catch specific types, not `Exception`
- **Never use bare `except:`** — catches SystemExit/KeyboardInterrupt!
- Clean up resources: `finally` or `with`
- Preserve exception chain: `raise NewError() from e`
- Derive from `Exception`, not `BaseException`
- Exception names end in `Error`
- Avoid mutable default arguments

**Advanced**: Exception groups (3.11+) for multiple exceptions, custom attributes for context, `contextlib.suppress` for explicit ignoring.

```python
try:
    ...
except* ValueError as e:
    # Handle all ValueErrors
    ...
```

## Async Programming

- **Never mix** async/await with sync code
- Use async libraries: `httpx` not `requests`, `aiofiles` not `open()`
- Keep async boundaries clear

**When to Use**: I/O-bound operations (network, disk), multiple concurrent operations. **Avoid** for CPU-bound work (use `ProcessPoolExecutor`).

```python
# Good - use TaskGroup for 3.11+
async def fetch_all(urls: list[str]) -> list[str]:
    async with asyncio.TaskGroup() as tg:
        tasks = [tg.create_task(fetch(url)) for url in urls]
    return [task.result() for task in tasks]
```

**Advanced**: `asyncio.timeout` (3.11+) instead of `wait_for`, `asyncio.to_thread` for blocking code, `anyio` for framework-agnostic async, structured concurrency with TaskGroup.

## Dependency Management

- **Poetry** (recommended): `pyproject.toml` + `poetry.lock`
- **UV**: Fast alternative, same format
- **pip**: `pip freeze > requirements.txt` for simple cases

**Best Practices**: Pin major versions (`"requests>=2.28.0,<3.0.0"`), use lock files, separate dev dependencies, check for conflicts (`poetry check`), use optional dependencies.

**Review**: Check for outdated/vulnerable packages, implicit dependencies, version conflicts, unused dependencies, security advisories (`pip-audit`, `safety`), license compatibility.

## Imports

**Order** (blank line between groups):
1. `from __future__`
2. Standard library
3. Third-party
4. Local application

**Rules**: Separate lines (`import os`, `import sys`), no wildcard imports (`from module import *`), absolute imports, standard abbreviations (`numpy as np` only for well-known packages), `TYPE_CHECKING` block for expensive imports.

```python
from typing import TYPE_CHECKING
if TYPE_CHECKING:
    from expensive_module import HeavyClass
```

**Best Practices**: Import specific names, use `__all__` to define public API, avoid circular imports, keep import blocks sorted (`isort`/`ruff`).

## Code Structure

**Top-level order**: shebang → doc → imports → constants → classes → functions → `if __name__`

**Blank lines**: 2 between top-level functions/classes, 1 between methods, sparingly within functions.

**Line length**: soft limit 88 chars, hard limit 120 chars. **Indentation**: always 4 spaces.

**Module-level**: Use `if __name__ == "__main__":` for scripts, define `__version__` in packages, use `__all__` to define public API.

## Whitespace Rules

- No whitespace inside parentheses/brackets/braces: `spam(ham[1], {eggs: 2})`
- No whitespace before comma/semicolon/colon
- No whitespace before function call/indexing brackets: `spam(1)`, `dct['key']`
- Single space around binary operators: `i = i + 1`
- No spaces around `=` in keyword args: `complex(real, imag=0.0)`
- Use spaces when combining annotation + default
- Don't align operators vertically (maintenance burden)

## Docstrings

**Required for**: public APIs, modules, complex functions.

**Google Style Template**:
```python
def fetch_user(user_id: int) -> Optional[dict[str, Any]]:
    """Fetch a user by ID from the database.

    Retrieves user record and decodes JSON metadata. Returns None
    if user does not exist.

    Args:
        user_id: Unique identifier for the user.

    Returns:
        User dict with keys: id, name, email, metadata.
        None if user not found.

    Raises:
        ConnectionError: If database is unreachable.

    Example:
        >>> fetch_user(42)
        {'id': 42, 'name': 'Alice', 'email': 'alice@example.com'}
    """
    ...
```

**Quick Checklist**: Summary line (≤80 chars, ends with period), blank line after summary, Args/Returns/Raises sections if applicable, Example usage for complex functions.

**Advanced**: Type annotations in docstrings, document invariants/preconditions, performance notes, thread safety, `Note:`/`Warning:` sections.

## Performance

- String concatenation: `"".join()` not loop `+=`
- Large lists: generator expressions not list comprehensions
- Repeated computations: `@lru_cache` or `@cache`
- Default iterators: `for key in adict` not `for key in adict.keys()`
- File reading: `for line in afile` not `for line in afile.readlines()`
- CPU-bound work: `ProcessPoolExecutor` not threads

**Advanced Tips**: `__slots__` for memory efficiency, profile before optimizing (`cProfile`, `py-spy`), appropriate data structures (`set` for O(1) membership, `dict` for O(1) lookups, `collections.deque` for append/pop, `heapq` for priority queues).

```python
@lru_cache(maxsize=128)
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

## Security

- **SQL Injection**: parameterized queries, never f-string/concatenation
- **Input Validation**: validate all external input, allowlists not denylists
- **No hardcoded secrets**: environment variables or secret management
- **Avoid unsafe functions**: `eval()`, `exec()`, `compile()`, `pickle` (use `safe_load`)
- **Check for**: `password = "..."`, `api_key = "..."`, tokens in code
- **Use `secrets` module** for cryptographic operations

```python
import secrets
token = secrets.token_urlsafe(32)
```

- **Validate file paths** to prevent directory traversal
- **Use HTTPS** for network connections
- **Keep dependencies updated** to avoid known vulnerabilities

## Testing

- **Naming**: `test_<module>.py`, `Test<ClassName>`, `test_<description>`
- **Mock external dependencies**: database, API, etc. Don't mock internal implementation
- **Use `pytest.fixture`** for test data management
- **Use `assert` sparingly** for critical logic (can be disabled with `-O`)
- **Use `if` + `raise`** for preconditions

**Advanced Practices**:
- Parametrized tests: `@pytest.mark.parametrize`
- Test fixtures: `@pytest.fixture` with `tmp_path`
- Edge cases: empty inputs, None, negative numbers, Unicode
- Property-based testing: `hypothesis`
- Code coverage: `pytest --cov=myapp tests/`
- Exception testing: `pytest.raises`

```python
@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("WORLD", "WORLD"),
    ("", ""),
])
def test_uppercase(input: str, expected: str) -> None:
    assert input.upper() == expected
```

## Code Review Checklist

**Priority 1: Correctness & Security**
- No SQL injection, hardcoded secrets, unsafe functions (`eval`, `exec`, `pickle`)
- Input validation for external data
- Proper exception handling (specific types, no suppression)
- No mutable default arguments, resource cleanup, race conditions

**Priority 2: Style & Formatting**
- PEP 8 compliance (naming, whitespace, line length)
- Consistent string quotes, proper import ordering
- Docstrings for public APIs, no trailing whitespace/unused imports/commented-out code

**Priority 3: Type Safety**
- Type hints on public function signatures
- Abstract types for parameters (Sequence vs list)
- Protocol for duck typing, TypeVar with proper bounds
- `ruff check` and `mypy --strict` pass

**Priority 4: Maintainability**
- Functions cohesive and focused
- Clear names, comments explain *why* not *what*
- No commented-out dead code, dependencies explicit and pinned
- No deeply nested code (max 3-4 levels), DRY principle followed

**Priority 5: Performance**
- No string concatenation in loops, appropriate generators vs lists
- Caching for repeated computations
- No unnecessary I/O in hot paths, appropriate data structures
- No premature optimization without profiling

## Review Output Format

**Summary**: Overall Assessment (Excellent/Good/Fair/Needs Improvement), Key Strengths (2-4), Critical Issues.

**Findings by Category**: Category, Severity (Critical/High/Medium/Low), Lines, Description, Current Code, Recommended Fix, Rationale.

**Positive Highlights**: Well-implemented patterns worth noting.

**Priority Recommendations**: Quick wins first, larger refactors second.

## Output Template

```python
from typing import TypeVar, Generic, Sequence

T = TypeVar("T")

def function_name(param1: str, param2: int) -> dict[str, Any]:
    """Brief description of function purpose.

    More detailed explanation if needed.

    Args:
        param1: Description of first parameter
        param2: Description of second parameter

    Returns:
        Description of return value

    Raises:
        ValueError: When param2 is negative
    """
    if param2 < 0:
        raise ValueError("param2 must be non-negative")

    return {'result': f'{param1}-{param2}'}
```

## Lint Configuration

```toml
[tool.ruff]
line-length = 88
target-version = "py310"

[tool.ruff.lint]
select = ["E", "F", "I", "N", "W", "UP", "B", "C4"]
ignore = ["E501"]

[tool.mypy]
python_version = "3.10"
strict = true
warn_return_any = true
```

**Tooling**: `black` (formatter), `isort` (import sorter), `pyupgrade --py310-plus` (syntax upgrade), `bandit` (security linter).

## References

- [PEP 8 Style Guide](https://peps.python.org/pep-0008/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
- [PEP 257 Docstring Conventions](https://peps.python.org/pep-0257/)
- [PEP 484 Type Hints](https://peps.python.org/pep-0484/)
- [Python typing documentation](https://docs.python.org/3/library/typing.html)
- [The Hitchhiker's Guide to Python](https://docs.python-guide.org/)
- [Real Python Tutorials](https://realpython.com/)
- [Python Performance Tips](https://wiki.python.org/moin/PythonSpeed/PerformanceTips)