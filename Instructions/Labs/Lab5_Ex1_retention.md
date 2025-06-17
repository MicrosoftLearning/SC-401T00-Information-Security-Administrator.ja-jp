---
lab:
  title: 演習 1 - 保持ポリシーを構成する
  module: Module 5 - Implement and manage retention
---

## WWL テナント - 使用条件

講師が指導するトレーニング配信の一環としてテナントを提供されている場合は、講師が指導するトレーニングでハンズオンラボをサポートする目的でテナントを利用できることに注意してください。

テナントを共有したり、ハンズオンラボ以外の目的で使用したりしないでください。 このコースで使われるテナントは試用版テナントであり、クラスが終了し、拡張機能の対象となっていない場合は、使用したりアクセスしたりすることはできません。

テナントを有料サブスクリプションに変換することはできません。 このコースの一環として取得したテナントは Microsoft Corporation の財産のままであり、当社はいつでもアクセス権とリポジトリを取得する権利を留保します。

# ラボ 5 - 演習 1 - 保持を実装して管理する

あなたは、Contoso Ltd のコンプライアンス管理者である Joni Sherman です。同社は、財務データや特権通信に関連するリスクの露出を軽減するために、データ セキュリティ戦略を強化しています。 Microsoft Purview のデータ保持ソリューションを構成して、監査準備のサポート、不要なデータ保持の制限、機密通信の適切な監視をするよう求められました。

**タスク**:

1. 保持ラベルを作成する
1. 保持ラベルを発行する
1. 保持ラベルの自動適用ポリシーを作成する。
1. 静的アイテム保持ポリシーを作成する
1. SharePoint コンテンツを回復する

## タスク 1 - 保持ラベルを作成する

このタスクでは、監査や調査のために保持する必要がある機密性の高い財務データの保持ラベルを作成します。

1. **SC-401-cl1\admin** アカウントで Client 1 VM (SC-401-CL1) にログインします。

1. Microsoft Edge で、`https://purview.microsoft.com` に移動し、Microsoft Purview ポータルに **Joni Sherman** `JoniS@WWLxZZZZZZ.onmicrosoft.com` としてログインします (ここで ZZZZZZ はラボ ホスティング プロバイダーから支給された一意のテナント ID です)。 Joni のパスワードは、前の演習で設定しました。

1. **[ソリューション]** > **[データ ライフサイクル管理]** > **[保持ラベル]** に移動します。

1. **[ラベル]** ページで、**[ラベルの作成]** を選択します。

1. **[保持ラベルに名前を付ける]** ページで、次の情報を入力します。

   - **名前**: `Sensitive Financial Records`
   - **ユーザー向けの説明**: `Use for financial files with sensitive data that must be retained for audit or security purposes.`
   - **管理者向けの説明**: `Retains high-impact financial data for 5 years to support audits and security investigations.`

1. [**次へ**] を選択します。

1. **[Define label settings](ラベル設定の定義)** ページで、 **[Retain items forever or for a specific period](アイテムを無期限に、または特定の期間保持する)** を選んでから、 **[次へ]** を選びます。

1. **[期間の定義]** ページで、保持期間の構成入力に対してこれらの値が設定されていることを確認します。

    - **期間はどれくらいですか?**: 5 年
    - **期間が開始されるのはいつですか?** : アイテムが変更されたとき

1. [**次へ**] を選択します。

1. **[保持期間後の処理の選択]** ページで **[アイテムを自動的に削除する]** を選択して、**[次へ]** を選択します。

1. **[確認と完了]** ページで、**[ラベルの作成]** を選びます。

1. **[保持ラベルが作成されました]** ページで、**[何もしない]** オプションを選択してから、**[完了]** を選択します。

財務コンテンツを 5 年間保持し、その後削除してデータの露出を減らす保持ラベルを作成しました。

## タスク 2: 保持ラベルを発行する

このタスクでは、保持ラベルを発行して、ユーザーが Exchange、SharePoint、OneDrive などの Microsoft 365 サービスに適用できるようにします。

1. Microsoft Purview で、**[ソリューション]** > **[データ ライフサイクル管理]** > **[保持ラベル]** に移動します。

1. **[機密性の高い財務記録]** ラベルの横にあるチェックボックスをオンにし、**[ラベルの発行]** アイコン (![ラベルの発行アイコン](../Media/publish-labels-icon.png)) を選択して、この保持ラベルを発行します。

1. **[発行するラベルの選択]** ページで、**[機密性の高い財務記録]** ラベルが選択されていることを確認し、**[次へ]** を選択します。

1. **[Policy Scope] (ポリシー スコープ)** ページで、**[次へ]** を選択します。

1. **[作成するアイテム保持ポリシーの種類を選択する]** ページで **[静的]** を選択し、**[次へ]** を選択します。

1. **[ラベルを発行する場所の選択]** ページで、**[特定の場所を選択]** を選択し、次を選択します。

    - Exchange メールボックス
    - SharePoint クラシック サイトとコミュニケーション サイト
    - OneDrive アカウント
    - 他のすべての場所を選択解除する

1. [**次へ**] を選択します。

1. **[ポリシーの名前を設定する]** で、以下を入力します。

    - **名前**: `Sensitive Financial Data Retention`
    - **説明**: `Makes the 'Sensitive Financial Records' label available to users in Exchange, SharePoint, and OneDrive.`

1. [**次へ**] を選択します。

1. **[完了]** ページで、**[送信]** を選択します。  

1. **[保持ラベルが発行されました]** ページで、**[完了]** を選びます。

保持ラベルを公開し、ユーザーが主要な Microsoft 365 サービスで適用できるようにします。

## タスク 3 - 保持ラベルの自動適用ポリシーを作成する

このタスクでは、個人の財務情報を含むコンテンツに保持ラベルを自動的に適用するポリシーを構成します。

1. Microsoft Purview で、**[ソリューション]**、**[データ ライフサイクル管理]**、**[ポリシー]**、**[ラベル ポリシー]** の順に移動します。

1. **[ラベル ポリシー]** ページで、**[ラベルの自動適用]** を選択してラベルの構成を開始します。

1. **[始めましょう]** ページで、次の情報を入力します。

   - **名前**: `Auto-apply Personal Financial PII`
   - **説明**: `Applies this label to personal financial data to help meet audit and investigation requirements. Retains content for 3 years.`

1. [**次へ**] を選択します。

1. **[このラベルを適用するコンテンツの種類を選択する]** ページで、**[機密情報が含まれているコンテンツにラベルを適用する]** を選択し、**[次へ]** を選択します。

1. **[機密情報を含むコンテンツ]** ページで、**[財務]** カテゴリを選択し、**[米国グラム リーチ ブライリー法 (GLBA)]** 規則を選択してから、**[次へ]** を選択します。

1. **[機密情報が含まれているコンテンツを定義する]** ページで、**[次へ]** を選択します。

1. **[ポリシー スコープ]** ページで、**[次へ]** を選びます。

1. **[Choose the type of retention policy to create](作成するアイテム保持ポリシーの種類を選択する)** ページで、 **[静的]** を選びます。

1. **[ラベルを発行する場所の選択]** ページで、**[特定の場所を選択]** を選択し、次を選択します。

    - Exchange メールボックス
    - SharePoint クラシック サイトとコミュニケーション サイト
    - OneDrive アカウント
    - 他のすべての場所を選択解除する

1. **[自動適用するラベルを選択する]** ページで **[ラベルの追加]** を選択します。

1. **[ラベルの選択]** ポップアップで、**[個人財務 PII]** を選択してから、**[追加]** を選択します。

1. **[自動適用するラベルを選択する]** ページに戻り、**[次へ]** を選択します。

1. **[ポリシーをテストするか実行するかを決定する]** で、**[実行前にポリシーをテストする]** を選択して、**[次へ]** を選択します。

1. **[確認と完了]** ページで、**[ポリシーを作成]** を選択してから、**[自動ラベル付けポリシーが作成されました]** ページで **[完了]** を選択します。

個人の財務データを識別し、保持ラベルを自動的に適用する自動適用ポリシーを作成しました。

## タスク 4 - 静的アイテム保持ポリシーを作成する

このタスクでは、長期的なデータ リスクの軽減に役立つ Microsoft Teams コンテンツの静的アイテム保持ポリシーを作成します。

1. Microsoft Purview で、**[ソリューション]**、**[データ ライフサイクル管理]**、**[ポリシー]**、**[アイテム保持ポリシー]** の順に移動します。

1. **[アイテム保持ポリシー]** ページで、**[新しいアイテム保持ポリシー]** を選択します。

1. **[アイテム保持ポリシーの名前を設定]** ページで以下を入力します。

   - **名前**: `Teams Retention`
   - **説明**: `Retains Teams chats and channel messages for 3 years, then deletes them to reduce long-term data risk.`

1. [**次へ**] を選択します。

1. **[ポリシー スコープ]** ページで、**[次へ]** を選びます。

1. **[作成するアイテム保持ポリシーの種類を選択する]** ページで **[静的]** を選び、**[次へ]** を選びます。

1. **[ポリシーを適用する場所の選択]** ページで、次を有効にします。

   - Teams チャネル メッセージ
   - Teams チャット
   - 他のすべての場所は無効のままにします。

1. [**次へ**] を選択します。

1. **[コンテンツを保持するか、削除するか、またはその両方を行うかを決定する]** ページで、保持の構成に次の値が設定されていることを確認します。

   - **[特定の期間アイテムを保持]** を選択します。
   - **[特定の期間アイテムを保持]** の下で、ドロップダウン リストから **[カスタム]** を選択します。
   - [年] フィールドを `3` に変更します。
   - **保持期間開始の条件**:アイテムが最後に変更されたとき
   - **[保持期間の終了時]** :アイテムを自動的に削除する

1. [**次へ**] を選択します。

1. **[確認と完了]** ページで **[送信]** を選択し、**[アイテム保持ポリシーが正常に作成されました]** ページで **[完了]** を選択します。

Teams メッセージを自動的に削除する前に 3 年間保持する静的アイテム保持ポリシーを構成しました。

<!------ Commenting out until tenant bug issues are resolved
## Task 5 – Create an adaptive scope

In this task, you'll define an adaptive scope that targets Microsoft 365 groups associated with leadership and operations roles.

1. In Microsoft Purview, **Settings** > **Roles and scopes** > **Adaptive scopes**.

1. On the **Adaptive scopes** page select **+ Create scope**.

1. On the **Name your adaptive policy scope** page enter:

    - **Name**: `Leadership and Ops Groups`
    - **Description**: `Targets Leadership and Operations M365 groups with privileged access to sensitive data.`

1. Select **Next**.

1. On the **Assign admin unit** page select **Next**.

1. On the **What type of scope do you want to create?** page select **Users**, then select **Next**.

1. On the **Create the query to define users** page, in the **User attributes** section, ensure these values are selected for the user attribute configuration:

   - Select the **Attribute** dropdown then select **Department**
   - Leave the default **is equal to** value in the next field
   - Enter `Leadership` as the **Value**

1. Add a second attribute by selecting **+ Add attribute** on the **Create the query to define users** page. In the new field under the one we just configured, configure these values:

   - Select the dropdown for the query operator and update it from And to **Or**
   - Select the **Attribute** dropdown then select **Department**
   - Leave the default **is equal to** value in the next field
   - Enter `Operations` as the **Value**

1. Select **Next**.

1. On the **Review and finish** page select **Submit**.

1. Once your adaptive scope is created select **Done** on the **Your scope was created** page.

You've created an adaptive scope to support targeted retention for privileged groups in the organization.

## Task 6 – Create an adaptive retention policy

In this task, you'll use the adaptive scope you created to configure a retention policy for Microsoft 365 groups with sensitive responsibilities.

1. In Microsoft Purview, navigate to **Solutions** > **Data Lifecycle Management** > **Policies** >  **Retention policies**.

1. On the **Retention policies** page, select **+ New retention policy**.

1. On the **Name your retention policy** page enter:

    - **Name**: `Privileged Group Retention`
    - **Description**: `Retains content from Leadership and Operations groups for 5 years to support audit and investigation.`

1. Select **Next**.

1. On the **Policy Scope** page select **Next**.

1. On the **Choose the type of retention policy to create** page select **Adaptive** then select **Next**.

1. On the **Choose adaptive policy scopes and locations** page select **+ Add scopes**.

1. On the **Choose adaptive policy scopes** flyout panel select the checkbox for **Leadership and Ops Groups** then select **Add** at the bottom of the panel.

1. Back on the **Choose locations to apply the policy** enable:

    - Microsoft 365 Group mailboxes & sites
    - Leave all other locations disabled.

1. Select **Next**.

1. On the **Decide if you want to retain content, delete it, or both** page, ensure these values are set for the retention configuration:

   - Select **Retain items for a specific period**.
   - Under **Retain items for a specific period**, select **5 years** from the dropdown list
   - **Start the retention period based on**: When items were last modified
   - **At the end of the retention period**: Delete items automatically

1. Select **Next**.

1. On the **Review and finish** page select **Submit**.

1. Select **Done** once the policy is created.

You've created a retention policy that applies to content owned by privileged groups, retaining it for five years before deletion.
-->

## タスク 5 - SharePoint コンテンツを復元する

このタスクでは、SharePoint サイトから削除されたドキュメントの復元をシミュレートして、回復オプションを検証します。

1. 引き続き、**SC-401-CL1\admin** アカウントで Client 1 VM (SC-401-CL1) にログインし、Joni Sherman として Microsoft Purview にログインしている必要があります。

1. 左上隅にあるアプリ起動ツール (グリッド アイコン) を選択し、サブメニューから **[SharePoint]** を選択します。

   ![アクション メニューを表示するための省略記号がある場所を示すスクリーンショット。](../Media/sharepoint-app-launcher.png)

1. SharePoint ランディング ページで、「`Benefits`」を検索し、検索結果から **Benefits @ Contoso** を選択します。

1. 左側のサイド バーで **[ドキュメント]** を選択します。

1. **[ドキュメント]** ページで、**Vacation Policies.pptx** の左側にあるチェック ボックスをオンにし、操作バーから **[削除]** を選択します。

1. **[削除しますか?]** ダイアログで **[削除する]** を選択します。

1. 左側のサイド バーで **[ごみ箱]** を選択します。

1. **[ごみ箱]** ページで、**Vacation Policies.pptx** を右クリックし、**[復元]** を選択します。

1. 左側のサイド バーで **[ドキュメント]** を選択し、ファイルが復元されたことを確認します。

SharePoint サイトの削除したドキュメントを復元することができました。
