# SequenceClassification

![SequenceClassification](https://user-images.githubusercontent.com/40932835/122213598-14b1e400-cee4-11eb-8733-73f1ae579f84.png)

↑の画像は[ここ](https://d2l.ai/chapter_natural-language-processing-applications/finetuning-bert.html)から

## pytorch_training
- SageMaker HuggingFaceコンテナを使用したPyTorchでのトレーニングジョブ
- Amazonの商品レビューデータセットを使用した二値分類
- [東北大学の日本語BERT](https://github.com/cl-tohoku/bert-japanese)を使用

## pytorch_inference_on_torchserve
- [TorchServe](https://github.com/pytorch/serve)のリアルタイム推論エンドポイントをSageMakerのModelクラスでを実行    

_NOTE: SageMaker HuggingFaceの推論コンテナがまだリリースされていないため_
