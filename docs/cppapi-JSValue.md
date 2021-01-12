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

### Member fields

| Name | Description |
|------|-------------|
| **[`Null`](#jsvaluenull-field)** | `JSValue` with [`JSValueType::Null`](JSValueType) type |
| **[`EmptyObject`](#jsvalueemptyobject-field)** | `JSValue` with empty object |
| **[`EmptyArray`](#jsvalueemptyarray-field)** | `JSValue` with empty array |
| **[`EmptyString`](#jsvalueemptystring-field)** | `JSValue` with empty string |

### Member functions

| Name | Description |
|------|-------------|
| **[`(constructor)`](#constructor)** | constructs the `JSValue` |
| **[`(destructor)`](#destructor)** | destructs the `JSValue` |
| **[`Copy`](#jsvaluecopy)** | does a deep copy of `JSValue` |
| **[`MoveObject`](#jsvaluemoveobject)** | moves out Object and sets this to [`JSValue::Null`](#jsvaluenull) |
| **[`MoveArray`](#jsvaluemovearray)** | moves out Array and sets this to [`JSValue::Null`](#jsvaluenull) |
| **[`Type`](#jsvaluetype)** | gets `JSValue` type |
| **[`IsNull`](#jsvalueisnull)** | checks if `JSValue` type is [`JSValueType::Null`](JSValueType) |
| **[`TryGetObject`](#jsvaluetrygetobject)** | gets pointer to [`JSValueObject`](cppapi-jsvalueobject) value if `JSValue` type is [`JSValueType::Object`](JSValueType) |
| **[`TryGetArray`](#jsvaluetrygetarray)** | gets pointer to [`JSValueArray`](cppapi-jsvaluearray) value if `JSValue` type is [`JSValueType::Array`](JSValueType) |
| **[`TryGetString`](#jsvaluetrygetstring)** | gets pointer to [`std::string`](https://en.cppreference.com/w/cpp/string/basic_string) value if `JSValue` type is [`JSValueType::String`](JSValueType) |
| **[`TryGetBoolean`](#jsvaluetrygetboolean)** | gets pointer to `bool` value if `JSValue` type is [`JSValueType::Boolean`](JSValueType) |
| **[`TryGetInt64`](#jsvaluetrygetint64)** | gets pointer to `int64_t` value if `JSValue` type is [`JSValueType::Int64`](JSValueType) |
| **[`TryGetDouble`](#jsvaluetrygetdouble)** | gets pointer to `double` value if `JSValue` type is [`JSValueType::Double`](JSValueType) |
| **[`PropertyCount`](#jsvaluepropertycount)** | gets object property count if `JSValue` type is [`JSValueType::Object`](JSValueType) |
| **[`TryGetObjectProperty`](#jsvaluetrygetobjectproperty)** | gets a pointer to `JSValue` object property value |
| **[`GetObjectProperty`](#jsvaluegetobjectproperty)** | gets a reference to `JSValue` object property value |
| **[`ItemCount`](#jsvalueitemcount)** | gets array item count if `JSValue` type is [`JSValueType::Array`](JSValueType) |
| **[`TryGetArrayItem`](#jsvaluetrygetarrayitem)** | gets a pointer to `JSValue` array item value |
| **[`GetArrayItem`](#jsvaluegetarrayitem)** | gets a reference to `JSValue` array item value |
| **[`Equals`](#jsvalueequals)** | strictly compares `JSValue` with another one |
| **[`JSEquals`](#jsvaluejsequals)** | compares `JSValue` with another one after value conversion |
| **[`operator =`](#jsvalueoperator-)** | move-only assignment |
| **[`operator []`](#jsvalueoperator--1)** | gets `JSValue` property or item |

### Member functions  for serialization

| Name | Description |
|------|-------------|
| **[`To`](#jsvalueto)** | creates value of type `T` from this `JSValue` |
| **[`From`](#jsvaluefrom)** | creates `JSValue` from a value of type `T` |
| **[`ReadFrom`](#jsvaluereadfrom)** | creates `JSValue` from a `IJSValueReader` |
| **[`ReadObjectFrom`](#jsvaluereadobjectFrom)** | creates `JSValueObject` from a `IJSValueReader` |
| **[`ReadArrayFrom`](#jsvaluereadarrayfrom)** | creates `JSValueArray` from a `IJSValueReader` |
| **[`WriteTo`](#jsvaluewriteto)** | writes this `JSValue` to `IJSValueWriter` |

### Member functions for value conversion

| Name | Description |
|------|-------------|
| **[`ToString`](#jsvaluetostring)** | converts `JSValue` to a readable string that can be used for logging |
| **[`AsObject`](#jsvalueasobject)** | gets `Object` representation of `JSValue`, or `EmptyObject` otherwise |
| **[`AsArray`](#jsvalueasarray)** | gest `Array` representation of `JSValue`, or `EmptyArray` otherwise |
| **[`AsString`](#jsvalueasstring)** | gets a string representation of `JSValue` |
| **[`AsBoolean`](#jsvalueasboolean)** | gets a Boolean representation of `JSValue` |
| **[`AsInt8`](#jsvalueasint8)** | gets an `int8_t` representation of `JSValue` |
| **[`AsInt16`](#jsvalueasint16)** | gets an `int16_t` representation of `JSValue` |
| **[`AsInt32`](#jsvalueasint32)** | gets an `int32_t` representation of `JSValue` |
| **[`AsInt64`](#jsvalueasint64)** | gets an `int64_t` representation of `JSValue` |
| **[`AsUInt8`](#jsvalueasuint8)** | gets an `uint8_t` representation of `JSValue` |
| **[`AsUInt16`](#jsvalueasuint16)** | gets an `uint16_t` representation of `JSValue` |
| **[`AsUInt32`](#jsvalueasuint32)** | gets an `uint32_t` representation of `JSValue` |
| **[`AsUInt64`](#jsvalueasuint64)** | gets an `uint64_t` representation of `JSValue` |
| **[`AsSingle`](#jsvalueassingle)** | gets a `float` representation of `JSValue` |
| **[`AsDouble`](#jsvalueasdouble)** | gets a `double` representation of `JSValue` |
| **[`AsJSString`](#jsvalueasjsstring)** | gets `std::string` value equivalent to JavaScript `String(value)` result |
| **[`AsJSBoolean`](#jsvalueasjsboolean)** | gets `bool` value equivalent to JavaScript `Boolean(value)` result |
| **[`AsJSNumber`](#jsvalueasjsnumber)** | gets `double` value equivalent to JavaScript `Number(value)` result |
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

### Non-member functions

| Name | Description |
|------|-------------|
| **[`operator ==`](#equality-operator-)** | checks equality using [`JSValue::Equals`](#jsvaluequals) |
| **[`operator !=`](#inequality-operator-)** | checks inequality using [`JSValue::Equals`](#jsvaluequals) |

---

## `JSValue::Null` field

```cpp
static JSValue const Null;
```

`JSValue` with [`JSValueType::Null`](JSValueType) type.

---

## `JSValue::EmptyObject` field

```cpp
static JSValue const EmptyObject;
```

`JSValue` with empty object and [`JSValueType::Object`](JSValueType) type.

---

## `JSValue::EmptyArray` field

```cpp
static JSValue const EmptyArray;
```

`JSValue` with empty array and [`JSValueType::Array`](JSValueType) type.

---

## `JSValue::EmptyString` field

```cpp
static JSValue const EmptyString;
```

`JSValue` with empty string and [`JSValueType::String`](JSValueType) type.

---

## `(constructor)`

```cpp
JSValue() noexcept;
```

Creates `JSValue` with [`JSValueType::Null`](JSValueType) type.

```cpp
explicit JSValue(std::nullptr_t) noexcept;
```

Creates `JSValue` with [`JSValueType::Null`](JSValueType) type.

```cpp
JSValue(JSValueObject &&value) noexcept;
```

Creates `JSValue` with [`JSValueType::Object`](JSValueType) type.

```cpp
JSValue(JSValueArray &&value) noexcept;
```

Creates `JSValue` with [`JSValueType::Array`](JSValueType) type.

```cpp
JSValue(std::string &&value) noexcept;
```

Creates `JSValue` with [`JSValueType::String`](JSValueType) type.

```cpp
template <class TStringView, std::enable_if_t<std::is_convertible_v<TStringView, std::string_view>, int> = 1>
JSValue(TStringView value) noexcept;
```

Creates `JSValue` with [`JSValueType::String`](JSValueType) type.
This constructor accepts any value that can be converted to [`std::string_view`](https://en.cppreference.com/w/cpp/string/basic_string_view)

```cpp
template <class TBool, std::enable_if_t<std::is_same_v<TBool, bool>, int> = 1>
JSValue(TBool value) noexcept;
```

Creates `JSValue` with [`JSValueType::Boolean`](JSValueType) type.

```cpp
template <class TInt, std::enable_if_t<std::is_integral_v<TInt> && !std::is_same_v<TInt, bool>, int> = 1>
JSValue(TInt value) noexcept;
```

Creates `JSValue` with [`JSValueType::Int64`](JSValueType) type from any integral type except `bool`.

```cpp
JSValue(double value) noexcept;
```

Creates `JSValue` with [`JSValueType::Double`](JSValueType) type.

```cpp
JSValue(const JSValue &other) = delete;
```

Deletes the copy constructor to avoid unexpected copies. Use the [`Copy`](#jsvaluecopy) method instead.

```cpp
JSValue(JSValue &&other) noexcept;
```

Move constructor. The `other` `JSValue` becomes [`JSValue::Null`](#jsvaluenull-field).

---

## `(destructor)`

```cpp
~JSValue() noexcept;
```

Destroys `JSValue`. We use the non-default destructor to clean up the union value.

---

## `JSValue::Copy`

```cpp
JSValue Copy() const noexcept;
```

Does a deep copy of `JSValue`.
Doing a deep copy could be an expensive operation.
For that reason the `JSValue` has no copy constructor to avoid accidental copies.

### Parameters

(none)

### Return value

New `JSValue` with a deep copy of all items from this `JSValue`.

---

## `JSValue::MoveObject`

```cpp
JSValueObject MoveObject() noexcept;
```

Moves out [`JSValueObject`](cppapi-jsvalueobject) and sets this `JSValue` to [`JSValue::Null`](#jsvaluenull-field) if current type is [`JSValueType::Object`](JSValueType).
Otherwise, it returns a copy of [`JSValue::EmptyObject`](#jsvalueemptyobject-field) and keeps this `JSValue` unchanged.

### Parameters

(none)

### Return value

Returns [`JSValueObject`](cppapi-jsvalueobject) if this `JSValue` type is [`JSValueType::Object`](JSValueType).
Otherwise, it returns a copy of [`JSValue::EmptyObject`](#jsvalueemptyobject-field).

---

## `JSValue::MoveArray`

```cpp
JSValueArray MoveArray() noexcept;
```

Moves out [`JSValueArray`](cppapi-jsvaluearray) and sets this `JSValue` to [`JSValue::Null`](#jsvaluenull-field) if current type is [`JSValueType::Array`](JSValueType).
Otherwise, it returns a copy of [`JSValue::EmptyArray`](#jsvalueemptyarray-field) and keeps this `JSValue` unchanged.

### Parameters

(none)

### Return value

Returns [`JSValueArray`](cppapi-jsvaluearray) if this `JSValue` type is [`JSValueType::Array`](JSValueType).
Otherwise, it returns a copy of [`JSValue::EmptyArray`](#jsvalueemptyarray-field).

---

## `JSValue::Type`

```cpp
JSValueType Type() const noexcept;
```

Gets `JSValue` type.

### Parameters

(none)

### Return value

Returns [`JSValueType`](JSValueType) of this `JSValue`.

---

## `JSValue::IsNull`

```cpp
bool IsNull() const noexcept;
```

Checks is this `JSValue` type is [`JSValueType::Null`](JSValueType).

### Parameters

(none)

### Return value

Returns **`true`** if `JSValue` type is [`JSValueType::Null`](JSValueType).
Otherwise, it returns **`false`**.

---

## `JSValue::TryGetObject`

```cpp
JSValueObject const *TryGetObject() const noexcept;
```

Gets pointer to read-only [`JSValueObject`](cppapi-jsvalueobject) value of this `JSValue`.

### Parameters

(none)

### Return value

Returns pointer to read-only [`JSValueObject`](cppapi-jsvalueobject) if this `JSValue` has type [`JSValueType::Object`](JSValueType). Otherwise, it returns **`nullptr`**.

---

## `JSValue::TryGetArray`

```cpp
JSValueArray const *TryGetArray() const noexcept;
```

Gets pointer to read-only [`JSValueArray`](cppapi-jsvaluearray) value of this `JSValue`.

### Parameters

(none)

### Return value

Returns pointer to read-only [`JSValueArray`](cppapi-jsvaluearray) if this `JSValue` has type [`JSValueType::Array`](JSValueType). Otherwise, it returns **`nullptr`**.

---

## `JSValue::TryGetString`

```cpp
std::string const *TryGetString() const noexcept;
```

Gets pointer to read-only [`std::string`](https://en.cppreference.com/w/cpp/string/basic_string) value of this `JSValue`.

### Parameters

(none)

### Return value

Returns pointer to read-only [`std::string`](https://en.cppreference.com/w/cpp/string/basic_string) if this `JSValue` has type [`JSValueType::String`](JSValueType). Otherwise, it returns **`nullptr`**.

---

## `JSValue::TryGetBoolean`

```cpp
bool const *TryGetBoolean() const noexcept;
```

Gets pointer to read-only `bool` value of this `JSValue`.

### Parameters

(none)

### Return value

Returns pointer to read-only `bool` if this `JSValue` has type [`JSValueType::Boolean`](JSValueType). Otherwise, it returns **`nullptr`**.

---

## `JSValue::TryGetInt64`

```cpp
int64_t const *TryGetInt64() const noexcept;
```

Gets pointer to read-only `int64_t` value of this `JSValue`.

### Parameters

(none)

### Return value

Returns pointer to read-only `int64_t` if this `JSValue` has type [`JSValueType::Int64`](JSValueType). Otherwise, it returns **`nullptr`**.

---

## `JSValue::TryGetDouble`

```cpp
double const *TryGetDouble() const noexcept;
```

Gets pointer to read-only `double` value of this `JSValue`.

### Parameters

(none)

### Return value

Returns pointer to read-only `double` if this `JSValue` has type [`JSValueType::Double`](JSValueType). Otherwise, it returns **`nullptr`**.

---

## `JSValue::PropertyCount`

```cpp
size_t PropertyCount() const noexcept;
```

Gets object property count if `JSValue` type is [`JSValueType::Object`](JSValueType).

### Parameters

(none)

### Return value

Returns object property count if `JSValue` type is [`JSValueType::Object`](JSValueType).
Otherwise, it returns 0.

---

## `JSValue::TryGetObjectProperty`

```cpp
JSValue const *TryGetObjectProperty(std::string_view propertyName) const noexcept;
```

Gets a pointer to a read-only `JSValue` object property value.

### Parameters

(none)

### Return value

Returns a pointer to a read-only `JSValue` object property value if `JSValue` type is [`JSValueType::Object`](JSValueType).
Otherwise, it returns **`nullptr`**.

---

## `JSValue::GetObjectProperty`

```cpp
JSValue const &GetObjectProperty(std::string_view propertyName) const noexcept;
```

Gets a reference to a read-only `JSValue` object property value.

### Parameters

(none)

### Return value

Returns a reference to a read-only `JSValue` object property value if `JSValue` type is [`JSValueType::Object`](JSValueType).
Otherwise, it returns a reference to [`JSValue::Null`](#jsvaluenull-field).

---

## `JSValue::ItemCount`

```cpp
size_t ItemCount() const noexcept;
```

Gets array item count if `JSValue` type is [`JSValueType::Array`](JSValueType).

### Parameters

(none)

### Return value

Returns array item count if `JSValue` type is [`JSValueType::Array`](JSValueType).
Otherwise, it returns 0.

---

## `JSValue::TryGetArrayItem`

```cpp
JSValue const *TryGetArrayItem(JSValueArray::size_type index) const noexcept;
```

Gets a pointer to a read-only `JSValue` array item value.

### Parameters

(none)

### Return value

Returns a pointer to a read-only `JSValue` array item value if `JSValue` type is [`JSValueType::Array`](JSValueType).
Otherwise, it returns **`nullptr`**.

---

## `JSValue::GetArrayItem`

```cpp
JSValue const &GetArrayItem(JSValueArray::size_type index) const noexcept;
```

Gets a reference to a read-only `JSValue` array item value.

### Parameters

(none)

### Return value

Returns a reference to a read-only `JSValue` array item value if `JSValue` type is [`JSValueType::Array`](JSValueType).
Otherwise, it returns a reference to [`JSValue::Null`](#jsvaluenull-field).

---

## `JSValue::Equals`

```cpp
bool Equals(JSValue const &other) const noexcept;
```

Checks strict equality with the `other` `JSValue`.

### Parameters

| Name | Description |
|------|-------------|
| **`other`** | a reference to another `JSValue` to compare with |

### Return value

Returns **`true`** if this `JSValue` is strictly equal to the `other` `JSValue`.
Both arrays must have the same size. Items must have the same type and value.
Otherwise, it returns **`false`**.

---

## `JSValueArray::JSEquals`

```cpp
bool JSEquals(JSValueArray const &other) const noexcept;
```

Checks JavaScript-like equality with the `other` `JSValueArray`.
It compares the size of `JSValueArray` and if it is the same, then it compares items using [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) method. The [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) compares values after they are converted to the same type.

### Parameters

| Name | Description |
|------|-------------|
| **`other`** | a reference to another `JSValueArray` |

### Return value

Returns **`true`** if this `JSValueArray` has the same size as the `other` `JSValueArray`,
and [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) returns **`true`** for each item.
Otherwise, it returns **`false`**.
