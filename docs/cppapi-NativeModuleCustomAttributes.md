---
id: cppapi-customattributes
title: Native Module Custom Attributes
---

## Native module custom attributes

| | |
|-|-|
| REACT_MODULE | |
| REACT_INIT | |
| REACT_METHOD | |
| REACT_METHOD | |
| REACT_SYNC_METHOD | |
| REACT_CONSTANT_PROVIDER | |
| REACT_CONSTANT | |
| REACT_EVENT | |
| REACT_FUNCTION | |
| REACT_STRUCT | |
| REACT_FIELD | |

// REACT_MODULE(moduleStruct, [opt] moduleName, [opt] eventEmitterName)
// Arguments:
// - moduleStruct (required) - the struct name the macro is attached to.
// - moduleName (optional) - the module name visible to JavaScript. Default is the moduleStruct name.
// - eventEmitterName (optional) - the default event emitter name used by REACT_EVENT.
//   Default is the RCTDeviceEventEmitter.
//
// REACT_MODULE annotates a C++ struct as a ReactNative module.
// It can be any struct which can be instantiated using a default constructor.
// Note that it must be a 'struct', not 'class' because macro does a forward declaration using the 'struct' keyword.
#define REACT_MODULE(/* moduleStruct, [opt] moduleName, [opt] eventEmitterName */...) \
  INTERNAL_REACT_MODULE(__VA_ARGS__)(__VA_ARGS__)

// REACT_INIT(method)
// Arguments:
// - method (required) - the method name the macro is attached to.
//
// REACT_INIT annotates a method that is called when a native module is initialized.
// It must have 'IReactContext const &' parameter.
// It must be an instance method.
#define REACT_INIT(method) INTERNAL_REACT_MEMBER_2_ARGS(InitMethod, method)

// REACT_METHOD(method, [opt] methodName)
// Arguments:
// - method (required) - the method name the macro is attached to.
// - methodName (optional) - the method name visible to JavaScript. Default is the method name.
//
// REACT_METHOD annotates a method to export to JavaScript.
// It declares an asynchronous method. To return a value:
// - Return void and have a Callback as a last parameter. The Callback type can be any std::function like type. E.g.
//   Func<void(Args...)>.
// - Return void and have two callbacks as last parameters. One is used to return value and another an error.
// - Return void and have a ReactPromise as a last parameter. In JavaScript the method returns Promise.
// - Return non-void value. In JavaScript it is treated as a method with one Callback. Return std::pair<Error, Value> to
//   be able to communicate error condition.
// It can be an instance or static method.
#define REACT_METHOD(/* method, [opt] methodName */...) INTERNAL_REACT_MEMBER(__VA_ARGS__)(AsyncMethod, __VA_ARGS__)

// REACT_SYNC_METHOD(method, [opt] methodName)
// Arguments:
// - method (required) - the method name the macro is attached to.
// - methodName (optional) - the method name visible to JavaScript. Default is the method name.
//
// REACT_SYNC_METHOD annotates a method that is called synchronously.
// It must be used rarely because it may cause out-of-order execution when used along with asynchronous methods.
// The method must return non-void value type.
// It can be an instance or static method.
#define REACT_SYNC_METHOD(/* method, [opt] methodName */...) INTERNAL_REACT_MEMBER(__VA_ARGS__)(SyncMethod, __VA_ARGS__)

// REACT_CONSTANT_PROVIDER(method)
// Arguments:
// - method (required) - the method name the macro is attached to.
//
// REACT_CONSTANT_PROVIDER annotates a method that defines constants.
// It must have 'ReactConstantProvider &' parameter.
// It can be an instance or static method.
#define REACT_CONSTANT_PROVIDER(method) INTERNAL_REACT_MEMBER_2_ARGS(ConstantMethod, method)

// REACT_CONSTANT(field, [opt] constantName)
// Arguments:
// - field (required) - the field name the macro is attached to.
// - constantName (optional) - the constant name visible to JavaScript. Default is the field name.
//
// REACT_CONSTANT annotates a field that defines a constant.
// It can be an instance or static field.
#define REACT_CONSTANT(/* field, [opt] constantName */...) \
  INTERNAL_REACT_MEMBER(__VA_ARGS__)(ConstantField, __VA_ARGS__)

// REACT_EVENT(field, [opt] eventName, [opt] eventEmitterName)
// Arguments:
// - field (required) - the field name the macro is attached to.
// - eventName (optional) - the JavaScript event name. Default is the field name.
// - eventEmitterName (optional) - the JavaScript module event emitter name. Default is module's eventEmitterName which
//   is by default 'RCTDeviceEventEmitter'.
//
// REACT_EVENT annotates a field that helps raise a JavaScript event.
// The field type can be any std::function like type. E.g. Func<void(Args...)>.
// It must be an instance field.
#define REACT_EVENT(/* field, [opt] eventName, [opt] eventEmitterName */...) \
  INTERNAL_REACT_MEMBER(__VA_ARGS__)(EventField, __VA_ARGS__)

// REACT_FUNCTION(field, [opt] functionName, [opt] moduleName)
// Arguments:
// - field (required) - the field name the macro is attached to.
// - functionName (optional) - the JavaScript function name. Default is the field name.
// - moduleName (optional) - the JavaScript module name. Default is module's moduleName which is by default the class
//   name.
//
// REACT_FUNCTION annotates a field that helps calling a JavaScript function.
// The field type can be any std::function like type. E.g. Func<void(Args...)>.
// It must be an instance field.
#define REACT_FUNCTION(/* field, [opt] functionName, [opt] moduleName */...) \
  INTERNAL_REACT_MEMBER(__VA_ARGS__)(FunctionField, __VA_ARGS__)

#define REACT_SHOW_METHOD_SIGNATURES(methodName, signatures)                      \
  " (see details below in output).\n"                                             \
  "  It must be one of the following:\n" signatures                               \
  "  The C++ method name could be different. In that case add the L\"" methodName \
  "\" to the attribute:\n"                                                        \
  "    REACT_METHOD(method, L\"" methodName "\")\n...\n"

#define REACT_SHOW_SYNC_METHOD_SIGNATURES(methodName, signatures)                 \
  " (see details below in output).\n"                                             \
  "  It must be one of the following:\n" signatures                               \
  "  The C++ method name could be different. In that case add the L\"" methodName \
  "\" to the attribute:\n"                                                        \
  "    REACT_SYNC_METHOD(method, L\"" methodName "\")\n...\n"

#define REACT_SHOW_METHOD_SPEC_ERRORS(index, methodName, signatures)                                        \
  static_assert(methodCheckResults[index].IsUniqueName, "Name '" methodName "' used for multiple methods"); \
  static_assert(                                                                                            \
      methodCheckResults[index].IsMethodFound,                                                              \
      "Method '" methodName "' is not defined" REACT_SHOW_METHOD_SIGNATURES(methodName, signatures));       \
  static_assert(                                                                                            \
      methodCheckResults[index].IsSignatureMatching,                                                        \
      "Method '" methodName "' does not match signature" REACT_SHOW_METHOD_SIGNATURES(methodName, signatures));

#define REACT_SHOW_SYNC_METHOD_SPEC_ERRORS(index, methodName, signatures)                                   \
  static_assert(methodCheckResults[index].IsUniqueName, "Name '" methodName "' used for multiple methods"); \
  static_assert(                                                                                            \
      methodCheckResults[index].IsMethodFound,                                                              \
      "Method '" methodName "' is not defined" REACT_SHOW_SYNC_METHOD_SIGNATURES(methodName, signatures));  \
  static_assert(                                                                                            \
      methodCheckResults[index].IsSignatureMatching,                                                        \
      "Method '" methodName "' does not match signature" REACT_SHOW_SYNC_METHOD_SIGNATURES(methodName, signatures));


// REACT_STRUCT(structType)
// Arguments:
// - structType (required) - the struct name the macro is attached to.
//
// REACT_STRUCT annotates a C++ struct that then can be serialized and deserialized with IJSValueReader and
// IJSValueWriter. With the help of REACT_FIELD it generates FieldMap associated with the struct which then used by
// ReactValue and ReactWrite methods. Cannot be nested inside REACT_MODULE.
#define REACT_STRUCT(structType) INTERNAL_REACT_STRUCT(structType)

// REACT_FIELD(field, [opt] fieldName)
// Arguments:
// - field (required) - the field the macro is attached to.
// - fieldName (optional) - the field name visible to JavaScript. Default is the field name.
//
// REACT_FIELD annotates a field to be added to FieldMap which then used by ReactValue and ReactWrite methods.
#define REACT_FIELD(/* field, [opt] fieldName */...) INTERNAL_REACT_FIELD(__VA_ARGS__)(__VA_ARGS__)