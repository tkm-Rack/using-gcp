# Hello, Cloud Run!!
## What is ?
GCP上のコンテナ実行環境</br>
Dockerfile か、Buildpacks を利用して動作させる。</br>
</br>
初期起動時間を短くするならば、alpine や distoless など比較的小さくクリーンなイメージを利用するとよき[^distoless]

## When use ?
よく比較されるのが、**Cloud Functions** 。 </br>
Functionsについてはサポートされる言語とランタイムの制約がある。</br>
Functions はイベントドリブンな実行に対して Cloud Run は HTTP Request ベースで動作する。</br>
実行時間やメモリなどに制約[^Quotas]があるので用途に合わせて使い分け。</br>
また、Cloud Run は concurrency ( 1コンテナで捌く上限 ) の設定が行えるのも異なる点。
|  Product  | Memory | Timeout  |
| ---- | ---- | ---- |
|  Cloud Functions | up to 8 GB |  up to 9 min |
|  Cloud Run  | up to 8 GB | up to 60 min |

## How use ?
チュートリアルについては、下記参照</br>
https://cloud.google.com/run/docs/tutorials/pubsub?hl=ja

Visual Studio Code で、エミュレータやデバッグの機能を[プラグイン](https://marketplace.visualstudio.com/items?itemName=GoogleCloudTools.cloudcode)で使えるので、そちらを利用して開発するのがよいかと。</br>
CD については、サービスをコマンドかコンソールで立ち上げて Github と Cloud Build を連携させて CD 環境は簡単に構成できる。</br>

## Limits
先ほどの Functions との比較でもありましたが、Cloud Run の代表的な上限値は下記のような感じ。
|  Resource  | Limit |
| ---- | ---- |
| インスタンス最大数 | 1000 |
| vCPU 最大数 | 4 |
| Memory 最大値 | 8 GB |
| コンテナインスタンスの最大起動時間 | 4 min |
| タイムアウト最大値 | 60 min |
| 同時リクエストの最大数 | 250 |

## Buildpacks
Buildpacks[^Buildpacks] は Google がオープンソースとして公開していて、</br>
任意の言語やソースコードからライブラリやフレームワークを読み取りベストプラクティスに則って Dockerfile を書かずにイメージを生成してくれる。</br>
ここで利用されるベースイメージは Google が提供しているものとなる。</br>
また、Hot reload 機能がありがたい。

Dockerfileが必要ないというものの、実行環境はDoceker。</br>
Docker Engine 上に minikube のコンテナを立て、その上で動作する。

## Reference
[^distoless]：https://github.com/GoogleContainerTools/distroless </br>
[^Quotas]: https://cloud.google.com/run/quotas?hl=ja </br>
[^Buildpacks]: https://github.com/GoogleCloudPlatform/buildpacks