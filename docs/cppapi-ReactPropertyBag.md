---
id: cppapi-reactpropertybag
title: ReactPropertyBag
---

Defined in `ReactPropertyBag.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactPropertyBag;
```

`ReactPropertyBag` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IReactPropertyBag` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| [`(constructor)`](#reactpropertybagreactpropertybag) | constructs the `ReactPropertyBag` |
| [`Get`](#reactpropertybagget) | creates new serial `ReactPropertyBag` based on a thread pool |
| [`GetOrCreate`](#reactpropertybaggetorcreate) | creates new serial `ReactPropertyBag` based on a thread pool |
| [`Handle`](#reactpropertybaghandle) | access the wrapped `IReactContext` |
| [`Remove`](#reactpropertybagremove) | creates new serial `ReactPropertyBag` based on a thread pool |
| [`Set`](#reactpropertybagset) | creates new serial `ReactPropertyBag` based on a thread pool |
| [`operator bool`](#reactpropertybagoperator-bool) | checks if the wrapped `IReactPropertyBag` is not null |

### Notes

A `ReactPropertyBag` may use different strategies to invoke callbacks.
While all `ReactPropertyBag`s invoke callbacks in a serial order, they may use different threads to do it.

- UI thread-based `ReactPropertyBag` uses UI thread for all callbacks. See `ReactContext::UIPropertyBag`.
- `ReactPropertyBag` may use a dedicated thread. E.g. see `ReactContext::JSPropertyBag`.
- `ReactPropertyBag` may use different threads from a thread pool. E.g. see `ReactPropertyBag::CreateSerialPropertyBag`.
This way the `ReactPropertyBag` does not hold any threads, but rather use them only when there is work to do.

### Examples

In this example we post a lambda to be executed in the `ReactPropertyBag`.

```cpp
propertybag.Post([]() noexcept {
  RunDispatchedCode();
}]);

```

In this example we use the `HasThreadAccess` to either run the code immediately or post it to the `ReactPropertyBag`.

```cpp
void InvokeElsePost(ReactPropertyBag const& propertybag, ReactPropertyBagCallback const &callback) {
  if (propertybag.HasThreadAccess()) {
    callback();
  } else {
    propertybag.Post(callback);
  }
}
```

In this example we create a new serial propertybag based on the thread pool and post some work to invoke there.

```cpp
auto propertybag = ReactPropertyBag::CreateSerialPropertyBag();
propertybag.Post([]() noexcept {
  RunDispatchedCode1();
}]);
propertybag.Post([]() noexcept {
  RunDispatchedCode2();
}]);
```

---

## `ReactPropertyBag::ReactPropertyBag`

```cpp
ReactPropertyBag(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactPropertyBag` with a null `IReactPropertyBag` handle.

```cpp
explicit ReactPropertyBag(IReactPropertyBag const &handle) noexcept;
```

Constructs a `ReactPropertyBag` with the provided `IReactPropertyBag` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactPropertyBag` handle |

---

## `ReactPropertyBag::CreateSerialPropertyBag`

```cpp
static ReactPropertyBag CreateSerialPropertyBag() noexcept;
```

Creates new serial `ReactPropertyBag` that uses thread pool threads to invoke work items in a sequential order.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactPropertyBagCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactPropertyBag::Handle`

```cpp
static ReactPropertyBag CreateSerialPropertyBag() noexcept;
```

Returns the `IReactPropertyBag` instance wrapped up by the `ReactPropertyBag`.

### Parameters

(none)

### Return value

The `IReactPropertyBag` wrapped by the `ReactPropertyBag`. It may be empty.

---

## `ReactPropertyBag::HasThreadAccess`

```cpp
bool HasThreadAccess() const noexcept;
```

Checks if the `ReactPropertyBag` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactPropertyBag`,
or the `ReactPropertyBag` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactPropertyBag::Post`

```cpp
void Post(ReactPropertyBagCallback const &callback) const noexcept;
```

Posts an `ReactPropertyBagCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactPropertyBagCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactPropertyBag::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactPropertyBag` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactPropertyBag` is not empty.
Otherwise it returns **false**.
