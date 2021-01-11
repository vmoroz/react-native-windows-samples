---
id: cppapi-reactnotificationsubscriptionrevoker
title: ReactNotificationSubscriptionRevoker
---

Defined in `ReactDispatcher.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactNotificationSubscriptionRevoker : ReactNotificationSubscription;
```

`ReactDispatcher` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IReactDispatcher` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| **[`(constructor)`](#constructor)** | constructs the `ReactDispatcher` |
| **[`(destructor)`](#destructor)** | constructs the `ReactDispatcher` |
| **[`operator =`](#reactdispatcheroperator-bool)** | checks if the wrapped `IReactDispatcher` is not null |

### Notes

A `ReactDispatcher` may use different strategies to invoke callbacks.
While all `ReactDispatcher`s invoke callbacks in a serial order, they may use different threads to do it.

- UI thread-based `ReactDispatcher` uses UI thread for all callbacks. See `ReactContext::UIDispatcher`.
- `ReactDispatcher` may use a dedicated thread. E.g. see `ReactContext::JSDispatcher`.
- `ReactDispatcher` may use different threads from a thread pool. E.g. see `ReactDispatcher::CreateSerialDispatcher`.
This way the `ReactDispatcher` does not hold any threads, but rather use them only when there is work to do.

### Examples

In this example we post a lambda to be executed in the `ReactDispatcher`.

```cpp
dispatcher.Post([]() noexcept {
  RunDispatchedCode();
}]);

```

In this example we use the `HasThreadAccess` to either run the code immediately or post it to the `ReactDispatcher`.

```cpp
void InvokeElsePost(ReactDispatcher const& dispatcher, ReactDispatcherCallback const &callback) {
  if (dispatcher.HasThreadAccess()) {
    callback();
  } else {
    dispatcher.Post(callback);
  }
}
```

In this example we create a new serial dispatcher based on the thread pool and post some work to invoke there.

```cpp
auto dispatcher = ReactDispatcher::CreateSerialDispatcher();
dispatcher.Post([]() noexcept {
  RunDispatchedCode1();
}]);
dispatcher.Post([]() noexcept {
  RunDispatchedCode2();
}]);
```

---

## `ReactDispatcher::ReactDispatcher`

```cpp
ReactDispatcher(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactDispatcher` with a null `IReactDispatcher` handle.

```cpp
explicit ReactDispatcher(IReactDispatcher const &handle) noexcept;
```

Constructs a `ReactDispatcher` with the provided `IReactDispatcher` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactDispatcher` handle |

---

## `ReactDispatcher::CreateSerialDispatcher`

```cpp
static ReactDispatcher CreateSerialDispatcher() noexcept;
```

Creates new serial `ReactDispatcher` that uses thread pool threads to invoke work items in a sequential order.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactDispatcherCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactDispatcher::Handle`

```cpp
static ReactDispatcher CreateSerialDispatcher() noexcept;
```

Returns the `IReactDispatcher` instance wrapped up by the `ReactDispatcher`.

### Parameters

(none)

### Return value

The `IReactDispatcher` wrapped by the `ReactDispatcher`. It may be empty.

---

## `ReactDispatcher::HasThreadAccess`

```cpp
bool HasThreadAccess() const noexcept;
```

Checks if the `ReactDispatcher` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactDispatcher`,
or the `ReactDispatcher` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactDispatcher::Post`

```cpp
void Post(ReactDispatcherCallback const &callback) const noexcept;
```

Posts an `ReactDispatcherCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactDispatcherCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactDispatcher::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactDispatcher` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactDispatcher` is not empty.
Otherwise it returns **false**.
