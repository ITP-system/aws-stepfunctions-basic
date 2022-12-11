# parameters

input をさらに加工して parameters を使うユースケースがどういったものがあるのか、イメージできなかったので、わかってきたことをまとめていきます.

- Map では input は配列でないといけないので、配列を渡しつつ、追加の情報を Parameters で渡す
  AWS の公式で以下を読むとわかりますが、この例を作ってみました.
  https://docs.aws.amazon.com/ja_jp/step-functions/latest/dg/amazon-states-language-map-state.html

- paramters で Input とは別に値を追加する
  https://developers.wano.co.jp/1601/
- サービス API でパラメータを渡す
  https://docs.aws.amazon.com/ja_jp/step-functions/latest/dg/connect-parameters.html

- ステートマシンの作成

```sh
$ aws stepfunctions --endpoint http://localhost:8083 create-state-machine --name "parameters-1" --role-arn "arn:aws:iam::012345678901:role/DummyRole" --definition file://./parameters/map.asl.json
```

- ステートマシンの実行

```sh
$ aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:parameters-1 --name parameters-1 --input file://./parameters/input.json
```
