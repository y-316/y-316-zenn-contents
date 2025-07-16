---
title: "第6章: 実践的なセキュリティ管理とコンプライアンス対応"
---

# はじめに

前章では、PowerShellを使用したクラウドリソースの効率的な管理について学びました。この章では、企業レベルのセキュリティ管理とコンプライアンス対応について、実践的な設定方法から監査対応まで詳しく学んでいきます。

セキュリティとコンプライアンスは、現代の企業IT運用において最も重要な要素です。新人エンジニアから経験豊富な管理者まで、実務で役立つ知識を段階的に提供します。

## 6.1 高度なセキュリティ機能の実装

### 6.1.1 Microsoft Defender for Office 365

#### 高度な脅威保護の概要

**Microsoft Defender for Office 365**は、電子メールとコラボレーションツールに対する高度な脅威保護を提供します。

**主要な保護機能：**
- **安全な添付ファイル**: 不審な添付ファイルの隔離・分析
- **安全なリンク**: URLの動的スキャンと書き換え
- **フィッシング対策**: 高度なフィッシング検出
- **スプーフィング対策**: 送信者偽装の検出と防止
- **脅威インテリジェンス**: 最新の脅威情報の活用

#### 安全な添付ファイルの設定

**📋 実践：安全な添付ファイルの設定**

**Microsoft 365 Defender ポータルでの設定：**
1. **Microsoft 365 Defender** (`security.microsoft.com`) にアクセス
2. **メール & コラボレーション** > **ポリシーと規則** > **脅威ポリシー**
3. **安全な添付ファイル** をクリック
4. **新しいポリシーの作成** を選択

**推奨設定：**
```
ポリシー名: 全社員向け安全な添付ファイル
対象ユーザー: すべてのユーザー
設定:
□ 不明なマルウェア応答: ブロック
□ 添付ファイルの再書き込み: 有効
□ 動的配信: 有効
□ 保護された添付ファイル: 有効
```

#### 安全なリンクの設定

**安全なリンク**は、メール内のURLを動的にスキャンし、悪意のあるリンクから保護します。

**📋 実践：安全なリンクの設定**

**設定手順：**
1. **Microsoft 365 Defender** > **安全なリンク**
2. **新しいポリシーの作成** をクリック
3. **ポリシー設定**:
   - **名前**: 「全社員向け安全なリンク」
   - **対象**: すべてのユーザー
   - **アクション**: 既知の悪意のあるリンクを書き換え
   - **リアルタイム URL スキャン**: 有効
   - **メッセージ内の疑わしいリンク**: 有効

### 6.1.2 Azure Security Center（Microsoft Defender for Cloud）

#### セキュリティ態勢の監視

**Microsoft Defender for Cloud**は、Azure環境のセキュリティ態勢を包括的に監視します。

**主要機能：**
- **セキュリティスコア**: 環境のセキュリティ状況を数値化
- **推奨事項**: 具体的な改善提案
- **脅威検出**: 異常な活動の検出
- **コンプライアンス**: 規制要件への準拠状況

#### 推奨事項の実装

**📋 実践：セキュリティ推奨事項の実装**

**Azure portalでの確認手順：**
1. **Azure portal** > **Microsoft Defender for Cloud**
2. **セキュリティ態勢** を選択
3. **推奨事項** タブで改善項目を確認
4. 優先度の高い項目から順次対応

**一般的な推奨事項：**
```
高優先度:
□ 仮想マシンで Just-In-Time アクセス制御を有効化
□ ストレージアカウントで安全な転送を有効化
□ SQL サーバーで脆弱性評価を有効化
□ 仮想マシンにエンドポイント保護をインストール

中優先度:
□ ネットワークセキュリティグループの規則を制限
□ 仮想マシンで適応型アプリケーション制御を有効化
□ Key Vault で診断ログを有効化
```

### 6.1.3 Identity Protection（身元保護）

#### リスクベースの条件付きアクセス

**Microsoft Entra ID Identity Protection**は、機械学習を活用してリスクを検出し、自動的に対応します。

**検出されるリスク：**
- **匿名IPアドレス**: Tor ネットワークなどからのアクセス
- **非定型なサインイン**: 普段と異なる場所からのアクセス
- **マルウェアリンク**: 感染したデバイスからのアクセス
- **漏洩した資格情報**: 公開されたパスワードリスト

**📋 実践：リスクベースの条件付きアクセス設定**

**設定手順：**
1. **Microsoft Entra ID** > **セキュリティ** > **Identity Protection**
2. **条件付きアクセス** > **新しいポリシー**
3. **ポリシー設定**:
   - **名前**: 「リスクベースアクセス制御」
   - **ユーザー**: すべてのユーザー
   - **条件**: サインインリスク「中」以上
   - **アクセス制御**: MFA を要求

**リスクレベル別の対応：**
```
低リスク:
- 通常のアクセス許可
- 追加の認証不要

中リスク:
- 多要素認証を要求
- パスワード変更を推奨

高リスク:
- アクセス拒否
- 管理者への通知
- パスワード強制リセット
```

## 6.2 コンプライアンス管理の実装

### 6.2.1 データ保護とプライバシー

#### Microsoft Purview の活用

**Microsoft Purview**は、データの分類、保護、ガバナンスを統合的に管理するソリューションです。

**主要機能：**
- **データ分類**: 機密度ラベルの自動適用
- **データ損失防止（DLP）**: 機密データの流出防止
- **情報ガバナンス**: データの保持と削除
- **インサイダーリスク管理**: 内部脅威の検出

#### 機密度ラベルの設定

**📋 実践：機密度ラベルの設定**

**Microsoft Purview コンプライアンス ポータルでの設定：**
1. **Microsoft Purview** (`compliance.microsoft.com`) にアクセス
2. **情報保護** > **機密度ラベル**
3. **ラベルの作成** をクリック

**推奨ラベル構成：**
```
機密度ラベル:
1. パブリック
   - 保護: なし
   - 用途: 一般公開可能な情報

2. 社内
   - 保護: 暗号化（組織内のみ）
   - 用途: 社内限定情報

3. 機密
   - 保護: 暗号化（指定ユーザーのみ）
   - 用途: 機密情報（契約書、財務情報等）

4. 極秘
   - 保護: 暗号化（役員のみ）
   - 用途: 最高機密情報
```

#### データ損失防止（DLP）ポリシー

**DLP ポリシー**は、機密データの不正な共有や流出を防止します。

**📋 実践：DLP ポリシーの設定**

**設定手順：**
1. **Microsoft Purview** > **データ損失防止** > **ポリシー**
2. **ポリシーの作成** をクリック
3. **テンプレート**: 「日本の個人情報保護法」を選択

**基本的な DLP ポリシー設定：**
```
ポリシー名: 個人情報保護
対象場所: Exchange Online, SharePoint Online, Teams
検出条件:
□ 日本の個人番号（マイナンバー）
□ クレジットカード番号
□ 銀行口座番号

アクション:
□ 外部共有時: ブロック
□ 内部共有時: ユーザーに警告
□ 管理者通知: 有効
```

### 6.2.2 情報ガバナンス

#### 保持ポリシーの設定

**保持ポリシー**は、データの保持期間と削除を自動化します。

**📋 実践：保持ポリシーの設定**

**設定手順：**
1. **Microsoft Purview** > **情報ガバナンス** > **保持ポリシー**
2. **新しい保持ポリシー** をクリック
3. **場所の選択**: Exchange, SharePoint, Teams を選択

**業務別保持ポリシー：**
```
会計関連文書:
- 保持期間: 7年
- 保持後のアクション: 削除

人事関連文書:
- 保持期間: 5年
- 保持後のアクション: 削除

営業関連文書:
- 保持期間: 3年
- 保持後のアクション: 削除

プロジェクト文書:
- 保持期間: 2年
- 保持後のアクション: 削除
```

#### 電子情報開示（eDiscovery）

**eDiscovery**は、法的要請や内部調査に対応するための機能です。

**📋 実践：eDiscovery の基本設定**

**設定手順：**
1. **Microsoft Purview** > **eDiscovery** > **Core**
2. **新しいケース** をクリック
3. **ケース情報**: 名前、説明、保管担当者を設定

**eDiscovery プロセス：**
```
1. ケース作成
   - 調査目的の明確化
   - 関係者の特定
   - 期間の設定

2. ホールドの設定
   - 対象データの保持
   - 自動削除の停止
   - 変更の防止

3. 検索の実行
   - キーワード検索
   - 日付範囲指定
   - 対象者の指定

4. エクスポート
   - 検索結果の出力
   - 法的形式での保存
   - 証拠保全
```

### 6.2.3 コンプライアンス評価

#### コンプライアンス マネージャー

**Microsoft Purview コンプライアンス マネージャー**は、組織のコンプライアンス状況を評価・管理します。

**主要機能：**
- **コンプライアンススコア**: 準拠状況の数値化
- **改善アクション**: 具体的な改善提案
- **評価テンプレート**: 各種規制への対応
- **証拠管理**: コンプライアンス証拠の保存

**📋 実践：コンプライアンス評価の実施**

**評価手順：**
1. **Microsoft Purview** > **コンプライアンス マネージャー**
2. **評価** > **評価テンプレート**から適切な規制を選択
3. **改善アクション**を確認し、優先度順に実装

**日本企業向け重要規制：**
```
個人情報保護法:
□ 個人データの適切な取扱い
□ 安全管理措置の実施
□ 本人同意の取得
□ 第三者提供の制限

不正競争防止法:
□ 営業秘密の保護
□ アクセス制御の実装
□ 従業員教育の実施

電子帳簿保存法:
□ 電子データの保存要件
□ 検索機能の提供
□ 改ざん防止措置
```

## 6.3 監査とログ管理

### 6.3.1 統合監査ログ

#### 監査ログの有効化

**統合監査ログ**は、Microsoft 365 全体の活動を記録します。

**📋 実践：統合監査ログの有効化**

**PowerShell での設定：**
```powershell
# Exchange Online PowerShell で接続
Connect-ExchangeOnline

# 統合監査ログの有効化
Set-AdminAuditLogConfig -UnifiedAuditLogIngestionEnabled $true

# 設定の確認
Get-AdminAuditLogConfig
```

**監査対象の活動：**
```
Exchange Online:
□ メールボックスアクセス
□ メッセージの送受信
□ フォルダーの操作
□ メールボックス設定の変更

SharePoint Online:
□ ファイルのアクセス
□ ファイルの変更・削除
□ 共有設定の変更
□ サイト設定の変更

Microsoft Teams:
□ チームの作成・削除
□ メンバーの追加・削除
□ 会議の作成・参加
□ ファイルの共有

Microsoft Entra ID:
□ ユーザーのサインイン
□ 管理者の操作
□ 条件付きアクセス
□ MFA の使用
```

### 6.3.2 監査ログの分析

#### 基本的な監査ログ検索

**📋 実践：監査ログの検索**

**Microsoft Purview での検索：**
1. **Microsoft Purview** > **監査**
2. **検索** タブで条件を設定
3. **検索** をクリック

**PowerShell での検索：**
```powershell
# 過去7日間の管理者活動を検索
$results = Search-UnifiedAuditLog -StartDate (Get-Date).AddDays(-7) -EndDate (Get-Date) -RecordType ExchangeAdmin

# 結果の表示
$results | Select-Object CreationDate, UserIds, Operations | Format-Table

# 特定のユーザーの活動を検索
$userResults = Search-UnifiedAuditLog -StartDate (Get-Date).AddDays(-7) -EndDate (Get-Date) -UserIds "user@contoso.com"

# CSVファイルに出力
$userResults | Export-Csv -Path "C:\Audit\UserActivity.csv" -NoTypeInformation
```

#### 高度な監査ログ分析

**📋 実践：高度な監査ログ分析**

```powershell
# 疑わしい活動の検出スクリプト
function Find-SuspiciousActivity {
    param(
        [int]$Days = 7,
        [string]$OutputPath = "C:\Audit\"
    )
    
    $startDate = (Get-Date).AddDays(-$Days)
    $endDate = Get-Date
    
    # 1. 異常なサインイン活動
    Write-Host "異常なサインイン活動を検索中..." -ForegroundColor Yellow
    $suspiciousSignins = Search-UnifiedAuditLog -StartDate $startDate -EndDate $endDate -Operations "UserLoggedIn" | 
                        Where-Object { $_.AuditData -like "*RiskState*" }
    
    # 2. 大量のファイルダウンロード
    Write-Host "大量のファイルダウンロードを検索中..." -ForegroundColor Yellow
    $downloads = Search-UnifiedAuditLog -StartDate $startDate -EndDate $endDate -Operations "FileDownloaded"
    $suspiciousDownloads = $downloads | Group-Object UserIds | Where-Object { $_.Count -gt 50 }
    
    # 3. 管理者権限の変更
    Write-Host "管理者権限の変更を検索中..." -ForegroundColor Yellow
    $adminChanges = Search-UnifiedAuditLog -StartDate $startDate -EndDate $endDate -Operations "Add member to role", "Remove member from role"
    
    # 4. 外部共有の活動
    Write-Host "外部共有の活動を検索中..." -ForegroundColor Yellow
    $externalSharing = Search-UnifiedAuditLog -StartDate $startDate -EndDate $endDate -Operations "SharingInvitationCreated", "AnonymousLinkCreated"
    
    # レポートの生成
    $report = @"
疑わしい活動レポート
生成日時: $(Get-Date)
対象期間: $($startDate.ToString('yyyy-MM-dd')) ～ $($endDate.ToString('yyyy-MM-dd'))

1. 異常なサインイン: $($suspiciousSignins.Count) 件
2. 大量ダウンロード: $($suspiciousDownloads.Count) ユーザー
3. 管理者権限変更: $($adminChanges.Count) 件
4. 外部共有: $($externalSharing.Count) 件
"@
    
    $report | Out-File -FilePath "$OutputPath\SuspiciousActivity_$(Get-Date -Format 'yyyyMMdd').txt"
    
    # 詳細データの出力
    $suspiciousSignins | Export-Csv -Path "$OutputPath\SuspiciousSignins.csv" -NoTypeInformation
    $suspiciousDownloads | Export-Csv -Path "$OutputPath\SuspiciousDownloads.csv" -NoTypeInformation
    $adminChanges | Export-Csv -Path "$OutputPath\AdminChanges.csv" -NoTypeInformation
    $externalSharing | Export-Csv -Path "$OutputPath\ExternalSharing.csv" -NoTypeInformation
    
    Write-Host "疑わしい活動レポート生成完了: $OutputPath" -ForegroundColor Green
}

# 実行例
Find-SuspiciousActivity -Days 30 -OutputPath "C:\SecurityAudit\"
```

### 6.3.3 Microsoft Sentinel との連携

#### Microsoft Sentinel の基本設定

**Microsoft Sentinel**は、AI を活用したセキュリティ情報およびイベント管理（SIEM）ソリューションです。

**📋 実践：Microsoft Sentinel の基本設定**

**Azure portal での設定：**
1. **Azure portal** > **Microsoft Sentinel**
2. **新しいワークスペース** をクリック
3. **Log Analytics ワークスペース** を作成
4. **Microsoft Sentinel** を追加

**データコネクタの設定：**
```
推奨データコネクタ:
□ Office 365 (監査ログ)
□ Azure Activity (Azure アクティビティ)
□ Azure AD (認証ログ)
□ Microsoft Defender for Office 365
□ Microsoft Defender for Cloud
□ Microsoft Defender for Identity
```

#### カスタム検出ルールの作成

**📋 実践：カスタム検出ルールの作成**

**KQL（Kusto Query Language）での検出ルール例：**

```kusto
// 複数回の失敗ログイン後の成功ログイン
SigninLogs
| where TimeGenerated > ago(1h)
| where ResultType != "0"
| summarize FailedCount = count() by UserPrincipalName, bin(TimeGenerated, 5m)
| where FailedCount >= 5
| join (
    SigninLogs
    | where TimeGenerated > ago(1h)
    | where ResultType == "0"
) on UserPrincipalName
| project UserPrincipalName, FailedCount, SuccessTime = TimeGenerated1, IPAddress
```

```kusto
// 異常な大量ファイルダウンロード
OfficeActivity
| where TimeGenerated > ago(24h)
| where Operation == "FileDownloaded"
| summarize DownloadCount = count() by UserId, bin(TimeGenerated, 1h)
| where DownloadCount > 100
| project UserId, DownloadCount, TimeGenerated
```

#### アラートとレスポンス

**📋 実践：アラートとレスポンス**

**自動応答の設定：**
```
アラート: 大量ファイルダウンロード
条件: 1時間に100ファイル以上のダウンロード
自動応答:
1. ユーザーアカウントの一時停止
2. 管理者への通知
3. インシデントの作成
4. フォレンジック調査の開始
```

## 6.4 セキュリティ運用の自動化

### 6.4.1 PowerShell による自動化

#### 定期的なセキュリティチェック

**📋 実践：セキュリティチェックの自動化**

```powershell
# 定期セキュリティチェック スクリプト
function Invoke-SecurityCheck {
    param(
        [string]$OutputPath = "C:\SecurityReports\",
        [string]$EmailTo = "security@company.com"
    )
    
    $checkDate = Get-Date
    $reportPath = "$OutputPath\SecurityCheck_$($checkDate.ToString('yyyyMMdd')).html"
    
    # 出力フォルダの作成
    if (-not (Test-Path $OutputPath)) {
        New-Item -ItemType Directory -Path $OutputPath -Force
    }
    
    # HTML レポートの開始
    $html = @"
<!DOCTYPE html>
<html>
<head>
    <title>セキュリティチェックレポート</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .header { color: #2c3e50; border-bottom: 2px solid #3498db; padding-bottom: 10px; }
        .section { margin: 20px 0; }
        .good { color: #27ae60; }
        .warning { color: #f39c12; }
        .error { color: #e74c3c; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <h1 class="header">セキュリティチェックレポート</h1>
    <p>生成日時: $($checkDate.ToString('yyyy年MM月dd日 HH:mm:ss'))</p>
"@
    
    # 1. MFA有効化状況
    Write-Host "MFA有効化状況をチェック中..." -ForegroundColor Yellow
    $html += "<div class='section'><h2>多要素認証（MFA）有効化状況</h2>"
    
    try {
        Connect-MgGraph -Scopes "User.Read.All" -NoWelcome
        $users = Get-MgUser -All
        $mfaEnabled = 0
        $mfaDisabled = 0
        
        foreach ($user in $users) {
            $authMethods = Get-MgUserAuthenticationMethod -UserId $user.Id
            if ($authMethods) {
                $mfaEnabled++
            } else {
                $mfaDisabled++
            }
        }
        
        $mfaPercentage = [math]::Round(($mfaEnabled / $users.Count) * 100, 2)
        
        if ($mfaPercentage -ge 95) {
            $html += "<p class='good'>✓ MFA有効化率: $mfaPercentage% （良好）</p>"
        } elseif ($mfaPercentage -ge 80) {
            $html += "<p class='warning'>⚠ MFA有効化率: $mfaPercentage% （要注意）</p>"
        } else {
            $html += "<p class='error'>✗ MFA有効化率: $mfaPercentage% （要改善）</p>"
        }
        
        $html += "<p>MFA有効: $mfaEnabled ユーザー</p>"
        $html += "<p>MFA無効: $mfaDisabled ユーザー</p>"
    }
    catch {
        $html += "<p class='error'>MFA状況の取得に失敗しました: $($_.Exception.Message)</p>"
    }
    
    $html += "</div>"
    
    # 2. 条件付きアクセスポリシー
    Write-Host "条件付きアクセスポリシーをチェック中..." -ForegroundColor Yellow
    $html += "<div class='section'><h2>条件付きアクセスポリシー</h2>"
    
    try {
        $policies = Get-MgIdentityConditionalAccessPolicy
        $enabledPolicies = $policies | Where-Object { $_.State -eq "enabled" }
        
        if ($enabledPolicies.Count -gt 0) {
            $html += "<p class='good'>✓ 有効な条件付きアクセスポリシー: $($enabledPolicies.Count) 件</p>"
            $html += "<table><tr><th>ポリシー名</th><th>状態</th></tr>"
            foreach ($policy in $enabledPolicies) {
                $html += "<tr><td>$($policy.DisplayName)</td><td>$($policy.State)</td></tr>"
            }
            $html += "</table>"
        } else {
            $html += "<p class='error'>✗ 有効な条件付きアクセスポリシーが見つかりません</p>"
        }
    }
    catch {
        $html += "<p class='error'>条件付きアクセスポリシーの取得に失敗しました: $($_.Exception.Message)</p>"
    }
    
    $html += "</div>"
    
    # 3. 最近のサインイン失敗
    Write-Host "最近のサインイン失敗をチェック中..." -ForegroundColor Yellow
    $html += "<div class='section'><h2>最近のサインイン失敗（過去24時間）</h2>"
    
    try {
        $signInLogs = Get-MgAuditLogSignIn -Top 1000 | Where-Object { 
            $_.CreatedDateTime -gt (Get-Date).AddDays(-1) -and 
            $_.Status.ErrorCode -ne 0 
        }
        
        if ($signInLogs.Count -gt 0) {
            $failuresByUser = $signInLogs | Group-Object UserPrincipalName | Sort-Object Count -Descending | Select-Object -First 10
            
            if ($failuresByUser[0].Count -gt 10) {
                $html += "<p class='warning'>⚠ 注意: 複数回のサインイン失敗が検出されました</p>"
            } else {
                $html += "<p class='good'>✓ 異常なサインイン失敗は検出されていません</p>"
            }
            
            $html += "<table><tr><th>ユーザー</th><th>失敗回数</th></tr>"
            foreach ($failure in $failuresByUser) {
                $html += "<tr><td>$($failure.Name)</td><td>$($failure.Count)</td></tr>"
            }
            $html += "</table>"
        } else {
            $html += "<p class='good'>✓ 過去24時間でサインイン失敗はありません</p>"
        }
    }
    catch {
        $html += "<p class='error'>サインインログの取得に失敗しました: $($_.Exception.Message)</p>"
    }
    
    $html += "</div>"
    
    # 4. 外部共有の状況
    Write-Host "外部共有の状況をチェック中..." -ForegroundColor Yellow
    $html += "<div class='section'><h2>外部共有の状況</h2>"
    
    try {
        # SharePoint Online 管理シェルが必要
        # この部分は実装に応じて調整
        $html += "<p>外部共有の詳細チェックは管理者が手動で確認してください</p>"
    }
    catch {
        $html += "<p class='error'>外部共有状況の取得に失敗しました: $($_.Exception.Message)</p>"
    }
    
    $html += "</div>"
    
    # レポートの終了
    $html += @"
    <div class='section'>
        <h2>推奨アクション</h2>
        <ul>
            <li>MFA有効化率が95%を下回る場合は、未設定ユーザーに対して設定を促してください</li>
            <li>条件付きアクセスポリシーが設定されていない場合は、基本的なポリシーを設定してください</li>
            <li>異常なサインイン失敗が検出された場合は、該当ユーザーに確認してください</li>
            <li>外部共有が過度に設定されている場合は、ガバナンスポリシーの見直しを検討してください</li>
        </ul>
    </div>
    <p><em>このレポートは自動生成されました。詳細な調査が必要な場合は、セキュリティ管理者にお問い合わせください。</em></p>
</body>
</html>
"@
    
    # ファイルに保存
    $html | Out-File -FilePath $reportPath -Encoding UTF8
    
    # メール送信（オプション）
    if ($EmailTo) {
        try {
            $subject = "セキュリティチェックレポート - $($checkDate.ToString('yyyy-MM-dd'))"
            $body = "セキュリティチェックレポートを添付いたします。詳細は添付ファイルをご確認ください。"
            
            # Send-MailMessage の実装は環境に応じて調整
            # Send-MailMessage -To $EmailTo -Subject $subject -Body $body -Attachments $reportPath
            
            Write-Host "レポートが生成されました: $reportPath" -ForegroundColor Green
        }
        catch {
            Write-Host "メール送信に失敗しました: $($_.Exception.Message)" -ForegroundColor Red
        }
    }
    
    Write-Host "セキュリティチェック完了: $reportPath" -ForegroundColor Green
}

# 実行例
Invoke-SecurityCheck -OutputPath "C:\SecurityReports\" -EmailTo "security@company.com"
```

### 6.4.2 インシデント対応の自動化

#### 自動インシデント対応

**📋 実践：自動インシデント対応**

```powershell
# インシデント対応自動化スクリプト
function Invoke-SecurityIncidentResponse {
    param(
        [string]$IncidentType,
        [string]$UserId,
        [string]$Details
    )
    
    $incidentId = New-Guid
    $timestamp = Get-Date
    
    Write-Host "セキュリティインシデント対応開始" -ForegroundColor Red
    Write-Host "インシデントID: $incidentId" -ForegroundColor Yellow
    Write-Host "種別: $IncidentType" -ForegroundColor Yellow
    Write-Host "対象ユーザー: $UserId" -ForegroundColor Yellow
    
    switch ($IncidentType) {
        "SuspiciousLogin" {
            # 疑わしいログインの対応
            Write-Host "疑わしいログインに対する対応を実行中..." -ForegroundColor Yellow
            
            # 1. ユーザーセッションの無効化
            try {
                Revoke-MgUserSignInSession -UserId $UserId
                Write-Host "✓ ユーザーセッションを無効化しました" -ForegroundColor Green
            }
            catch {
                Write-Host "✗ セッション無効化に失敗: $($_.Exception.Message)" -ForegroundColor Red
            }
            
            # 2. 条件付きアクセスによる一時ブロック
            # 実装は条件付きアクセスポリシーの設定に依存
            
            # 3. 管理者への通知
            $subject = "セキュリティアラート: 疑わしいログイン"
            $body = @"
セキュリティインシデントが発生しました。

インシデントID: $incidentId
種別: 疑わしいログイン
対象ユーザー: $UserId
発生時刻: $timestamp
詳細: $Details

対応済み:
- ユーザーセッションの無効化

要対応:
- ユーザーへの確認
- パスワードリセットの検討
- 追加調査の実施
"@
            
            # メール送信処理（環境に応じて実装）
            Write-Host "✓ 管理者への通知を送信しました" -ForegroundColor Green
        }
        
        "MassDownload" {
            # 大量ダウンロードの対応
            Write-Host "大量ダウンロードに対する対応を実行中..." -ForegroundColor Yellow
            
            # 1. ユーザーの一時停止
            try {
                Update-MgUser -UserId $UserId -AccountEnabled:$false
                Write-Host "✓ ユーザーアカウントを一時停止しました" -ForegroundColor Green
            }
            catch {
                Write-Host "✗ アカウント停止に失敗: $($_.Exception.Message)" -ForegroundColor Red
            }
            
            # 2. 管理者への緊急通知
            $subject = "緊急: 大量ファイルダウンロード検出"
            $body = @"
緊急セキュリティインシデントが発生しました。

インシデントID: $incidentId
種別: 大量ファイルダウンロード
対象ユーザー: $UserId
発生時刻: $timestamp
詳細: $Details

対応済み:
- ユーザーアカウントの一時停止

要対応:
- 緊急調査の実施
- 影響範囲の確認
- 法的対応の検討
"@
            
            Write-Host "✓ 緊急通知を送信しました" -ForegroundColor Green
        }
        
        "PrivilegeEscalation" {
            # 権限昇格の対応
            Write-Host "権限昇格に対する対応を実行中..." -ForegroundColor Yellow
            
            # 1. 変更の記録
            $auditLog = @{
                IncidentId = $incidentId
                Type = $IncidentType
                UserId = $UserId
                Timestamp = $timestamp
                Details = $Details
                Response = "Privilege escalation detected and logged"
            }
            
            # 2. 管理者への即座の通知
            Write-Host "✓ 権限昇格を記録し、管理者に通知しました" -ForegroundColor Green
        }
        
        default {
            Write-Host "未知のインシデントタイプ: $IncidentType" -ForegroundColor Red
        }
    }
    
    # インシデントログの記録
    $logEntry = @{
        IncidentId = $incidentId
        Timestamp = $timestamp
        Type = $IncidentType
        UserId = $UserId
        Details = $Details
        Status = "Responded"
    }
    
    $logPath = "C:\SecurityLogs\Incidents_$((Get-Date).ToString('yyyyMM')).json"
    
    # ログディレクトリの作成
    if (-not (Test-Path (Split-Path $logPath -Parent))) {
        New-Item -ItemType Directory -Path (Split-Path $logPath -Parent) -Force
    }
    
    # ログの追記
    $logEntry | ConvertTo-Json | Add-Content -Path $logPath
    
    Write-Host "インシデント対応完了: $incidentId" -ForegroundColor Green
}

# 使用例
# Invoke-SecurityIncidentResponse -IncidentType "SuspiciousLogin" -UserId "user@contoso.com" -Details "異常な地域からのログイン"
```

## 6.5 継続的なセキュリティ改善

### 6.5.1 セキュリティ指標の監視

#### KPI（重要業績評価指標）の設定

**📋 実践：セキュリティKPIの設定**

```powershell
# セキュリティKPI測定スクリプト
function Measure-SecurityKPIs {
    param(
        [string]$OutputPath = "C:\SecurityReports\KPI\"
    )
    
    $measurementDate = Get-Date
    $reportPath = "$OutputPath\SecurityKPI_$($measurementDate.ToString('yyyyMMdd')).json"
    
    # 出力フォルダの作成
    if (-not (Test-Path $OutputPath)) {
        New-Item -ItemType Directory -Path $OutputPath -Force
    }
    
    Write-Host "セキュリティKPIを測定中..." -ForegroundColor Yellow
    
    $kpis = @{}
    
    try {
        # 1. MFA導入率
        Connect-MgGraph -Scopes "User.Read.All" -NoWelcome
        $users = Get-MgUser -All
        $mfaEnabledUsers = 0
        
        foreach ($user in $users) {
            $authMethods = Get-MgUserAuthenticationMethod -UserId $user.Id
            if ($authMethods.Count -gt 1) {  # パスワード以外の認証方法がある
                $mfaEnabledUsers++
            }
        }
        
        $kpis["MFA_Adoption_Rate"] = [math]::Round(($mfaEnabledUsers / $users.Count) * 100, 2)
        
        # 2. 条件付きアクセスポリシーカバレッジ
        $policies = Get-MgIdentityConditionalAccessPolicy
        $enabledPolicies = $policies | Where-Object { $_.State -eq "enabled" }
        $kpis["Conditional_Access_Policies"] = $enabledPolicies.Count
        
        # 3. セキュリティインシデント数（過去30日）
        $incidentLogs = Get-ChildItem -Path "C:\SecurityLogs\Incidents_*.json" -ErrorAction SilentlyContinue
        $recentIncidents = 0
        
        foreach ($logFile in $incidentLogs) {
            $incidents = Get-Content $logFile | ConvertFrom-Json
            $recentIncidents += ($incidents | Where-Object { 
                [datetime]$_.Timestamp -gt (Get-Date).AddDays(-30) 
            }).Count
        }
        
        $kpis["Security_Incidents_30d"] = $recentIncidents
        
        # 4. パスワードリセット率（過去30日）
        $passwordResets = Search-UnifiedAuditLog -StartDate (Get-Date).AddDays(-30) -EndDate (Get-Date) -Operations "Reset user password"
        $kpis["Password_Resets_30d"] = $passwordResets.Count
        
        # 5. 外部共有率
        $externalSharing = Search-UnifiedAuditLog -StartDate (Get-Date).AddDays(-30) -EndDate (Get-Date) -Operations "SharingInvitationCreated"
        $kpis["External_Sharing_30d"] = $externalSharing.Count
        
        # 6. セキュリティアラート対応時間（平均）
        # 実装は監視システムに依存
        $kpis["Alert_Response_Time_Hours"] = 2.5  # 例：平均2.5時間
        
        # 7. セキュリティトレーニング完了率
        # 実装は研修システムに依存
        $kpis["Security_Training_Completion"] = 87.5  # 例：87.5%
        
        Write-Host "✓ KPI測定完了" -ForegroundColor Green
        
    }
    catch {
        Write-Host "✗ KPI測定エラー: $($_.Exception.Message)" -ForegroundColor Red
    }
    
    # 測定結果の保存
    $kpiReport = @{
        MeasurementDate = $measurementDate
        KPIs = $kpis
        Targets = @{
            MFA_Adoption_Rate = 95
            Conditional_Access_Policies = 5
            Security_Incidents_30d = 0
            Password_Resets_30d = 10
            External_Sharing_30d = 50
            Alert_Response_Time_Hours = 2
            Security_Training_Completion = 90
        }
    }
    
    $kpiReport | ConvertTo-Json -Depth 3 | Out-File -FilePath $reportPath -Encoding UTF8
    
    # コンソール出力
    Write-Host "`n=== セキュリティKPIレポート ===" -ForegroundColor Cyan
    Write-Host "測定日: $($measurementDate.ToString('yyyy-MM-dd HH:mm:ss'))" -ForegroundColor White
    Write-Host "MFA導入率: $($kpis['MFA_Adoption_Rate'])% (目標: 95%)" -ForegroundColor White
    Write-Host "条件付きアクセスポリシー数: $($kpis['Conditional_Access_Policies']) (目標: 5)" -ForegroundColor White
    Write-Host "セキュリティインシデント数: $($kpis['Security_Incidents_30d']) (目標: 0)" -ForegroundColor White
    Write-Host "パスワードリセット数: $($kpis['Password_Resets_30d']) (目標: 10以下)" -ForegroundColor White
    Write-Host "外部共有数: $($kpis['External_Sharing_30d']) (目標: 50以下)" -ForegroundColor White
    
    Write-Host "`nKPIレポート保存: $reportPath" -ForegroundColor Green
}

# 実行例
Measure-SecurityKPIs -OutputPath "C:\SecurityReports\KPI\"
```

### 6.5.2 セキュリティ文化の醸成

#### ユーザー教育とトレーニング

**段階的なセキュリティ教育プログラム：**

```
レベル1: 基礎教育（全従業員対象）
□ パスワードの重要性
□ フィッシング詐欺の識別
□ 多要素認証の使用方法
□ 安全な情報共有

レベル2: 管理者教育（管理職対象）
□ セキュリティポリシーの理解
□ インシデント対応の基本
□ リスク評価の方法
□ 法的要件とコンプライアンス

レベル3: 技術者教育（IT部門対象）
□ 高度な脅威の対処法
□ セキュリティツールの活用
□ インシデント調査の技法
□ セキュリティ設定の最適化
```

### 6.5.3 定期的なセキュリティ評価

#### セキュリティ評価チェックリスト

**📋 実践：定期的なセキュリティ評価**

```markdown
# 四半期セキュリティ評価チェックリスト

## 1. 技術的統制

### 1.1 アクセス制御
- [ ] MFA有効化率 95% 以上
- [ ] 条件付きアクセスポリシーの有効性確認
- [ ] 特権アカウントの最小限使用
- [ ] 定期的なアクセス権限レビュー

### 1.2 データ保護
- [ ] 機密度ラベルの適切な適用
- [ ] DLPポリシーの有効性確認
- [ ] 暗号化状況の確認
- [ ] バックアップの完全性確認

### 1.3 監視・検出
- [ ] 監査ログの収集・分析
- [ ] セキュリティアラートの対応状況
- [ ] 異常検知システムの有効性
- [ ] インシデント対応時間の測定

## 2. 管理的統制

### 2.1 ポリシー・手順
- [ ] セキュリティポリシーの更新
- [ ] 手順書の最新化
- [ ] 役割と責任の明確化
- [ ] 承認プロセスの有効性

### 2.2 教育・訓練
- [ ] セキュリティ研修の実施状況
- [ ] フィッシング訓練の結果
- [ ] インシデント対応訓練
- [ ] 新入社員向けオリエンテーション

### 2.3 ベンダー管理
- [ ] 第三者のセキュリティ評価
- [ ] 契約におけるセキュリティ条項
- [ ] データ処理委託先の監査
- [ ] クラウドサービスの設定確認

## 3. 物理的統制

### 3.1 デバイス管理
- [ ] モバイルデバイスの管理状況
- [ ] リモートワーク環境の安全性
- [ ] 紛失・盗難時の対応手順
- [ ] デバイスの暗号化状況

### 3.2 環境セキュリティ
- [ ] オフィスのアクセス制御
- [ ] 機密文書の管理
- [ ] 廃棄処理の適切性
- [ ] 清掃・立入規則の遵守

## 4. 法的・規制要件

### 4.1 コンプライアンス
- [ ] 個人情報保護法の遵守
- [ ] 業界固有の規制要件
- [ ] 国際的な規制への対応
- [ ] 内部監査の実施

### 4.2 契約・法務
- [ ] 利用規約の更新
- [ ] プライバシー・ポリシーの見直し
- [ ] データ処理同意の取得
- [ ] 法的通知の対応

## 5. 改善計画

### 5.1 前回評価からの改善
- [ ] 前回指摘事項の対応確認
- [ ] 改善計画の進捗確認
- [ ] 効果測定の実施
- [ ] 追加対策の検討

### 5.2 今回の改善事項
- [ ] 新たに発見された課題
- [ ] 優先度の設定
- [ ] 責任者の割り当て
- [ ] 実施スケジュールの策定

## 評価結果まとめ

### 総合評価
- 優秀（90-100点）: [ ]
- 良好（80-89点）: [ ]
- 普通（70-79点）: [ ]
- 要改善（60-69点）: [ ]
- 不適切（60点未満）: [ ]

### 主要な改善点
1. ________________________________
2. ________________________________
3. ________________________________

### 次回評価予定日
実施予定日: ____年____月____日
担当者: ________________________________
```

## まとめ

この章では、企業レベルのセキュリティ管理とコンプライアンス対応について学びました。

### 重要なポイント

1. **高度なセキュリティ機能の実装**
   - Microsoft Defender for Office 365の活用
   - Azure Security Center による監視
   - Identity Protection によるリスク管理

2. **コンプライアンス管理**
   - データ保護とプライバシー対応
   - 情報ガバナンスの実装
   - 法的要件への対応

3. **監査とログ管理**
   - 統合監査ログの活用
   - 監査ログの分析と対応
   - Microsoft Sentinel との連携

4. **セキュリティ運用の自動化**
   - 定期的なセキュリティチェック
   - インシデント対応の自動化
   - PowerShell による効率化

5. **継続的な改善**
   - セキュリティ指標の監視
   - 定期的な評価の実施
   - セキュリティ文化の醸成

セキュリティとコンプライアンスは一度設定すれば終わりではなく、継続的な監視と改善が必要です。技術の進歩や脅威の変化に対応し、組織全体でセキュリティ意識を高めることが重要です。

次章では、日常的な運用業務とトラブルシューティングについて、実践的な運用ノウハウを詳しく学んでいきます。