# TokenClassification

![TokenClassification](https://user-images.githubusercontent.com/40932835/124863920-ef039080-dff2-11eb-9ac6-de26f9d6d8e5.png)

↑の画像は[ここ](https://d2l.ai/chapter_natural-language-processing-applications/finetuning-bert.html)から

## pytorch_ner_training
- [BERTによる自然言語処理入門 Transformersを使った実践プログラミング](https://www.ohmsha.co.jp/book/9784274227264/)の固有表現抽出(Named Entity Recognition)の章を参考にAmazon SageMakerで動作する様に変更を加えたものです。
  - IO法とBIO法があります。
- [ストックマーク株式会社](https://stockmark.co.jp/)さんの作成された[Wikipediaを用いた日本語の固有表現抽出データセット](https://github.com/stockmarkteam/ner-wikipedia-dataset)を使用。
- [東北大学の日本語BERT](https://github.com/cl-tohoku/bert-japanese)を使用したFine-Tuning

#### Local環境での推論
```python
original_text=[]
entities_list = [] # 正解の固有表現を追加していく
entities_predicted_list = [] # 抽出された固有表現を追加していく

for sample in tqdm(dataset_test):
    text = sample['text']
    original_text.append(text)
    encoding, spans = tokenizer.encode_plus_untagged(
        text, return_tensors='pt'
    )
    #encoding = { k: v.cuda() for k, v in encoding.items() }  # GPUで推論する場合

    with torch.no_grad():
        output = model(**encoding)
        scores = output.logits
        scores = scores[0].cpu().numpy().tolist()

    # 分類スコアを固有表現に変換する
    entities_predicted = tokenizer.convert_bert_output_to_entities(
        text, scores, spans
    )

    entities_list.append(sample['entities'])
    entities_predicted_list.append( entities_predicted )

print("テキスト: ", original_text[0])
print("正解: ", entities_list[0])
print("抽出: ", entities_predicted_list[0])
# テキスト:  なお、代表取締役は若林史江ではなく別の人物である。
# 正解:  [{'name': '若林史江', 'span': [9, 13], 'type_id': 1}]
# 抽出:  [{'name': '若林史江', 'span': [9, 13], 'type_id': 1}]
```


### Reference
- [BERTによる自然言語処理入門 Transformersを使った実践プログラミング](https://www.ohmsha.co.jp/book/9784274227264/)
- BERTによる自然言語処理入門 Transformersを使った実践プログラミングの[サポートページ](https://github.com/stockmarkteam/bert-book)
- [cl-tohoku/bert-japanese](https://github.com/cl-tohoku/bert-japanese)
- [Wikipediaを用いた日本語の固有表現抽出データセット](https://github.com/stockmarkteam/ner-wikipedia-dataset)
