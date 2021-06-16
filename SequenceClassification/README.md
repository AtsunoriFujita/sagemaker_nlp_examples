# SequenceClassification

![SequenceClassification](https://github.com/AtsunoriFujita/sagemaker_nlp_examples/blob/main/SequenceClassification/assets/bert-one-seq.svg, "SequenceClassification")

## pytorch_training
- SageMaker HuggingFaceコンテナを使用したトレーニングジョブ
- Amazonの商品レビューデータセットを使用した二値分類
- [東北大学の日本語BERT](https://github.com/cl-tohoku/bert-japanese)を使用

## pytorch_inference_on_torchserve
- [TorchServe](https://github.com/pytorch/serve)をSageMakerのModelクラスでリアルタイム推論エンドポイントを構築    

_NOTE: SageMaker HuggingFaceの推論コンテナがまだリリースされていないため_
