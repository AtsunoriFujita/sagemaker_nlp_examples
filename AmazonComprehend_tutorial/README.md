# Amazon Comprehend テキスト分析チュートリアル

## 前提条件
本コンテンツはAWS上でシステム開発経験のある方を対象としています。Amazon Comprehendの機能をAPI経由で呼び出し、テキストデータの分析を行います。APIを呼び出す環境として**Amazon SageMaker Notebookインスタンス**、開発言語は**Python**を利用します。Amazon SageMaker、Jupyter Notebook、Python自体の説明については本コンテンツの対象外となります。また、今回はPythonを利用しますが、本コンテンツで実行している処理はAWS CLIや、他の開発言語でも実行可能です。

```python
review_text ='ハワイアンの心和む音楽の中、ちょっとシリアスなドラマが展開していきます。音楽の力ってすごいな、って思いました。'

# Entity抽出
result = client.detect_entities(Text=review_text, LanguageCode='ja')
entities = result['Entities']
for entity in entities:
    print('Entity', entity)

# Entity {'Score': 0.9531810283660889, 'Type': 'OTHER', 'Text': 'ハワイアン', 'BeginOffset': 0, 'EndOffset': 5}

# KeyPhrase抽出
result = client.detect_key_phrases(Text=review_text, LanguageCode='ja')
keyPhrases = result['KeyPhrases']
for keyPhrase in keyPhrases:
    print('KeyPhrase', keyPhrase)

# KeyPhrase {'Score': 0.9969016909599304, 'Text': 'ハワイアンの心', 'BeginOffset': 0, 'EndOffset': 7}
# KeyPhrase {'Score': 0.9962161183357239, 'Text': '音楽の中', 'BeginOffset': 9, 'EndOffset': 13}
# KeyPhrase {'Score': 0.9926095604896545, 'Text': 'ドラマ', 'BeginOffset': 23, 'EndOffset': 26}
# KeyPhrase {'Score': 0.9999393820762634, 'Text': '音楽の力', 'BeginOffset': 36, 'EndOffset': 40}

# Sentiment分析
result = client.detect_sentiment(Text=review_text, LanguageCode='ja')
sentimentScores = result['SentimentScore']
for sentiment in sentimentScores:
    print(sentiment, sentimentScores[sentiment])

# Positive 0.9588087201118469
# Negative 0.0004567811847664416
# Neutral 0.040275391191244125
# Mixed 0.0004591413598973304

```

## 事前準備
まずはAWSマネジメントコンソールを開きます。    
ブラウザから https://console.aws.amazon.com/ にアクセスし、マネジメントコンソールにログインしてください。
最初にAmazon SageMakerのノートブックインスタンスの作成を行います。    
**ノートブックインスタンスは起動中は課金対象となりますので、終了後に停止・削除してください。**    
マネジメントコンソールが表示されたら、「サービスを検索する」の部分のテキストボックスに sagemakerと入力し、ドロップダウンに`Amazon SageMaker`と表示されたら、クリックします。

![Image 1](assets/図1.png)

画面左の「ノートブックインスタンス」をクリックします。
![Image 2](assets/図2.png)

「ノートブックインスタンスの作成」をクリックします。
![Image 3](assets/図3.png)

「ノートブックインスタンス名」に任意の名前を入力します。
![Image 4](assets/図4.png)

アクセス許可と暗号化 のセクションでは、IAMロールのドロップダウンから「新しいロールの作成」を選択します。
![Image 5](assets/図5.png)

「ロールの作成」をクリックします。
![Image 6](assets/図6.png)

「成功! IAMロールを作成しました」の下のIAMロール名のリンクをクリックします。
![Image 7](assets/図7.png)

作成したIAMロールの管理画面が表示されます。    
- ここからは作成したIAMロールに対して、チュートリアルに必要な権限設定を行います。ここでは実施を簡単にするためにSageMakerの実行ロールに対して必要な権限を与えていきますが、実運用時には必要最小限の権限を持つIAMロールを作成して利用するようにしてください。

「ポリシーをアタッチします」をクリックします。
![Image 8](assets/図8.png)

`ComprehendFullAceess`をアタッチします。
![Image 9](assets/図9.png)

インラインポリシーを追加します。
![Image 10](assets/図10.png)

「JSON」タブを開いてポリシーを記入します。
![Image 11](assets/図11.png)

下記のポリシーを入力します。
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "comprehend.amazonaws.com"
                }
            }
        }
    ]
}
```
![Image 12](assets/図12.png)

「ポリシーの確認」をクリックします。
![Image 13](assets/図13.png)

任意のポリシー名を入力し「ポリシーの作成」をクリック後、作成したポリシーをアタッチします。
![Image 14](assets/図14.png)

「信頼関係」タブをクリックします。
![Image 15](assets/図15.png)

「信頼関係の編集」をクリックします。
![Image 16](assets/図16.png)

入力済の値がありますが、下記の内容で上書きし、「信頼ポリシーの更新」をクリックします。
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sagemaker.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    },
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "comprehend.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
![Image 17](assets/図17.png)

信頼されたエンティティに`Comprehend`が追加されます
![Image 18](assets/図18.png)

Amazon SageMakerのノートブックインスタンス作成画面に戻り、「ノートブックインスタンス作成」をクリックします。
![Image 19](assets/図19.png)

ステータスが `InService` になるのを待ちます。
![Image 20](assets/図20.png)

ステータスが `InService` になったら「Jupyter を開く」をクリックします
![Image 21](assets/図21.png)

Jupyter Notebookの画面が開きます。    
Jupyter Notebookの画面の「Upload」をクリックし、`AmazonComprehend-Text-Analysis.ipynb` をアップロードします（このリポジトリをクローンするでも構いません）。
![Image 22](assets/図22.png)

「Upload」をクリックします。
![Image 23](assets/図23.png)

`AmazonComprehend-Text-Analysis.ipynb`のリンクをクリックして、ノートブックを開きます。
![Image 24](assets/図24.png)

ノートブックが起動した後はノートブックに記載されている手順に従ってください。
![Image 25](assets/図25.png)
