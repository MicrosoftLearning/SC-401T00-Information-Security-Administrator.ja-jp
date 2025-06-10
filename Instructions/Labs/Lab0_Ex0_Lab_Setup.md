---
lab:
  title: ラボのセットアップ - 管理環境を準備する
  module: Lab setup
---

## WWL テナント - 使用条件

講師が指導するトレーニング配信の一環としてテナントを提供されている場合は、講師が指導するトレーニングでハンズオンラボをサポートする目的でテナントを利用できることに注意してください。

テナントを共有したり、ハンズオンラボ以外の目的で使用したりしないでください。 このコースで使われるテナントは試用版テナントであり、クラスが終了し、拡張機能の対象となっていない場合は、使用したりアクセスしたりすることはできません。

テナントを有料サブスクリプションに変換することはできません。 このコースの一環として取得したテナントは Microsoft Corporation の財産のままであり、当社はいつでもアクセス権とリポジトリを取得する権利を留保します。

# ラボのセットアップ - 管理環境を準備する

このラボでは、管理タスク用に環境を構成して準備します。 必要な機能をアクティブ化し、管理アクセス許可を設定し、主要な要素を適切に構成します。

**タスク**:

1. Microsoft Purview ポータルで監査を有効にする
1. ラボ演習のユーザー パスワードを設定する
1. デバイスのオンボードを有効にする
1. インサイダー リスク分析を有効にする

## タスク 1 - Microsoft Purview ポータルで監査を有効にする

このタスクでは、Microsoft Purview ポータルで監査を有効にして、ポータル アクティビティを監視します。

1. Client 1 VM (SC-401-CL1) に **SC-401-CL1\admin** アカウントでログインし、Microsoft 365 には MOD 管理者アカウントでログインします。

1. Microsoft Edge で、Microsoft Purview ポータル `https://purview.microsoft.com` にアクセスして、ログインします。

1. 新しい Microsoft Purview ポータルに関するメッセージが画面に表示されます。 **[はじめに]** を選択して、新しいポータルにアクセスします。

    ![[ようこそ新しい Microsoft Purview ポータルへ] 画面のスクリーンショット。](../Media/welcome-purview-portal.png)

1. 左側のサイド バーから **[ソリューション]** を選択し、**[監査]** を選択します。

1. **[検索]** ページで、**[ユーザーと管理者のアクティビティの記録を開始する]** バーを選択して監査ログを有効にします。

    ![[ユーザーと管理者のアクティビティの記録を開始する] ボタンを示すスクリーンショット。](../Media/enable-audit-button.png)

1. このオプションを選択すると、このページに青いバーが表示されなくなるはずです。

<!----- PowerShell instructions

1. Open an elevated Terminal window by selecting the Windows button with the right mouse button and then select **Terminal (Admin)**.

1. Run the **Install Module** cmdlet in the terminal window to install the latest **Exchange Online PowerShell** module version:

    ```powershell
    Install-Module ExchangeOnlineManagement
    ```

1. Confirm the NuGet provider prompt  by typing **Y** for Yes and press **Enter**.

1. Confirm the Untrusted repository security dialog with **Y** for Yes and press **Enter**.  This process may take some time to complete.

1. Run the **Set-ExecutionPolicy** cmdlet to change your execution policy and press **Enter**

    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    ```

1. Close the PowerShell window.

1. Open a regular (non-elevated) PowerShell window by right-clicking the Windows button and selecting **Terminal**.

1. Run the **Connect-ExchangeOnline** cmdlet to use the Exchange Online PowerShell module and connect to your tenant:

    ```powershell
    Connect-ExchangeOnline
    ```

1. When the **Sign in** window is displayed, sign in as `admin@WWLxZZZZZZ.onmicrosoft.com` (where ZZZZZZ is your unique tenant ID provided by your lab hosting provider). Admin's password should be provided by your lab hosting provider.

1. To check if Audit is enabled, run the **Get-AdminAuditLogConfig** cmdlet:

    ```powershell
    Get-AdminAuditLogConfig | FL UnifiedAuditLogIngestionEnabled
    ```

1. If _UnifiedAuditLogIngestionEnabled_ returns false, then Audit is disabled.

1. To enable the Audit log, run the **Set-AdminAuditLogConfig** cmdlet and set the **UnifiedAuditLogIngestionEnabled** to _true_:

    ```powershell
    Set-AdminAuditLogConfig -UnifiedAuditLogIngestionEnabled $true
    ```

1. To verify that Audit is enabled, run the **Get-AdminAuditLogConfig** cmdlet again:

    ```powershell
    Get-AdminAuditLogConfig | FL UnifiedAuditLogIngestionEnabled
    ```

1. _UnifiedAuditLogIngestionEnabled_ should return _true_ to let you know Audit is enabled.

-->

Microsoft 365 で監査を正常に有効にしました。

## タスク 2 - ラボ演習のユーザー パスワードを設定する

このタスクでは、ラボに必要なユーザー アカウントのパスワードを設定します。

1. **SC-401-CL1\admin** アカウントを使って Client 1 VM (SC-401-CL1) にログインします。 パスワードは、ラボ ホスティング プロバイダーから支給されます。

1. **Microsoft Edge** を開き、**`https://admin.microsoft.com`** にアクセスし、Microsoft 365 管理センターに MOD 管理者 `admin@WWLxZZZZZZ.onmicrosoft.com` としてログインします (ここで ZZZZZZ はラボ ホスティング プロバイダーから支給された一意のテナント ID です)。

1. 左側のナビゲーション ウィンドウで **[ユーザー]** を展開し、**[アクティブ ユーザー]** を選びます。

1. **[Joni Sherman]**、**[Lynne Robbins]**、および **[Megan Bowen]** の左側にあるチェックボックスをオンにします。

   これらのアカウントは、ラボ演習を終了するまで使用します。

   ![リセットする必要があるユーザー アカウントを示すスクリーンショット。](../Media/user-accounts.png)

1. 上部のナビゲーションから **[パスワードのリセット]** ボタンを選択し、3 つのパスワードをすべてリセットします。

   ![Microsoft 365 管理センターの [パスワードのリセット] ボタンを示すスクリーンショット。](../Media/reset-password-button.png)

1. 右側の **[パスワードのリセット]** ポップアップ ページで、両方のチェックボックスがオフになっていることを確認します。

   この結果、演習に使用する 3 人のユーザーのパスワードを選択できるようになり、最初にサインインするときにこれらのパスワードをリセットする必要がなくなります。

1. "**パスワード**" フィールドに、今後の演習で使用するユーザー パスワードをリセットするための覚えやすいパスワードを入力します。

1. **[パスワードのリセット]** ポップアップ ページの下部で、**[パスワードのリセット]** ボタンを選択します。

1. **[パスワードがリセットされました]** ページに、リセットされた 3 つのユーザー アカウントが表示されているはずです。 このポップアップ ページの下部にある **[閉じる]** を選択します。

ラボ演習のパスワードが正常にリセットされました。

## タスク 3: デバイスのオンボードを有効にする

このタスクでは、組織向けにデバイスのオンボードを有効にします。

1. Client 1 VM (SC-401-CL1) には引き続き **SC-401-CL1\admin** アカウントでログインし、Microsoft 365 には MOD 管理者アカウントでログインしている必要があります。

1. **Microsoft Edge** で、**`https://purview.microsoft.com`** に移動して Microsoft Purview にログインし、左側のサイド バーから **[設定]** を選択します。

1. 左サイドバーで、**[デバイスのオンボード]** を展開し **[デバイス]** を選択します。

1. **[デバイス]** ページで **[デバイスのオンボードを有効にする]** を選択し、**[OK]** を選択してデバイスのオンボードを有効にします。

1. プロンプトが表示されたら、**[OK]** を選択して、デバイスの監視が有効になっていることを確認します。

これでデバイスのオンボーディングが有効となりました。デバイスをエンドポイント DLP ポリシーで保護するためにオンボードを開始できます。 この機能を有効にするプロセスには、最大 30 分かかる場合があります。

## タスク 4 - インサイダー リスク分析とデータ共有を有効にする

このタスクでは、インサイダー リスク管理の分析とデータ共有を有効にします。

1. Client 1 VM (SC-401-CL1) には引き続き **SC-401-CL1\admin** アカウントでログインし、Microsoft Purview には MOD 管理者アカウントでログインしている必要があります。

1. Microsoft Purview で、**[設定]** > **[インサイダー リスク管理]** > **[分析]** に移動します。

1. これらの設定を **[オン]** に切り替えます。

   - **テナント レベルの分析情報を表示する**

   - **ユーザー レベルの分析情報を表示する**

1. ページの下部にある **[保存]** を選択します。

1. 左側のナビゲーション ウィンドウで **[データ共有]** を選択します。

1. [データ共有] セクションで、**[ユーザー リスクの詳細を他のセキュリティ ソリューションと共有する]** を **[オン]** に切り替えます。

1. ページの下部にある **[保存]** を選択します。

インサイダー リスク管理の分析とデータ共有を有効にしました。

## タスク 5 - Microsoft Defender XDR を初期化する

このタスクでは、Microsoft Defender を開き、Microsoft Defender XDR の初期化が完了するまで待機します。

1. Client 1 VM (SC-401-CL1) には引き続き **SC-401-CL1\admin** アカウントでログインし、Microsoft Purview には MOD 管理者アカウントでログインしている必要があります。

1. **Microsoft Edge** で、**`https://security.microsoft.com/`** に移動して Microsoft Defender を開きます。

1. ナビゲーション ウィンドウで、**[調査と対応]** > **[インシデントとアラート]** > **[インシデント]** を選択します。

1. Microsoft Defender XDR が準備中であることを示すメッセージが表示されます。 このプロセスは自動的に実行され、数分かかる場合があります。

   ![Microsoft Defender XDR がオンボードされていることを示すスクリーンショット。](../Media/enable-defender-xdr.png)

Microsoft Defender XDR を初期化しています。 セットアップが完了するまで、他のタスクを続行できます。
