---
id: cppapi-jsvalueobject
title: JSValueObject
---

Defined in `JSValue.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

## Definition

```cpp
struct JSValueObject : std::map<std::string, JSValue, std::less<>>
```

The `JSValueObject` has two roles:

- `JSValueObject` is used as [`JSValue`](cppapi-jsvalue) object builder.
- read-only `JSValueObject` is used as [`JSValue`](cppapi-jsvalue) object value.

### Member functions

| Name | Description |
|------|-------------|
| **[`(constructor)`](#constructor)** | constructs the `JSValueObject` |
| **[`Copy`](#jsvalueobjectcopy)** | does a deep copy of the `JSValueObject` |
| **[`Equals`](#jsvalueobjectequals)** | strictly compares `JSValueObject` with another one |
| **[`JSEquals`](#jsvalueobjectjsequals)** | compares `JSValueObject` with another one after value conversion |
| **[`ReadFrom`](#jsvalueobjectreadfrom)** | creates new `JSValueObject` from `IJSValueReader` |
| **[`WriteTo`](#jsvalueobjectwriteto)** | writes the `JSValueObject` to `IJSValueWriter` |
| **[`operator =`](#jsvalueobjectoperator-)** | `JSValueObject` assignment operator |
| **[`operator []`](#jsvalueobjectoperator--1)** | access `JSValueObject` property |

### Non-member functions

| Name | Description |
|------|-------------|
| **[`operator ==`](#equality-operator-)** | checks equality using `JSValueObject::Equals` |
| **[`operator !=`](#inequality-operator-)** | checks inequality using `JSValueObject::Equals` |

### Notes

The [`JSValue`](cppapi-jsvalue) is an immutable value.
Since objects and arrays are complex values, we need special builder classes for them.
The `JSValueObject` is a builder for objects, while the [`JSValueArray`](cppapi-jsvaluearray) is a builder for array.

Like [`JSValue`](cppapi-jsvalue), the `JSValueObject` has no copy constructor or assignment operator. This is to avoid accidental expensive copies.
Use the [`Copy`](#jsvalueobjectcopy) method to do the explicit copy.

`JSValueObject` is based on
[`std::map<std::string, JSValue, std::less<>>`](https://en.cppreference.com/w/cpp/container/map) class.
The [`std::less<>`](https://en.cppreference.com/w/cpp/utility/functional/less) comparer specialization allows to use [`std::string_view`](https://en.cppreference.com/w/cpp/string/basic_string_view) to lookup properties without creation of [`std::string`](https://en.cppreference.com/w/cpp/string/basic_string) instances.
We can use any `std::map` methods to work with the `JSValueObject`.
In addition to the `std::map` methods, the `JSValueObject` has the following helper methods:

- Special constructor to move-initialize from the `std::intializer_list<JSValueObjectKeyValue>`. It initializes `JSValueObject` from any value pairs where the first value can be passed to [`std::string_view`](https://en.cppreference.com/w/cpp/string/basic_string_view) constructor and the second value to the `JSValue` constructor. E.g. we can write `JSValueObject{{"X", 5}, {"Y", true}}`.
- [`Equals`](#jsvalueobjectequals) method and standalone [equality ==](#equality-operator-) and [inequality !=](#inequality-operator-) operators to do strict deep comparison.
- [`JSEquals`](#jsvalueobjectjsequals) method to do JavaScript-like deep comparison where property values are converted to the same type before comparison.
- [`ReadFrom`](#jsvalueobjectreadfrom) method to construct `JSValueObject` from [`IJSValueReader`](IJSValueReader).
- [`WriteTo`](#jsvalueobjectwriteto) method to serialize `JSValueObject` to [`IJSValueWriter`](IJSValueWriter).
- [`operator []`](#jsvalueobjectoperator--1) to access property values using [`std::string_view`](https://en.cppreference.com/w/cpp/string/basic_string_view).

### Examples

Construct `JSValueObject` from an initializer list:

```cpp
auto obj = JSValueObject{{"p1", "X"}, {"p2", 42}, {"p3", nullptr}, {"p4", true}};
```

Use `JSValueObject` and `JSValueArray` in the initializer list:

```cpp
auto obj = JSValueObject{
  {"p1", "X"},
  {"p2", 42},
  {"p3", nullptr},
  {"p4", true}, 
  {"p5", JSValueObject{1, "2", 3}},
  {"p6", JSValueObject{{"Foo", 5}, {"Bar", 10}}}
};
```

The `JSValueObject` in inherited from `std::map` and we can use all `std::map` methods.  
Add `JSValueObject` properties using [`try_emplace`](https://en.cppreference.com/w/cpp/container/map/try_emplace) method:

```cpp
auto obj = JSValueObject();
obj.try_emplace("p1", "X");
obj.try_emplace("p2", 42);
obj.try_emplace("p3", nullptr);
obj.try_emplace("p4", true);
obj.try_emplace("p5", JSValueObject{1, "2", 3});
obj.try_emplace("p6", JSValueObject{{"Foo", 5}, {"Bar", 10}});
```

Get object property count.

```cpp
JSValueObject obj = ...;
auto propertyCount = obj.size();
```

Accessing object properties by name. We use `auto&` to avoid copy. Since `JSValue` has no copy constructor, the accidental use of `auto` will cause a compilation error.

```cpp
JSValueObject obj = ...;
auto& propValue = obj["myProp"];
```

To get a copy of a property value, we use the [`JSValue::Copy`](cppapi-jsvalue#jsvaluecopy) method. Note, that [`JSValue::Copy`](cppapi-jsvalue#jsvaluecopy) method does a deep copy that can be expensive.

```cpp
JSValueObject obj = ...;
auto propValueCopy = arr["myProp"].Copy(); // when we absolutely must to get a property value copy.
```

---

## `(constructor)`

```cpp
JSValueObject() = default;
```

Default constructor. Constructs an empty `JSValueObject`.

```cpp
template <class TMoveInputIterator>
JSValueObject(TMoveInputIterator first, TMoveInputIterator last) noexcept;
```

Constructs `JSValueObject` from the move iterator.

```cpp
JSValueObject(std::initializer_list<JSValueObjectKeyValue> initObject) noexcept;
```

Move-constructs `JSValueObject` from the initializer list.
The `JSValueObjectKeyValue` is an internal helper struct that initializes its `std::string_view` and `JSValue` fields with a pair of values that can be passed to `std::string_view` and `JSValue` constructors. The `JSValueObjectKeyValue` must not be used directly. Instead, provide a value pair which can initialize the `JSValueObjectKeyValue`.

```cpp
JSValueObject(std::map<std::string, JSValue, std::less<>> &&other) noexcept;
```

Move-constructs `JSValueObject` from the `std::string`-`JSValue` map.

```cpp
JSValueObject(JSValueObject const &) = delete;
```

Deletes copy constructor to avoid unexpected copies. Use the [`Copy`](#jsvalueobjectcopy) method instead.

```cpp
JSValueObject(JSValueObject &&) = default;
```

Default move constructor.

---

## `JSValueObject::Copy`

```cpp
JSValueObject Copy() const noexcept;
```

Does a deep copy of the `JSValueObject`.
Doing a deep copy could be an expensive operation.
For that reason the `JSValueObject` has no copy constructor to avoid accidental copies.

### Parameters

(none)

### Return value

New `JSValueObject` with a deep copy of all items from this `JSValueObject`.

---

## `JSValueObject::Equals`

```cpp
bool Equals(JSValueObject const &other) const noexcept;
```

Checks strict equality with the `other` `JSValueObject`.

### Parameters

| Name | Description |
|------|-------------|
| **`other`** | a reference to another `JSValueObject` to compare with |

### Return value

Returns **`true`** if this `JSValueObject` is strictly equal to the `other` `JSValueObject`.
Both objects must have the same set of properties where the corresponding values are matched by [`JSValue::Equals`](cppapi-jsvalue#jsvalueequals) method.
Otherwise, it returns **`false`**.

---

## `JSValueObject::JSEquals`

```cpp
bool JSEquals(JSValueObject const &other) const noexcept;
```

Checks JavaScript-like equality with the `other` `JSValueObject`.
Both objects must have the same set of properties where the corresponding values are matched by [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) method. The [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) compares values after they are converted to the same type.

### Parameters

| Name | Description |
|------|-------------|
| **`other`** | a reference to another `JSValueObject` |

### Return value

Returns **`true`** if this `JSValueObject` has the same set of properties as the `other` `JSValueObject`,
and [`JSValue::JSEquals`](cppapi-jsvalue#jsvaluejsequals) returns **`true`** for each property value with the same name.
Otherwise, it returns **`false`**.

---

## `JSValueObject::ReadFrom`

```cpp
static JSValueObject ReadFrom(IJSValueReader const &reader) noexcept;
```

Creates new `JSValueObject` from the `IJSValueReader`.

### Parameters

| Name | Description |
|------|-------------|
| **`reader`** | a `IJSValueReader` to read values from |

### Return value

Returns new `JSValueObject` initialized from the `IJSValueReader`.
If the provided `IJSValueReader` is not in the object reading state, then it returns an empty `JSValueObject` and state of `IJSValueReader` is not changed.

---

## `JSValueObject::WriteTo`

```cpp
void WriteTo(IJSValueWriter const &writer) const noexcept;
```

Writes this `JSValueObject` to the `IJSValueWriter`.

### Parameters

| Name | Description |
|------|-------------|
| **`writer`** | a `IJSValueWriter` to write values to |

### Return value

(none)

---

## `JSValueObject::operator =`

```cpp
JSValueObject &operator=(JSValueObject &&) = default;
```

Default move assignment.

```cpp
JSValueObject &operator=(JSValueObject const &) = delete;
```

Delete copy assignment to avoid unexpected copies. Use the [`Copy`](#jsvalueobjectcopy) method instead.

---

## `JSValueObject::operator []`

```cpp
JSValue &operator[](std::string_view propertyName) noexcept;
```

Gets a reference to object property value if the property is found,
or a reference to a new property created with JSValue::Null value otherwise.

```cpp
JSValue const &operator[](std::string_view propertyName) const noexcept;
```

Gets a reference to object property value if the property is found,
or a reference to [`JSValue::Null`](cppapi-jsvalue#jsvaluenull) otherwise.

---

## Equality `operator ==`

```cpp
bool operator==(JSValueObject const &left, JSValueObject const &right) noexcept;
```

### Parameters

| Name | Description |
|------|-------------|
| **`left`** | `JSValueObject` to compare |
| **`right`** | `JSValueObject` to compare |

### Return value

It returns result of the `left.Equals(right)` operation.
It returns **`true`** if both objects have the same size and `JSValue::Equals` returns **`true`** for each pair of items.
Otherwise, it returns **`false`**.

---

## Inequality `operator !=`

```cpp
bool operator!=(JSValueObject const &left, JSValueObject const &right) noexcept;
```

### Parameters

| Name | Description |
|------|-------------|
| **`left`** | `JSValueObject` to compare |
| **`right`** | `JSValueObject` to compare |

### Return value

It returns result of the `!(left == right)` operation.
