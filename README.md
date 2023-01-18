# Minimal reproduction of SvelteKit dev server OOM

### Steps to reproduce the issue:

1. Run `npm run dev`.
2. Load `localhost:5173/largefile` 

### Observed behavior:   
Upon loading the page the dev server (not the browser) suddenly consumes more than 2GB of memory and crashes due to reaching the heap allocation limit (on my system).

Example console output of one such crash (produced on a Windows 10 computer with 8GB of RAM):
```
C:\...\sveltekit-oom-repro>npm run dev

> sveltekit-oom-repro@0.0.1 dev
> vite dev



  VITE v4.0.4  ready in 707 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help

<--- Last few GCs --->

[14108:0000027B8A881300]    97882 ms: Mark-sweep 2035.1 (2081.8) -> 2035.1 (2081.8) MB, 984.5 / 0.0 ms  (average mu = 0.218, current mu = 0.010) allocation failure; scavenge might not succeed
[14108:0000027B8A881300]    99854 ms: Mark-sweep 2050.9 (2081.8) -> 2050.9 (2113.8) MB, 1964.3 / 0.0 ms  (average mu = 0.111, current mu = 0.004) allocation failure; scavenge might not succeed


<--- JS stacktrace --->

FATAL ERROR: Reached heap limit Allocation failed - JavaScript heap out of memory
 1: 00007FF6DE509E7F node_api_throw_syntax_error+175967
 2: 00007FF6DE490C06 SSL_get_quiet_shutdown+65750
 3: 00007FF6DE491FC2 SSL_get_quiet_shutdown+70802
 4: 00007FF6DEF2A214 v8::Isolate::ReportExternalAllocationLimitReached+116
 5: 00007FF6DEF15572 v8::Isolate::Exit+674
 6: 00007FF6DED973CC v8::internal::EmbedderStackStateScope::ExplicitScopeForTesting+124
 7: 00007FF6DED945EB v8::internal::Heap::CollectGarbage+3963
 8: 00007FF6DEDAA823 v8::internal::HeapAllocator::AllocateRawWithLightRetrySlowPath+2099
 9: 00007FF6DEDAB0CD v8::internal::HeapAllocator::AllocateRawWithRetryOrFailSlowPath+93
10: 00007FF6DEDBA903 v8::internal::Factory::NewFillerObject+851
11: 00007FF6DEAABEB5 v8::internal::DateCache::Weekday+1349
12: 00007FF6DEFC78B1 v8::internal::SetupIsolateDelegate::SetupHeap+558193
13: 00007FF65F2BE29A

C:\...\sveltekit-oom-repro>
```
