# APDS TP 03: Unsloth, LoRA, and Fine-Tuning

**Course:** Advanced Programming for Data Science (APDS)  
**Lecturer:** Mr. VANN Visal  
**Student:** Heang Sotheara (ID: e20221213)  
**Date:** October 27, 2026  

---

## 1. Overview of Unsloth

**Unsloth** is an open-source framework developed to accelerate training and fine-tuning pipelines for Large Language Models (LLMs). It seamlessly connects with Hugging Face libraries and provides targeted kernel optimizations for architectures including Llama, Mistral, Gemma, and Qwen.

### Key Motivations for Adopting Unsloth:
- **Accelerated Compute:** Handcrafted GPU backpropagation and Triton kernels shorten training loops significantly.
- **Reduced VRAM Footprint:** Smart memory management techniques allow training on consumer hardware or smaller cloud instances.
- **Custom Domain Adaptation:** Facilitates rapid adaptation of base models toward specialized downstream tasks.

> **Note on Benchmark Claims:** The widely cited ~70% memory reduction represents maximum savings under specific benchmark conditions rather than an absolute guarantee. Practical resource consumption is influenced by context sequence length, quantization precision, batch size, and hardware architecture. Additionally, speed gains do not automatically boost model performance—data curation and hyperparameter selection remain decisive factors.

---

## 2. Environment & System Prerequisites

Before initiating GPU-accelerated training, verify the following baseline prerequisites:
- An isolated Python virtual environment (e.g., using `venv` or Conda).
- An NVIDIA GPU backed by up-to-date drivers (`nvidia-smi`) and a CUDA-compatible PyTorch build (`torch.cuda.is_available() == True`).
- Sufficient VRAM and storage capacity suited to the base checkpoint and maximum token context.
- Valid authentication tokens or access rights for gated Hugging Face repositories (such as Llama 3).

---

## 3. Installation & Model Initialization

### Package Installation
Inside your designated environment, install Unsloth alongside necessary dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install unsloth
```
*(In Jupyter / Google Colab environments, run `%pip install unsloth` and restart the session if prompted.)*

### Loading Pretrained Weights (4-bit QLoRA)
Load a quantized foundation model using `FastLanguageModel`. Loading initializes the base checkpoint into memory without updating any weights.

```python
from unsloth import FastLanguageModel

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/llama-3-8b-Instruct-bnb-4bit",
    max_seq_length=2048,
    dtype=None,        # Auto-detects optimal compute precision (bfloat16 or float16)
    load_in_4bit=True, # Enforces 4-bit NormalFloat (NF4) quantization
)
```

- **`model_name`:** Checkpoint identifier on Hugging Face Hub.
- **`max_seq_length`:** Context window limit (in tokens) processed during training.
- **`load_in_4bit=True`:** Substantially curbs baseline VRAM consumption by storing base parameters in 4-bit precision.

---

## 4. Parameter-Efficient Adaptation with LoRA & QLoRA

**LoRA (Low-Rank Adaptation)** freezes the massive base weight matrices and inserts low-rank trainable decomposition matrices into chosen attention and feed-forward modules. **QLoRA** takes this a step further by keeping the frozen backbone in a 4-bit quantized format while computing adapter gradients in higher precision.

- **Parameter Efficiency:** Modifies under 1–2% of the model's total weights.
- **Lightweight Storage:** Produces small adapter checkpoint files (often a few megabytes) instead of multi-gigabyte full weights.
- **Modular Deployment:** Multiple specialized LoRA adapters can be swapped atop the exact same frozen base model.

### Adapter Configuration

```python
model = FastLanguageModel.get_peft_model(
    model,
    r=16,
    target_modules=[
        "q_proj", "k_proj", "v_proj", "o_proj",
        "gate_proj", "up_proj", "down_proj",
    ],
    lora_alpha=16,
    lora_dropout=0,
    bias="none",
    use_gradient_checkpointing="unsloth",
    random_state=3407,
    use_rslora=False,
    loftq_config=None,
)
```

| Parameter | Function & Usage |
| :--- | :--- |
| `r=16` | Rank dimension of adapter decomposition matrices. Higher ranks introduce more capacity at the cost of additional trainable parameters. |
| `lora_alpha=16` | Scaling multiplier applied to adapter updates. Setting $\alpha = r$ provides a balanced baseline. |
| `target_modules` | Linear projection layers (attention queries/keys/values/outputs and MLP projections) augmented with adapters. |
| `lora_dropout=0` | Adapter dropout rate (set to 0 for maximum throughput in Unsloth). |
| `bias="none"` | Keeps bias parameters frozen throughout adaptation. |
| `use_gradient_checkpointing="unsloth"` | Reduces peak activation memory by selectively recalculating intermediate activations during backpropagation. |
| `random_state=3407` | Fixed random seed for experimental reproducibility. |
| `use_rslora=False` | Standard LoRA scaling ($\frac{\alpha}{r}$). Enabling `True` scales updates by $\frac{\alpha}{\sqrt{r}}$ for rank stability. |

---

## 5. Fine-Tuning Execution

### Practical Fine-Tuning Lifecycle
1. **Model Selection:** Pick an appropriate foundation model and parameter scale (e.g., Llama-3-8B).
2. **Data Pipeline:** Curate high-quality, diverse domain examples; split into training and evaluation sets.
3. **Template Formatting:** Apply the official chat template consistently across training sequences.
4. **Adapter Attachment:** Wrap the base model with PEFT/LoRA modules via `FastLanguageModel.get_peft_model`.
5. **Supervised Fine-Tuning:** Train using an optimizer (e.g., AdamW 8-bit) and trainer harness (`SFTTrainer`) with gradient accumulation and learning rate warmup.
6. **Validation & Assessment:** Benchmark responses against unseen test cases and baseline checkpoints.
7. **Export:** Save the lightweight adapter weights (`model.save_pretrained`) or merge them back into full FP16/GGUF formats for serving.


