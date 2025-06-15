# 🗂 Indie Lab 構築スケジュール詳細タスクリスト（最新版）

## ✅ フェーズ0：前準備

### 0-1. プロジェクト要件定義
- [x] プロジェクト名・背景・目的の策定
- [x] ツール構成と非機能要件の整理

### 0-2. ネットワーク設計 + UniFi設定
- [ ] VLAN設計（Default=1, ServerNet=50）
- [ ] DHCP/IP範囲・予約整理
- [ ] USW-16-PoE ポートマップ設定
- [ ] UDM-Proルーティング & DNS設定

### 0-3. Proxmox構築（2ノード）
- [ ] OSインストール（Ubuntu Server）
- [ ] ブリッジネットワーク作成（vmbr0）
- [ ] SSH/GUI設定確認
- [ ] NTP設定・ホスト名設定

---

## 🧱 フェーズ1：仮想基盤 & クラスタ構築

### 1-1. NFSストレージ構築
- [ ] Linux PCへNFSサーバ構成（`storage-pc-1`）
- [ ] `/exports/nfs/k3s` ディレクトリ作成・権限調整
- [ ] 各VMからのマウントテスト

### 1-2. k3sクラスタ構築
- [ ] `k3s-pc-1` にControl Plane構築（1台）
- [ ] `k3s-pc-2〜6` をWorkerとしてJoin
- [ ] NFSベースのPV確認
- [ ] `kubectl get nodes` で状態確認

### 1-3. MetalLB + Ingress + CoreDNS
- [ ] MetalLBで `192.168.50.120〜169` を割当
- [ ] nginx-ingress コントローラー配置
- [ ] CoreDNSに `*.home.lab` を登録
- [ ] 踏み台からFQDNアクセス確認

---

## 🔧 フェーズ2：CI/CD & GitOps

### 2-1. GitLab構築
- [ ] GitLab VM作成（`192.168.50.31`）
- [ ] 管理ユーザー登録と初期設定
- [ ] GitOps用リポジトリ作成（インフラ）

### 2-2. GitLab Runner構築
- [ ] `runner` VM構築（`192.168.50.32`）
- [ ] GitLabと登録（トークン登録）
- [ ] テスト用 `.gitlab-ci.yml` 作成

### 2-3. Harbor構築（レジストリ）
- [ ] `harbor` VM構築（`192.168.50.33`）
- [ ] Dockerログイン＆push確認
- [ ] GitLab/Argoと連携設定

### 2-4. ArgoCD構築（GitOps）
- [ ] `argocd` VM作成・Argoインストール
- [ ] GitLab連携＆リポジトリ登録
- [ ] アプリ自動同期設定（self-healあり）

---

## 📊 フェーズ3：監視・信頼性設計

### 3-1. 可観測性環境構築
- [ ] `grafana`, `prometheus`, `efk` VM起動
- [ ] Podメトリクス可視化（Grafanaダッシュボード）
- [ ] Fluent Bitログ転送→Elasticsearch確認

### 3-2. Litmus Chaos導入 & 想定SLO定義
- [ ] `litmus` namespace作成
- [ ] ChaosOperatorとLitmus Portal導入
- [ ] 初期シナリオ（Pod削除、Stress）登録
- [ ] GameDayスケジュール設計
- [ ] 想定SLO（例：MTTR < 5分、可用性99.9%）を仮定義

---

## 🧠 フェーズ4：分析・高度化（任意）

- [ ] Chaos実験ログの生成AI連携（例：SageMaker, Claude）
- [ ] 自動修復戦略のテンプレート化（ArgoCD + Rollback）
- [ ] SLOダッシュボード設計と通知設計（Slack/PagerDuty）