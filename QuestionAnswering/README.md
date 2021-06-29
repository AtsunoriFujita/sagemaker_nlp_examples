# QuestionAnswering

![QuestionAnswering](https://user-images.githubusercontent.com/40932835/123745696-9b56c000-d8eb-11eb-8bd5-22a05a00f28b.png)

↑の画像は[ここ](https://d2l.ai/chapter_natural-language-processing-applications/finetuning-bert.html)から

## SQuAD_training
- SageMaker HuggingFaceコンテナを使用したPyTorchでのトレーニングジョブ
  - HuggingFaceの[run_squad.py](https://github.com/huggingface/transformers/blob/master/examples/legacy/question-answering/run_squad.py)をSageMakerのトレーニングジョブで動作するよう一部変更
- [運転ドメインQAデータセット](https://nlp.ist.i.kyoto-u.ac.jp/index.php?Driving%20domain%20QA%20datasets)と[東北大学の日本語BERT](https://github.com/cl-tohoku/bert-japanese)を使用したFine-Tuning
  - データセットは許諾に同意の上ダウンロードしていただく必要があります。

#### Local環境での推論
```python
context = 'まぁ，何回か改正してるわけで，自転車を走らせる領域を変更しないって言うのは，怠慢っていうか責任逃れっていうか，道交法に携わってるヤツはみんな馬鹿なのか．大体の人はここまで極端な意見ではないだろうけど，自転車は歩道を走るほうが自然だとは考えているだろう．というのも， みんな自転車乗ってる時歩道を走るでしょ？自転車で歩道走ってても歩行者にそこまで危険な目に合わせないと考えているし，車道に出たら明らかに危険な目に合うと考えている．'
question = '大体の人は自転車はどこを走るのが自然だと思っている？'

inputs = tokenizer.encode_plus(question, context, add_special_tokens=True, return_tensors="pt")
input_ids = inputs["input_ids"].tolist()[0]
output = model(**inputs)
answer_start = torch.argmax(output.start_logits)  
answer_end = torch.argmax(output.end_logits) + 1
answer = tokenizer.convert_tokens_to_string(tokenizer.convert_ids_to_tokens(input_ids[answer_start:answer_end]))

print("質問: "+question)
print("回答: "+answer)

# 質問: 大体の人は自転車はどこを走るのが自然だと思っている？
# 回答: 歩道
```

## SQuAD_inference
- coming soon

### Reference
- [運転ドメインQAデータセット](https://nlp.ist.i.kyoto-u.ac.jp/index.php?Driving%20domain%20QA%20datasets)
- [cl-tohoku/bert-japanese](https://github.com/cl-tohoku/bert-japanese)
- [Huggingface Transformers 入門 (14) - 日本語の質問応答の学習](https://note.com/npaka/n/na8721fdc3e24)
