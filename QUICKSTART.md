# ⚡ クイックスタートガイド

最短でアプリを動かすための簡易ガイドです。詳細は `SETUP_GUIDE.md` を参照してください。

## 🎯 5ステップで開始

### ステップ1: Firebaseプロジェクトを作成 (5分)

1. [Firebase Console](https://console.firebase.google.com/) にアクセス
2. 「プロジェクトを追加」→ プロジェクト名を入力 → 作成

### ステップ2: 認証とデータベースを設定 (5分)

1. **Authentication**
   - 「構築」→「Authentication」→「始める」
   - 「メール/パスワード」を有効化
   - 「ユーザーを追加」で管理者アカウント作成（UIDをメモ）

2. **Firestore Database**
   - 「構築」→「Firestore Database」→「作成」
   - ロケーション: `asia-northeast1 (Tokyo)`
   - 「本番モードで開始」

3. **管理者情報を追加**
   - Firestoreで「コレクションを開始」
   - コレクションID: `users`
   - ドキュメントID: （上記のUID）
   - フィールド追加:
     ```
     uid: string = （UID）
     name: string = "管理者"
     email: string = "admin@school.jp"
     role: string = "admin"
     createdAt: timestamp = （現在時刻）
     ```

4. **セキュリティルールを設定**
   - 「ルール」タブ
   - `firestore.rules` の内容をコピー＆貼り付け
   - 「公開」をクリック

### ステップ3: Firebase設定を取得 (2分)

1. プロジェクト設定（⚙️）→「アプリ」セクション
2. 「</>」（ウェブ）をクリック
3. アプリ登録後、`firebaseConfig` をコピー

### ステップ4: コードに設定を反映 (3分)

`index.html` と `admin.html` の両方で以下を置き換え:

```javascript
// 約35行目と50行目あたり
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",           // ← ここを変更
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

### ステップ5: Vercelにデプロイ (5分)

1. [Vercel](https://vercel.com/) にアクセス＆サインアップ
2. 「New Project」をクリック
3. GitHubリポジトリを連携 or ファイルをアップロード
4. 「Deploy」をクリック
5. デプロイ完了後、URLにアクセス

## 🎉 完了！

以下のURLでアクセス可能になりました：

- **生徒向けアプリ**: `https://your-project.vercel.app/`
- **管理者ツール**: `https://your-project.vercel.app/admin.html`

ステップ2で作成した管理者アカウントでログインしてください。

## 📝 次にやること

### 1. ユーザーを追加
管理者ツールの「ユーザー管理」から職員・生徒を追加

### 2. 投稿を作成
管理者ツールの「投稿管理」から最初のお知らせを投稿

### 3. イベントを登録
管理者ツールの「イベント管理」から行事を登録

### 4. URLを共有
各ユーザーにURLとログイン情報を配布

## 🆘 トラブルシューティング

### ログインできない
→ Firebase設定（firebaseConfig）が正しいか確認

### データが表示されない
→ Firestoreセキュリティルールが設定されているか確認

### 管理者ツールが使えない
→ ユーザーの `role` が `admin` になっているか確認

## 📚 詳細ガイド

より詳しい情報は以下を参照：

- **詳細セットアップ**: `SETUP_GUIDE.md`
- **使い方ガイド**: `USER_GUIDE.md`
- **プロジェクト概要**: `README.md`

---

**所要時間**: 約20分  
**難易度**: ⭐⭐☆☆☆（初級）