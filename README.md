<div align="center">

<img width="4375" height="1312" alt="stat_card" src="https://github.com/user-attachments/assets/80ab838d-40a6-48a0-9dc7-ce9c4e7d0856" />

# Final Year Project | Aetherion VZ Alpha 400B

### AI Powered Medical Ecosystem covering LLM for Reasoning, Retrieval Augmented Generation for answer accuracy, Convolutional Neural Networks for disease detection.  

</div>

---

## Overview

**Aetherion VZ Alpha 400B** (internally engineered under the codename **SentiVita**) was my Final Year Project at APU, Malaysia — a full-stack, AI-driven clinical decision support interface that fused computer vision diagnostics, a retrieval-grounded medical assistant, and biometric patient identity verification into a single system, wrapped in a custom-built, heavily animated web interface.

The interface itself leans into an immersive "quantum medical intelligence" visual theme — the on-screen system stats, futuristic labels, and sci-fi framing are presentation design choices, not literal engineering specifications. The sections below describe what was actually engineered and shipped.

---

## Large Language Model Integration

The conversational reasoning layer runs on **Llama 4 Maverick**, Meta's natively multimodal model built on a **Mixture-of-Experts (MoE)** architecture. Unlike a dense transformer — where every parameter is used for every token — Maverick routes each token through only a small slice of its total network:

- **400 billion total parameters**, but only **17 billion active per token**, giving most of the reasoning quality of a much larger dense model at a fraction of the inference cost.
- **128 routed experts per MoE layer** plus **one shared expert** that every token always passes through. A learned router selects one routed expert per token in addition to the shared one, so each token effectively activates two experts, not all 128.
- MoE and standard dense feed-forward layers **alternate** through the network rather than every layer being MoE — a design choice for training stability and inference efficiency.
- **Early-fusion multimodality**: text and image tokens are merged into a single backbone early in the network, rather than being processed by separate encoders and stitched together later, which is what allows Maverick to reason jointly over clinical images and text in the same forward pass.

For this project, Maverick replaced an earlier, locally-hosted **GPT4All** reasoning model that had been used for on-device inference during initial development, once API access to a stronger model became practical.

---

## Disease Models Integrated

### CNN Model for Vision : ResNet50

All 14 diagnostic classifiers are built on **ResNet50**, a 50-layer convolutional network from the ResNet ("Residual Network") family. Its defining feature is the **residual/skip connection** — each block learns a residual function relative to its input rather than an unreferenced mapping, which lets gradients flow through very deep networks during backpropagation without vanishing. This is what makes it practical to train a network this deep at all, and is a large part of why ResNet50 remains a standard transfer-learning backbone for medical imaging tasks with comparatively small, specialized datasets — exactly the situation with each of the 14 datasets below.

14 independently fine-tuned ResNet50 models, each trained on a distinct real-world medical imaging dataset, served through a unified Flask inference API with dynamic model-switching:

| # | Model | Classes |
|---|---|---|
| 1 | Diabetic Retinopathy | Mild · Moderate · No DR · Proliferate DR · Severe |
| 2 | Acne Level Classification | Level 0 · 1 · 2 |
| 3 | Kvasir (Gastrointestinal) | 8 GI tract findings (polyps, esophagitis, ulcerative colitis, etc.) |
| 4 | Lung Cancer (MRI) | Cancerous · Non-cancerous |
| 5 | Brain Tumor (MRI) | Tumor · No tumor |
| 6 | Pneumonia (X-Ray) | Normal · Pneumonia |
| 7 | COVID-19 (X-Ray) | COVID-19 · Normal |
| 8 | Breast Cancer (Ultrasound) | Benign · Malignant |
| 9 | Tuberculosis (X-Ray) | Normal · Tuberculosis |
| 10 | Skin Cancer Classification | Benign · Malignant · No disease |
| 11 | Augmented Skin Conditions | Acne · Carcinoma · Eczema · Keratosis · Milia · Rosacea |
| 12 | Alzheimer's (MRI) | No / Very Mild / Mild / Moderate Impairment |
| 13 | Breast Cancer Histopathology | Benign · Malignant |
| 14 | Advanced Brain Cancer Classification | Glioma · Meningioma · No tumor · Pituitary |

Each classifier is paired with an image analysis dashboard (edge detection, histogram equalization, gradient magnitude, ROI contour extraction, thermal remapping via OpenCV/CLAHE) and a second, graph-based mathematical dashboard that renders the same signals as statistical plots (intensity histograms, per-row edge profiles, gradient magnitude distributions, ROI area/region statistics) rather than images.

---

## Retrieval Augmented Generation

The medical assistant is grounded using **Retrieval Augmented Generation (RAG)** — rather than relying purely on the language model's parametric knowledge, relevant medical passages are retrieved from a dedicated corpus at query time and injected into the prompt as context before the model generates a response. This keeps answers anchored to real biomedical literature instead of the model's own recall.

The specific architecture used is what's commonly called **Naive RAG**: a single-pass pipeline of *embed query → retrieve top-k passages by similarity → pass them to the LLM as context → generate*. It does not include the re-ranking, query rewriting, or agentic multi-hop retrieval found in more advanced RAG designs. This was an intentional and honest limitation at the time — RAG system design was still new territory for me during this project, and the priority was getting a working, grounded pipeline shipped end-to-end.

- **Embedding model**: [`sentence-transformers/all-MiniLM-L6-v2`](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) — a distilled, 6-layer, ~22.7M-parameter sentence-transformer that maps text into a 384-dimensional dense vector space. It was used to embed the PubMed-derived corpus into a vector index, and the same model embeds each incoming query so it can be compared against that index by cosine similarity for retrieval.
- **Corpus sources**: an aggregate biomedical/clinical QA corpus assembled from open sources — **PubMed**, **BioASQ**, **ReasonMed**, **ChatDoctor** / **ChatDoctor-110k**, **MIRIAD-5.8M**, and **Lavita**.
- **Generation model**: **Llama 4 Maverick** (see architecture notes above), preceded during early development by a locally-hosted **GPT4All** model.

---

## Biometric Identity Verification

Role-based registration and authentication (Doctor / Patient / Receptionist / Pharmacist) using OpenCV Haar cascades for face detection and the `face_recognition` library for face-encoding comparison, captured live via webcam through the browser.

---

## Multilingual Interface

Full UI translation across 8 languages (English, Bahasa Malaysia, Tamil, Telugu, Hindi, Urdu, Punjabi, Russian, French), reflecting Malaysia's multicultural patient base.

---

## Cognitive Health Companion

A built-in memory-match game with tiered difficulty (mild / moderate / severe), designed as a lightweight cognitive-engagement tool, alongside a guided "Heal Yourself" relaxation module (ambient video/audio + rotating affirmations).

---

## Real-Time Analytics Layer

A live medical-news feed, simulated vitals monitoring, and a dual image + graph visualization dashboard for interpreting model predictions beyond a single label output.

---

## Recognition

<table>
<tr>
<td width="50%" align="center">
<img src="https://github.com/user-attachments/assets/ca8724e4-6c3c-424f-bb86-a585e0e1349a" width="100%"/>
<sub>Aetherion — main interface</sub>
</td>
<td width="50%" align="center">
<img src="https://github.com/user-attachments/assets/ad67296b-e4de-4e81-962b-703ecfb0d4ca" width="100%"/>
<sub>Disease Classifier Interface</sub>
</td>
</tr>
<tr>
<td width="50%" align="center">
<img src="https://github.com/user-attachments/assets/70d5d785-7532-492b-b97e-47d4e8accbb1" width="100%"/>
<sub>Live demonstration to Malaysia's Deputy Minister of Health</sub>
</td>
<td width="50%" align="center">
<img src="https://github.com/user-attachments/assets/dc6298cd-e0cb-409e-b954-28ffe0002f8f" width="100%"/>
<sub>APU named official consultant to MOH's Health Performance Unit</sub>
</td>
</tr>
</table>

This project was demonstrated live to Malaysia's **Deputy Minister of Health**, coinciding with **APU being named an official consultant to the Ministry of Health's Health Performance Unit**, spearheading national HealthTech advancement initiatives.

*(Individual competition medals from this and related work are listed separately in my profile.)*

---

## Tech Stack

`Python` · `Flask` · `PyTorch / torchvision (ResNet50)` · `OpenCV` · `face_recognition` · `sentence-transformers (all-MiniLM-L6-v2)` · `GPT4All` → `Llama 4 Maverick API` · `HTML5 / CSS3 / vanilla JS` · `Three.js` · `Chart.js` · `ngrok` (local API tunneling during development)

---

## A Note on Scope

This was a solo-engineered, single-semester final year project — a research/demo prototype, not a certified or clinically validated medical device. It was built to demonstrate end-to-end feasibility of combining vision-based diagnostics, retrieval-grounded LLM reasoning, and biometric identity flows in one coherent clinical interface, and to explore what a "quantum-themed" futuristic medical UI could look and feel like in practice.

</div>
