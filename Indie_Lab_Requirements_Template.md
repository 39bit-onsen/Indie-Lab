# 📋 Indie Lab プロジェクト要件定義書

## 1. プロジェクト概要
- 名称：**Indie Lab**
- サブタイトル：みんなでつくる、遊べる分散クラウド
- 概要：個人・小規模チームでもフル機能のDevOps環境を保有・運用できるよう、Proxmox＋k3sを用いたCI/CD + GitOps基盤を構築する。

---

## 2. 目的・背景
- AIやローコードによりアプリ開発の民主化が進む一方、公開・運用インフラは中央集権的である
- オフラインで完結する「ひらめきの実験場」「分散型開発基地」を目指す
- GitHub（アプリ）、GitLab（インフラ）を連携し、自動化・信頼性テスト・復旧検証を内製化する

---

## 3. 構築対象（システム構成）
### 仮想基盤（Proxmox）:
- ノード数：2
- VM数：13以上（k3s 6台、CI/CD系 6台以上、NFS、管理PC）

### ネットワーク：
- セグメント：192.168.50.0/24（ServerNet） / VLAN 50
- MetalLB IP範囲：192.168.50.120〜169
- DNS構成：CoreDNS＋hosts定義、FQDN形式（例：*.home.lab）

---

## 4. 主なツール・構成要素
- K3s（軽量Kubernetes）
- GitHub + GitLab（アプリ・インフラ分離）
- GitLab Runner / Harbor / ArgoCD
- Prometheus / Grafana / EFK
- Litmus Chaos（カオスエンジニアリング）
- Terraform（構成自動化）予定

---

## 5. 要件分類

### 🔧 機能要件
- GitOpsによる自動デプロイ
- CI/CDパイプラインによるアプリ配信と検証
- Chaosテストと自動復旧シナリオ

### 🔒 非機能要件
- 外部非公開（VPN+SSH経由管理）
- セグメント・VLAN分離でセキュリティ担保
- ノード故障時も安定稼働（後述）

### 📊 性能要件（目標）
- CI/CD完了まで：10分以内
- SLO目標：99.9%以上（今後定義）
- MTTR：5分以内を目指す（Chaosログ分析より）

---

## 6. 実行スケジュール（フェーズ構成）
1. 要件定義（本ドキュメント）
2. ネットワーク設計＋UniFi設定
3. Proxmox構築
4. NFSストレージ設定
5. k3sクラスタ構築
6. MetalLB / Ingress / CoreDNS
7. GitLab / Runner
8. Harbor
9. ArgoCD
10. Grafana / EFK
11. Litmus Chaos統合

---

## 7. 今後の検討項目（メモ）
- Progressive Delivery（Argo Rolloutsなど）
- AIによるChaos分析 → 自動対応支援
- SLOダッシュボード設計