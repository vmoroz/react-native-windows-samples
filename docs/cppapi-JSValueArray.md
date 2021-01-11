---
id: cppapi-jsvaluearray
title: JSValueArray
---

Defined in `JSValue.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct JSValueArray : std::vector<JSValue>
```

`JSValueArray` is based on
[`std::vector<JSValue>`](https://en.cppreference.com/w/cpp/container/vector).
It has a constructor to move-initialize from the `std::intializer_list<JSValue>` that allows us to write `JSValueArray{"X", 42, nullptr, true}` and then assign it to `JSValue`. It also has `ReadFrom` method to initialize from `IJSValueReader` and `WriteTo` to serialize to `IJSValueWriter`.

### Member functions

| Name | Description |
|------|-------------|
| **[`(constructor)`](#constructor)** | constructs the `JSValueArray` |
| **[`Copy`](#jsvaluearraycopy)** | does a deep copy of the `JSValueArray` |
| **[`Equals`](#jsvaluearrayequals)** | strictly compares `JSValueArray` with another one |
| **[`JSEquals`](#jsvaluearrayjsequals)** | compares `JSValueArray` with another one after value type conversion |
| **[`ReadFrom`](#jsvaluearrayreadfrom)** | creates new `JSValueArray` from `IJSValueReader` |
| **[`WriteTo`](#jsvaluearraywriteto)** | writes the `JSValueArray` to `IJSValueWriter` |
| **[`operator =`](#jsvaluearrayoperator-)** | `JSValueArray` assignment operator |

### Non-member functions

| Name | Description |
|------|-------------|
| **[`operator ==`](#operator-)** | checks equality using `JSValueArray::Equals` |
| **[`operator !=`](#operator--1)** | checks inequality using `JSValueArray::Equals` |

---

## `(constructor)`

```cpp
JSValueArray() = default;
```

Default constructor. Constructs an empty `JSValueArray`.

```cpp
explicit JSValueArray(size_type size) noexcept;
```

Constructs `JSValueArray` with `size` count of `JSValue::Null` elements.

```cpp
JSValueArray(size_type size, JSValue const &defaultValue) noexcept;
```

Constructs `JSValueArray` with `size` count elements.
Each element is a copy of `defaultValue`.

```cpp
template <class TMoveInputIterator, std::enable_if_t<!std::is_integral_v<TMoveInputIterator>, int> = 1>
JSValueArray(TMoveInputIterator first, TMoveInputIterator last) noexcept;
```

Constructs `JSValueArray` from the move iterator.

```cpp
JSValueArray(std::initializer_list<JSValueArrayItem> initObject) noexcept;
```

Move-constructs `JSValueArray` from the initializer list.

```cpp
  JSValueArray(std::vector<JSValue> &&other) noexcept;
```

Move-constructs `JSValueArray` from the `JSValue` vector.

```cpp
JSValueArray(JSValueArray const &) = delete;
```

Deletes copy constructor to avoid unexpected copies. Use the `Copy` method instead.

```cpp
JSValueArray(JSValueArray &&) = default;
```

Default move constructor.

---

## `JSValueArray::Copy`

```cpp
JSValueArray Copy() const noexcept;
```

Does a deep copy of the `JSValueArray`.
Doing a deep copy could be an expensive operation.
For that reason the `JSValueArray` has no copy constructor to avoid accidental copies.

### Parameters

(none)

### Return value

New `JSValueArray` with a deep copy of all items in the current `JSValueArray`.

---

## `JSValueArray::Equals`

```cpp
bool Equals(JSValueArray const &other) const noexcept;
```

Checks strict equality with the `other` `JSValueArray`.

### Parameters

| Name | Description |
|------|-------------|
| **`other`** | a reference to another `JSValueArray` to compare with |

### Return value

Returns **`true`** if this `JSValueArray` is strictly equal to the `other` `JSValueArray`.
Both arrays must have the same size. Items must have the same type and value.
Otherwise, it returns **`false`**.

---

## `JSValueArray::JSEquals`

```cpp
bool JSEquals(JSValueArray const &other) const noexcept;
```

Checks JavaScript-like equality with the `other` `JSValueArray`.
It compares the size of `JSValueArray` and if it is the same, then it compares items using [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) method. The [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) compares items after their values is converted to the same type.

### Parameters

| Name | Description |
|------|-------------|
| **`other`** | a reference to another `JSValueArray` |

### Return value

Returns **`true`** if this `JSValueArray` has the same size as the `other` `JSValueArray`,
and [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) returns **`true`** for each item.
Otherwise, it returns **`false`**.

---

## `JSValueArray::ReadFrom`

```cpp
static JSValueArray ReadFrom(IJSValueReader const &reader) noexcept;
```

Creates new `JSValueArray` from the `IJSValueReader`.

### Parameters

| Name | Description |
|------|-------------|
| **`reader`** | a `IJSValueReader` to read values from |

### Return value

Returns new `JSValueArray` initialized from the `IJSValueReader`.
If the provided `IJSValueReader` is not in the array reading state, then it returns an empty `JSValueArray` and state of `IJSValueReader` is not changed.

---

## `JSValueArray::WriteTo`

```cpp
void WriteTo(IJSValueWriter const &writer) const noexcept;
```

Writes this `JSValueArray` to the `IJSValueWriter`.

### Parameters

| Name | Description |
|------|-------------|
| **`writer`** | a `IJSValueWriter` to write values to |

### Return value

(none)

---

## `JSValueArray::operator =`

```cpp
JSValueArray &operator=(JSValueArray &&) = default;
```

Default move assignment.

```cpp
JSValueArray &operator=(JSValueArray const &) = delete;
```

Delete copy assignment to avoid unexpected copies. Use the `Copy` method instead.

---

## `operator ==`

```cpp
bool operator==(JSValueArray const &left, JSValueArray const &right) noexcept;
```

### Parameters

| Name | Description |
|------|-------------|
| **`left`** | `JSValueArray` to compare |
| **`right`** | `JSValueArray` to compare |

### Return value

It returns result of the `left.Equals(right)` operation.
It returns **`true`** if both arrays have the same size and `JSValue::Equals` returns **`true`** for each pair of items.
Otherwise, it returns **`false`**.

---

## `operator !=`

```cpp
bool operator!=(JSValueArray const &left, JSValueArray const &right) noexcept;
```

### Parameters

| Name | Description |
|------|-------------|
| **`left`** | `JSValueArray` to compare |
| **`right`** | `JSValueArray` to compare |

### Return value

It returns result of the `!(left == right)` operation.
