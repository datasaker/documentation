# dsk-base-agent

`base-agent` はサーバから発生する様々な情報をリアルタイムで収集します。
たとえば、メモリ、CPU使用率など、サーバーのパフォーマンス指標、ネットワークトラフィック情報など、さまざまな情報を収集できます。
これにより、顧客はリアルタイムでサーバーの状態を監視でき、サーバーのパフォーマンスを最適化し、信頼性を向上させることができます。
お客様のニーズに合わせてエージェント設定を調整して、最適な結果を提供します。

## Datasaker先行作業を行いましたか？

現在の環境で `DataSaker`の先行操作が進行しなかった場合は、 `DataSaker`先行操作を先に進んでください。 [DataSaker先行操作](README.md)

## Base agentのインストール

### 1. パッケージのインストール

`DataSaker`の `Base agent`をインストールするにはsudo権限が必要です。
```shell
yum install dsk-node-agent
```
### 2. Base agentの設定
```shell
vi /etc/datasaker/dsk-node-agent/agent-config.yml
```
必要に応じて次の内容を修正します。
```yaml
agent:
  agent_name: "dsk-base-agent"
  cluster_id: "my-cluster-id"
```
各設定の説明は次のとおりです。

| **Settings** | **Description** | **Default** | **Required** |
| -------------------------- | ---------------------------------------------------------------------------------------------------- | :---------: | :----------: |
| `agent_name` |エージェント名（エイリアス）| dsk-base-agent |いいえ
| `cluster_id` |管理対象となる環境がどのクラスタにまとめられているかを設定します。 | unknown |いいえ

### 3. パッケージの実行
```shell
systemctl enable dsk-node-agent --now
```
### 4. パッケージ実行状態の確認
```shell
systemctl status dsk-node-agent
```
## Base agentを削除する

### 1. パッケージの中断
```shell
systemctl stop dsk-node-agent
```
### 2. パッケージの削除
```shell
yum remove dsk-node-agent
```
