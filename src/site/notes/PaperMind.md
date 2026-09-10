---
{"dg-publish":true,"dg-permalink":"craft/papermind","permalink":"/craft/papermind/","title":"PaperMind","hideInFiletree":true,"tags":["craft"],"noteIcon":"","dg-note-properties":{"title":"PaperMind","tags":["craft"],"created":"2025-09-10","updated":"2026-09-10"}}
---

PaperMind adalah chatbot RAG PDF fullstack: backend FastAPI, frontend Next.js 15, dan Supabase Postgres pgvector + Auth untuk tanya-jawab berbasis dokumen.

Upload banyak PDF sekaligus dengan parsing, chunking, dan embedding otomatis; chat streaming SSE menampilkan jawaban token-per-token lengkap dengan sitasi nomor halaman dan riwayat multi-sesi.

Dukung BYOK Gemini, OpenAI, OpenRouter, dan OpenAI-Compatible dengan kunci terenkripsi AES-256-GCM, embedding lock, enrichment otomatis, RLS, dan UI minimalis serta mendukung dark dan ligth mode.
![20260910-aiCBzfBM.webp\|126](/img/user/Attachments/20260910-aiCBzfBM.webp) ![20260910-fyApae4y.webp\|422](/img/user/Attachments/20260910-fyApae4y.webp)

Project ini terinspirasi dari paper yang ditulis oleh Praneeth Vadlapati (2025) https://www.researchgate.net/publication/397745877_Index-RAG_Storing_Text_Location_in_Vector_Databases_for_QA_tasks
