---
id: cppapi-reactnotificationsubscription
title: ReactNotificationSubscription
---

Defined in `ReactNotificationSubscription.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactNotificationSubscription;
```

`ReactNotificationSubscription` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IReactNotificationSubscription` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| **[`(constructor)`](#reactdispatcherreactdispatcher)** | constructs the `ReactNotificationSubscription` |
| **[`Dispatcher`](#reactdispatchercreateserialdispatcher)** | creates new serial `ReactNotificationSubscription` based on a thread pool |
| **[`Handle`](#reactdispatcherhandle)** | access the wrapped `IReactNotificationSubscription` |
| **[`IsSubscribed`](#reactdispatcherhasthreadaccess)** | checks if the `ReactNotificationSubscription` has access to the current thread |
| **[`NotificationName`](#reactdispatcherpost)** | posts `ReactNotificationSubscriptionCallback` for asynchronous execution |
| **[`Unsubscribe`](#reactdispatcherpost)** | posts `ReactNotificationSubscriptionCallback` for asynchronous execution |
| **[`operator bool`](#reactdispatcheroperator-bool)** | checks if the wrapped `IReactNotificationSubscription` is not null |

### Notes

A `ReactNotificationSubscription` may use different strategies to invoke callbacks.
While all `ReactNotificationSubscription`s invoke callbacks in a serial order, they may use different threads to do it.

- UI thread-based `ReactNotificationSubscription` uses UI thread for all callbacks. See `ReactContext::UIDispatcher`.
- `ReactNotificationSubscription` may use a dedicated thread. E.g. see `ReactContext::JSDispatcher`.
- `ReactNotificationSubscription` may use different threads from a thread pool. E.g. see `ReactNotificationSubscription::CreateSerialDispatcher`.
This way the `ReactNotificationSubscription` does not hold any threads, but rather use them only when there is work to do.

### Examples

In this example we post a lambda to be executed in the `ReactNotificationSubscription`.

```cpp
dispatcher.Post([]() noexcept {
  RunDispatchedCode();
}]);

```

In this example we use the `HasThreadAccess` to either run the code immediately or post it to the `ReactNotificationSubscription`.

```cpp
void InvokeElsePost(ReactNotificationSubscription const& dispatcher, ReactNotificationSubscriptionCallback const &callback) {
  if (dispatcher.HasThreadAccess()) {
    callback();
  } else {
    dispatcher.Post(callback);
  }
}
```

In this example we create a new serial dispatcher based on the thread pool and post some work to invoke there.

```cpp
auto dispatcher = ReactNotificationSubscription::CreateSerialDispatcher();
dispatcher.Post([]() noexcept {
  RunDispatchedCode1();
}]);
dispatcher.Post([]() noexcept {
  RunDispatchedCode2();
}]);
```

---

## `ReactNotificationSubscription::ReactNotificationSubscription`

```cpp
ReactNotificationSubscription(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactNotificationSubscription` with a null `IReactNotificationSubscription` handle.

```cpp
explicit ReactNotificationSubscription(IReactNotificationSubscription const &handle) noexcept;
```

Constructs a `ReactNotificationSubscription` with the provided `IReactNotificationSubscription` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactNotificationSubscription` handle |

---

## `ReactNotificationSubscription::CreateSerialDispatcher`

```cpp
static ReactNotificationSubscription CreateSerialDispatcher() noexcept;
```

Creates new serial `ReactNotificationSubscription` that uses thread pool threads to invoke work items in a sequential order.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationSubscriptionCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationSubscription::Handle`

```cpp
static ReactNotificationSubscription CreateSerialDispatcher() noexcept;
```

Returns the `IReactNotificationSubscription` instance wrapped up by the `ReactNotificationSubscription`.

### Parameters

(none)

### Return value

The `IReactNotificationSubscription` wrapped by the `ReactNotificationSubscription`. It may be empty.

---

## `ReactNotificationSubscription::HasThreadAccess`

```cpp
bool HasThreadAccess() const noexcept;
```

Checks if the `ReactNotificationSubscription` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactNotificationSubscription`,
or the `ReactNotificationSubscription` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactNotificationSubscription::Post`

```cpp
void Post(ReactNotificationSubscriptionCallback const &callback) const noexcept;
```

Posts an `ReactNotificationSubscriptionCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationSubscriptionCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationSubscription::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactNotificationSubscription` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactNotificationSubscription` is not empty.
Otherwise it returns **false**.
