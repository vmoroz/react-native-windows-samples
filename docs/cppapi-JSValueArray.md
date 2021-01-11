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

The `JSValueArray` has two roles:

- `JSValueArray` is used as [`JSValue`](cppapi-jsvalue) array builder.
- read-only `JSValueArray` is used as [`JSValue`](cppapi-jsvalue) array value.

### Member functions

| Name | Description |
|------|-------------|
| **[`(constructor)`](#constructor)** | constructs the `JSValueArray` |
| **[`Copy`](#jsvaluearraycopy)** | does a deep copy of the `JSValueArray` |
| **[`Equals`](#jsvaluearrayequals)** | strictly compares `JSValueArray` with another one |
| **[`JSEquals`](#jsvaluearrayjsequals)** | compares `JSValueArray` with another one after value conversion |
| **[`ReadFrom`](#jsvaluearrayreadfrom)** | creates new `JSValueArray` from `IJSValueReader` |
| **[`WriteTo`](#jsvaluearraywriteto)** | writes the `JSValueArray` to `IJSValueWriter` |
| **[`operator =`](#jsvaluearrayoperator-)** | `JSValueArray` assignment operator |

### Non-member functions

| Name | Description |
|------|-------------|
| **[`operator ==`](#equality-operator-)** | checks equality using `JSValueArray::Equals` |
| **[`operator !=`](#inequality-operator-)** | checks inequality using `JSValueArray::Equals` |

### Notes

The [`JSValue`](cppapi-jsvalue) is an immutable value.
Since objects and arrays are complex values, we need special builder classes for them.
The `JSValueArray` is a builder for arrays, while the [`JSValueObject`](cppapi-jsvalueobject) is a builder for objects.

Like [`JSValue`](cppapi-jsvalue), the `JSValueArray` has no copy constructor or assignment operator. This is to avoid accidental expensive copies.
Use the [`Copy`](#jsvaluearraycopy) method to do the explicit copy.

`JSValueArray` is based on
[`std::vector<JSValue>`](https://en.cppreference.com/w/cpp/container/vector).
We can use any `std::vector` methods to work with the `JSValueArray`.
In addition to the `std::vector` methods, the `JSValueArray` has the following helper methods:

- Special constructor to move-initialize from the `std::intializer_list<JSValueArrayItem>`. It initializes `JSValueArray` from any values that can be passed to `JSValue` constructor. E.g. we can write `JSValueArray{"X", 42, nullptr, true}`.
- [`Equals`](#jsvaluearrayequals) method and standalone [equality ==](#equality-operator-) and [inequality !=](#inequality-operator-) operators to do strict deep comparison.
- [`JSEquals`](#jsvaluearrayjsequals) method to do JavaScript-like deep comparison where values are converted to the same type before comparison.
- [`ReadFrom`](#jsvaluearrayreadfrom) method to construct `JSValueArray` from [`IJSValueReader`](IJSValueReader).
- [`WriteTo`](#jsvaluearraywriteto) method to serialize `JSValueArray` to [`IJSValueWriter`](IJSValueWriter).

### Examples

Construct `JSValueArray` from an initializer list:

```cpp
auto arr = JSValueArray{"X", 42, nullptr, true};
```

Use `JSValueArray` and `JSValueObject` in the initializer list:

```cpp
auto arr = JSValueArray{
  "X",
  42,
  nullptr,
  true, 
  JSValueArray{1, "2", 3},
  JSValueObject{{"Foo", 5}, {"Bar", 10}}
};
```

The `JSValueArray` in inherited from `std::vector` and we can use all `std::vector` methods.  
Add `JSValueArray` items using [`emplace_back`](https://en.cppreference.com/w/cpp/container/vector/emplace_back) method:

```cpp
auto arr = JSValueArray();
arr.emplace_back("X");
arr.emplace_back(42);
arr.emplace_back(nullptr);
arr.emplace_back(true);
arr.emplace_back(JSValueArray{1, "2", 3});
arr.emplace_back(JSValueObject{{"Foo", 5}, {"Bar", 10}});
```

Get size of the array.

```cpp
JSValueArray arr = ...;
auto itemCount = arr.size();
```

Accessing array items by index. We use `auto&` to avoid copy. Since `JSValue` has no copy constructor, the accidental use of `auto` will cause a compilation error.

```cpp
JSValueArray arr = ...;
auto& item = arr[2];
```

To get a copy of an item, we use the [`JSValue::Copy`](cppapi-jsvalue#jsvaluecopy) method. Note, that [`JSValue::Copy`](cppapi-jsvalue#jsvaluecopy) method does a deep copy that can be expensive.

```cpp
JSValueArray arr = ...;
auto item = arr[2].Copy(); // when we absolutely must to get an item copy.
```

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
The `JSValueArrayItem` is an internal helper struct that initializes its `JSValue` field with any value that can be passed to `JSValue` constructor. The `JSValueArrayItem` must not be used directly. Instead, provide a value which can initialize the `JSValueArrayItem`.

```cpp
JSValueArray(std::vector<JSValue> &&other) noexcept;
```

Move-constructs `JSValueArray` from the `JSValue` vector.

```cpp
JSValueArray(JSValueArray const &) = delete;
```

Deletes copy constructor to avoid unexpected copies. Use the [`Copy`](jsvaluearraycopy) method instead.

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
It compares the size of `JSValueArray` and if it is the same, then it compares items using [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) method. The [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) compares values after they are converted to the same type.

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

## Equality `operator ==`

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

## Inequality `operator !=`

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
