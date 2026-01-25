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

このラボでは、管理タスク用に環境を構成して準備します。 必要な機能を有効にし、アクセス許可を構成し、管理用にコア サービスを準備します。

**タスク**:

1. Microsoft Purview ポータルで監査を有効にする  
1. デバイスのオンボードを有効にする  
1. インサイダー リスク分析とデータ共有を有効にする  
1. Microsoft Defender XDR を初期化する
1. Microsoft Entra で多要素認証を構成する

**推定所要時間:** 30 ～ 45 分

## タスク 1 - Microsoft Purview ポータルで監査を有効にする

このタスクでは、Microsoft Purview ポータルで監査を有効にして、ポータル アクティビティを監視します。

1. **管理者**アカウントを使用して Client 1 VM (SC-401-CL1) にログインします。

1. Microsoft Edge を開きます。

1. **Microsoft Edge** で、`https://purview.microsoft.com` に移動し、**MOD 管理者**である `admin@WWLxZZZZZZ.onmicrosoft.com` としてサインインします (この ZZZZZZ は、ラボ ホスティング プロバイダーから提供された自分専用のテナント プレフィックスです)。 管理者のパスワードは、ラボ ホスティング プロバイダーから支給されます。

1. Microsoft Edge で、Microsoft Purview ポータル `https://purview.microsoft.com` にアクセスして、ログインします。

1. 新しい Microsoft Purview ポータルに関するメッセージが画面に表示されます。 **[はじめに]** を選択して、新しいポータルにアクセスします。

    ![[ようこそ新しい Microsoft Purview ポータルへ] 画面のスクリーンショット。](../Media/welcome-purview-portal.png)

1. 左側のサイド バーから **[ソリューション]** を選択し、**[監査]** を選択します。

1. **[検索]** ページで、**[ユーザーと管理者のアクティビティの記録を開始する]** バーを選択して監査ログを有効にします。

    ![[ユーザーと管理者のアクティビティの記録を開始する] ボタンを示すスクリーンショット。](../Media/enable-audit-button.png)

1. このオプションを選択すると、このページに青いバーが表示されなくなるはずです。

    >[!Note] **注:[監査] ボタンでログが有効にならない場合**
    >
    >一部のテナントでは、**[ユーザーと管理者のアクティビティの記録を開始する]** を選択しても監査が有効にならないことがあります。  
    >
    >このような場合は、代わりに PowerShell を使用して監査を有効にすることができます。
    >
    >1. Windows ボタンを右クリックし、**[ターミナル (管理者)]** を選択して、管理者特権のターミナル ウィンドウを開きます。  
    >
    >1. 最新の **Exchange Online PowerShell** モジュールをインストールします。
    >
    >     ```powershell
    >     Install-Module ExchangeOnlineManagement
    >     ```
    >
    >     すべてのプロンプトに対して「**Y**」(Yes を示します) と入力し、**Enter** キーを押します。
    >
    >1. 次のコマンドを実行して、実行ポリシーを変更します。
    >
    >     ```powershell
    >     Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    >     ```
    >
    >1. 管理者特権のターミナル ウィンドウを閉じて、通常の PowerShell セッションを開きます。
    >
    >1. Exchange Online に接続します。
    >
    >     ```powershell
    >     Connect-ExchangeOnline
    >     ```
    >
    >    `admin@WWLxZZZZZZ.onmicrosoft.com` としてサインインします (この ZZZZZZ は、ラボ ホスティング プロバイダーから提供された自分専用のテナント プレフィックスです)。 管理者のパスワードは、ラボ ホスティング プロバイダーから支給されます。
    >
    >1. [監査] が有効になっているかどうかを確認します。
    >
    >     ```powershell
    >     Get-AdminAuditLogConfig | FL UnifiedAuditLogIngestionEnabled
    >     ```
    >
    >    **_False_** が返された場合は、[監査] を有効にします。
    >
    >     ```powershell
    >     Set-AdminAuditLogConfig -UnifiedAuditLogIngestionEnabled $true
    >     ```
    >
    >1. 有効になっていることを確認します。
    >
    >     ```powershell
    >     Get-AdminAuditLogConfig | FL UnifiedAuditLogIngestionEnabled
    >     ```
    >
    >    [監査] が有効になると、このコマンドにより **_True_** が返されます。

Microsoft 365 で監査を正常に有効にしました。

## タスク 2: デバイスのオンボードを有効にする

このタスクでは、組織向けにデバイスのオンボードを有効にします。

1. Client 1 VM (SC-401-CL1) には引き続き **SC-401-CL1\admin** アカウントでログインし、Microsoft Purview には MOD 管理者アカウントでログインしている必要があります。

1. 左側のサイドバーから **[設定]** を選択し、**[デバイスのオンボード]** を展開します。

1. **[デバイスのオンボード]** ページで、**[デバイス]** を選択します。

1. **[デバイス]** ページで、**[デバイスのオンボードを有効にする]** を選択し **[OK]** を選択して確認します。

1. プロンプトが表示されたら、**[OK]** を選択して、デバイスの監視が有効になっていることを確認します。

これでデバイスのオンボーディングが有効となりました。デバイスをエンドポイント DLP ポリシーで保護するためにオンボードを開始できます。 この機能を有効にするプロセスには、最大 30 分かかる場合があります。

## タスク 3 - インサイダー リスク分析とデータ共有を有効にする

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

<!---

Removing this task in favor of using lab passwords

## Task 4 - Set user passwords for lab exercises

In this task, you'll set passwords for the user accounts needed for the labs.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account and logged in as the MOD Administrator in Microsoft 365.

1. Open **Microsoft Edge** and navigate to **`https://admin.microsoft.com`** to log into the Microsoft 365 admin center as the MOD Administrator, `admin@WWLxZZZZZZ.onmicrosoft.com` (where ZZZZZZ is your unique tenant prefix provided by your lab hosting provider). Admin's password should be provided by your lab hosting provider.

    > [!Note] **Note: Skip MFA for the Microsoft 365 Admin center**
    >
    > In some tenants, you might see a Portal MFA Enforcement prompt when signing in. If this prompt appears:
    >
    > - Select **Skip for now** to temporarily delay MFA setup.
    >
    >   ![Screenshot showing the option to postpone MFA.](../Media/postpone-mfa.png)
    >
    > - On the **Let us know why you're skipping MFA** dialogue, select any justification, then select **Send and skip**.
    >
    > This postpones MFA enforcement in the Microsoft 365 Admin center for the tenant and allows you to proceed with the lab.

1. On the left navigation pane, expand **Users** then select **Active users**.

1. Select the checkbox to the left of **Joni Sherman**, **Lynne Robbins**, and **Megan Bowen**.

   These accounts will be used throughout the lab exercises.

   ![Screenshot showing user accounts that need to be reset.](../Media/user-accounts.png)

1. Select the **Reset password** button from the top navigation to reset all three passwords.

   ![Screenshot showing the Reset password button in the Microsoft 365 admin center.](../Media/reset-password-button.png)

1. In the **Reset Password** flyout page on the right, ensure that both checkboxes are deselected.

   This will ensure that you can select a password for the three users being used for exercises, and that these passwords won't need to be reset when you first sign in.

1. In the **Password** field, enter a password you can remember to reset the user passwords to be used in future exercises.

1. At the bottom of the **Reset password** flyout page, select the **Reset password** button.

1. On the **Passwords have been reset** page, you should see the three user accounts that have been reset. At the bottom of this flyout page, select **Close**.

You have successfully reset passwords for lab exercises.
-->

## タスク 4 - Microsoft Defender XDR を初期化する

このタスクでは、Microsoft Defender に移動し、Microsoft Defender XDR が初期化されるまで待機します。

1. Client 1 VM (SC-401-CL1) には引き続き **SC-401-CL1\admin** アカウントでログインし、Microsoft Purview には MOD 管理者アカウントでログインしている必要があります。

1. **Microsoft Edge** で、**`https://security.microsoft.com/`** に移動して Microsoft Defender を開きます。

1. ナビゲーション ウィンドウで、**[調査と対応]** > **[インシデントとアラート]** > **[インシデント]** を選択します。

    > [!Note] **注:Microsoft Defender XDR の初期化**
    >
    > ラボ テナントによっては、Microsoft Defender XDR の初期化画面が表示される場合と表示されない場合があります。

1. Microsoft Defender XDR が準備中であることを示すメッセージが表示されます。 このプロセスは自動的に実行され、数分かかる場合があります。

   ![Microsoft Defender XDR がオンボードされていることを示すスクリーンショット。](../Media/enable-defender-xdr.png)

Microsoft Defender XDR を初期化しています。 セットアップが完了するまで、他のタスクを続行できます。

## タスク 5 - Microsoft Entra で多要素認証を構成する

このタスクでは、管理者アカウントの多要素認証 (MFA) を構成して、Microsoft Entra やその他の接続済みの Microsoft 365 サービスへのアクセスをセキュリティで保護します。

1. **Microsoft Edge** で **`https://entra.microsoft.com/`** に移動して Microsoft Entra を開きます。

1. **[最初にアプリを取得します]** 画面でデバイスのアプリ ストアから **Microsoft Authenticator** アプリをインストールするか、既にインストールされている場合はそれを開きます。

   ![多要素認証の [アカウントのセキュリティ保護] 画面を示すスクリーンショット。](../Media/mfa-entra.png)

   - 別のアプリを使用する場合は、**[別の認証アプリを使用します]** を選択し、画面の指示に従います。

1. [**次へ**] を選択します。

1. **[アカウントの設定]** 画面で、携帯電話の指示に従って通知を許可し、**[次へ]** を選択します。  

   - Microsoft Authenticator アプリのインストールと構成を既に完了している場合は、この画面が表示されないことがあります。 その場合は、次の手順に進みます。

1. **[QR コードをスキャンします]** 画面で、デバイスの Microsoft Authenticator アプリを使用して画面に表示されている QR コードをスキャンし、**[次へ]** を選択します。

1. ブラウザーに表示されている番号を携帯電話に入力してサインイン要求を承認します。

1. 要求を承認すると、**[通知が承認されました]** 画面が表示されます。 [**次へ**] を選択します。

1. **[成功]** 画面の **[既定のサインイン方法]** に **[Microsoft Authenticator]** が表示されていることを確認し、**[完了]** を選択します。

1. 再度サインインを求められた場合は、携帯電話でサインイン要求を承認して本人確認を行います。

1. 承認が完了すると、**Microsoft Entra 管理センター**にリダイレクトされます。

Microsoft Entra 管理者アカウントの多要素認証の構成と確認はこれで完了です。
