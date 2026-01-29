---
lab:
  title: 演習 3 - 秘密度ラベルを作成して管理する
  module: Module 1 - Implement Information Protection
---

# ラボ 1 - 演習 3 - 秘密度ラベルを作成して管理する

Contoso Ltd. の情報セキュリティ管理者である Joni Sherman は、組織全体のデータ保護を強化するために、最新の秘密度ラベル付けフレームワークをデプロイしています。 Joni は、暗号化、自動ラベル付け、二重キー暗号化 (DKE) など、コンテンツを分類して保護するための秘密度ラベル グループとラベルを作成して公開します。 また、Microsoft Purview と Microsoft Defender for Cloud Apps を統合して、クラウドの場所に格納されているファイルにデータ保護制御を拡張します。

**タスク**:

1. 秘密度ラベルのサポートを有効にする
1. ラベル グループを作成する
1. 子ラベルを作成する
1. ラベルを発行する
1. 自動ラベル付けを構成する
1. 機密コンテンツの DKE ラベルを作成して発行する
1. Microsoft Purview と Defender for Cloud Apps との統合を有効にする
1. 外部共有ファイルにラベルを付けるファイル ポリシーを作成する

**推定所要時間:** 60 ～ 90 分

## タスク 1 - 秘密度ラベルのサポートを有効にする

このタスクでは、秘密度ラベルの共同編集を有効にします。これにより、SharePoint と OneDrive のファイルの秘密度ラベルも有効になります。

1. 引き続き、**SC-401-CL1\admin** アカウントで Client 1 VM (SC-401-CL1) にログインし、Joni Sherman として Microsoft Purview にログインしておく必要があります。

1. **Microsoft Edge** を開き、`https://purview.microsoft.com` に移動します。

1. 左側のナビゲーションで、**[設定]** > **[Microsoft Information Protection]** を選択します。

1. **[Information Protection の設定]** ページで、**[秘密度ラベルを持つファイルの共同編集]** タブが表示されていることを確認します。

1. **[秘密度ラベルのあるファイルの共同編集を有効にする]** チェック ボックスをオンにします。

1. 画面の下部にある **[適用]** を選択します。

これで、SharePoint および OneDrive での秘密度ラベルのサポートが有効になりました。

<!--

## Task 2 – Migrate to the modern label scheme

With sensitivity label support enabled, you'll confirm that the tenant uses the modern label scheme. If it still uses the classic scheme, you'll migrate to the modern one to ensure new labels and policies use the latest configuration model.

1. You should still be logged into Client 1 VM (SC-401-CL1) as the **SC-401-CL1\admin** account and logged into Microsoft Purview as Joni Sherman.

1. In the left navigation, select **Information Protection** > **Sensitivity labels**.

1. On the **Sensitivity labels** page, review the information box for **Migrate to the modern label scheme**, then select **Get started**.

1. In the **Migrate to the modern label scheme** flyout, review the summary to understand how existing labels and policies will be affected.

1. At the bottom of the flyout, select **Migrate**.

1. In the **Confirm migration to modern label scheme** dialog box, select **Confirm migration**.

    > [!Note] **Note**: The migration process might take a few minutes to complete.

You've successfully migrated your tenant to the modern sensitivity label scheme. New labels and policies you create will now use the modern configuration experience.

-->
## タスク 2 – ラベル グループを作成する

このタスクでは、内部秘密度ラベルを整理するためのラベル グループを作成します。 ラベル グループは、部署や事業単位の分類など、関連するラベルのコンテナーとして機能します。

1. 引き続き Client 1 VM (SC-401-CL1) に **SC-401-CL1\admin** アカウントでログインしている必要があります。

1. **Microsoft Edge** で、 `https://purview.microsoft.com` に移動します。

1. Microsoft Purview ポータルの左サイドバーで、**[ソリューション]** を選択してから、**[Microsoft Information Protection]** を選択します。

1. **[Microsoft Information Protection]** ページの左サイドバーで、**[秘密度ラベル]** を選択します。

1. **[秘密度ラベル]** ページで、**[+ 作成]**  >  **[ラベル グループ]** の順に選択します。

1. **[新しいラベル グループ]** の構成が開始されます。 **[このラベル グループの基本的な詳細を指定します]** で、次のように入力します。

    - **名前**: `Internal`
    - **表示名**: `Internal`
    - **ユーザー向けの説明**: `Internal sensitivity label.`
    - **管理者向けの説明**: `Internal sensitivity label group for Contoso.`

1. [**次へ**] を選択します。

1. **[設定を確認して完了]** ページで、**[ラベル グループの作成]** を選択します。

1. **[ラベル グループが正常に作成されました]** ページで、**[ラベルをまだ作成しない]** を選択し、**[完了]** を選択します。

内部用のラベル グループを作成しました。 このグループは、特定の部署またはデータ カテゴリの関連ラベルを管理するのに役立ちます。

## タスク 3 – 子ラベルを作成する

ラベル グループを作成したので、人事関連コンテンツ用の子ラベルを追加します。 このラベルは、暗号化とコンテンツ マーキングを適用して、承認されていないアクセスから人事データを保護します。

1. **[秘密度ラベル]** ページで、**[内部]** 秘密度ラベル グループを見つけます。 その横にある垂直の省略記号 (**[...]**) を選択し、ドロップダウン メニューから **[+ グループにラベルを作成する]** を選択します。

    ![秘密度ラベルのグループにラベルを作成するための [アクション] メニューを示すスクリーンショット。](../Media/create-label-in-group.png)

1. **[新しい秘密度ラベル]** ウィザードが起動します。 **[このラベルの基本的な詳細を指定します]** で、次のように入力します。

   - **名前**: `Employee data (HR)`
   - **表示名**: `Employee data (HR)`
   - **ユーザー向けの説明**: `This HR label is the default label for all specified documents in the HR Department.`
   - **管理者向けの説明**: `This label is created in consultation with Ms. Jones (Head of the HR department). Contact her if you need to change the label settings.`

1. [**次へ**] を選択します。

1. **[このラベルの範囲を定義する]** ページで、**[ファイルと他のデータ資産]**、**[メール]** の順に選択します。 **[会議]** のチェック ボックスがオンになっている場合は、選択が解除されていることを確認します。

1. [**次へ**] を選択します。

1. **[選択した項目の種類の保護設定を選択します]** ページで、**[アクセスの制御]**、**[コンテンツマーキングを適用する]** オプション、**[次へ]** の順に選択します。

1. **[アクセスの制御]** ページで **[アクセス制御設定の構成]** を選択します。

1. 次のオプションを使用して暗号化設定を構成します。

   - **アクセス許可を今すぐ割り当てますか、それともユーザーが決定するようにしますか?** :アクセス許可を今すぐ割り当てる
   - **コンテンツに対するユーザーのアクセス許可の期限**:許可しない
   - **オフライン アクセスを許可する**:数日のみ
   - **ユーザーがコンテンツへオフラインでアクセスできる日数**:15
   - **[アクセス許可の割り当て]** リンクを選択します。 **[アクセス許可の割り当て]** ポップアップ パネルで **[+ 認証されたユーザーの追加]** を選択し、**[保存]** を選択してこの設定を適用します。

1. **[アクセスの制御]** ページで、**[次へ]** を選択します。

1. **[コンテンツ マーキング]** ページで、トグルを選択して**コンテンツ マーキング**を有効にします。

1. 次のマーキング タイプごとに、チェック ボックスをオンにし、編集アイコンを選択してテキストを入力します。

   |マーキング タイプ|Text|
   |:---|:---|
   |透かしの追加|`INTERNAL USE ONLY`|
   |ヘッダーの追加|`Internal Document`|
   |フッターの追加|`Contoso Confidential`|

1. [**次へ**] を選択します。

1. **[ファイルとメールの自動ラベル付け]** ページで、**[次へ]** をクリックします。

1. **[グループとサイトの保護設定を定義]** ページで、**[次へ]** をクリックします。

1. **[設定を確認して完了]** ページで、**[ラベルを作成]** を選択します。

1. **[秘密度ラベルが作成されました]** ページ上で、**[ポリシーをまだ作成しない]** を選択し、**[完了]** を選択します。

"内部" ラベル グループ内に子ラベルを作成しました。 このラベルは、人事ドキュメントに暗号化とコンテンツのマーキングを適用して、機密データを簡単に識別し、ポリシーで保護できるようにします。

## タスク 4 – ラベルを発行する

次に、人事部門のユーザーがドキュメントにラベルを適用できるように、"内部" ラベル グループから人事ラベルを発行します。

1. Client 1 VM (SC-401-CL1) には **SC-401-cl1\admin** アカウントでログインし、Microsoft Purview には **Joni Sherman** としてログインしておく必要があります。

1. **Microsoft Edge** で、Microsoft Purview ポータルのタブがまだ開かれているはずです。 そうでない場合は、**`https://purview.microsoft.com`** > **[ソリューション]** > **[Microsoft Information Protection]** > **[秘密度ラベル]** に移動します。

1. **[秘密度ラベル]** ページで **[ラベルを発行]** を選択します。

1. 秘密度ラベルの発行構成が起動します。

1. **[発行する秘密度ラベルを選ぶ]** ページで、**[発行する秘密度ラベルを選ぶ]** リンクを選択します。

1. **[発行する秘密度ラベル]** ポップアップ パネルで、**[内部/従業員データ (人事)]** チェックボックスをオンにし、ポップアップ ページの下部にある **[追加]** を選択します。

1. **[発行する秘密度ラベルを選ぶ]** ページに戻り、**[次へ]** を選択します。

1. [**管理単位を割り当てる**] ページで、[**次へ**] を選択します。

1. **[ユーザーとグループに発行]** ページで **[次へ]** を選択します。

1. **[ポリシー設定]** ページで **[次へ]** を選択します。

1. **[ドキュメントの既定の設定]** で、 **[次へ]** を選択します。

1. **[メールの既定の設定]** で、 **[次へ]** を選択します。

1. **[会議と予定表イベントの既定の設定]** で、 **[次へ]** を選択します。

1. **[Fabric および Power BI コンテンツの既定の設定]** ページで、**[次へ]** を選択します。

1. **[ポリシーの名前の設定] ページ**で、以下を入力します。

   - **名前**: `Internal HR employee data`

   - **秘密度ラベル ポリシーの説明を入力してください**: `This HR label is to be applied to internal HR employee data.`

1. [**次へ**] を選択します。

1. **[確認と完了]** ページで、**[送信]** を選択します。

1. **[新しいポリシーが作成されました]** ページで、 **[完了]** を選択して、ラベル ポリシーの発行を完了します。

"内部" ラベル グループとその人事ラベルを発行したので、ユーザーは、それを人事ドキュメントに適用できるようになりました。 ポリシーがサービス全体に伝達されるまでに最大 24 時間かかる場合があります。

## タスク 5 – 自動ラベル付けを構成する

次に、財務データの子ラベルを作成し、クレジット カードや銀行の支店番号などの財務識別子を含むコンテンツに自動的に適用されるように構成します。

1. 引き続き Client 1 VM (SC-401-CL1) に **SC-401-cl1\admin** アカウントでログインしている必要があります。

1. **Microsoft Edge** で、 `https://purview.microsoft.com` に移動し、**Joni Sherman** として Microsoft Purview ポータルにログインします。

1. Microsoft Purview ポータルで、**[ソリューション]**  >  **[Information Protection]**  >  **[秘密度ラベル]** の順に選択します。

1. **[秘密度ラベル]** ページで、**Internal** 秘密度ラベルを見つけます。 垂直の省略記号 (**[...]**) を選択し、ドロップダウン メニューから **[+ グループにラベルを作成する]** を選択します。

1. **[このラベルの基本的な詳細を指定します]** で、次のように入力します。

   |詳細|Text|
   |---|---|
   |**名前**|`Financial Data`|
   |**表示名**|`Financial Data`|
   |**ユーザー向けの説明**|`This content contains financial data that must be labeled and protected.`|
   |**管理者向けの説明**|`This label is used for content that includes sensitive financial identifiers.`|

1. [**次へ**] を選択します。

1. **[このラベルの範囲を定義する]** ページで、**[ファイルと他のデータ資産]**、**[メール]** の順に選択します。 **[会議]** のチェック ボックスがオンになっている場合は、選択が解除されていることを確認します。

1. [**次へ**] を選択します。

1. **[選択した項目の種類の保護設定を選択します]** ページで、 **[次へ]** を選択します。

1. **[ファイルとメールの自動ラベル付け]** ページで、**[ファイルとメールの自動ラベル付け]** を有効に設定します。

1. **[これらの条件に一致するコンテンツを検出する]** セクションで、**[+条件の追加]** >  を選択し、**[コンテンツに含まれている]** を選択します。

1. **[コンテンツに含まれている]** セクションで、**[追加]**  >  **[機密情報の種類]** の順に選択します。

1. **[機密情報の種類]** ポップアップ ページで、次の機密情報の種類を検索して選択します。

   - `Credit Card Number`
   - `ABA Routing Number`
   - `SWIFT Code`

1. **[追加]** を選択します。

1. [**ファイルとメールの自動ラベル付け**] ページで、[**次へ**] をクリックします。

1. **[グループとサイトの保護設定を定義]** ページで、**[次へ]** をクリックします。

1. **[設定を確認して完了]** ページで、**[ラベルを作成]** を選択します。

1. **[秘密度ラベルが作成されました]** ページで、**[機密コンテンツにラベルを自動的に適用する]** を選択し、**[完了]** を選択します。

1. **[自動ラベル付けポリシーの作成]** ポップアップ ページで、**[ポリシーを確認する]** を選択します。

1. **[自動ラベル付けポリシーの名前]** ページで、既定値のままにして、**[次へ]** を選択します。

1. **[自動適用するラベルを選択する]** ページで、_Internal/Financial Data_ ラベルが表示されていることを確認し、**[次へ]** を選択します。

1. [**管理単位を割り当てる**] ページで、[**次へ**] を選択します。

1. **[ラベルを適用する場所の選択する]** ページで、次のオプションを選択します。

   - Exchange メール
   - SharePoint サイト
   - OneDrive アカウント

1. [**次へ**] を選択します。

1. **[共通ルールまたは詳細ルールを設定する]** ページで、デフォルトの **[共通ルール]** を選択したまま、**[次へ]** を選択します。

1. **[すべての場所のコンテンツのルールを定義する]** ページで、_[Financial Data 件のルール]_ を展開して期待されるルールが定義されていることを確認してから、**[次へ]** を選択します。

1. **[追加のメール設定]** ページで、**[次へ]** を選択します。

1. **[ポリシーを今すぐまたは後でテストするかを決定する]** ページで、**[シミュレーション モードでポリシーを実行する]** を選択し、**[シミュレーションで 7 日経過しても変更されていない場合は、ポリシーを自動的にオンにします]** のチェックボックスをオンにします。

1. [**次へ**] を選択します。

1. **[確認と完了]** ページで、**[ポリシーを作成]** を選択します。

1. **[自動ラベル付けポリシーが作成されました]** ページで、**[完了]** を選択します。

財務データ用の子ラベルを作成し、財務情報を含むコンテンツを検出してラベル付けする自動ラベル付けポリシーを構成しました。

## タスク 6 - 機密コンテンツの DKE ラベルを作成して発行する

次に、"内部" グループに、二重キー暗号化 (DKE) と動的透かしを使用して機密の法的コンテンツを保護する子ラベルを作成します。

1. 引き続き Client 1 VM (SC-401-CL1) に **SC-401-cl1\admin** アカウントでログインしている必要があります。

1. **Microsoft Edge** で、 `https://purview.microsoft.com` に移動し、**Joni Sherman** として Microsoft Purview ポータルにログインします。

1. Microsoft Purview ポータルで、**[ソリューション]**  >  **[Information Protection]**  >  **[秘密度ラベル]** の順に選択します。

1. **[秘密度ラベル]** ページで、**[内部]** 秘密度ラベル グループを見つけます。 垂直の省略記号 (**[...]**) を選択し、ドロップダウン メニューから **[+ グループにラベルを作成する]** を選択します。

1. **[このラベルの基本的な詳細を指定します]** で、次のように入力します。

   |詳細|Text|
   |---|---|
   |**名前**|`Confidential Legal`|
   |**表示名**|`Confidential Legal`|
   |**ユーザー向けの説明**|`Use this label for highly sensitive legal content that must be encrypted using Double Key Encryption.`|
   |**管理者向けの説明**|`Label configured with DKE and dynamic watermarking for highly sensitive legal content.`|

1. [**次へ**] を選択します。

1. **[このラベルの範囲を定義する]** ページで、**[ファイルと他のデータ資産]**、**[メール]** の順に選択します。 **[会議]** のチェック ボックスがオンになっている場合は、選択が解除されていることを確認します。

1. **[選択した項目の種類の保護設定を選択します]** ページで、**[アクセスの制御]** を選択し、**[次へ]** を選択します。

1. **[アクセスの制御]** ページで **[アクセス制御設定の構成]** を選択します。

1. 次のオプションを使用して暗号化設定を構成します。

   - **アクセス許可を今すぐ割り当てますか、それともユーザーが決定するようにしますか?** :アクセス許可を今すぐ割り当てる

   - **コンテンツに対するユーザーのアクセス許可の期限**: ラベル適用後の日数

   - **ラベルが適用されてから、アクセスの期限が切れるまでの日数**: 5

   - **オフライン アクセスを許可する**: 許可しない

   - **[アクセス許可の割り当て]** リンクを選択します。 **[アクセス許可の割り当て]** ポップアップ パネルで、**[+ ユーザーまたはグループの追加]** を選択します。

   - **[ユーザーまたはグループの追加]** ポップアップ ページで、`Legal Team`と`Joni Sherman`を検索して選択し、**[追加]** を選択します。

   - **[アクセス許可の割り当て]** ページで、**[保存]** を選択します。

1. **[アクセスの制御]** ページで、**[ダイナミックウォーターマーキングを使用する]** のチェックボックスをオンにし、**[テキストのカスタマイズ (オプション)]** を選択します。

1. **[透かしにカスタム テキストを追加する (オプション)]** ページで、「`Confidential`」と入力し、**[UPN]** と **[タイムスタンプ]** を選択します。

1. ポップアップ ページの下部にある **[保存]** を選択します。

1. **[アクセス制御]** ページに戻り、**[二重キー暗号化を使用する]** のチェック ボックスをオンにし、二重キー暗号化サービスの URL として「`https://testingdke1.azurewebsites.net/Test`」と入力します。

1. [**次へ**] を選択します。

1. **[ファイルとメールの自動ラベル付け]** ページで、**[次へ]** をクリックします。

1. **[グループとサイトの保護設定を定義]** ページで、**[次へ]** をクリックします。

1. **[設定を確認して完了]** ページで、**[ラベルを作成]** を選択します。

1. **[秘密度ラベルが作成されました]** ページで、**[ユーザーのアプリにラベルを発行する]** を選択し、**[完了]** を選択します。

1. **[ラベルの発行]** ポップアップ ページで、**[新しいラベル ポリシーを作成する]** を選択します。

1. **[発行する秘密度ラベルを選ぶ]** ページで、**[発行する秘密度ラベルを選ぶ]** を選択し、**[Internal/Confidential Legal]** ラベルを追加し、**[追加]** を選択します。

1. [**次へ**] を選択します。

1. [**管理単位を割り当てる**] ページで、[**次へ**] を選択します。

1. **[ユーザーとグループに発行する]** ページで、既定値を選択したままにし、**[次へ]** を選択します。

1. **[ポリシーの設定]** ページで、**[ユーザーは、ラベルを削除したり、ラベルの分類を下位のものにしたりする場合に、理由を示す必要がある]** チェックボックスをオンにして、**[次へ]** を選択します。

1. **[ドキュメントの既定の設定]** ページで、**[次へ]** を選択します。

1. **[メールの既定の設定]** ページで、**[次へ]** を選択します。

1. **[会議と予定表イベントの既定の設定]** ページで、**[次へ]** を選択します。

1. **[Fabric および Power BI コンテンツの既定の設定]** ページで、**[次へ]** を選択します。

1. **[ポリシーの名前の設定] ページ**で、以下を入力します。

   - **名前**: `Confidential Legal`

   - **説明**: `Enables manual use of the DKE label for confidential content accessible by Legal.`

1. [**次へ**] を選択します。

1. **[確認と完了]** ページで、**[送信]** を選択します。

1. **[新しいポリシーが作成されました]** ページで、**[完了]** を選択します。

二重キー暗号化と動的透かしを使用して子ラベルを作成し、発行しました。 このラベルは、許可されているユーザーへのアクセスを制限し、分類のダウングレードの正当な理由を強制します。

## タスク 7 - Defender for Cloud Apps で Microsoft Purview との統合を有効にする

秘密度ラベルを作成して公開したので、次に、Microsoft Purview と Microsoft Defender for Cloud Apps を統合します。 この統合により、Defender で、ファイルの秘密度ラベルをスキャンし、ファイルの監視を適用できるようになります。

1. Client 1 VM (SC-401-CL1) には **SC-401-CL1\admin** でログインし、Joni Sherman としてログインしておく必要があります。

1. **Microsoft Edge** を開き、`https://security.microsoft.com` に移動して **Microsoft Defender** を開きます。

1. 左側のナビゲーションで **[設定]** を選択し、**[クラウド アプリ]** を選択します。

1. 左側のウィンドウの **[Microsoft Information Protection]** セクションで、**[Microsoft Information Protection]** を選択します。

1. **[Microsoft Information Protection]** ページで、ページで使用できる両方のチェック ボックスをオンにします。

    ![Defender ポータルで Information Protection が有効になっている両方のチェック ボックスを示すスクリーンショット。](../Media/defender-information-protection-settings.png)

   - **Microsoft Information Protection の秘密度ラベルとコンテンツ検査警告のために新しいファイルを自動的にスキャンします**

      Defender for Cloud Apps で、Microsoft Purview からの秘密度ラベルとコンテンツ検査の警告について、新しいファイルまたは変更されたファイルを自動的にスキャンできるようにします。

   - **このテナントからの Microsoft Information Protection 秘密度ラベルとコンテンツ検査警告のためのファイルをスキャンのみ**

      スキャンを、独自の組織で作成されたラベルと警告に制限します。 外部テナントによって適用されるラベルは無視されます。

1. **[保存]** を選んで設定を適用します。

1. 左側ペインの **[Microsoft Information Protection]** セクションで、**[ファイル]** を選択します。

1. **[ファイル]** ページで、**[ファイルの監視を有効にする]** をオンにします。

1. **[保存]** を選んで設定を適用します。

Defender for Cloud Apps で Microsoft Purview 統合を有効にしました。 Defender で秘密度ラベルを検出し、ポリシー評価とガバナンス アクションのためにファイルを監視できるようになりました。

<!---

NOTE - Reworking task 8 due to tenant issue

## Task 8 – Create a file policy to auto-label externally shared files

Now that label scanning is enabled, you'll create a file policy that applies the **Highly Confidential - Project - Falcon** sensitivity label to files in the Mark 8 Project folders that are shared outside your organization. This policy is categorized as DLP because it protects sensitive data from unintended exposure.

1. In **Microsoft Defender**, navigate to **Cloud apps** > **Policies** > **Policy management**.

1. Select the **Information protection** tab, then select **Create policy** > **File policy**.

    ![Screenshot showing where to navigate to create a file policy in Microsoft Defender.](../Media/file-policy-defender.png)

1. On the **Create file policy** page, configure:

   - **Policy name**: `Auto-label external sharing for Project Falcon files`

   - **Policy severity**: **High**

   - **Category**: **DLP**

   - **Apply to**: **Selected folders**

      - Select **Add folder(s)**, then search for `Project` in the **File name** field.

      - Select the checkbox for the **Mark 8 Project Team Notebook** and **Mark 8 Project team** SharePoint folders.

      - Select **Done** to close the **Select a folder** window.

   - In the **Files matching all of the following section**:

      - For the first filter, configure the dropdowns to: **Access level equals external**

      - For the second filter, configure the dropdowns to: **Last modified after (date)** and use today's date

          ![Screenshot showing the filter settings in Defender.](../Media/configure-file-policy-filter.png)

   - Under **Governance actions**, expand **Microsoft OneDrive for Business**:

      - Select the checkbox for **Apply sensitivity label**

      - In the dropdown select **Highly Confidential-Project - Falcon**

   - Repeat the same process for **Microsoft SharePoint Online**

      - Select the checkbox for **Apply sensitivity label**

      - Select **Highly Confidential-Project - Falcon** from the dropdown

1. Select **Create** to finish creating the file policy.

You've created a file policy that applies a highly confidential sensitivity label to externally shared files located in the Mark 8 Project folders in SharePoint and OneDrive. Once a matching file is detected, Defender for Cloud Apps will apply the label automatically.
-->

## タスク 8 - 外部共有ファイルにラベルを付けるファイル ポリシーを作成する

最後に、外部で共有されているファイルに秘密度ラベルを自動的に適用するファイル ポリシーを作成します。 これにより、機密コンテンツは、組織外で共有されている場合でも確実に保護されます。

1. **Microsoft Defender** で、**[クラウド アプリ]**、**[ポリシー]**、**[ポリシー管理]** の順に移動します。

1. **[Microsoft Information protection]** タブを選択し、**[ポリシーの作成]**、**[ファイル ポリシー]** の順に選択します。

    ![Microsoft Defender でファイル ポリシーを作成する場所を示すスクリーンショット。](../Media/file-policy-defender.png)

1. **[ファイル ポリシーの作成]** ページで、次の構成を行います。

   - **ポリシー名**: `Auto-label externally shared files`

   - **ポリシー重大度**: **高**

   - **カテゴリ**: **DLP**

   - **[次のすべてに一致するファイル] セクション**で、次を行います。

      - 最初のフィルターは、ドロップダウンを **[アクセス レベルが外部と等しい]** に設定します。

      - 2 番目のフィルターでは、ドロップダウンを次のように構成します。 **最終変更後 (日付)** および今日の日付を使用します。

          ![Defender でフィルター設定を示すスクリーンショット。](../Media/configure-file-policy-filter.png)

   - **ガバナンス アクション**で、**Microsoft OneDrive for Business** を次のように展開します。

      - **[秘密度ラベルの適用]** のチェック ボックスをオンにする

      - ドロップダウンから **[極秘 - 指定ユーザー]** を選択します。

   - **Microsoft SharePoint Online** に対して同じプロセスを繰り返す

      - **[秘密度ラベルの適用]** のチェック ボックスをオンにする

      - ドロップダウンから **[極秘 - 指定ユーザー]** を選択します

1. **[作成]** を選択してファイル ポリシーの作成を完了します。

外部共有ファイルに秘密度ラベルを適用するファイル ポリシーを作成しました。 このポリシーにより、情報保護戦略が、クラウドに保存されたコンテンツに拡張されます。
