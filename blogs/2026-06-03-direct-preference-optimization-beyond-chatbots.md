---
title: "Direct Preference Optimization Beyond Chatbots"
url: "https://huggingface.co/blog/Dharma-AI/direct-preference-optimization-beyond-chatbots"
date: "2026-06-03"
author: "Erick Lachmann"
feed_url: "https://huggingface.co/blog/feed.xml"
---
The article explores how Direct Preference Optimization (DPO), a technique typically used for chatbot alignment, can address text degeneration in structured OCR tasks. Rather than treating degenerate outputs as noise, the DharmaOCR pipeline deliberately used them as rejected examples in preference pairs. This approach achieved an average 59.4% reduction in degeneration across five model families, demonstrating that optimizing over complete preference pairs addresses failure modes that supervised fine-tuning alone cannot resolve.
