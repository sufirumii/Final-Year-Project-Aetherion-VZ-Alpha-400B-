<div align="center">

<img width="3750" height="937" alt="banner" src="https://github.com/user-attachments/assets/05ba3e24-8838-4194-9ed8-5d20d39c7149" />


# Final Year Project | Aetherion VZ Alpha 400B
### *AI-Driven Clinical Decision Support Interface — B.Eng Computer Engineering (Honours), APU Malaysia*

![Status](https://img.shields.io/badge/status-archived%20prototype-informational)
![Domain](https://img.shields.io/badge/domain-Medical%20AI-00bfff)
![Models](https://img.shields.io/badge/ResNet50%20classifiers-14-00ff9d)
![Backend](https://img.shields.io/badge/backend-Flask%20%7C%20PyTorch%20%7C%20OpenCV-ff00ff)

</div>

---

## Overview

**Aetherion VZ Alpha 400B** (internally engineered under the codename **SentiVita**) was my Final Year Project at APU, Malaysia — a full-stack, AI-driven clinical decision support interface that fused **computer vision diagnostics**, a **retrieval-grounded medical assistant**, and **biometric patient identity verification** into a single system, wrapped in a custom-built, heavily animated web interface.

The interface itself leans into an immersive "quantum medical intelligence" visual theme — the on-screen system stats, futuristic labels, and sci-fi framing are **presentation design choices**, not literal engineering specifications. The sections below describe what was actually engineered and shipped.

---

## What Was Actually Built

### 🧬 Multi-Disease Classification Engine
14 independently fine-tuned **ResNet50** models, each trained on a distinct real-world medical imaging dataset, served through a unified Flask inference API with dynamic model-switching:

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

Each classifier is paired with an **image analysis dashboard** (edge detection, histogram equalization, gradient magnitude, ROI contour extraction, thermal remapping via OpenCV/CLAHE) and a second, **graph-based mathematical dashboard** that renders the same signals as statistical plots (intensity histograms, per-row edge profiles, gradient magnitude distributions, ROI area/region statistics) rather than images.

### 🧠 Retrieval-Grounded Medical Assistant
A conversational medical AI layer, grounded on a corpus assembled from open biomedical/clinical QA sources, including **PubMed**, **BioASQ**, **ReasonMed**, **ChatDoctor** / **ChatDoctor-110k**, **MIRIAD-5.8M**, and **Lavita**. The assistant stack evolved over the project's lifetime — starting on a locally-hosted **GPT4All** reasoning model for on-device inference, later migrated to **Llama 4 Maverick** via API for stronger reasoning quality.

### 🔐 Biometric Identity Verification
Role-based registration and authentication (Doctor / Patient / Receptionist / Pharmacist) using **OpenCV Haar cascades** for face detection and the **`face_recognition`** library for face-encoding comparison, captured live via webcam through the browser.

### 🌐 Multilingual Interface
Full UI translation across **8 languages** (English, Bahasa Malaysia, Tamil, Telugu, Hindi, Urdu, Punjabi, Russian, French), reflecting Malaysia's multicultural patient base.

### 🎮 Cognitive Health Companion
A built-in memory-match game with tiered difficulty (mild / moderate / severe), designed as a lightweight cognitive-engagement tool, alongside a guided "Heal Yourself" relaxation module (ambient video/audio + rotating affirmations).

### 📊 Real-Time Analytics Layer
A live medical-news feed, simulated vitals monitoring, and a dual image + graph visualization dashboard for interpreting model predictions beyond a single label output.

---

## Recognition

<table>
<tr>
<td width="33%">
<img src="https://github.com/user-attachments/assets/ad67296b-e4de-4e81-962b-703ecfb0d4ca" width="100%"/>
<p align="center"><sub>Disease Classifier Interface</sub></p>
</td>
<td width="33%">
<img src="https://github.com/user-attachments/assets/70d5d785-7532-492b-b97e-47d4e8accbb1" width="100%"/>
<p align="center"><sub>Live demonstration to Malaysia's Deputy Minister of Health</sub></p>
</td>
<td width="33%">
<img src="https://github.com/user-attachments/assets/dc6298cd-e0cb-409e-b954-28ffe0002f8f" width="100%"/>
<p align="center"><sub>APU named official consultant to MOH's Health Performance Unit</sub></p>
</td>
</tr>
</table>

This project was demonstrated live to Malaysia's **Deputy Minister of Health**, coinciding with **APU being named an official consultant to the Ministry of Health's Health Performance Unit**, spearheading national HealthTech advancement initiatives.

*(Individual competition medals from this and related work are listed separately in my profile.)*

---

## Tech Stack

`Python` · `Flask` · `PyTorch / torchvision (ResNet50)` · `OpenCV` · `face_recognition` · `GPT4All` → `Llama 4 Maverick API` · `HTML5 / CSS3 / vanilla JS` · `Three.js` · `Chart.js` · `ngrok` (local API tunneling during development)

---

## A Note on Scope

This was a solo-engineered, single-semester final year project — a research/demo prototype, not a certified or clinically validated medical device. It was built to demonstrate end-to-end feasibility of combining vision-based diagnostics, retrieval-grounded LLM reasoning, and biometric identity flows in one coherent clinical interface, and to explore what a "quantum-themed" futuristic medical UI could look and feel like in practice.

</div>
