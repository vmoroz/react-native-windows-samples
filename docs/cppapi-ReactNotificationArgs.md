---
id: cppapi-reactnotificationargs
title: ReactNotificationArgs
---

Defined in `ReactNotificationArgsBase.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactNotificationArgsBase;
```

`ReactNotificationArgsBase` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IReactNotificationArgsBase` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| **[`(constructor)`](#reactdispatcherreactdispatcher)** | constructs the `ReactNotificationArgsBase` |
| **[`Handle`](#reactdispatcherhandle)** | access the wrapped `IReactNotificationArgsBase` |
| **[`Subscription`](#reactdispatcherhasthreadaccess)** | checks if the `ReactNotificationArgsBase` has access to the current thread |
| **[`operator bool`](#reactdispatcheroperator-bool)** | checks if the wrapped `IReactNotificationArgsBase` is not null |

### Notes

A `ReactNotificationArgsBase` may use different strategies to invoke callbacks.
While all `ReactNotificationArgsBase`s invoke callbacks in a serial order, they may use different threads to do it.

- UI thread-based `ReactNotificationArgsBase` uses UI thread for all callbacks. See `ReactContext::UIDispatcher`.
- `ReactNotificationArgsBase` may use a dedicated thread. E.g. see `ReactContext::JSDispatcher`.
- `ReactNotificationArgsBase` may use different threads from a thread pool. E.g. see `ReactNotificationArgsBase::CreateSerialDispatcher`.
This way the `ReactNotificationArgsBase` does not hold any threads, but rather use them only when there is work to do.

### Examples

In this example we post a lambda to be executed in the `ReactNotificationArgsBase`.

```cpp
dispatcher.Post([]() noexcept {
  RunDispatchedCode();
}]);

```

In this example we use the `HasThreadAccess` to either run the code immediately or post it to the `ReactNotificationArgsBase`.

```cpp
void InvokeElsePost(ReactNotificationArgsBase const& dispatcher, ReactNotificationArgsBaseCallback const &callback) {
  if (dispatcher.HasThreadAccess()) {
    callback();
  } else {
    dispatcher.Post(callback);
  }
}
```

In this example we create a new serial dispatcher based on the thread pool and post some work to invoke there.

```cpp
auto dispatcher = ReactNotificationArgsBase::CreateSerialDispatcher();
dispatcher.Post([]() noexcept {
  RunDispatchedCode1();
}]);
dispatcher.Post([]() noexcept {
  RunDispatchedCode2();
}]);
```

---

## `ReactNotificationArgsBase::ReactNotificationArgsBase`

```cpp
ReactNotificationArgsBase(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactNotificationArgsBase` with a null `IReactNotificationArgsBase` handle.

```cpp
explicit ReactNotificationArgsBase(IReactNotificationArgsBase const &handle) noexcept;
```

Constructs a `ReactNotificationArgsBase` with the provided `IReactNotificationArgsBase` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactNotificationArgsBase` handle |

---

## `ReactNotificationArgsBase::CreateSerialDispatcher`

```cpp
static ReactNotificationArgsBase CreateSerialDispatcher() noexcept;
```

Creates new serial `ReactNotificationArgsBase` that uses thread pool threads to invoke work items in a sequential order.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationArgsBaseCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationArgsBase::Handle`

```cpp
static ReactNotificationArgsBase CreateSerialDispatcher() noexcept;
```

Returns the `IReactNotificationArgsBase` instance wrapped up by the `ReactNotificationArgsBase`.

### Parameters

(none)

### Return value

The `IReactNotificationArgsBase` wrapped by the `ReactNotificationArgsBase`. It may be empty.

---

## `ReactNotificationArgsBase::HasThreadAccess`

```cpp
bool HasThreadAccess() const noexcept;
```

Checks if the `ReactNotificationArgsBase` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactNotificationArgsBase`,
or the `ReactNotificationArgsBase` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactNotificationArgsBase::Post`

```cpp
void Post(ReactNotificationArgsBaseCallback const &callback) const noexcept;
```

Posts an `ReactNotificationArgsBaseCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationArgsBaseCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationArgsBase::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactNotificationArgsBase` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactNotificationArgsBase` is not empty.
Otherwise it returns **false**.
