# minGPT

![mingpt](mingpt.jpg)

**minGPT** is a minimal, clean, and educational PyTorch re-implementation of GPT-style language models (GPT-1 / GPT-2–like). The goal is simple: make GPT **easy to read, understand, and hack on**.

The entire model lives in ~300 lines of readable code ([`mingpt/model.py`](mingpt/model.py)). At its core, minGPT is just:

> token indices → Transformer → next-token probability distribution

Most of the remaining complexity exists purely for **efficient batching and training**.  Conflicting change for lab3.

---

## ⚠️ Project Status (Jan 2023)

minGPT is now **semi-archived**.

As the project became widely referenced (courses, blogs, notebooks, books), making breaking changes became difficult. I also wanted to move beyond a purely educational focus toward something more practical and performant.

👉 For an actively maintained successor, see **[nanoGPT](https://github.com/karpathy/nanoGPT)**:
- still simple and hackable
- faster and more efficient
- capable of reproducing medium-scale industry benchmarks

minGPT remains valuable as a **learning reference**.

---

## Project Structure

The library consists of **three core files**:

- **`mingpt/model.py`**  
  Transformer / GPT model definition

- **`mingpt/bpe.py`**  
  Byte Pair Encoding (BPE) tokenizer, compatible with OpenAI GPT tokenization

- **`mingpt/trainer.py`**  
  GPT-agnostic PyTorch training boilerplate

### Included Demos & Projects (`projects/`)

- **`adder/`** – trains a GPT from scratch to add numbers  
  (inspired by GPT-3 arithmetic experiments)

- **`chargpt/`** – character-level language modeling on raw text

- **`demo.ipynb`** – minimal notebook demo (sorting task)

- **`generate.ipynb`** – load a pretrained GPT-2 and generate text from a prompt

---

## Installation

To use `mingpt` as a library:

```bash
git clone https://github.com/karpathy/minGPT.git
cd minGPT
pip install -e .
