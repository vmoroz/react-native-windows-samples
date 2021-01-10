---
id: cppapi-reactcontext
title: ReactContext
---

Defined in `ReactContext.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct ReactContext;
```

`ReactContext` is the API for the current ReactNative instance given to a native module.
It wraps up the `IReactContext` C++/WinRT generated interface.

### Member functions

| | |
|-|-|
| [`(constructor)`](#reactcontextreactcontext) | constructs the `ReactContext` |
| [`CallJSFunction`](#reactcontextcalljsfunction) | calls ReactNative JavaScript function |
| [`EmitJSEvent`](#reactcontextemitjsevent) | emits ReactNative event in JavaScript code |
| [`Handle`](#reactcontexthandle) | access the wrapped `IReactContext` |
| [`JSDispatcher`](#reactcontextjsdispatcher) | gets [`ReactDispatcher`](cppapi-reactdispatcher) for the JavaScript dispatcher |
| [`Notifications`](#reactcontextnotifications) | gets the [`ReactNotificationService`](cppapi-reactnotificationservice) associated with the `ReactNativeHost` |
| [`Properties`](#reactcontextproperties) | gets the [`ReactPropertyBag`](cppapi-reactpropertybag) associated with the `ReactNativeHost` |
| [`UIDispatcher`](#reactcontextuidispatcher) | gets [`ReactDispatcher`](cppapi-reactdispatcher) for the UI thread |
| [`operator bool`](#reactcontextoperator-bool) | checks if the wrapped `IReactContext` is not null |

---

## `ReactContext::ReactContext`

```cpp
ReactContext(std::nullptr_t = nullptr) noexcept;
```

Constructs a `ReactContext` with a null `IReactContext` handle.

```cpp
explicit ReactContext(IReactContext const &handle) noexcept;
```

Constructs a `ReactContext` with the provided `IReactContext` handle.

### Parameters

| | |
|-|-|
| `handle` | the `IReactContext` handle |

---

## `ReactContext::CallJSFunction`

```cpp
template <class... TArgs>
void CallJSFunction(std::wstring_view moduleName, std::wstring_view methodName, TArgs &&... args) const noexcept;
```

Calls JavaScript function.

### Parameters

| | |
|-|-|
| `moduleName` | name of JavaScript module for the JavaScript function |
| `methodName` | name of JavaScript method |
| `args` | arguments passed to JavaScript function. They are serialized to [`IJSValueWriter`](cppapi-jsvaluewriter) using one of the [`WriteValue`](cppapi-jsvaluewriter) overloaded methods. |

### Return value

(none)

---

## `ReactContext::EmitJSEvent`

```cpp
template <class... TArgs>
void EmitJSEvent(std::wstring_view eventEmitterName, std::wstring_view eventName, TArgs &&... args) const noexcept;
```

Emits JavaScript event.

### Parameters

| | |
|-|-|
| `eventEmitterName` | name of the JavaScript emitter |
| `eventName` | name of JavaScript event |
| `args` | arguments passed to JavaScript event. They are serialized to [`IJSValueWriter`](cppapi-jsvaluewriter) using one of the [`WriteValue`](cppapi-jsvaluewriter) overloaded methods. |

### Return value

(none)

---

## `ReactContext::Handle`

```cpp
IReactContext const &Handle() const noexcept;
```

Returns the `IReactContext` instance wrapped up by the `ReactContext`.

### Parameters

(none)

### Return value

The `IReactContext` wrapped by the `ReactContext`. It may be empty.

---

## `ReactContext::JSDispatcher`

```cpp
ReactDispatcher JSDispatcher() const noexcept;
```

Gets [`ReactDispatcher`](cppapi-reactdispatcher) for the JavaScript dispatcher associated with the `ReactContext`.

### Parameters

(none)

### Return value

An instance of JavaScript [`ReactDispatcher`](cppapi-reactdispatcher) associated with the `ReactContext`.
The returned [`ReactDispatcher`](cppapi-reactdispatcher) is never empty.

---

## `ReactContext::Notifications`

```cpp
ReactNotificationService Notifications() const noexcept;
```

Gets a [`ReactNotificationService`](cppapi-reactnotificationservice) associated with the `ReactNativeHost` that created the current `ReactContext`.
This [`ReactNotificationService`](cppapi-reactnotificationservice) helps sending notifications between native modules and the host application.

### Parameters

(none)

### Return value

An instance of [`ReactNotificationService`](cppapi-reactnotificationservice) associated with the `ReactNativeHost`.
The returned [`ReactNotificationService`](cppapi-reactnotificationservice) is never empty.

---

## `ReactContext::Properties`

```cpp
ReactPropertyBag Properties() const noexcept;
```

Gets a [`ReactPropertyBag`](cppapi-reactpropertybag) associated with the `ReactNativeHost` that created the current `ReactContext`.
This [`ReactPropertyBag`](cppapi-reactpropertybag) helps sharing data between native modules and the host application.

### Parameters

(none)

### Return value

An instance of [`ReactPropertyBag`](cppapi-reactpropertybag) associated with the `ReactNativeHost`.
The returned [`ReactPropertyBag`](cppapi-reactpropertybag) is never empty.

---

## `ReactContext::UIDispatcher`

```cpp
ReactDispatcher UIDispatcher() const noexcept;
```

Gets [`ReactDispatcher`](cppapi-reactdispatcher) for the UI thread associated with the `ReactContext`.

### Parameters

(none)

### Return value

An instance of UI [`ReactDispatcher`](cppapi-reactdispatcher)  associated with the `ReactContext`.
The returned [`ReactDispatcher`](cppapi-reactdispatcher)  could be empty if the app has no UI.

---

## `ReactContext::operator bool`

```cpp
explicit operator bool() const noexcept;
```

Checks if the wrapped `IReactContext` is not empty.

### Parameters

(none)

### Return value

**true** if the wrapped `IReactContext` is not empty.
Otherwise it returns **false**.
