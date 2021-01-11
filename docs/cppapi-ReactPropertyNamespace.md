---
id: cppapi-reactpropertynamespace
title: ReactPropertyNamespace
---

Defined in `ReactPropertyBag.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactPropertyNamespace;
```

`ReactPropertyNamespace` represents an atomic property namespace object.
It wraps up the `IReactPropertyNamespace` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| **[`(constructor)`](#constructor)** | constructs the `ReactPropertyNamespace` |
| **[`Global`](#reactpropertynamespaceglobal)** | creates new serial `ReactPropertyNamespace` based on a thread pool |
| **[`Handle`](#reactpropertynamespacehandle)** | access the wrapped `IReactPropertyNamespace` |
| **[`NamespaceName`](#reactpropertynamespacenamespacename)** | checks if the `ReactPropertyNamespace` has access to the current thread |
| **[`operator bool`](#reactpropertynamespaceoperator-bool)** | checks if the wrapped `IReactPropertyNamespace` is not null |

### Notes

A `ReactPropertyNamespace` may use different strategies to invoke callbacks.
While all `ReactPropertyNamespace`s invoke callbacks in a serial order, they may use different threads to do it.

- UI thread-based `ReactPropertyNamespace` uses UI thread for all callbacks. See `ReactContext::UIDispatcher`.
- `ReactPropertyNamespace` may use a dedicated thread. E.g. see `ReactContext::JSDispatcher`.
- `ReactPropertyNamespace` may use different threads from a thread pool. E.g. see `ReactPropertyNamespace::CreateSerialDispatcher`.
This way the `ReactPropertyNamespace` does not hold any threads, but rather use them only when there is work to do.

### Examples

In this example we post a lambda to be executed in the `ReactPropertyNamespace`.

```cpp
dispatcher.Post([]() noexcept {
  RunDispatchedCode();
}]);

```

In this example we use the `HasThreadAccess` to either run the code immediately or post it to the `ReactPropertyNamespace`.

```cpp
void InvokeElsePost(ReactPropertyNamespace const& dispatcher, ReactPropertyNamespaceCallback const &callback) {
  if (dispatcher.HasThreadAccess()) {
    callback();
  } else {
    dispatcher.Post(callback);
  }
}
```

In this example we create a new serial dispatcher based on the thread pool and post some work to invoke there.

```cpp
auto dispatcher = ReactPropertyNamespace::CreateSerialDispatcher();
dispatcher.Post([]() noexcept {
  RunDispatchedCode1();
}]);
dispatcher.Post([]() noexcept {
  RunDispatchedCode2();
}]);
```

---

## `(constructor)`

```cpp
ReactPropertyNamespace(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactPropertyNamespace` with a null `IReactPropertyNamespace` handle.

```cpp
explicit ReactPropertyNamespace(IReactPropertyNamespace const &handle) noexcept;
```

Constructs a `ReactPropertyNamespace` with the provided `IReactPropertyNamespace` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactPropertyNamespace` handle |

---

## `ReactPropertyNamespace::Global`

```cpp
static ReactPropertyNamespace Global() noexcept;
```

Gets predefined Global namespace.

### Parameters

(none)

### Return value

(none)

---

## `ReactPropertyNamespace::Handle`

```cpp
IReactPropertyNamespace const &Handle() const noexcept;
```

Returns the `IReactPropertyNamespace` instance wrapped up by the `ReactPropertyNamespace`.

### Parameters

(none)

### Return value

The `IReactPropertyNamespace` wrapped by the `ReactPropertyNamespace`. It may be empty.

---

## `ReactPropertyNamespace::NamespaceName`

```cpp
std::wstring NamespaceName() const noexcept;
```

Checks if the `ReactPropertyNamespace` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactPropertyNamespace`,
or the `ReactPropertyNamespace` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactPropertyNamespace::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactPropertyNamespace` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactPropertyNamespace` is not empty.
Otherwise it returns **false**.
