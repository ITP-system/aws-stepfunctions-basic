# ループさせる

ループの実現方法は リファレンスにあるように

- Lambda 関数を使用する方法
- JsonPath を使う方法(array.asl.json)

がありますが、Map ステートを使う方法もあります.

MaxConcurrency を 1 にすると同時実行数が 1 になるので、事実上ループとなる方法です.
同時実行数を増やした場合は、もちろん処理順は Input で与えた通りではありません.
Lambda 関数を作る必要がないことやステートも減らせます.

```json
{
  "Comment": "A description of my state machine",
  "StartAt": "Map",
  "States": {
    "Map": {
      "Type": "Map",
      "Iterator": {
        "StartAt": "RunJob",
        "States": {
          "RunJob": {
            "Type": "Wait",
            "Seconds": 10,
            "End": true
          }
        }
      },
      "End": true,
      "MaxConcurrency": 1
    }
  }
}
```

JsonPath でループを実現する方法も保存してあるので、こちらも試してみてください.

# リファレンス

Lambda を使用してループを反復する
https://docs.aws.amazon.com/ja_jp/step-functions/latest/dg/tutorial-create-iterate-pattern-section.html

Using JSONPath effectively in AWS Step Functions
https://aws.amazon.com/jp/blogs/compute/using-jsonpath-effectively-in-aws-step-functions/

JsonPath
https://github.com/json-path/JsonPath

マップ
https://docs.aws.amazon.com/ja_jp/step-functions/latest/dg/amazon-states-language-map-state.html

- ステートマシンの作成

```sh
aws stepfunctions --endpoint http://localhost:8083 create-state-machine --name "machine3" --role-arn "arn:aws:iam::012345678901:role/DummyRole" --definition file://./loop/map.asl.json
```

```sh
aws stepfunctions --endpoint http://localhost:8083 create-state-machine --name "machine4" --role-arn "arn:aws:iam::012345678901:role/DummyRole" --definition file://./loop/jsonpath.asl.json
```

- ステートマシンの実行

```sh
aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:machine3 --name map --input "["1","2"]"
```

```sh
aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:machine4 --name jsonpath
```

- ステートマシンの削除

```sh
aws stepfunctions --endpoint http://localhost:8083 delete-state-machine --state-machine-arn arn:aws:states:us-east-1:123456789012:stateMachine:machine3
```

```sh
aws stepfunctions --endpoint http://localhost:8083 delete-state-machine --state-machine-arn arn:aws:states:us-east-1:123456789012:stateMachine:machine4
```
