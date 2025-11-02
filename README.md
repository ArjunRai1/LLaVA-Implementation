# LLaVA-Implementation
# LLaVA Mini Replication (Local / Colab)

This project is a **minimal replication** of the paper
**“Visual Instruction Tuning” (LLaVA: Large Language and Vision Assistant)**.

It demonstrates how a multimodal model can learn to connect visual(image) and language(text) understanding in two training stages, using open-source tools only.

------

## 🧠 Model Architecture

The system combines:

* **Vision Encoder:** `openai/clip-vit-large-patch14` (pretrained CLIP ViT-L/14)
* **Language Model:** `TinyLlama/TinyLlama-1.1B-Chat-v1.0`
* **Projector:** a small linear layer that maps CLIP image features to TinyLlama’s text embedding space

Architecture flow:

```
Image → CLIP Encoder → Projector → LLM (TinyLlama) → Text Response
```

This design follows the same concept as LLaVA but uses a lighter and fully open setup suitable for Colab or local GPUs.

---

## ⚙️ Training Overview

### **Stage 1: Feature Alignment**

* Goal: Align CLIP’s visual embeddings with TinyLlama’s language space.
* Data: Simple caption-style pairs (e.g., image + short description).
* Process:

  * Freeze CLIP encoder (no fine-tuning).
  * Train only the projector using image–caption pairs (`chat.json` format).
  * Result: The projector learns to translate CLIP outputs into TinyLlama’s embedding space.

### **Stage 2: Visual Instruction Tuning**

* Goal: Make the model follow human-style image–question conversations.
* Data: Instruction-like dialogs (e.g., *“Describe the image” → model response*).
* Process:

  * Use the frozen CLIP + trained projector + TinyLlama (with LoRA adapter).
  * Fine-tune LoRA layers on multimodal conversation data.
  * Result: The model learns multimodal reasoning and natural responses.

------

## 🧪 Sanity Check (Evaluation)

After both stages:

1. Load the trained projector and LoRA adapter.
2. Provide a sample from `chat.json`.
3. Run inference:

   ```
   PROMPT: Provide a brief description of the given image.
   GENERATED: olive oil is a healthy ingredient used liberally.
   ```
4. If you see a relevant, coherent output — the multimodal alignment is working.


## 📖 Notes

* This is a simplified, educational replication — not the full LLaVA scale.
* Inspired by: [LLaVA GitHub](https://github.com/haotian-liu/LLaVA)
* Models used:

  * [TinyLlama-1.1B-Chat-v1.0](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0)
  * [CLIP-ViT-L/14](https://huggingface.co/openai/clip-vit-large-patch14)

------

## 🏁 Output Example

| Input Prompt            | Model Output                         |
| ----------------------- | ------------------------------------ |
| "Describe the image."   | "A bowl of fruit on a wooden table." |
| "What object is shown?" | "A green apple."                     |

------



Implementation and experiment by **[Your Name]**,
based on open-source LLaVA methodology and Hugging Face models.
