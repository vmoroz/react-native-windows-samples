---
id: cppapi-reactpropertyname
title: ReactPropertyName
---

Defined in `ReactPropertyName.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactPropertyName;
```

`ReactPropertyName` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IReactPropertyName` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| **[`(constructor)`](#constructor)** | constructs the `ReactPropertyName` |
| **[`Handle`](#reactpropertynamehandle)** | access the wrapped `IReactPropertyName` |
| **[`LocalName`](#reactpropertynamelocalname)** | checks if the `ReactPropertyName` has access to the current thread |
| **[`Namespace`](#reactpropertynamenamespace)** | checks if the `ReactPropertyName` has access to the current thread |
| **[`NamespaceName`](#reactpropertynamenamespacename)** | checks if the `ReactPropertyName` has access to the current thread |
| **[`operator bool`](#reactpropertynameoperator-bool)** | checks if the wrapped `IReactPropertyName` is not null |

---

## `(constructor)`

```cpp
ReactPropertyName(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactPropertyName` with a null `IReactPropertyName` handle.

```cpp
explicit ReactPropertyName(IReactPropertyName const &handle) noexcept;
```

Constructs a `ReactPropertyName` with the provided `IReactPropertyName` handle.

### Parameters

| | |
|-|-|
| **`handle`** | the `IReactPropertyName` handle |

---

## `ReactPropertyName::Handle`

```cpp
static ReactPropertyName CreateSerialDispatcher() noexcept;
```

Returns the `IReactPropertyName` instance wrapped up by the `ReactPropertyName`.

### Parameters

(none)

### Return value

The `IReactPropertyName` wrapped by the `ReactPropertyName`. It may be empty.

---

## `ReactPropertyName::LocalName`

```cpp
std::wstring LocalName() const noexcept;
```

Checks if the `ReactPropertyName` has access to the current thread.

### Parameters

(none)

### Return value

**true** if the current thread is either associated with the `ReactPropertyName`,
or the `ReactPropertyName` currently invokes one of its work items in the current thread.
Otherwise, it returns **false**.

---

## `ReactPropertyName::Namespace`

```cpp
ReactPropertyNamespace Namespace() const noexcept;
```

Posts an `ReactPropertyNameCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactPropertyNameCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactPropertyName::NamespaceName`

```cpp
std::wstring NamespaceName() const noexcept;
```

Posts an `ReactPropertyNameCallback` for an asynchronous invocation.
It adds the `callback` to a queue. It will be invoked after all previous callbacks in the queue are invoked.

### Parameters

| | |
|-|-|
| **`callback`** | a `ReactPropertyNameCallback` to be invoked asynchronously |

### Return value

(none)

---

## `ReactPropertyName::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactPropertyName` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactPropertyName` is not empty.
Otherwise it returns **false**.
