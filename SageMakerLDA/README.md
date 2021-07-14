# Amazon SageMaker built-in Latent Dirichlet Allocation (LDA)

![](https://upload.wikimedia.org/wikipedia/commons/4/4d/Smoothed_LDA.png)

## AmazonSageMaker-built-in-LDA
- AmazonSageMakerのビルトインアルゴリズムの一つであるLatent Dirichlet Allocation (LDA)を使用したexample
- [株式会社ロンウイット](https://www.rondhuit.com/)さんの公開している[livedoor ニュースコーパス](https://www.rondhuit.com/download.html)を使用
- ユースケース：テキストからのトピック抽出、テキストのジャンル・カテゴリ推定など

#### 文書から抽出した潜在トピック
```
Topic 0
=====================
チョコレート       0.014655
人気           0.012782
女性           0.012509
クリスマス        0.012506
限定           0.011682
ブランド         0.010410
サイト          0.010372
多い           0.009846
女子           0.009572
アイテム         0.009305
話題           0.009260
cm           0.008218
自分           0.008211
商品           0.008137
関連           0.007626

Topic 1
=====================
映画           0.076599
作品           0.027791
公開           0.024247
監督           0.020890
本作           0.019164
世界           0.016903
映像           0.015342
今回           0.010171
全国           0.009590
サイト          0.009230
movie        0.008435
記事           0.008399
主演           0.008226
公式           0.008088
特集           0.007954

Topic 2
=====================
自分           0.055845
結婚           0.024290
女性           0.020051
相手           0.017729
仕事           0.015982
多い           0.015885
男性           0.012857
気持ち          0.012326
恋愛           0.012303
独女           0.012218
好き           0.012192
本当           0.009555
幸せ           0.008410
子供           0.007736
人生           0.007436

Topic 3
=====================
...

...


Topic 19
=====================
...
```

#### 推論Output
```python
results = lda_inference.predict(bags_of_words_test[:1])
print(json.dumps(results, sort_keys=True, indent=2))

# results
{
  "predictions": [
    {
      "topic_mixture": [
        0.0,
        0.9114203453063965,
        0.0,
        0.0,
        0.0,
        0.07842560857534409,
        0.010154089890420437,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0
      ]
    }
  ]
}
```

### Reference
- [Amazon SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/lda.html)
- [An Introduction to SageMaker LDA](https://github.com/aws/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/lda_topic_modeling/LDA-Introduction.ipynb)
- [A Scientific Deep Dive Into SageMaker LDA](https://github.com/aws/amazon-sagemaker-examples/blob/master/scientific_details_of_algorithms/lda_topic_modeling/LDA-Science.ipynb)
- [LDA on Sagemaker](https://github.com/rangle/ai-lda-sagemaker)
- [livedoor ニュースコーパス](https://www.rondhuit.com/download.html)
- [GiNZA](https://megagonlabs.github.io/ginza/)
