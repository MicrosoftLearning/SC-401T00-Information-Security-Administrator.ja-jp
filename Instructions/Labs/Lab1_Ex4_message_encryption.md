---
lab:
  title: 演習 4 - Microsoft Purview Message Encryption をデプロイする
  module: Module 1 - Implement Information Protection
---
<!--

=======
This lab is broken by Exchange provisioning issues on the tenant
=======

# Lab 1 - Exercise 4 - Deploy Microsoft Purview Message Encryption

Joni Sherman, the Information Security Administrator for Contoso Ltd., has been tasked with ensuring secure communication between departments. To support this, she is configuring Microsoft Purview Message Encryption for Contoso, including modifying the default settings and creating a custom branding experience for the finance department.

**Tasks**:

1. Verify Azure RMS functionality
1. Modify default branding template
1. Validate default branding behavior
1. Create custom branding template
1. Validate custom branding behavior

## Task 1 – Verify Azure RMS functionality

In this task, you'll verify the correct Azure RMS functionality of your tenant.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account.

1. Open PowerShell by right-clicking the Start button in the taskbar and selecting **Terminal (Admin)**.

1. Run the **Install Module** cmdlet in the terminal window to install the latest **Exchange Online PowerShell** module version:

    ```powershell
    Install-Module ExchangeOnlineManagement
    ```

1. Confirm the Untrusted repository security dialog with **Y** for Yes and press **Enter**.  This process may take some time to complete.

1. Run the **Connect-ExchangeOnline** cmdlet to use the Exchange Online PowerShell module and connect to your tenant:

    ```powershell
    Connect-ExchangeOnline
    ```

1. When the **Sign in** window is displayed, sign in as `JoniS@WWLxZZZZZZ.onmicrosoft.com` (where ZZZZZZ is your unique tenant ID provided by your lab hosting provider). You will use the password you reset Joni's to in a previous lab.

1. Run the **Get-IRMConfiguration** cmdlet to verify Azure RMS and IRM is activated in your tenant:

    ```powershell
    Get-IRMConfiguration | fl AzureRMSLicensingEnabled
    ```

   The **AzureRMSLicensingEnabled** result should be **True**.

1. Run the **Test-IRMConfiguration** cmdlet to test Azure RMS functionality using Office 365 Message Encryption with **Megan Bowen** as both sender and recipient:

    ```powershell
    Test-IRMConfiguration -Sender MeganB@contoso.com -Recipient MeganB@contoso.com
    ```

    ![IRM validation script result. ](../Media/irm-validation.png)

    Verify all tests are in the status PASS and no errors are shown.

1. Leave the PowerShell window open.

You have successfully installed the Exchange Online PowerShell module, connected to your tenant, and verified the correct functionality of Azure RMS.

## Task 2 – Modify default branding template

There is a requirement in your organization to restrict trust for foreign identity providers, such as Google or Facebook. Because these social IDs are activated by default for accessing messages protected with message encryption, you need to deactivate the use of social IDs for all users in your organization.

1. You should still be logged into your Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account and there should still be an open PowerShell window with Exchange Online connected.

1. Run the **Get-OMEConfiguration** cmdlet to view the default configuration:

    ```powershell
    Get-OMEConfiguration -Identity "OME Configuration" | fl
    ```

   Review the settings and confirm that the SocialIdSignIn property is set to **True**.

    ![Screenshot showing the SocialIdSignIn value set to True. ](../Media/socialidsignin-value-true.png)

1. Run the **Set-OMEConfiguration** cmdlet to restrict the use of social IDs for accessing messages from your tenant protected with OME:

    ```powershell
    Set-OMEConfiguration -Identity "OME Configuration" -SocialIdSignIn:$false
    ```

1. Confirm the warning message for customizing the default template by entering **Y** for Yes then press **Enter**.

1. Run the **Get-OMEConfiguration** cmdlet to check the default configuration again and validate:

    ```powershell
    Get-OMEConfiguration -Identity "OME Configuration" | fl
    ```

    ![Screenshot showing the SocialIdSignIn value set to False. ](../Media/socialidsignin-value-false.png)

   Notice the result should show the SocialIdSignIn is set to **False**. Leave the PowerShell window and client open.

You've successfully disabled social identity providers, helping ensure that encrypted emails from Contoso can only be opened using Microsoft accounts or one-time passcodes—improving control over sensitive message access.

## Task 3 – Validate default branding behavior

You must confirm that no social IDs dialog is displayed for external recipients when receiving a message protected with Office 365 Message Encryption from users of your tenant and they need to use the OTP at any time accessing the encrypted content.

> [!alert] External email delivery might be blocked in some lab environments. This task might not complete as expected.

1. You should still be logged into your Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin**.

1. Open **Microsoft Edge** in an InPrivate window by right clicking Microsoft Edge from the task bar and selecting **New InPrivate window**.

1. Navigate to **`https://outlook.office.com`** and log into Outlook on the web as `LynneR@WWLxZZZZZZ.onmicrosoft.com` (where ZZZZZZ is your unique tenant ID provided by your lab hosting provider). Lynne's password was set in a previous exercise.

1. On the **Stay signed in?** dialog box, select the checkbox for **Don't show this again** then select **No**.

1. In Outlook on the web, select **New mail**.

1. In the **To** line enter your personal or other third-party email address that isn't in the tenant domain. Enter **`Secret Message`** in the subject line and **`My super-secret message.`** in the body of the email.

1. From the top pane, select **Options** then **Encrypt** to encrypt the message. Once you've successfully encrypted the message, you should see a notice that says "Encrypt: This message is encrypted. Recipients can't remove encryption."

      ![Screenshot of Encryption settings](../Media/OptionsEncrypt.png)

1. Select **Send** to send the message. Leave the Outlook window open.

1. Sign into your personal email account in a new window and open the message from Lynne Robbins. If you sent this email to a Microsoft account (like @outlook.com) the encryption might be processed automatically, and you'll see the message automatically. If you sent the email to another email service like (@gmail.com), you might have to perform the next steps to process the encryption and read the message.

    > [!Note] **Note**: You might need to check your junk or spam folder for the message from Lynne Robbins.

1. Select **Read the message**.

1. Because social IDs are disabled, you shouldn't see an option to sign in with a third-party account.

1. Select **Sign in with a One-time passcode** to receive a limited time passcode.

1. Go to your personal email portal and open the message with subject **Your one-time passcode to view the message**.

1. Copy the passcode, paste it into the'portal and select **Continue**.

1. Review the encrypted message.

You have successfully tested the modified default'template with deactivated social IDs.

## Task 4 – Create custom branding template

Protected messages sent by your organizations finance department require special branding, including customized introduction and body texts and a Disclaimer link in the footer. The finance messages shall also expire after seven days. In this task, you will create a new custom'configuration and create a transport rule to apply the'configuration to all mail sent from the finance department.

1. You should still be logged into your Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin**, and there should still be an open PowerShell window with Exchange Online connected.

1. Run the **New-OMEConfiguration** cmdlet to create a new configuration:

    ```powershell
    New-OMEConfiguration -Identity "Finance Department" -ExternalMailExpiryInDays 7
    ```

1. Confirm the warning message for customizing the template with **Y** for Yes and press **Enter**.

1. Run the **Set-OMEConfiguration** cmdlet with the _IntroductionText_ parameter to change the introduction text:

    ```powershell
    Set-OMEConfiguration -Identity "Finance Department" -IntroductionText " from Contoso Ltd. finance department has sent you a secure message."
    ```

1. Confirm the warning message for customizing the template with **Y** for Yes and press **Enter**.

1. Run the **Set-OMEConfiguration** cmdlet with the _EmailText_ parameter to update the body text of the encrypted email:

    ```powershell
    Set-OMEConfiguration -Identity "Finance Department" -EmailText "Encrypted message sent from Contoso Ltd. finance department. Handle the content responsibly."
    ```

1. Confirm the warning message for customizing the template with **Y** for Yes and press **Enter**.

1. Run the **Set-OMEConfiguration** cmdlet with the _PrivacyStatementURL_ parameter to change the disclaimer URL to point to Contoso's privacy statement site:

    ```powershell
    Set-OMEConfiguration -Identity "Finance Department" -PrivacyStatementURL "https://contoso.com/privacystatement.html"
    ```

1. Confirm the warning message for customizing the template with **Y** for Yes and press **Enter**.

1. Run the **New-TransportRule** cmdlet to create a mail flow rule, which applies the custom'template to all messages sent from the finance team. This process might take a few seconds to complete.

    ```powershell
    New-TransportRule -Name "Encrypt all mails from Finance team" -FromScope InOrganization -FromMemberOf "Finance Team" -ApplyRightsProtectionCustomizationTemplate "Finance Department" -ApplyRightsProtectionTemplate Encrypt
    ```

1. Run the **Get-OMEConfiguration** cmdlet to verify changes.

    ```powershell
    Get-OMEConfiguration -Identity "Finance Department" | Format-List
    ```

1. Close the PowerShell window after reviewing the results

You've configured a transport rule that ensures emails from the finance department are encrypted and branded consistently, reinforcing Contoso's messaging and security standards.

## Task 5 – Validate custom branding behavior

To validate the new custom configuration, you need to use the account of Lynne Robbins again, who is a member of the finance team.

> [!alert] External email restrictions might prevent this message from being received. Branding might not appear as expected.

1. Go back to **Microsoft Edge**  with the InPrivate Outlook on the web window where you should still be logged in as **Lynne Robbins**.

1. Select **New mail** from the upper left side part of Outlook on the web.

1. In the **To** line enter your personal or other third-party email address that isn't in the tenant domain. Enter **`Finance Report`** in the subject line and enter **`Secret finance information.`** in the body of the email.

1. Select **Send** to send the message, then close the InPrivate window where you're logged in as Lynne.

1. Sign into your personal email account and open the message from Lynne Robbins.

1. You should see a message from Lynne Robbins that looks like the image below.  Select **Read the message**.

    ![Sample encrypted email from Lynne Robbins. ](../Media/EncryptedEmail.png)

1. In the customized configuration, both authentication options are available, indicating that social ID sign-in is enabled. Select **Sign in with a One-time passcode** to receive a limited time passcode.

1. Go to your personal email portal and open the message with subject **Your one-time passcode to view the message**.

1. Copy the passcode, paste it into the portal and select **Continue**.

1. Review the encrypted message with custom branding. Close the window with your email account open.

You have successfully tested the new customized template.

-->

# ラボ 1 - 演習 4 - Microsoft Purview Message Encryption をデプロイする

Contoso Ltd. の情報セキュリティ管理者である Joni Sherman は、部門間で交換される機密情報を保護するために、セキュリティで保護されたメール通信を実装しています。 この取り組みの一環として、Exchange 管理センターを使用して Microsoft Purview Message Encryption (OME) を構成し、財務部門から送信されたメッセージを自動的に暗号化し、メッセージが安全に送信されたことを明確に通知します。

**タスク**:

1. 財務部門からのメッセージを暗号化するメール フロー ルールを作成する
1. 暗号化されたメッセージに免責事項を追加する
1. メール フロー ルールを有効にする  
1. メッセージの暗号化を検証する

## タスク 1 - 財務部門からのメッセージを暗号化するメール フロー ルールを作成する

このタスクでは、Exchange 管理センターを使用して、財務チーム グループのメンバーから送信されるすべてのメッセージに Microsoft Purview Message Encryption を適用するメール フロー ルールを作成します。

1. **Microsoft Edge** で、`https://admin.exchange.microsoft.com` に移動し、JoniS@WWLxZZZZZZ.onmicrosoft.com としてサインインします (ZZZZZZ はご自身のテナント ID に置き換えてください)。

1. 左側のナビゲーション ウィンドウで **[メール フロー]** を展開し、**[ルール]** を選択します。

1. **[ルール]** ページで、**[+ ルールの追加]** > **[Office 365 Message Encryption と権利保護をメッセージに適用する]** を選択します。

1. **[ルール条件の設定]** ページで、以下のように構成します。

   - **名前**: `Encrypt messages from Finance department`

   - **[このルールを適用する条件] **セクションで、以下のように構成します。

      - ドロップダウン 1:**送信者**

      - ドロップダウン 2: **[はこのグループのメンバーです]** については、**[メンバーの選択]** ポップアップで **[財務チーム]** と **[保存]** を選択します。

   - **[以下を実行します]** セクション:

     - 既定の **[メッセージのセキュリティを変更する]** と **[Office 365 Message Encryption と権利保護を適用する]** をオンのままにします

     - **[以下を実行します]** セクションの **[1 つ選択する]** リンクを選択します。

       ![Exchange 管理センターで [1 つ選択する] を選択する場所を示すスクリーンショット。](../Media/rights-protect-message-options.png)

     - **[RMS テンプレートの選択]** ポップアップで **[暗号化]** を選択し、**[保存]** を選択します。

     - **[ルール条件の設定]** ページで **[次へ]** を選択します。

1. **[ルールの設定]** ページで、既定値を選択したままにして、**[次へ]** を選択します。

1. **[レビューと完了]** ページでメール フロー ルールを確認し、**[完了]** を選択します。

1. メール フロー ルールが作成されたら、**[完了]** を選択します。

Microsoft Purview Message Encryption を使用して財務部門から送信されたメッセージを暗号化するメール フロー ルールの作成はこれで完了です。 これにより、機密性の高い財務通信が組織外に送信される前に確実に保護されます。

## タスク 2 - 暗号化されたメッセージに免責事項を追加する

次に、既存の暗号化ルールを変更して免責事項を追加します。 この免責事項は、メッセージが Contoso Ltd. によって安全に送信されたことを受信者に通知する、メッセージのブランド化の単純な形式として機能します。

1. **[ルール]** ページで、新しく作成した **[財務部門からのメッセージを暗号化する]** を選択します。

1. **[財務部門からのメッセージを暗号化する]** ポップアップで、**[ルール条件の編集]** を選択します。

1. 別のアクションを追加するには、**[以下を実行します]** セクションの右側にある **+** を選択します。

   ![別のメール フロー アクションを追加するプラス (+) の位置を示すスクリーンショット。](../Media/add-mail-flow-condition.png)

1. 新しく作成された **[And]** セクション:

   - ドロップダウン 1:**メッセージに免責事項を適用する**

   - ドロップダウン 2: **免責事項を追加する**。

   - ドロップダウンで **[テキストの入力]** を選択し、**[免責事項のテキストを指定します]** ポップアップに「`This email has been encrypted and sent securely by Contoso Ltd.`」と入力します。

   - ポップアップの下部にある **[保存]** を選択します。

   - フォールバック アクションを追加するには、リンクを選択します。 **[フォールバック アクションを指定します]** ポップアップで **[折り返す]** を選択し、ポップアップの下部にある **[保存]** を選択します。

1. 下部の **[財務部門からのメッセージを暗号化する]** ポップアップで **[保存]** を選択します。

1. ルールが変更されると、"**トランスポート ルールが正常に更新されました**" というメッセージが表示されます。

1. ポップアップの右上隅にある **[X]** を選択して、ポップアップを閉じます。

暗号化ルールを更新して、保護された各メッセージに免責事項を追加しました。 これにより、メールが暗号化され、Contoso Ltd. から安全に送信されたことが受信者にとって明確になります。

## タスク 3 - メール フロー ルールを有効にする

既定では、新しいメール フロー ルールは無効な状態で作成されます。 このタスクでは、暗号化ルールを有効にして、財務部門からのメッセージの保護を開始できるようにします。

1. **[ルール]** ページで、新しく作成した **[財務部門からのメッセージを暗号化する]** に対して **[無効]** を選択します。

1. **[財務部門からのメッセージを暗号化する]** ポップアップで、**[ルールを有効または無効にする]** の下のトグルを **[有効]** に設定します。

1. メール フロー ルールは自動的に有効になります。 "**ルールの状態を更新しています。お待ちください...**" というメッセージが表示されます。ルールが有効になると、"**ルールの状態が正常に更新されました**" というメッセージが表示されます。

1. ポップアップの右上隅にある **[X]** を選択して、ポップアップを閉じます。

暗号化ルールがアクティブになり、財務部門から送信されるメッセージに対して Microsoft Purview Message Encryption が適用されます。 今後、財務ユーザーから送信されるメッセージは自動的に暗号化され、Contoso Ltd. の免責事項が含まれるようになります。

## タスク 4 - メッセージの暗号化を検証する

このタスクでは、財務部門のメンバーからテスト メールを送信し、Microsoft Purview Message Encryption が自動的に適用され、セキュリティで保護されたメッセージの通知が受信者に表示されることを確認します。

> [!alert] 一部のラボ環境では、外部メールの配信がブロックされる場合があります。 このタスクは、想定どおりに完了しない可能性があります。

1. InPrivate ウィンドウで **Microsoft Edge** を開くには、タスク バーから Microsoft Edge を右クリックし、**[新しい InPrivate ウィンドウ]** を選択します。

1. **`https://outlook.office.com`** に移動し、Outlook on the web に `LynneR@WWLxZZZZZZ.onmicrosoft.com` としてログインします (ZZZZZZ はラボ ホスティング プロバイダーから支給された固有のテナント ID)。 Lynne のパスワードは、前の演習で設定しました。

1. **[サインインの状態を維持しますか?]** ダイアログボックスで、**[今後、このメッセージを表示しない]** チェックボックスをオンにして、**[いいえ]** を選択します。

1. Outlook on the web で、**[新しいメール]** を選択します。

1. **[To]** に、テナント ドメインには存在しない、個人用またはその他サード パーティのメール アドレスを入力します。 メールの件名に「**`Secret Message`**」と入力し、本文には「**`My super-secret message.`**」と入力します。

1. **[送信]** を選択してメッセージを送信します。 Outlook ウィンドウは開いたままにしておきます。

1. 新しいウィンドウで個人用のメール アカウントにサインインし、Lynne Robbins からのメッセージを開きます。 メッセージを Microsoft アカウント (@outlook.com など) に送信した場合、自動的に開かれることがあります。 メールを他のメール サービス (@gmail.com など) に送信した場合、暗号化を処理し、メッセージを読み取る次の手順を実行する必要がある場合があります。

    > [!Note] **注:** 迷惑メール フォルダーに Lynne Robbins からのメッセージがあるかどうかを確認する必要があるかもしれません。

1. **[メッセージを読む]** を選択します。

1. **[ワンタイム パスコードを使用してサインイン]** を選択して、制限時間付きパスコードを受け取ります。

1. 個人用のメール ポータルを開き、**"メッセージを表示するためのワンタイム パスコード"** という件名のメッセージを開きます。

1. パスコードをコピーしてポータルに貼り付け、**[続行]** を選択します。

1. 暗号されたメッセージを確認します。 メールの下部に "**このメールは Contoso Ltd. によって暗号化され、安全に送信されています **" というメッセージが表示されます。

財務部門からのメッセージが自動的に暗号化されることと Contoso の免責事項が含まれることの検証と、Microsoft Purview Message Encryption が想定どおりに動作していることの確認は、これで完了です。
