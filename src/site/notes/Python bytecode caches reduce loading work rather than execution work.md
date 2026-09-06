---
{"dg-publish":true,"permalink":"/python-bytecode-caches-reduce-loading-work-rather-than-execution-work/","title":"Python bytecode caches reduce loading work rather than execution work","hideInFiletree":true,"tags":["programming"],"noteIcon":"","dg-note-properties":{"title":"Python bytecode caches reduce loading work rather than execution work","categories":["Programming Languages"],"tags":["programming"],"created":"2026-09-07","updated":"2026-09-07"}}
---

Python's compiled module cache saves repeated loading work; it does not make the same program execute its instructions faster. The [module tutorial](https://docs.python.org/3/tutorial/modules.html#compiled-python-files) distinguishes these two costs explicitly.

CPython can compile imported modules and cache the result as version-tagged `.pyc` files under `__pycache__`. This happens automatically, so calling Python interpreted does not imply that compilation never occurs. The directly executed command-line module is an important exception: it is recompiled without storing that cache.

The practical distinction is between preparing code to run and performing its work. A warm module cache can affect startup without improving an expensive computation. [[NumPy vectorization moves loops into compiled operations\|NumPy vectorization moves loops into compiled operations]] addresses a different cost by changing where repeated operations execute.

This also qualifies the measurement principle in [[Desktop performance claims need workload measurements\|Desktop performance claims need workload measurements]]: loading time and active-work latency answer different questions. Evaluate cache effects against startup requirements, not as evidence that the underlying algorithm became faster.
