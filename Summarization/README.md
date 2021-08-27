# Summarization

## T5_training
- SageMaker HuggingFaceコンテナを使用したPyTorchでのトレーニングジョブ
  - HuggingFaceの[run_summarization.py](https://github.com/huggingface/transformers/blob/master/examples/pytorch/summarization/run_summarization.py)を使用

- [wikiHow日本語要約データセット](https://github.com/Katsumata420/wikihow_japanese)と[sonoisa/t5-base-japanese](https://huggingface.co/sonoisa/t5-base-japanese#%E6%97%A5%E6%9C%AC%E8%AA%9Et5%E4%BA%8B%E5%89%8D%E5%AD%A6%E7%BF%92%E6%B8%88%E3%81%BF%E3%83%A2%E3%83%87%E3%83%AB)を使用したFine-Tuning

**SageMakerノートブックインスタンスのボリュームサイズは20GB以上必要になります（ローカル推論の際にモデルをs3からダウンロードする必要があるため）。**

#### Local環境での推論
```python
import torch
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

model = AutoModelForSeq2SeqLM.from_pretrained('output/')    
tokenizer = AutoTokenizer.from_pretrained('sonoisa/t5-base-japanese')
model.eval()

text = '''
犬は、前足と後ろ足に体重をかけて歩きます。犬が立つと、後ろ脚の足と膝の間に足首があるのが分かります。人間も、つま先立ちをすると、これに似た格好になります。犬の前脚には足首はありません。人間の腕に足首がないのと同じです。しかし、前脚にも別のタイプの捻挫が起きることがあり、足首の捻挫と同じように対処します。たいていの犬は激しく動き回ります。犬の激しい動きは関節に多大な力と負荷をかけ、時にはこれが怪我を招きます。走る、跳ねる、急に方向転換をするといった動きは関節に大きな負担をかけます。犬の運動量は一様ではありませんが、犬によっては許容量以上の負荷を関節にかけてしまうことがあります。他にも、転ぶ、高所から落ちる、穴に足がはまるといったアクシデントや、ソファーの上り下りのような日常的な行動が原因で捻挫が起こることがあります。捻挫が起こると、まず最初に犬が足を引きずっていることに気づくでしょう。これは捻挫の最も分かりやすい兆候です。捻挫した犬は、痛めた足に体重をかけないようにすることがあります。怪我の度合いにもよりますが、痛めた足を完全に宙に持ち上げて、その足を使わないようにするかもしれません。他の原因でも足を引きずることがあります。腰や膝や足元に怪我を負った犬も足を引きずるかもしれません。捻挫した犬は、足首のまわりが赤く腫れているかもしれません。捻挫した箇所をよく舐める場合もあります。怪我をした犬は、普段とは違う行動をとることがあります。以下の行動の変化に注意しましょう。食欲の変化：怪我をしている犬には食欲の減退が見られます。運動量の変化：睡眠時間が増えたり、動くのを嫌がるようになります。声での訴え：足首を動かしたり触られた時に、吠える、唸る、クンクンと鳴くなどして不快感を訴えます。
'''
inputs = tokenizer.encode(text, return_tensors="pt", max_length=1024, truncation=True)

print('='*5,'原文', '='*5)
print(text)
print('-'*5, '要約', '-'*5)

with torch.no_grad():
    summary_ids = model.generate(inputs, max_length=1024, do_sample=False, num_beams=1)
    summary = tokenizer.decode(summary_ids[0])
    print(summary)

'''
===== 原文 =====
犬は、前足と後ろ足に体重をかけて歩きます。犬が立つと、後ろ脚の足と膝の間に足首があるのが分かります。人間も、つま先立ちをすると、これに似た格好になります。犬の前脚には足首はありません。人間の腕に足首がないのと同じです。しかし、前脚にも別のタイプの捻挫が起きることがあり、足首の捻挫と同じように対処します。たいていの犬は激しく動き回ります。犬の激しい動きは関節に多大な力と負荷をかけ、時にはこれが怪我を招きます。走る、跳ねる、急に方向転換をするといった動きは関節に大きな負担をかけます。犬の運動量は一様ではありませんが、犬によっては許容量以上の負荷を関節にかけてしまうことがあります。他にも、転ぶ、高所から落ちる、穴に足がはまるといったアクシデントや、ソファーの上り下りのような日常的な行動が原因で捻挫が起こることがあります。捻挫が起こると、まず最初に犬が足を引きずっていることに気づくでしょう。これは捻挫の最も分かりやすい兆候です。捻挫した犬は、痛めた足に体重をかけないようにすることがあります。怪我の度合いにもよりますが、痛めた足を完全に宙に持ち上げて、その足を使わないようにするかもしれません。他の原因でも足を引きずることがあります。腰や膝や足元に怪我を負った犬も足を引きずるかもしれません。捻挫した犬は、足首のまわりが赤く腫れているかもしれません。捻挫した箇所をよく舐める場合もあります。怪我をした犬は、普段とは違う行動をとることがあります。以下の行動の変化に注意しましょう。食欲の変化：怪我をしている犬には食欲の減退が見られます。運動量の変化：睡眠時間が増えたり、動くのを嫌がるようになります。声での訴え：足首を動かしたり触られた時に、吠える、唸る、クンクンと鳴くなどして不快感を訴えます。
----- 要約 -----
<pad><extra_id_0>。犬の足の捻挫に注意する。犬の行動を観察する。</s>
'''
```

### Reference
- [wikiHow日本語要約データセット](https://github.com/Katsumata420/wikihow_japanese)
- [sonoisa/t5-base-japanese](https://huggingface.co/sonoisa/t5-base-japanese#%E6%97%A5%E6%9C%AC%E8%AA%9Et5%E4%BA%8B%E5%89%8D%E5%AD%A6%E7%BF%92%E6%B8%88%E3%81%BF%E3%83%A2%E3%83%87%E3%83%AB)
- [Huggingface Transformers 入門 (25) - 日本語の要約の学習](https://note.com/npaka/n/n6df9ef13a0ed)
