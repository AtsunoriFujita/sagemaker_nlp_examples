# SequenceClassification

![SequenceClassification](https://user-images.githubusercontent.com/40932835/122213598-14b1e400-cee4-11eb-8733-73f1ae579f84.png)

↑の画像は[ここ](https://d2l.ai/chapter_natural-language-processing-applications/finetuning-bert.html)から

## pytorch_training
- SageMaker HuggingFaceコンテナを使用したPyTorchでのトレーニングジョブ
  - HuggingFaceの[SageMaker example](https://github.com/huggingface/notebooks/tree/master/sagemaker)から01_getting_started_pytorch, 05_spot_instances, 06_sagemaker_metricsを参照
- Amazonの商品レビューデータセットを使用した二値分類
- [東北大学の日本語BERT](https://github.com/cl-tohoku/bert-japanese)を使用したFine-Tuning

## pytorch_inference_on_torchserve
- [TorchServe](https://github.com/pytorch/serve)の推論エンドポイントをSageMakerのModelクラスでデプロイ    
- `pytorch_training`で学習したモデルを使用

```python
payload ='ハワイアンの心和む音楽の中、ちょっとシリアスなドラマが展開していきます。音楽の力ってすごいな、って思いました。'
predictor.predict(data=payload).decode(encoding='utf-8')
# 'Positive'
```

_NOTE: SageMaker HuggingFaceの推論コンテナが現時点でリリースされていないためTorchServeを使用_

### Reference
- [Run training on Amazon SageMaker](https://huggingface.co/transformers/sagemaker.html#access-trained-model)
- [cl-tohoku/bert-japanese](https://github.com/cl-tohoku/bert-japanese)
- [TorchServeを使用した大規模な推論のためのPyTorchモデルをデプロイする](https://aws.amazon.com/jp/blogs/news/deploying-pytorch-models-for-inference-at-scale-using-torchserve/)    
- [torchserve-examples](https://github.com/shashankprasanna/torchserve-examples)
- [Deploy FastAI Trained PyTorch Model in TorchServe and Host in Amazon SageMaker Inference Endpoint](https://github.com/aws-samples/amazon-sagemaker-endpoint-deployment-of-fastai-model-with-torchserve)
