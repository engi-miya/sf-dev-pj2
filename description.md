```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Salesforce as Salesforce（アプリ）
    participant Google as Google（認証サーバー）

    User->>Salesforce: Google にログインしたい！
    Salesforce->>Google: 「Google カレンダー API を使う許可」を求める
    Google->>User: ログイン画面を表示
    User->>Google: Google にログインして「許可」ボタンを押す
    Google->>Salesforce: 認証コード (`authCode`) を発行
    Salesforce->>Google: 認証コード (`authCode`) をアクセストークン (`access_token`) に交換
    Google->>Salesforce: アクセストークン (`access_token`) を発行
    Salesforce->>Google: `access_token` を使ってカレンダーのデータを取得・更新！
```

## OAuth2.0

```mermaid
sequenceDiagram
    box salesforce
        participant Salesforce as クライアントアプリケーション
    end
    box google
        participant Google as 認可サーバー
        participant CalendarAPI as リソースサーバー
    end
    box user
        participant User as ユーザー
    end

    Salesforce->>Google: 認可リクエスト（認可コードグラント）
    activate Google
    Google->>User: Googleアカウントへのアクセス許可をリクエスト
    activate User
    User->>Google: アクセスを許可
    deactivate User
    Google->>Salesforce: 認可コードを発行
    deactivate Google
    Salesforce->>Google: 認可コードとクライアント情報を交換
    activate Google
    Google->>Salesforce: アクセストークンを発行
    deactivate Google
    Salesforce->>CalendarAPI: アクセストークンを使ってカレンダーAPIにアクセス
    activate CalendarAPI
    CalendarAPI->>Salesforce: カレンダー登録結果を返す
    deactivate CalendarAPI
```
図の説明

Salesforce（クライアントアプリケーション）が、Google（認可サーバー）に認可コードグラントをリクエストします。
Googleは、ユーザーにSalesforceがGoogleアカウントにアクセスすることを許可するかどうかを確認します。
ユーザーが許可すると、GoogleはSalesforceに認可コードを発行します。
Salesforceは、認可コードとクライアント情報（クライアントID、クライアントシークレットなど）をGoogleに渡して、アクセストークンを要求します。
Googleは、アクセストークンをSalesforceに発行します。
Salesforceは、アクセストークンを使ってGoogle Calendar API（リソースサーバー）にアクセスし、カレンダー登録を行います。
Google Calendar APIは、カレンダー登録結果をSalesforceに返します。
補足

認可コードグラントは、Webアプリケーションなどでよく使われる、最も一般的なOAuth 2.0のグラントタイプです。
アクセストークンは、クライアントアプリケーションがユーザーの代わりにリソースサーバーにアクセスするための鍵のようなものです。
アクセストークンには有効期限があり、期限が切れたら再発行する必要があります。
Web Senpaiからのアドバイス

OAuth 2.0の認可コードグラントは、セキュリティと利便性を両立させるための優れた仕組みです。SalesforceとGoogle Calendar APIを連携させる際には、ぜひOAuth 2.0を活用してください。

何か質問があれば、遠慮なく聞いてくださいね




## SF認証プロバイダーのフロー
```mermaid
graph TD;
    A[Salesforce 認証プロバイダー] -->|Google の認証エンドポイントを設定| B[Google OAuth 認証]
    B -->|ユーザーがログイン| C[認証コード発行]
    C -->|アクセストークン取得| D[Salesforce から Google API にアクセス]
    D -->|カレンダーのデータ取得・更新| E[Google カレンダー]

```