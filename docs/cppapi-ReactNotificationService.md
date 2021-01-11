---
id: cppapi-reactnotificationservice
title: ReactNotificationService
---

Defined in `ReactNotificationService.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactNotificationService;
```

`ReactNotificationService` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IReactNotificationService` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| **[`(constructor)`](#reactdispatcherreactdispatcher)** | constructs the `ReactNotificationService` |
| **[`Subscribe`](#reactdispatchercreateserialdispatcher)** | creates new serial `ReactNotificationService` based on a thread pool |
| **[`SendNotification`](#reactdispatcherhandle)** | access the wrapped `IReactNotificationService` |
| **[`HasThreadAccess`](#reactdispatcherhasthreadaccess)** | checks if the `ReactNotificationService` has access to the current thread |
| **[`Post`](#reactdispatcherpost)** | posts `ReactNotificationServiceCallback` for asynchronous execution |
| **[`operator bool`](#reactdispatcheroperator-bool)** | checks if the wrapped `IReactNotificationService` is not null |

### Notes

A `ReactNotificationService` may use different strategies to invoke callbacks.
While all `ReactNotificationService`s invoke callbacks in a serial order, they may use different threads to do it.

- UI thread-based `ReactNotificationService` uses UI thread for all callbacks. See `ReactContext::UIDispatcher`.
- `ReactNotificationService` may use a dedicated thread. E.g. see `ReactContext::JSDispatcher`.
- `ReactNotificationService` may use different threads from a thread pool. E.g. see `ReactNotificationService::CreateSerialDispatcher`.
This way the `ReactNotificationService` does not hold any threads, but rather use them only when there is work to do.

### Examples

In this example we post a lambda to be executed in the `ReactNotificationService`.

```cpp
dispatcher.Post([]() noexcept {
  RunDispatchedCode();
}]);

```

In this example we use the `HasThreadAccess` to either run the code immediately or post it to the `ReactNotificationService`.

```cpp
void InvokeElsePost(ReactNotificationService const& dispatcher, ReactNotificationServiceCallback const &callback) {
  if (dispatcher.HasThreadAccess()) {
    callback();
  } else {
    dispatcher.Post(callback);
  }
}
```

In this example we create a new serial dispatcher based on the thread pool and post some work to invoke there.

```cpp
auto dispatcher = ReactNotificationService::CreateSerialDispatcher();
dispatcher.Post([]() noexcept {
  RunDispatchedCode1();
}]);
dispatcher.Post([]() noexcept {
  RunDispatchedCode2();
}]);
```

---

## `ReactNotificationService::ReactNotificationService`

```cpp
ReactNotificationService(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactNotificationService` with a null `IReactNotificationService` handle.

```cpp
explicit ReactNotificationService(IReactNotificationService const &handle) noexcept;
```

Constructs a `ReactNotificationService` with the provided `IReactNotificationService` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactNotificationService` handle |

---

## `ReactNotificationService::CreateSerialDispatcher`

```cpp
static ReactNotificationService CreateSerialDispatcher() noexcept;
```

Creates new serial `ReactNotificationService` that uses thread pool threads to invoke work items in a sequential order.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationServiceCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationService::Handle`

```cpp
static ReactNotificationService CreateSerialDispatcher() noexcept;
```

Returns the `IReactNotificationService` instance wrapped up by the `ReactNotificationService`.

### Parameters

(none)

### Return value

The `IReactNotificationService` wrapped by the `ReactNotificationService`. It may be empty.

---

## `ReactNotificationService::HasThreadAccess`

```cpp
bool HasThreadAccess() const noexcept;
```

Checks if the `ReactNotificationService` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactNotificationService`,
or the `ReactNotificationService` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactNotificationService::Post`

```cpp
void Post(ReactNotificationServiceCallback const &callback) const noexcept;
```

Posts an `ReactNotificationServiceCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactNotificationServiceCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactNotificationService::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactNotificationService` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactNotificationService` is not empty.
Otherwise it returns **false**.
