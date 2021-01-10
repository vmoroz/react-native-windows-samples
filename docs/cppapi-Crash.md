---
id: cppapi-crash
title: Crash
---

Defined in `ReactDispatcher.h`  
Namespace: **`winrt::Microsoft::ReactNative`**  
Namespace alias: **`React`**

#ifndef VerifyElseCrash
#define VerifyElseCrash(condition) \
  do {                             \
    if (!(condition)) {            \
      assert(false && #condition); \
      std::terminate();            \
    }                              \
  } while (false)
#endif

#ifndef VerifyElseCrashSz
#define VerifyElseCrashSz(condition, message) \