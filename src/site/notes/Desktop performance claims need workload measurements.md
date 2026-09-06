---
{"dg-publish":true,"permalink":"/desktop-performance-claims-need-workload-measurements/","title":"Desktop performance claims need workload measurements","hideInFiletree":true,"tags":["javascript","architecture"],"noteIcon":"","dg-note-properties":{"title":"Desktop performance claims need workload measurements","categories":["Frameworks"],"tags":["javascript","architecture"],"created":"2026-09-06","updated":"2026-09-06"}}
---


A small installer does not reveal how much memory an application consumes during real work.

I compare desktop implementations using representative workloads rather than assuming their framework determines user experience.

[Electron recommends profiling](https://www.electronjs.org/docs/latest/tutorial/performance) to identify expensive code, memory usage, and interaction bottlenecks before optimizing.

Measurements should separate startup time, idle memory, active workload latency, CPU consumption, and distribution size.

If [[Desktop runtime choices trade package size for rendering control\|Desktop runtime choices trade package size for rendering control]], package size measures only one architectural consequence.

Because [[Native desktop access needs explicit trust boundaries\|Native desktop access needs explicit trust boundaries]], performance experiments must retain equivalent security settings.

Use comparable devices, application features, and workload inputs before attributing differences to the selected runtime.

Choose the implementation that meets measured product requirements without inventing universal speed or cost advantages.
