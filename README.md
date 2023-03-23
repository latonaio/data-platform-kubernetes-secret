# kubernetes-secret
kubernetes-secret は、ユーザー名やパスワード等の機密情報をSecret を用いることにより安全で共通化した管理を行うためのマイクロサービスリポジトリです。

## Secret の作成
エンコードした値を以下のコマンドで作成します。
```sh
$ echo -n latona | base64
bGF0b25h
```
```sh
$ echo -n Latona2020! | base64
TGF0b25hMjAyMCE=
```

以下のように記述します。
```
apiVersion: v1
kind: Secret
metadata:
  name: sample-secret
type: Opaque
data:
  username: bGF0b25h
  password: TGF0b25hMjAyMCE=
```

## Pod への導入方法
### env.valueFrom での指定方法
以下のように記述します。
```
env:
  - name: SECRET_USERNAME
    valueFrom:
      secretKeyRef:
        name: sample-secret
        key: username
  - name: SECRET_PASSWORD
    valueFrom:
      secretKeyRef:
        name: sample-secret
        key: password
```

### envFrom での指定方法
以下のように記述します。

環境変数名のprefix を指定することができます。
```
envFrom:
  - prefix: SECRET_
    secretRef:
      name: sampele-secret
```