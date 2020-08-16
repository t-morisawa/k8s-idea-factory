
# local

```
docker build -t idea-factory-xyz .
docker run -p 8080:80 idea-factory-xyz
open http://localhost:8080/
```

# deploy (GCP)

## push docker image

```
docker tag idea-factory-xyz gcr.io/[PROJECT-ID]/idea-factory-xyz
docker push gcr.io/[PROJECT-ID]/idea-factory-xyz
```

## create k8s cluster

```
gcloud container clusters create idea-factory-xyz --num-nodes=2
gcloud container clusters describe idea-factory-xyz
```

## deploy k8s objects

```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

# メモ

## k8sで静的ファイルホスティングを行う場合
 - イメージに静的ファイルを含めて配信します。これによりimageの管理が必要になりますが、k8sのエコシステムに含めることができます。
 - confファイルはConfigMap Objectを利用することでイメージに含めずに利用することもできます。
 - 一般的には静的ファイルホスティングでk8sを選択するケースはあまりないかと思います。

## マニフェストファイルの作成

クラスタ作成後、以下を実行すると雛形のyamlが出力されます

```
kubectl create deployment idea-factory-xyz --image=nginx --dry-run -o yaml
```

あるいはGCPのコンソール上から作成したオブジェクトのyamlファイルを参照する手もあります。

## Podの疎通確認

Serviceを作成しなくても、ローカルへポートフォワーディングしてpodへの疎通確認ができます。

```
kubectl get pods
kubectl port-forward <POD NAME> 8080:80
```
