# Step Functions の 制御パラメータを理解する

## 任意の　 Input パラメータ を Output 　パラメータに 引き継ぐ方法

Input パラメータ の値を Parameters などで参照する場合、そのキーが Input にないとエラーとなります.

```json
{
  "StartAt": "ParametersTest",
  "States": {
    "ParametersTest": {
      "Type": "Pass",
      "Parameters": {
        "fieldname.$": "$.fieldname"
      },
      "ResultPath": "$",
      "Next": "Completed"
    },
    "Completed": {
      "Type": "Succeed"
    }
  }
}
```

```
"An error occurred while executing the state 'XXXXXXX' (entered at the event id #2). The JSONPath '$.fieldname' specified for the field 'fieldname.$' could not be found in the input
```

このような場合、コンテキストオブジェクトを使用して Output パラメータに引き継ぐことが可能です.

```json
{
  "StartAt": "ParametersTest",
  "States": {
    "ParametersTest": {
      "Type": "Pass",
      "Parameters": {
        "Input.$": "$$.Execution.Input"
      },
      "ResultPath": "$",
      "Next": "Completed"
    },
    "Completed": {
      "Type": "Succeed"
    }
  }
}
```

## リファレンス

Context オブジェクト

https://docs.aws.amazon.com/ja_jp/step-functions/latest/dg/input-output-contextobject.html

AWS step functions and optional parameters

https://stackoverflow.com/questions/59056043/aws-step-functions-and-optional-parameters

## サンプルの実行の仕方

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
