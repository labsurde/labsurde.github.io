---
layout: post
title: "Tuning C++ (CppCon 2015)"
date:   2021-01-03 17:00:00 +0900
categories: Optimization
---

# Tuning C++
- Source
  - [Tuning C++ by Chandler Carruth on CppCon 2015](https://youtu.be/nXaxk27zwlk)

# Methodology
- Measure first, tune what matters

# Macro benchmarking
- Go see [CppCon 2015 "Benchmarking C++ Code" by Bryce Adelstein Lelbach](https://youtu.be/zWxSZcpeS8Q)

# Micro benchmarking
- 시스템의 일부 narrow, tiny code를 측정하는 것
- Test a piece of code with benchmark tool. After proving your code is faster, merge the code into master.

## [Google Benchmark](https://github.com/google/benchmark)
- great micro benchmark framework
- Internally it uses `std::chronos` and it just make benchmark easier

## Profiler
- Chandler Carruth는 당시 `perf`를 가장 많이 쓴다고 한다.
- 통계보기
```
$perf stat ./bench
```
- profiling 결과 데이터를 덤프
```
$perf record ./bench
```
- profiling 결과 데이터 분석
  - 각 힘수별 소요 시간을 보여준다
```
$perf report
```
  - 그런데 어떤 함수가 어떤 함수로부터 불리웠는지를 알 수는 없다.
- Call graph 포함
```
$ perf record -g ./bench
$ perf report
```
  - 그런데 컴파일러에서 온갓 최적화를 하면서 call frame 이 정확히 볼 수 없는 상태이다. 그래서 정보가 별로 도움이 안되나.
  - 이 때는 컴파일할 때 `-fno-omit-frame-pointer` 옵션을 주어야 한다.
     - 컴파일러에게 frame pointer를 지우지 말라고 하는 옵션.
     - 물론 이 옵션을 주면 최적화가 100% 되지는 않지만 profiling 때는 할만한 가치가 있다.
  - 그런데 더 깊은 depth 로 그래프가 들어가지 않는 경우 `perf report -g` 때 다른 옵션을 더 주어야 한다.
- 좀 더 정확한 정보를 보기
```
$ perf report 'graph,0.5,caller'
```
  - `graph`는 시간%를 전체 시간에 normalize해서 보여준다.
  - `0.5`는 filter인데, noise 정보를 필터한다.
  - `caller`는 그래프를 _invert_ 한다. (보기 좋게 해준다 정도의 의미....?)
- `perf report`화면에서 shortcut key _a_ 를 누르면 assembly 를 보여준다.
- In assembly code, `jmp` makes cachec miss. So there should be ways to gather frequently running code together.
  - He used another magical code `__builtin_expect((bool)(x), 0)`.

# Things that should be studied again
- He showed some magical code: `asm volatile(....)`
  - [let's study more about this later](https://youtu.be/nXaxk27zwlk?list=PLHTh1InhhwT75gykhs7pqcR_uSiG601oh&t=2523)
- Another magical code `__builtin_expect((bool)(x), 0)`. [link](https://youtu.be/nXaxk27zwlk?list=PLHTh1InhhwT75gykhs7pqcR_uSiG601oh&t=4109)

# Other interesting skills
- 컴파일 할 때 -O3 같은 플래그가 많을 때 'flag' 파일로 저장하고 아래 명령어로 컴파일
    - `$(cat flag)` 도 동일함
```
$ clang++ $(< flag) -o bench bench.cpp -lbenchmark
```

