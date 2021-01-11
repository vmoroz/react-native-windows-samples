---
id: cppapi-reactnotificationid
title: ReactNotificationId
---

Defined in `ReactNotificationArgs.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
template <class T>
struct ReactNotificationArgs : ReactNotificationArgsBase;
```

`ReactNotificationArgs` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IReactNotificationArgs` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| **[`(constructor)`](#reactdispatcherreactdispatcher)** | constructs the `ReactNotificationArgs` |
| **[`Data`](#reactdispatchercreateserialdispatcher)** | creates new serial `ReactNotificationArgs` based on a thread pool |

### Notes

A `ReactNotificationArgs` may use different strategies to invoke callbacks.
While all `ReactNotificationArgs`s invoke callbacks in a serial order, they may use different threads to do it.

- UI thread-based `ReactNotificationArgs` uses UI thread for all callbacks. See `ReactContext::UIDispatcher`.
- `ReactNotificationArgs` may use a dedicated thread. E.g. see `ReactContext::JSDispatcher`.
- `ReactNotificationArgs` may use different threads from a thread pool. E.g. see `ReactNotificationArgs::CreateSerialDispatcher`.
This way the `ReactNotificationArgs` does not hold any threads, but rather use them only when there is work to do.

### Examples

In this example we post a lambda to be executed in the `ReactNotificationArgs`.

```cpp
dispatcher.Post([]() noexcept {
  RunDispatchedCode();
}]);

```

In this example we use the `HasThreadAccess` to either run the code immediately or post it to the `ReactNotificationArgs`.

```cpp
void InvokeElsePost(ReactNotificationArgs const& dispatcher, ReactNotificationArgsCallback const &callback) {
  if (dispatcher.HasThreadAccess()) {
    callback();
  } else {
    dispatcher.Post(callback);
  }
}
```

In this example we create a new serial dispatcher based on the thread pool and post some work to invoke there.

```cpp
auto dispatcher = ReactNotificationArgs::CreateSerialDispatcher();
dispatcher.Post([]() noexcept {
  RunDispatchedCode1();
}]);
dispatcher.Post([]() noexcept {
  RunDispatchedCode2();
}]);
```

---

## `ReactNotificationArgs::ReactNotificationArgs`

```cpp
ReactNotificationArgs(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactNotificationArgs` with a null `IReactNotificationArgs` handle.

```cpp
explicit ReactNotificationArgs(IReactNotificationArgs const &handle) noexcept;
```

Constructs a `ReactNotificationArgs` with the provided `IReactNotificationArgs` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactNotificationArgs` handle |

---

## `ReactNotificationArgs::CreateSerialDispatcher`

```cpp
static ReactNotificationArgs CreateSerialDispatcher() noexcept;
```

Creates new serial `ReactNotificationArgs` that uses thread pool threads to invoke work items in a sequential order.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationArgsCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationArgs::Handle`

```cpp
static ReactNotificationArgs CreateSerialDispatcher() noexcept;
```

Returns the `IReactNotificationArgs` instance wrapped up by the `ReactNotificationArgs`.

### Parameters

(none)

### Return value

The `IReactNotificationArgs` wrapped by the `ReactNotificationArgs`. It may be empty.

---

## `ReactNotificationArgs::HasThreadAccess`

```cpp
bool HasThreadAccess() const noexcept;
```

Checks if the `ReactNotificationArgs` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactNotificationArgs`,
or the `ReactNotificationArgs` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactNotificationArgs::Post`

```cpp
void Post(ReactNotificationArgsCallback const &callback) const noexcept;
```

Posts an `ReactNotificationArgsCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationArgsCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationArgs::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactNotificationArgs` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactNotificationArgs` is not empty.
Otherwise it returns **false**.
