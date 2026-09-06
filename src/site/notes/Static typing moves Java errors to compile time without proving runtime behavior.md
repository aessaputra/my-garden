---
{"dg-publish":true,"permalink":"/static-typing-moves-java-errors-to-compile-time-without-proving-runtime-behavior/","title":"Static typing moves Java errors to compile time without proving runtime behavior","hideInFiletree":true,"tags":["programming","testing"],"noteIcon":"","dg-note-properties":{"title":"Static typing moves Java errors to compile time without proving runtime behavior","categories":["Programming Languages"],"tags":["programming","testing"],"created":"2026-09-07","updated":"2026-09-07"}}
---

Java rejects mismatched types during compilation, so many defects surface before any code runs. A method expecting an integer refuses a string argument before execution starts.

I treat this check as early feedback about type contradictions, not as proof that program logic is correct. Sound handling still depends on tests, reviews, and validation of external data.

The [language specification](https://docs.oracle.com/javase/specs/jls/se25/html/jls-1.html) distinguishes compile-time errors from runtime failures, with bytecode generation between them.

If [[Type checking catches contradictions before execution, not every runtime failure\|static checks catch early]], Java still needs [[Dynamic inputs require runtime validation despite static types\|runtime validation]] at boundaries.

[[Type checking belongs in CI but cannot replace behavioral tests\|Checking in CI]] keeps this feedback consistent across every change developers merge.

Use static types to constrain development decisions, not to guarantee runtime outcomes.
