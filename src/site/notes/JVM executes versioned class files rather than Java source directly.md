---
{"dg-publish":true,"permalink":"/jvm-executes-versioned-class-files-rather-than-java-source-directly/","title":"JVM executes versioned class files rather than Java source directly","hideInFiletree":true,"tags":["programming","architecture"],"noteIcon":"","dg-note-properties":{"title":"JVM executes versioned class files rather than Java source directly","categories":["Programming Languages"],"tags":["programming","architecture"],"created":"2026-09-07","updated":"2026-09-07"}}
---

A Java compiler emits bytecode into class files, and the virtual machine loads those files at runtime. Developers ship the compiled artifact, never the source text, to execution environments.

I treat the class file as the real delivery vehicle, with source language as authoring convenience. Any language that emits valid class files can ride the same runtime and tooling.

The [virtual machine specification](https://docs.oracle.com/javase/specs/jvms/se25/html/jvms-1.html) states the JVM knows only the class format, plus versioned major numbers.

When [[TypeScript extends JavaScript without changing runtime behavior\|emitted code defines runtime behavior]], class files form [[Web APIs live in the runtime, not the language\|the shared runtime contract]] for JVM languages.

Target a class file version the runtime accepts, then verify behavior on that runtime.
