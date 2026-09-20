# Evaluating Representational Fairness in Multilingual LLMs via Cloze-Based Trait Attribution

> Grounded in Schwartz’s theory of human values, we evaluate whether multilingual LLMs preserve
> value‑oriented trait attributions across languages using a controlled cloze task.

---

## 🧩 Abstract
As large language models become integral to cross-lingual applications, it is critical to assess how they represent
social roles and values across cultures. Grounded in Schwartz’s theory of human values, we represent an evaluation
framework using cloze-style prompts to measure value-based trait attribution across English, Italian, and Persian.
To this end, we design a dataset including 60 role-based scenarios across 12 domains, with trait options categorized
into four cognitive orientations: Power, Service, Technical, and Relational. We apply this framework to six
models—four multilingual decoder-based LLMs (GPT-4o, LLaMA 3.1:70B, LLaMA 3.2, Qwen2.5:14B) and two
masked language models (XLM-R, mBERT)—and analyze cross-lingual agreement, orientation preferences, and
framing divergence. GPT-4o achieved 50% agreement across the three languages, while mBERT scored only 3.3%,
revealing sharp disparities in semantic consistency. Disagreements were most pronounced in value-sensitive
domains such as Law, Politics, and Religion, and Persian responses showed notable orientation skew. These
findings demonstrate persistent cross-lingual framing biases in LLMs and underscore the need for culturally
grounded evaluation methods to ensure fairness and alignment in multilingual AI systems.

---

## 📁 Repository Layout
```
Cross-Lingual Bias Detection/
├─ A. Datasets/                # EN(High), IT(Mid), FA(Low) + shuffled variants
├─ B. Language Model Tests/    # Runs for GPT-4o, LLaMA, Qwen, mBERT, XLM-R
├─ C. Merging LM Responses/    # Merge EN/IT/FA outputs per model
├─ D. Analysis/                # Agreement %, disagreement types, domains, entropy, CIs
└─ README.md                   # This landing page
```

Each subfolder includes its own `README.md` with details.

## 🚀 Quickstart
```bash
# Python 3.9+ recommended
pip install -U pandas numpy torch transformers openai

# 1) Run models on the datasets
# GPT‑4o: add OpenAI key to B. Language Model Tests/secret.py as: apikey = "YOUR_KEY"
python "B. Language Model Tests/GPT4o_TEST.py"

# 2) Merge outputs (align EN/IT/FA)
python "C. Merging LM Responses/merge_gpt4o.py"

# 3) Analyze agreement/disagreement/entropy
python "D. Analysis/analyze_gpt4o.py"
```

> Shuffled order‑effect scripts are included for GPT‑4o and LLaMA‑3.1‑70B.

---

## 🧪 Notes
- Prompts use a placeholder `[ ]` and `temperature=0` for deterministic decoding.
- Scripts auto‑resume if interrupted and save per‑row progress.
- Consider Git LFS if any CSVs are very large.
---

## 📬 Contact
1. Saba Ghanbari Haez: sghanbarihaez@fbk.eu  ghanbari.haez.saba@gmail.com
2. Mauro Dragoni: mdragoni@fbk.eu

   
Paper link will be added upon publication.
