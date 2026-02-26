---
title: "Evaluating & Optimizing RAG"
tags: [learning, tutorial, rag, genai]
created: 2026-02-26
---

# Evaluating & Optimizing RAG

## **Part 1: "I don't use RAG, I just retrieve documents"**

With [Ben Clavié](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/g3hnhwumo5qq27u3/aHR0cHM6Ly9zY2hvbGFyLmdvb2dsZS5jb20vY2l0YXRpb25zP2hsPWVuJnVzZXI9dnVNbG45OEFBQUFK), who has created projects like [RAGatouille](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/9qhzhdud3m558qiz/aHR0cHM6Ly9naXRodWIuY29tL2Fuc3dlcmRvdGFpL3JhZ2F0b3VpbGxl) and the [rerankers](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/3ohphdu39lpp82bp/aHR0cHM6Ly9naXRodWIuY29tL2Fuc3dlcmRvdGFpL3JlcmFua2Vycw==) library, which broaden access to state-of-the-art retrieval methods. Recently, Ben also co-led the [ModernBERT project](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/wnh2h6uqvp00d3cl/aHR0cHM6Ly9odWdnaW5nZmFjZS5jby9ibG9nL21vZGVybmJlcnQ=), which has modernized the widely used BERT backbone.

Abstract: They say RAG is dead, but is it really? Its killer, “agentic search”, has become the hot new way to Retrieve relevant context. In this session, we’ll discuss the greatly exaggerated rumours of RAG’s demise. We will discuss (long-known) limitations of “vector search”, the vast array of existing tools, and, finally, new approaches researchers are uncovering for retrieval.

## **Part 2: "Modern information retrieval (IR) evaluation in the RAG Era"**

With [Nandan Thakur](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/08hwhgu2w5ll4msp/aHR0cHM6Ly9zY2hvbGFyLmdvb2dsZS5jb20vY2l0YXRpb25zP2hsPWVuJnVzZXI9Q0U5R0pvTUFBQUFK), RAG researcher at UWaterloo. Nandan is the creator of the widely used BEIR and MIRACL benchmarks. Nadan will discuss how to design retrieval evals to optimize RAG.

Abstract: Traditional IR benchmarks fall short for real-world RAG applications due to stale data, incomplete labels, and unrealistic queries. This talk introduces FreshStack, a new benchmark built from recent StackOverflow and GitHub content, designed to reflect real programming queries.

## **Part 3: "Reasoning Opens up New Retrieval Frontiers"**

With [Orion Weller](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/l2heh6ulv9xxp6tg/aHR0cHM6Ly9zY2hvbGFyLmdvb2dsZS5jb20vY2l0YXRpb25zP2hsPWVuJnVzZXI9U1lZZDRpQUFBQUFK), RAG and Information Retrieval Researcher at John Hopkins. Orion is going to talk about how reasoning models affect RAG.

Abstract: Just as prompting for language models has enabled complex user tasks and zero-shot optimization via prompts, so can prompts in retrieval enable complex information needs with zero-shot adaptation. In this talk, we describe efforts to move beyond semantic search by building retrieval models that can follow user instructions, be prompted, and find relevant documents through reasoning capabilities.

## **Part 4: "Going Further: Late Interaction Beats Single Vector Limits"**

With [Antoine Chaffin](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/x0hph3ued6qqmvsg/aHR0cHM6Ly9zY2hvbGFyLmdvb2dsZS5jb20vY2l0YXRpb25zP2hsPWVuJnVzZXI9R1FfdFZod0FBQUFK), R&D Machine Learning Engineer at LightOn. He is will discuss state of the art mutli-vector models and the impact of this approach on RAG. Antoine will also provide a short tutorial of the [PyLate](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/kkhmh2un2qxxpxsk/aHR0cHM6Ly9naXRodWIuY29tL2xpZ2h0b25haS9weWxhdGU=) library.

Abstract: Single vector search is the standard for RAG pipelines, but struggles in real-world applications due to poor out-of-domain generalization and long-context handling. Multi-vector models overcome these limitations and show strong performance on modern retrieval tasks, including reasoning-intensive retrieval. PyLate enables easy switching with sentence-transformers-like syntax.
## **Part 5: "The Map Is Not The Territory – Highly Multimodal Search"**

With [Bryan Bischof,](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/x0hph3ued6qqgrtg/aHR0cHM6Ly93d3cubGlua2VkaW4uY29tL2luL2JyeWFuLWJpc2Nob2Yv) Head of AI at Theory and [Ayush Chaurasia](https://click.convertkit-mail2.com/o8u22w0432sqh68wolmhvhq2mvrrrhoh97g7e/58hvh8ugo866eoh7/aHR0cHM6Ly93d3cubGlua2VkaW4uY29tL2luL2F5dXNoY2hhdXJhc2lhLw==), ML Eng at LanceDB. They will talk about how to handle multimodal data and the importance of multiple representations for each piece of data.

Abstract: For AI applications, or for any retrieval based product, neural search continues to prove itself as one of the most essential technologies for powering the web. Unfortunately, amidst the noise of new techniques and applications, it’s easy to miss the path from the old techniques.