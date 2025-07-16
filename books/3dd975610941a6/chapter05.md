---
title: "第5章: PowerShell入門〜クラウドリソースの効率的な管理〜"
---

# はじめに

前章では、Microsoft Azureの基本操作について学びました。この章では、PowerShellを使用したクラウドリソースの効率的な管理について、自動化と運用の観点から実践的に学んでいきます。

PowerShellは、Microsoft 365やAzureの管理を自動化する強力なツールです。新人エンジニアの方でも理解しやすいよう、基本概念から実用的なスクリプトまで段階的に説明していきます。

## 5.1 PowerShell環境の準備

### 5.1.1 PowerShellとは

#### PowerShellの役割と利点

**PowerShell**は、Microsoftが開発したコマンドラインツールおよびスクリプト言語です。

**PowerShellの特徴：**
- **オブジェクト指向**: テキストではなくオブジェクトを操作
- **豊富なコマンドレット**: 多様な操作を簡単に実行
- **パイプライン**: コマンドを連結して複雑な処理を実現
- **スクリプト機能**: 繰り返し作業の自動化
- **クロスプラットフォーム**: Windows、Linux、macOS で動作

#### コマンドライン vs GUIの使い分け

**GUI（グラフィカル・ユーザー・インターフェース）**
- **メリット**: 直感的、学習コストが低い
- **デメリット**: 大量処理に不向き、自動化が困難

**CLI（コマンドライン・インターフェース）**
- **メリット**: 高速処理、自動化可能、正確性
- **デメリット**: 学習コストが高い、コマンドの記憶が必要

**📊 手動作業 vs PowerShell自動化の比較**

```
┌─────────────────────┬─────────────────────┐
│     手動作業        │  PowerShell自動化   │
├─────────────────────┼─────────────────────┤
│ 100ユーザー作成     │ 100ユーザー作成     │
│ 時間: 8時間         │ 時間: 10分          │
│ エラー率: 5%        │ エラー率: 0%        │
│ 再実行: 8時間       │ 再実行: 10分        │
└─────────────────────┴─────────────────────┘
```

### 5.1.2 PowerShell環境の構築

#### PowerShell 7の導入

**PowerShell 7**は、最新のクロスプラットフォーム版PowerShellです。

**PowerShell 7の利点：**
- **最新機能**: 新しい機能とパフォーマンス向上
- **互換性**: Windows PowerShell 5.1との互換性
- **統一性**: 複数のプラットフォームで同じ動作
- **長期サポート**: 3年間のサポート

**📋 実践：PowerShell 7の導入手順**

**Windows環境での導入：**
1. **Microsoft Store**から「PowerShell」を検索
2. **PowerShell**をインストール
3. **スタートメニュー**から「PowerShell 7」を起動

**または、公式サイトからダウンロード：**
1. `https://github.com/PowerShell/PowerShell/releases`
2. **Assets**から`PowerShell-7.x.x-win-x64.msi`をダウンロード
3. インストーラーを実行

#### 必要なモジュールのインストール

**主要モジュール：**
- **Az**: Azure管理用モジュール
- **ExchangeOnlineManagement**: Exchange Online管理用
- **MicrosoftTeams**: Microsoft Teams管理用
- **Microsoft.Graph**: Microsoft Graph API用

**📋 実践：モジュールのインストール**

```powershell
# Azure PowerShell モジュール
Install-Module -Name Az -AllowClobber -Force

# Exchange Online モジュール
Install-Module -Name ExchangeOnlineManagement -Force

# Microsoft Teams モジュール
Install-Module -Name MicrosoftTeams -Force

# Microsoft Graph モジュール
Install-Module -Name Microsoft.Graph -Force
```

### 5.1.3 基本的なPowerShellコマンド

#### 基本的なコマンドレット

**PowerShell**では、コマンドを「コマンドレット」と呼びます。

**重要なコマンドレット：**
- **Get-Help**: ヘルプ情報の取得
- **Get-Command**: 利用可能なコマンドの一覧
- **Get-Member**: オブジェクトのプロパティとメソッド
- **Where-Object**: オブジェクトのフィルタリング
- **Select-Object**: オブジェクトのプロパティ選択

**📋 実践：はじめてのPowerShellコマンド**

```powershell
# ヘルプの確認
Get-Help Get-Process

# 実行中のプロセス一覧
Get-Process

# 特定のプロセスを検索
Get-Process | Where-Object {$_.Name -like "chrome*"}

# CPU使用率順でソート
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
```

#### パイプラインの概念

**パイプライン**は、複数のコマンドを「|」で連結する機能です。

**パイプラインの例：**
```powershell
# サービス一覧 → 実行中のみ → 名前順でソート
Get-Service | Where-Object {$_.Status -eq "Running"} | Sort-Object Name
```

**パイプラインの利点：**
- **効率的**: 一度に複数の処理を実行
- **読みやすい**: 処理の流れが分かりやすい
- **再利用可能**: 部分的な組み合わせが可能

## 5.2 クラウドサービスへの接続

### 5.2.1 Azure PowerShellでの認証と接続

#### Azure PowerShellモジュールの基本

**Az モジュール**は、Azure管理用のPowerShellモジュールです。

**主要なコマンドレット：**
- **Connect-AzAccount**: Azureへの接続
- **Get-AzSubscription**: サブスクリプション一覧
- **Set-AzContext**: サブスクリプションの選択
- **Get-AzResourceGroup**: リソースグループ一覧

**📋 実践：Azure PowerShellの接続手順**

```powershell
# Azure への接続
Connect-AzAccount

# サブスクリプション一覧の確認
Get-AzSubscription

# 特定のサブスクリプションを選択
Set-AzContext -SubscriptionId "12345678-1234-1234-1234-123456789012"

# 接続状態の確認
Get-AzContext
```

#### 認証方法の選択

**認証方法：**
- **インタラクティブ**: ブラウザでのログイン
- **サービスプリンシパル**: 自動化用の認証
- **マネージドID**: Azure内での認証

**インタラクティブ認証：**
```powershell
# デフォルトの認証方法
Connect-AzAccount

# 特定のテナントを指定
Connect-AzAccount -TenantId "12345678-1234-1234-1234-123456789012"
```

**サービスプリンシパル認証：**
```powershell
# 認証情報の作成
$credential = Get-Credential
Connect-AzAccount -ServicePrincipal -Credential $credential -TenantId "tenant-id"
```

### 5.2.2 Exchange Online PowerShellでの接続

#### Exchange Online PowerShellの基本

**ExchangeOnlineManagement モジュール**は、Exchange Online管理用のモジュールです。

**主要なコマンドレット：**
- **Connect-ExchangeOnline**: Exchange Online への接続
- **Get-Mailbox**: メールボックス一覧
- **Get-DistributionGroup**: 配布グループ一覧
- **Get-TransportRule**: メールフロールール一覧

**📋 実践：Exchange Online PowerShellの接続手順**

```powershell
# Exchange Online への接続
Connect-ExchangeOnline

# または、特定のユーザーで接続
Connect-ExchangeOnline -UserPrincipalName admin@contoso.com

# 接続状態の確認
Get-ConnectionInformation

# 切断
Disconnect-ExchangeOnline
```

#### 権限の確認方法

**必要な権限：**
- **Exchange Administrator**: 全ての Exchange Online 管理
- **Global Administrator**: 全ての Microsoft 365 管理
- **View-Only Organization Management**: 読み取り専用

**権限の確認：**
```powershell
# 現在のユーザーの権限確認
Get-ManagementRole | Where-Object {$_.Name -like "*Admin*"}

# 特定のユーザーの権限確認
Get-ManagementRoleAssignment -RoleAssignee "user@contoso.com"
```

### 5.2.3 Microsoft Teams PowerShellでの接続

#### Microsoft Teams PowerShellの基本

**MicrosoftTeams モジュール**は、Microsoft Teams管理用のモジュールです。

**主要なコマンドレット：**
- **Connect-MicrosoftTeams**: Teams への接続
- **Get-Team**: チーム一覧
- **Get-TeamChannel**: チャネル一覧
- **Get-CsTeamsMeetingPolicy**: 会議ポリシー

**📋 実践：Microsoft Teams PowerShellの接続手順**

```powershell
# Microsoft Teams への接続
Connect-MicrosoftTeams

# チーム一覧の確認
Get-Team

# 特定のチームの詳細確認
Get-Team -DisplayName "営業部"

# 切断
Disconnect-MicrosoftTeams
```

#### 基本的な情報取得コマンド

**チーム情報の取得：**
```powershell
# すべてのチーム
Get-Team

# 特定のチームのチャネル
Get-TeamChannel -GroupId "team-group-id"

# チームメンバー
Get-TeamUser -GroupId "team-group-id"
```

## 5.3 基本的な管理スクリプト

### 5.3.1 ユーザー情報の取得と管理

#### ユーザー一覧の取得

**Microsoft Graph を使用したユーザー管理：**

```powershell
# Microsoft Graph への接続
Connect-MgGraph -Scopes "User.Read.All"

# すべてのユーザー一覧
Get-MgUser

# 特定の条件でフィルタリング
Get-MgUser -Filter "Department eq '営業部'"

# 詳細情報の取得
Get-MgUser -Select "DisplayName,UserPrincipalName,Department,JobTitle"
```

#### ライセンス情報の確認

**📋 実践：ユーザー一覧の取得スクリプト**

```powershell
# ライセンス情報付きユーザー一覧
$users = Get-MgUser -Select "DisplayName,UserPrincipalName,AssignedLicenses"

foreach ($user in $users) {
    Write-Host "ユーザー: $($user.DisplayName)"
    Write-Host "メール: $($user.UserPrincipalName)"
    
    if ($user.AssignedLicenses) {
        Write-Host "ライセンス: 割り当てあり"
    } else {
        Write-Host "ライセンス: 割り当てなし"
    }
    Write-Host "------------------------"
}
```

#### 新規ユーザー作成の自動化

**📋 サンプルスクリプト：新規ユーザー作成の自動化**

```powershell
# 新規ユーザー作成関数
function New-CompanyUser {
    param(
        [string]$DisplayName,
        [string]$UserPrincipalName,
        [string]$Department,
        [string]$JobTitle
    )
    
    # パスワードの生成
    $PasswordProfile = @{
        Password = "TempPassword123!"
        ForceChangePasswordNextSignIn = $true
    }
    
    # ユーザー作成
    $newUser = New-MgUser -DisplayName $DisplayName `
                          -UserPrincipalName $UserPrincipalName `
                          -Department $Department `
                          -JobTitle $JobTitle `
                          -PasswordProfile $PasswordProfile `
                          -AccountEnabled $true
    
    Write-Host "ユーザー作成完了: $($newUser.DisplayName)"
    return $newUser
}

# 使用例
New-CompanyUser -DisplayName "田中太郎" `
                -UserPrincipalName "tanaka@contoso.com" `
                -Department "営業部" `
                -JobTitle "営業担当"
```

### 5.3.2 Exchange Onlineの基本管理

#### メールボックス情報の取得

**基本的なメールボックス管理：**

```powershell
# Exchange Online への接続
Connect-ExchangeOnline

# すべてのメールボックス
Get-Mailbox

# 特定のメールボックスの詳細
Get-Mailbox -Identity "user@contoso.com"

# メールボックスの統計情報
Get-MailboxStatistics -Identity "user@contoso.com"
```

#### 配布リストの管理

**📋 実践：メールボックス情報の取得**

```powershell
# 容量使用率レポート
$mailboxes = Get-Mailbox -ResultSize Unlimited

foreach ($mailbox in $mailboxes) {
    $stats = Get-MailboxStatistics -Identity $mailbox.Identity
    
    $usagePercent = if ($stats.TotalItemSize -and $mailbox.ProhibitSendQuota) {
        ($stats.TotalItemSize.Value.ToBytes() / $mailbox.ProhibitSendQuota.Value.ToBytes()) * 100
    } else { 0 }
    
    [PSCustomObject]@{
        DisplayName = $mailbox.DisplayName
        PrimarySmtpAddress = $mailbox.PrimarySmtpAddress
        TotalItemSize = $stats.TotalItemSize
        ItemCount = $stats.ItemCount
        UsagePercent = [math]::Round($usagePercent, 2)
    }
}
```

#### メール設定の一括変更

**📋 サンプルスクリプト：メール設定の一括変更**

```powershell
# 部署別の自動返信設定
function Set-DepartmentAutoReply {
    param(
        [string]$Department,
        [string]$AutoReplyMessage
    )
    
    # 特定部署のユーザーを取得
    $users = Get-User -Filter "Department -eq '$Department'"
    
    foreach ($user in $users) {
        # 自動返信の設定
        Set-MailboxAutoReplyConfiguration -Identity $user.Identity `
                                         -AutoReplyState Enabled `
                                         -InternalMessage $AutoReplyMessage `
                                         -ExternalMessage $AutoReplyMessage
        
        Write-Host "自動返信設定完了: $($user.DisplayName)"
    }
}

# 使用例
Set-DepartmentAutoReply -Department "営業部" `
                       -Message "営業部では迅速な対応を心がけています。"
```

### 5.3.3 レポート作成の自動化

#### 利用状況レポートの自動生成

**Microsoft Graph を使用したレポート作成：**

```powershell
# Office 365 利用状況レポート
Connect-MgGraph -Scopes "Reports.Read.All"

# Teams 利用状況レポート
$teamsReport = Get-MgReportTeamsUserActivityUserDetail -Period D30
$teamsReport | Export-Csv -Path "Teams_Usage_Report.csv" -NoTypeInformation

# Exchange 利用状況レポート
$exchangeReport = Get-MgReportEmailActivityUserDetail -Period D30
$exchangeReport | Export-Csv -Path "Exchange_Usage_Report.csv" -NoTypeInformation
```

#### CSVファイルへの出力

**📋 実践：利用状況レポートの自動生成**

```powershell
# 総合利用状況レポート
function Generate-UsageReport {
    param(
        [string]$OutputPath = "C:\Reports\",
        [int]$Days = 30
    )
    
    # 出力フォルダの作成
    if (-not (Test-Path $OutputPath)) {
        New-Item -ItemType Directory -Path $OutputPath
    }
    
    # 日付の設定
    $reportDate = Get-Date -Format "yyyy-MM-dd"
    
    # ユーザー基本情報
    $users = Get-MgUser -Select "DisplayName,UserPrincipalName,Department,JobTitle,CreatedDateTime"
    $users | Export-Csv -Path "$OutputPath\Users_$reportDate.csv" -NoTypeInformation
    
    # ライセンス情報
    $licenses = Get-MgSubscribedSku
    $licenses | Export-Csv -Path "$OutputPath\Licenses_$reportDate.csv" -NoTypeInformation
    
    Write-Host "レポート生成完了: $OutputPath"
}

# 実行例
Generate-UsageReport -OutputPath "C:\Reports\" -Days 30
```

#### 定期レポート作成

**📋 サンプルスクリプト：定期レポート作成**

```powershell
# 月次レポート自動作成
function New-MonthlyReport {
    $reportDate = Get-Date -Format "yyyy-MM"
    $reportPath = "C:\Reports\Monthly_$reportDate.html"
    
    # HTML レポートの開始
    $html = @"
<!DOCTYPE html>
<html>
<head>
    <title>月次レポート - $reportDate</title>
    <style>
        body { font-family: Arial, sans-serif; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <h1>Microsoft 365 月次レポート</h1>
    <h2>作成日: $(Get-Date -Format 'yyyy年MM月dd日')</h2>
"@
    
    # ユーザー数の集計
    $totalUsers = (Get-MgUser).Count
    $html += "<h3>ユーザー統計</h3>"
    $html += "<p>総ユーザー数: $totalUsers</p>"
    
    # 部署別ユーザー数
    $departmentStats = Get-MgUser -Select "Department" | 
                      Group-Object Department | 
                      Sort-Object Count -Descending
    
    $html += "<h3>部署別ユーザー数</h3><table><tr><th>部署</th><th>ユーザー数</th></tr>"
    foreach ($dept in $departmentStats) {
        $html += "<tr><td>$($dept.Name)</td><td>$($dept.Count)</td></tr>"
    }
    $html += "</table>"
    
    # HTML の終了
    $html += "</body></html>"
    
    # ファイルに保存
    $html | Out-File -FilePath $reportPath -Encoding UTF8
    
    Write-Host "月次レポート作成完了: $reportPath"
}

# 実行例
New-MonthlyReport
```

## 5.4 Azure リソースの基本管理

### 5.4.1 Azureリソースの情報取得

#### サブスクリプションとリソースグループの確認

**基本的なリソース管理：**

```powershell
# Azure への接続
Connect-AzAccount

# サブスクリプション一覧
Get-AzSubscription

# リソースグループ一覧
Get-AzResourceGroup

# 特定のリソースグループの詳細
Get-AzResourceGroup -Name "rg-prod-web"

# リソースグループ内のリソース一覧
Get-AzResource -ResourceGroupName "rg-prod-web"
```

#### 仮想マシンの状態確認

**📋 実践：サブスクリプションとリソースグループの確認**

```powershell
# VM状態の一括確認
function Get-VMStatus {
    param(
        [string]$ResourceGroupName = $null
    )
    
    if ($ResourceGroupName) {
        $vms = Get-AzVM -ResourceGroupName $ResourceGroupName
    } else {
        $vms = Get-AzVM
    }
    
    foreach ($vm in $vms) {
        $status = Get-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Status
        
        [PSCustomObject]@{
            Name = $vm.Name
            ResourceGroup = $vm.ResourceGroupName
            Location = $vm.Location
            Size = $vm.HardwareProfile.VmSize
            PowerState = $status.Statuses | Where-Object {$_.Code -like "PowerState/*"} | Select-Object -ExpandProperty DisplayStatus
            ProvisioningState = $status.Statuses | Where-Object {$_.Code -like "ProvisioningState/*"} | Select-Object -ExpandProperty DisplayStatus
        }
    }
}

# 使用例
Get-VMStatus | Format-Table
```

### 5.4.2 基本的なリソース管理

#### 仮想マシンの開始・停止

**VM の電源管理：**

```powershell
# VM の開始
Start-AzVM -ResourceGroupName "rg-prod-web" -Name "vm-web-001"

# VM の停止（割り当て解除）
Stop-AzVM -ResourceGroupName "rg-prod-web" -Name "vm-web-001" -Force

# VM の再起動
Restart-AzVM -ResourceGroupName "rg-prod-web" -Name "vm-web-001"
```

#### ストレージアカウントの管理

**📋 実践：仮想マシンの開始・停止**

```powershell
# 開発環境 VM の一括停止スクリプト
function Stop-DevelopmentVMs {
    param(
        [string]$Environment = "dev"
    )
    
    # 開発環境のリソースグループを取得
    $resourceGroups = Get-AzResourceGroup | Where-Object {$_.ResourceGroupName -like "*$Environment*"}
    
    foreach ($rg in $resourceGroups) {
        $vms = Get-AzVM -ResourceGroupName $rg.ResourceGroupName
        
        foreach ($vm in $vms) {
            Write-Host "VM停止中: $($vm.Name)"
            Stop-AzVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Force
            Write-Host "VM停止完了: $($vm.Name)"
        }
    }
}

# 使用例（平日18時に実行）
Stop-DevelopmentVMs -Environment "dev"
```

#### 定期メンテナンススクリプト

**📋 サンプルスクリプト：定期メンテナンス**

```powershell
# 定期メンテナンス関数
function Invoke-AzureMaintenanceCheck {
    param(
        [string]$SubscriptionId
    )
    
    # サブスクリプションの選択
    Set-AzContext -SubscriptionId $SubscriptionId
    
    Write-Host "=== Azure 定期メンテナンスチェック ===" -ForegroundColor Green
    Write-Host "開始時間: $(Get-Date)" -ForegroundColor Yellow
    
    # 1. 停止中のVMで課金されているものを確認
    Write-Host "`n1. 停止中VM（課金中）の確認" -ForegroundColor Cyan
    $vms = Get-AzVM -Status
    $stoppedVMs = $vms | Where-Object {$_.PowerState -eq "VM stopped"}
    
    if ($stoppedVMs) {
        Write-Host "課金中の停止VM:" -ForegroundColor Red
        $stoppedVMs | Select-Object Name, ResourceGroupName, PowerState | Format-Table
    } else {
        Write-Host "課金中の停止VMはありません" -ForegroundColor Green
    }
    
    # 2. 未使用のディスクを確認
    Write-Host "`n2. 未使用ディスクの確認" -ForegroundColor Cyan
    $disks = Get-AzDisk
    $unusedDisks = $disks | Where-Object {$_.ManagedBy -eq $null}
    
    if ($unusedDisks) {
        Write-Host "未使用のディスク:" -ForegroundColor Red
        $unusedDisks | Select-Object Name, ResourceGroupName, DiskSizeGB | Format-Table
    } else {
        Write-Host "未使用のディスクはありません" -ForegroundColor Green
    }
    
    # 3. 未使用のパブリックIPを確認
    Write-Host "`n3. 未使用パブリックIPの確認" -ForegroundColor Cyan
    $publicIPs = Get-AzPublicIpAddress
    $unusedIPs = $publicIPs | Where-Object {$_.IpConfiguration -eq $null}
    
    if ($unusedIPs) {
        Write-Host "未使用のパブリックIP:" -ForegroundColor Red
        $unusedIPs | Select-Object Name, ResourceGroupName, IpAddress | Format-Table
    } else {
        Write-Host "未使用のパブリックIPはありません" -ForegroundColor Green
    }
    
    Write-Host "`n=== メンテナンスチェック完了 ===" -ForegroundColor Green
    Write-Host "終了時間: $(Get-Date)" -ForegroundColor Yellow
}

# 実行例
Invoke-AzureMaintenanceCheck -SubscriptionId "your-subscription-id"
```

### 5.4.3 コスト管理とアラート

#### コスト情報の取得

**Azure Cost Management の活用：**

```powershell
# 今月のコスト情報
$costData = Get-AzConsumptionUsageDetail -StartDate (Get-Date).AddDays(-30) -EndDate (Get-Date)

# リソースグループ別のコスト集計
$costByResourceGroup = $costData | Group-Object ResourceGroup | 
                       Select-Object Name, @{Name="TotalCost";Expression={($_.Group | Measure-Object PretaxCost -Sum).Sum}}

$costByResourceGroup | Sort-Object TotalCost -Descending | Format-Table
```

#### 予算アラートの設定

**📋 実践：コスト情報の取得**

```powershell
# コストレポート生成関数
function New-CostReport {
    param(
        [int]$Days = 30,
        [string]$OutputPath = "C:\Reports\Cost_Report.html"
    )
    
    # 期間の設定
    $endDate = Get-Date
    $startDate = $endDate.AddDays(-$Days)
    
    # コストデータの取得
    $costData = Get-AzConsumptionUsageDetail -StartDate $startDate -EndDate $endDate
    
    # リソースグループ別集計
    $costByRG = $costData | Group-Object ResourceGroup | 
                Select-Object Name, @{Name="Cost";Expression={($_.Group | Measure-Object PretaxCost -Sum).Sum}}
    
    # サービス別集計
    $costByService = $costData | Group-Object ConsumedService | 
                     Select-Object Name, @{Name="Cost";Expression={($_.Group | Measure-Object PretaxCost -Sum).Sum}}
    
    # HTML レポートの生成
    $html = @"
<!DOCTYPE html>
<html>
<head>
    <title>Azure コストレポート</title>
    <style>
        body { font-family: Arial, sans-serif; }
        table { border-collapse: collapse; width: 100%; margin-bottom: 20px; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <h1>Azure コストレポート</h1>
    <p>期間: $($startDate.ToString("yyyy-MM-dd")) ～ $($endDate.ToString("yyyy-MM-dd"))</p>
    
    <h2>リソースグループ別コスト</h2>
    <table>
        <tr><th>リソースグループ</th><th>コスト (USD)</th></tr>
"@
    
    foreach ($item in ($costByRG | Sort-Object Cost -Descending)) {
        $html += "<tr><td>$($item.Name)</td><td>$($item.Cost.ToString("F2"))</td></tr>"
    }
    
    $html += "</table>"
    $html += "</body></html>"
    
    # ファイルに保存
    $html | Out-File -FilePath $OutputPath -Encoding UTF8
    
    Write-Host "コストレポート生成完了: $OutputPath"
}

# 実行例
New-CostReport -Days 30 -OutputPath "C:\Reports\Azure_Cost_Report.html"
```

### 5.4.4 コストレポートの自動生成

**📋 サンプルスクリプト：コストレポートの自動生成**

```powershell
# 月次コストレポート自動送信
function Send-MonthlyCostReport {
    param(
        [string]$ToEmail,
        [string]$FromEmail,
        [string]$SmtpServer,
        [pscredential]$Credential
    )
    
    # レポートの生成
    $reportPath = "C:\Reports\Monthly_Cost_Report.html"
    New-CostReport -Days 30 -OutputPath $reportPath
    
    # 総コストの計算
    $costData = Get-AzConsumptionUsageDetail -StartDate (Get-Date).AddDays(-30) -EndDate (Get-Date)
    $totalCost = ($costData | Measure-Object PretaxCost -Sum).Sum
    
    # メール送信
    $subject = "Azure 月次コストレポート - 総額: $([math]::Round($totalCost, 2)) USD"
    $body = @"
Azure 月次コストレポートを送信いたします。

総コスト: $([math]::Round($totalCost, 2)) USD
期間: $((Get-Date).AddDays(-30).ToString("yyyy-MM-dd")) ～ $((Get-Date).ToString("yyyy-MM-dd"))

詳細は添付のレポートをご確認ください。
"@
    
    Send-MailMessage -To $ToEmail -From $FromEmail -Subject $subject -Body $body -SmtpServer $SmtpServer -Credential $Credential -Attachments $reportPath
    
    Write-Host "月次コストレポート送信完了"
}

# 使用例
# $cred = Get-Credential
# Send-MonthlyCostReport -ToEmail "manager@company.com" -FromEmail "admin@company.com" -SmtpServer "smtp.company.com" -Credential $cred
```

## 5.5 スクリプトの実践的な活用

### 5.5.1 スクリプト作成のベストプラクティス

#### エラーハンドリングの基本

**適切なエラーハンドリング：**

```powershell
# Try-Catch を使用したエラーハンドリング
function New-UserWithErrorHandling {
    param(
        [string]$DisplayName,
        [string]$UserPrincipalName
    )
    
    try {
        # ユーザー作成の試行
        $user = New-MgUser -DisplayName $DisplayName -UserPrincipalName $UserPrincipalName
        Write-Host "ユーザー作成成功: $DisplayName" -ForegroundColor Green
        return $user
    }
    catch {
        Write-Error "ユーザー作成エラー: $($_.Exception.Message)"
        Write-Host "対象ユーザー: $DisplayName" -ForegroundColor Red
        return $null
    }
}
```

#### ログ出力の実装

**📋 実践：ログ出力の実装**

```powershell
# ログ出力関数
function Write-Log {
    param(
        [string]$Message,
        [string]$Level = "INFO",
        [string]$LogFile = "C:\Logs\PowerShell.log"
    )
    
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $logEntry = "$timestamp [$Level] $Message"
    
    # コンソールに出力
    switch ($Level) {
        "ERROR" { Write-Host $logEntry -ForegroundColor Red }
        "WARN"  { Write-Host $logEntry -ForegroundColor Yellow }
        "INFO"  { Write-Host $logEntry -ForegroundColor Green }
        default { Write-Host $logEntry }
    }
    
    # ファイルに出力
    if (-not (Test-Path (Split-Path $LogFile -Parent))) {
        New-Item -ItemType Directory -Path (Split-Path $LogFile -Parent) -Force
    }
    
    Add-Content -Path $LogFile -Value $logEntry
}

# 使用例
Write-Log -Message "スクリプト開始" -Level "INFO"
Write-Log -Message "警告メッセージ" -Level "WARN"
Write-Log -Message "エラーが発生しました" -Level "ERROR"
```

#### 実用的なスクリプトの構造

**📋 テンプレート：実用的なスクリプトの構造**

```powershell
<#
.SYNOPSIS
    Microsoft 365 ユーザー管理スクリプト

.DESCRIPTION
    新規ユーザーの作成、ライセンス割り当て、グループ追加を自動化

.PARAMETER InputFile
    ユーザー情報が記載されたCSVファイル

.PARAMETER LogFile
    ログファイルのパス

.EXAMPLE
    .\New-BulkUsers.ps1 -InputFile "users.csv" -LogFile "log.txt"

.AUTHOR
    IT管理者

.DATE
    2024-01-01
#>

param(
    [Parameter(Mandatory=$true)]
    [string]$InputFile,
    
    [Parameter(Mandatory=$false)]
    [string]$LogFile = "C:\Logs\BulkUsers.log"
)

# 関数の定義
function Write-Log {
    param([string]$Message, [string]$Level = "INFO")
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $logEntry = "$timestamp [$Level] $Message"
    Write-Host $logEntry
    Add-Content -Path $LogFile -Value $logEntry
}

# メイン処理
try {
    Write-Log "スクリプト開始" "INFO"
    
    # CSV ファイルの読み込み
    if (-not (Test-Path $InputFile)) {
        throw "入力ファイルが見つかりません: $InputFile"
    }
    
    $users = Import-Csv $InputFile
    Write-Log "ユーザー数: $($users.Count)" "INFO"
    
    # 各ユーザーの処理
    foreach ($user in $users) {
        try {
            # ユーザー作成処理
            Write-Log "ユーザー作成中: $($user.DisplayName)" "INFO"
            # 実際の作成処理をここに記述
            
            Write-Log "ユーザー作成完了: $($user.DisplayName)" "INFO"
        }
        catch {
            Write-Log "ユーザー作成エラー: $($user.DisplayName) - $($_.Exception.Message)" "ERROR"
        }
    }
    
    Write-Log "スクリプト正常終了" "INFO"
}
catch {
    Write-Log "スクリプトエラー: $($_.Exception.Message)" "ERROR"
    exit 1
}
```

### 5.5.2 定期実行とタスクスケジューラ

#### Windows タスクスケジューラでの自動実行

**タスクスケジューラの設定：**

```powershell
# タスクスケジューラ用のスクリプト設定
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -File C:\Scripts\DailyReport.ps1"

$trigger = New-ScheduledTaskTrigger -Daily -At "09:00"

$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries

$task = New-ScheduledTask -Action $action -Trigger $trigger -Settings $settings

Register-ScheduledTask -TaskName "Daily Microsoft 365 Report" -InputObject $task
```

#### スクリプトの実行ログ管理

**📋 実践：Windows タスクスケジューラでの自動実行**

```powershell
# 定期実行用のラッパースクリプト
param(
    [string]$ScriptPath,
    [string]$LogPath = "C:\Logs\ScheduledTasks.log"
)

function Write-TaskLog {
    param([string]$Message, [string]$Level = "INFO")
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $logEntry = "$timestamp [$Level] [SCHEDULED] $Message"
    Add-Content -Path $LogPath -Value $logEntry
}

try {
    Write-TaskLog "定期実行開始: $ScriptPath" "INFO"
    
    # スクリプトの実行
    & $ScriptPath
    
    Write-TaskLog "定期実行正常終了: $ScriptPath" "INFO"
}
catch {
    Write-TaskLog "定期実行エラー: $ScriptPath - $($_.Exception.Message)" "ERROR"
    
    # エラー通知メール（オプション）
    # Send-MailMessage -To "admin@company.com" -Subject "スクリプトエラー" -Body $_.Exception.Message
}
```

### 5.5.3 チーム共有とドキュメント化

#### スクリプトのドキュメント化

**📋 注意点：自動実行時の考慮事項**

```powershell
# 自動実行時の考慮事項チェックリスト

<#
自動実行の設定確認項目：
1. 実行権限
   - PowerShell実行ポリシーの設定
   - 必要なモジュールのインストール
   - サービスアカウントの権限

2. 認証情報
   - 証明書ベースの認証
   - サービスプリンシパルの利用
   - 資格情報の安全な保存

3. エラーハンドリング
   - 適切なTry-Catch処理
   - ログ出力の実装
   - 通知機能の設定

4. スクリプトの保護
   - スクリプトの暗号化
   - アクセス権限の制限
   - バージョン管理

5. 監視と保守
   - 定期的な動作確認
   - ログの定期的な確認
   - スクリプトの更新管理
#>
```

#### チームでの共有方法

**スクリプト共有のベストプラクティス：**

1. **バージョン管理**: Git を使用したソースコード管理
2. **ドキュメント**: 使用方法と注意事項の文書化
3. **テスト**: 本番環境前のテスト実行
4. **レビュー**: チームメンバーによるコードレビュー

**📋 テンプレート：スクリプト仕様書の作成**

```markdown
# スクリプト仕様書

## 基本情報
- **スクリプト名**: New-BulkUsers.ps1
- **作成者**: IT管理者
- **作成日**: 2024-01-01
- **バージョン**: 1.0

## 概要
Microsoft 365 の新規ユーザーを一括作成するスクリプト

## 前提条件
- PowerShell 7.0 以上
- Microsoft.Graph モジュール
- Global Administrator 権限

## 使用方法
```powershell
.\New-BulkUsers.ps1 -InputFile "users.csv" -LogFile "log.txt"
```

## 入力ファイル形式
CSV形式で以下の列を含む：
- DisplayName: 表示名
- UserPrincipalName: ユーザープリンシパル名
- Department: 部署
- JobTitle: 役職

## 出力
- ログファイル: 実行結果の詳細
- エラーファイル: エラーが発生した場合

## 注意事項
- 大量のユーザー作成時は時間がかかる場合があります
- 重複するユーザー名は作成されません
- 実行前に必ずテスト環境で確認してください

## 更新履歴
- v1.0: 初版作成
```

## まとめ

この章では、PowerShellを使用したクラウドリソースの効率的な管理について学びました。

### 重要なポイント

1. **PowerShell環境の構築**
   - PowerShell 7の導入
   - 必要なモジュールのインストール
   - 基本的なコマンドの理解

2. **クラウドサービスへの接続**
   - Azure PowerShell
   - Exchange Online PowerShell
   - Microsoft Teams PowerShell

3. **基本的な管理スクリプト**
   - ユーザー管理の自動化
   - レポート作成の自動化
   - 日常業務の効率化

4. **Azure リソースの管理**
   - リソース情報の取得
   - 仮想マシンの操作
   - コスト管理とレポート

5. **スクリプトの実践的な活用**
   - エラーハンドリング
   - ログ出力
   - 定期実行の設定

PowerShellによる自動化は、日常の管理業務を大幅に効率化し、人的ミスを削減します。まずは簡単なスクリプトから始めて、徐々に複雑な処理に挑戦していくことをお勧めします。

次章では、企業レベルのセキュリティ管理とコンプライアンス対応について詳しく学んでいきます。