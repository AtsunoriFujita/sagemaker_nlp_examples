# Language Modeling

## T5
- SageMaker HuggingFaceコンテナを使用したJAX/Flaxでのトレーニングジョブ
  - HuggingFaceの[run_t5_mlm_flax.py](https://github.com/huggingface/transformers/blob/master/examples/flax/language-modeling/run_t5_mlm_flax.py)を使用
  - Tokenizerには[SentencePiece](https://github.com/google/sentencepiece)を使用

### Reference
- [Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer](https://arxiv.org/abs/1910.10683)
- [T5: Text-To-Text Transfer Transformer](https://github.com/google-research/text-to-text-transfer-transformer#gpu-usage)
- [はじめての自然言語処理 第7回 T5 によるテキスト生成の検証](https://www.ogis-ri.co.jp/otc/hiroba/technical/similar-document-search/part7.html)
- [sonoisa/t5-japanese](https://github.com/sonoisa/t5-japanese)
- [megagonlabs/t5-japanese](https://github.com/megagonlabs/t5-japanese)
