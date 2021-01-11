---
id: cppapi-jsvalue
title: JSValue
---

Defined in `JSValue.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct JSValue;
```

`JSValue` allows to post work items to a queue for asynchronous execution in a sequential order.
It wraps up the `IJSValue` C++/WinRT generated interface.

### Member functions

| Name | Description |
|------|-------------|
| **[`(constructor)`](#constructor)** | constructs the `JSValue` |
| **[`(destructor)`](#destructor)** | destructs the `JSValue` |
| **[`Null`](#jsvaluenull)** | `JSValue` with `JSValueType::Null` |
| **[`EmptyObject`](#jsvalueemptyobject)** | `JSValue` with empty object |
| **[`EmptyArray`](#jsvalueemptyarray)** | `JSValue` with empty array |
| **[`EmptyString`](#jsvalueemptystring)** | `JSValue` with empty string |
| **[`Copy`](#jsvaluecopy)** | does a deep copy of `JSValue` |
| **[`MoveObject`](#jsvaluemoveobject)** | moves out Object and set this to `JSValue::Null` |
| **[`MoveArray`](#jsvaluemovearay)** | moves out Array and set this to `JSValue::Null` |
| **[`Type`](#jsvaluetype)** | gets `JSValue` type |
| **[`IsNull`](#jsvalueisnull)** | returns `true` if `JSValue` type is `JSValueType::Null` |
| **[`TryGetObject`](#jsvaluetrygetobject)** | returns pointer to `JSValueObject` if `JSValue` type is `Object`, or `nullptr` otherwise |
| **[`TryGetArray`](#jsvaluetrygetarray)** | returns pointer to `JSValueArray` if `JSValue` type is `Array`, or `nullptr` otherwise |
| **[`TryGetString`](#jsvaluetrygetstring)** | returns pointer to `std::string` if `JSValue` type is `String`, or `nullptr` otherwise |
| **[`TryGetBoolean`](#jsvaluetrygetboolean)** | returns pointer to `bool` if `JSValue` type is `Boolean`, or `nullptr` otherwise |
| **[`TryGetInt64`](#jsvaluetrygetint64)** | returns pointer to `bool` if `JSValue` type is `Int64`, or `nullptr` otherwise |
| **[`TryGetDouble`](#jsvaluetrygetdouble)** | returns pointer to `bool` if `JSValue` type is `Double`, or `nullptr` otherwise |
| **[`AsObject`](#jsvalueasobject)** | returns `Object` representation of `JSValue`, or `EmptyObject` otherwise |
| **[`AsArray`](#jsvalueasarray)** | returns `Array` representation of `JSValue`, or `EmptyArray` otherwise |
| **[`AsString`](#jsvalueasstring)** | returns a string representation of `JSValue` |
| **[`AsBoolean`](#jsvalueasboolean)** | returns a Boolean representation of `JSValue` |
| **[`AsInt8`](#jsvalueasint8)** | returns an `int8_t` representation of `JSValue` |
| **[`AsInt16`](#jsvalueasint16)** | returns an `int16_t` representation of `JSValue` |
| **[`AsInt32`](#jsvalueasint32)** | returns an `int32_t` representation of `JSValue` |
| **[`AsInt64`](#jsvalueasint64)** | returns an `int64_t` representation of `JSValue` |
| **[`AsUInt8`](#jsvalueasuint8)** | returns an `uint8_t` representation of `JSValue` |
| **[`AsUInt16`](#jsvalueasuint16)** | returns an `uint16_t` representation of `JSValue` |
| **[`AsUInt32`](#jsvalueasuint32)** | returns an `uint32_t` representation of `JSValue` |
| **[`AsUInt64`](#jsvalueasuint64)** | returns an `uint64_t` representation of `JSValue` |
| **[`AsSingle`](#jsvalueassingle)** | returns a `float` representation of `JSValue` |
| **[`AsDouble`](#jsvalueasdouble)** | returns a `double` representation of `JSValue` |
| **[`AsJSString`](#jsvalueasjsstring)** | returns `std::string` value equivalent to JavaScript `String(value)` result |
| **[`AsJSBoolean`](#jsvalueasjsboolean)** | returns `bool` value equivalent to JavaScript `Boolean(value)` result |
| **[`AsJSNumber`](#jsvalueasjsnumber)** | returns `double` value equivalent to JavaScript `Number(value)` result |
| **[`ToString`](#jsvaluetostring)** | converts `JSValue` to a readable string that can be used for logging |
| **[`To`](#jsvalueto)** | serializes `JSValue` to the the type `T` |
| **[`From`](#jsvaluefrom)** | creates `JSValue` from a value of type `T` |
| **[`PropertyCount`](#jsvaluepropertycount)** | returns property count if `JSValue` is `Object`, or 0 otherwise |
| **[`TryGetObjectProperty`](#jsvaluetrygetobjectproperty)** | gets a pointer to `JSValue` object property value |
| **[`GetObjectProperty`](#jsvaluegetobjectproperty)** | gets a reference to `JSValue` object property value |
| **[`ItemCount`](#jsvalueitemcount)** | gets item count of `JSValue` array |
| **[`TryGetArrayItem`](#jsvaluetrygetarrayitem)** | gets a pointer to `JSValue` array item value |
| **[`GetArrayItem`](#jsvaluegetarrayitem)** | gets a reference to `JSValue` array item value |
| **[`Equals`](#jsvalueequals)** | returns `true` if this `JSValue` is strictly equal to another `JSValue` |
| **[`JSEquals`](#jsvaluejsequals)** | returns `true` if this `JSValue` is strictly equal to another `JSValue` after they are converted to the same type |
| **[`ReadFrom`](#jsvaluereadfrom)** | creates `JSValue` from a `IJSValueReader` |
| **[`ReadObjectFrom`](#jsvaluereadobjectFrom)** | creates `JSValueObject` from a `IJSValueReader` |
| **[`ReadArrayFrom`](#jsvaluereadarrayfrom)** | creates `JSValueArray` from a `IJSValueReader` |
| **[`WriteTo`](#jsvaluewriteto)** | writes this `JSValue` to `IJSValueWriter` |
| **[`operator =`](#jsvaluetrygetdouble)** | move-only assignment |
| **[`operator std::string`](#jsvaluetrygetdouble)** | casts `JSValue` to `std::string` using `AsString` call |
| **[`operator bool`](#jsvaluetrygetdouble)** | casts `JSValue` to `bool` using `AsBoolean` call |
| **[`operator int8_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `int8_t` using `AsInt8` call |
| **[`operator int16_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `int16_t` using `AsInt16` call |
| **[`operator int32_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `int32_t` using `AsInt32` call |
| **[`operator int64_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `int64_t` using `AsInt64` call |
| **[`operator uint8_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `uint8_t` using `AsUInt8` call |
| **[`operator uint16_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `uint16_t` using `AsUInt16` call |
| **[`operator uint32_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `uint32_t` using `AsUInt32` call |
| **[`operator uint64_t`](#jsvaluetrygetdouble)** | casts `JSValue` to `uint64_t` using `AsUInt64` call |
| **[`operator float`](#jsvaluetrygetdouble)** | casts `JSValue` to `float` using `AsSingle` call |
| **[`operator double`](#jsvaluetrygetdouble)** | casts `JSValue` to `double` using `AsDouble` call |
| **[`operator[]`](#jsvaluetrygetdouble)** | gets `JSValue` property or item |

---

## `(constructor)`

```cpp
JSValue() noexcept;
```

Creates a Null `JSValue`.

```cpp
explicit JSValue(std::nullptr_t) noexcept;
```

Creates a Null `JSValue`.

```cpp
JSValue(JSValueObject &&value) noexcept;
```

Creates an Object `JSValue`.

```cpp
JSValue(JSValueArray &&value) noexcept;
```

Creates an Array `JSValue`.

```cpp
JSValue(std::string &&value) noexcept;
```

Creates a String `JSValue`.

```cpp
template <class TStringView, std::enable_if_t<std::is_convertible_v<TStringView, std::string_view>, int> = 1>
JSValue(TStringView value) noexcept;
```

Creates a String `JSValue`.

```cpp
template <class TBool, std::enable_if_t<std::is_same_v<TBool, bool>, int> = 1>
JSValue(TBool value) noexcept;
```

Creates a Boolean `JSValue`.

```cpp
template <class TInt, std::enable_if_t<std::is_integral_v<TInt> && !std::is_same_v<TInt, bool>, int> = 1>
JSValue(TInt value) noexcept;
```

Creates an Int64 `JSValue` from any integral type except `bool`.

```cpp
JSValue(double value) noexcept;
```

Creates an Double `JSValue`.

```cpp
JSValue(const JSValue &other) = delete;
```

Deletes the copy constructor to avoid unexpected copies. Use the `Copy` method instead.

```cpp
JSValue(JSValue &&other) noexcept;
```

Move constructor. The `other` `JSValue` becomes `JSValue::Null`.

---

## `(destructor)`

```cpp
~JSValue() noexcept;
```

Destroys `JSValue`.

---

## `JSValue::Null`

```cpp
static JSValue const Null;
```

`JSValue` with `JSValueType::Null`.

---

## `JSValue::EmptyObject`

```cpp
static JSValue const EmptyObject;
```

`JSValue` with empty object.

---

## `JSValue::EmptyArray`

```cpp
static JSValue const EmptyArray;
```

`JSValue` with empty array.

---

## `JSValue::EmptyString`

```cpp
static JSValue const EmptyString;
```

`JSValue` with empty string.

---

## `JSValue::Copy`

```cpp
JSValue Copy() const noexcept;
```

Does a deep copy of `JSValue`.

### Parameters

(none)

### Return value

The `JSValue` deep copy.

---

## `JSValue::MoveObject`

```cpp
JSValueObject MoveObject() noexcept;
```

Moves out `JSValueObject` and sets this to `JSValue::Null`. It returns `JSValue::EmptyObject`
and keeps this `JSValue` unchanged if current type is not `JSValue::Object`.

### Parameters

(none)

### Return value

The `JSValue` deep copy.
