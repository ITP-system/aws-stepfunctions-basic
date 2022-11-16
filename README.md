# Step Functions の 制御パラメータを理解する

## Parameters を使いつつ、Input パラメータを引き継ぐ方法を確認する

Input パラメータの特定のキーの値を引き継ぐ時に、Input パラメータにそのキーがセットされていないとエラーとなる.
Input パラメータが設定されたり設定されなかったりするときに、Parameters も使う時にどのように引き継いでいったら良いかを確認する.

- Step Function ローカルの起動

```sh
docker run -p 8083:8083 amazon/aws-stepfunctions-local
```

- ステートマシンの作成

```sh
aws stepfunctions --endpoint http://localhost:8083 create-state-machine --name "machine1" --role-arn "arn:aws:iam::012345678901:role/DummyRole" --definition file://./test01.asl.json
```

```sh
aws stepfunctions --endpoint http://localhost:8083 create-state-machine --name "machine2" --role-arn "arn:aws:iam::012345678901:role/DummyRole" --definition file://./test02.asl.json
```

- ステートマシンの実行

```sh
aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:machine1 --name test01
```

```sh
aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:machine2 --name test01
```

- ステートマシンの削除

```sh
aws stepfunctions --endpoint http://localhost:8083 delete-state-machine --state-machine-arn arn:aws:states:us-east-1:123456789012:stateMachine:machine1
```

```sh
aws stepfunctions --endpoint http://localhost:8083 delete-state-machine --state-machine-arn arn:aws:states:us-east-1:123456789012:stateMachine:machine2
```
