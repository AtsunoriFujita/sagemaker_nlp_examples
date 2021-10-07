# SageMaker NLP examples for Japanese

[Amazon SageMaker](https://aws.amazon.com/jp/sagemaker/)で実行できる（主に日本語の）自然言語処理のサンプル集    
推論周りはSequenceClassificationの下にまとまっています（SageMakerのリアルタイムエンドポイント、バッチ変換ジョブ、TorchServe、Lambda）。

## Examples
- SequenceClassification
  - AmazonSageMaker, 🤗 Transformers, 日本語BERT, Amazonレビューデータセット
- TokenClassification
  - AmazonSageMaker, 🤗 Transformers, 日本語BERT, Wikipediaを用いた日本語の固有表現抽出データセット
- QuestionAnswering
  - AmazonSageMaker, 🤗 Transformers, 日本語BERT, 運転ドメインQAデータセット
- Summarization
  - AmazonSageMaker, 🤗 Transformers, 日本語T5, wikiHow日本語要約データセット
- GPT2 Fine-Tuning
  - AmazonSageMaker, 🤗 Transformers, 日本語GPT2, wikiHow日本語要約データセット
- BlazingText
  - AmazonSageMaker, built-in BlazingText(教師なし, Word2vec), ja.text8
  - AmazonSageMaker, built-in BlazingText(教師あり, テキスト分類), Amazonレビューデータセット
- LatentDirichletAllocation(LDA)
  - AmazonSageMaker, built-in LDA, livedoorニュースコーパス
- AmazonCoprehend_tutorial
  - AmazonCoprehend, Entity抽出, KeyPhrase抽出, Sentiment分析, Amazonレビューデータセット

Others?    
Stay tuned!
