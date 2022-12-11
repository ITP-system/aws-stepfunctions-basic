# parameters

input をさらに加工して parameters を使うユースケースがどういったものがあるのか、イメージできなかったので、わかってきたことをまとめていきます.

- paramters で Input とは別に値を追加する
  https://developers.wano.co.jp/1601/
- Map では input は配列でないといけないので、配列を渡しつつ、追加の情報を Parameters で渡す
  https://docs.aws.amazon.com/ja_jp/step-functions/latest/dg/amazon-states-language-map-state.html
- サービス API でパラメータを渡す
  https://docs.aws.amazon.com/ja_jp/step-functions/latest/dg/connect-parameters.html
