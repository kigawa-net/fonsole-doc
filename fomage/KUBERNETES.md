# Kubernetes ガイド

このガイドでは、`fomage`アプリケーションをKubernetesクラスタにデプロイする方法を説明します。

## 前提条件

- `kubectl`がインストールされ、Kubernetesクラスタにアクセスできるように設定されていること。
- 動作中のKubernetesクラスタがあること（例: Minikube, Docker Desktop, GKE, AKS, EKSなど）。

## 🚀 デプロイ手順

`k8s`ディレクトリに含まれるマニフェストファイルを使用して、`fomage`とその依存サービスをデプロイします。

### 1. PersistentVolumeClaimの作成

まず、MongoDBが使用する永続ボリュームを要求します。

```bash
kubectl apply -f k8s/mongodb-pvc.yaml
```

### 2. MongoDBのデプロイ

次に、MongoDBのDeploymentとServiceをデプロイします。

```bash
kubectl apply -f k8s/mongodb-deployment.yaml
kubectl apply -f k8s/mongodb-service.yaml
```

### 3. Fomageアプリケーションのデプロイ

最後に、`fomage`アプリケーションのConfigMap, Deployment, Serviceをデプロイします。

```bash
kubectl apply -f k8s/fomage-configmap.yaml
kubectl apply -f k8s/fomage-deployment.yaml
kubectl apply -f k8s/fomage-service.yaml
```

### 4. デプロイ状態の確認

すべてのPodが正常に起動していることを確認します。

```bash
kubectl get pods -l app=fomage-app
kubectl get pods -l app=mongodb
```

`STATUS`が`Running`になるまで数分かかることがあります。

### 5. アプリケーションへのアクセス

`fomage-app-service`の外部IPを確認します。

```bash
kubectl get service fomage-app-service
```

`EXTERNAL-IP`が割り当てられたら、そのIPアドレスにブラウザでアクセスします。
(LoadBalancerタイプのServiceがクラウドプロバイダーによってサポートされている必要があります)

## 🔧 アンデプロイ手順

デプロイしたリソースを削除するには、以下のコマンドを実行します。

```bash
kubectl delete -f k8s/fomage-service.yaml
kubectl delete -f k8s/fomage-deployment.yaml
kubectl delete -f k8s/fomage-configmap.yaml
kubectl delete -f k8s/mongodb-service.yaml
kubectl delete -f k8s/mongodb-deployment.yaml
kubectl delete -f k8s/mongodb-pvc.yaml
``` 