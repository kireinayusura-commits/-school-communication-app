# 🚀 学校コミュニケーションアプリ - セットアップガイド

このガイドでは、アプリケーションを最初から設定する手順を詳しく説明します。

## 📋 準備するもの

- Googleアカウント（Firebase用）
- GitHubアカウント（Vercel連携用、オプション）
- メールアドレス（管理者用）

## ⏱️ 所要時間

約30分〜1時間

---

## ステップ1: Firebaseプロジェクトの作成 (5分)

### 1-1. Firebase Consoleにアクセス

1. ブラウザで [https://console.firebase.google.com/](https://console.firebase.google.com/) を開く
2. Googleアカウントでログイン

### 1-2. 新しいプロジェクトを作成

1. **「プロジェクトを追加」** をクリック
2. プロジェクト名を入力
   - 例: `school-communication-app`
   - 注意: プロジェクト名は後で変更できません
3. **「続行」** をクリック
4. Google アナリティクスの有効化（推奨: オフ）
5. **「プロジェクトを作成」** をクリック
6. 作成完了を待つ（約30秒）
7. **「続行」** をクリック

---

## ステップ2: Firebase Authentication の設定 (3分)

### 2-1. Authentication を有効化

1. 左メニューから **「構築」** → **「Authentication」** を選択
2. **「始める」** をクリック
3. 「Sign-in method」タブを選択
4. **「メール/パスワード」** をクリック
5. **「有効にする」** をオンに変更
6. **「保存」** をクリック

### 2-2. 初期管理者ユーザーを作成

1. 「Users」タブを選択
2. **「ユーザーを追加」** をクリック
3. 管理者のメールアドレスを入力
   - 例: `admin@school.jp`
4. パスワードを入力（8文字以上推奨）
   - **このパスワードは必ずメモしてください**
5. **「ユーザーを追加」** をクリック
6. 作成されたユーザーの **UID をコピー**（後で使用）

---

## ステップ3: Firestore Database の設定 (5分)

### 3-1. Firestore を有効化

1. 左メニューから **「構築」** → **「Firestore Database」** を選択
2. **「データベースの作成」** をクリック
3. ロケーションを選択
   - 推奨: **asia-northeast1 (Tokyo)**
4. **「本番モードで開始」** を選択
5. **「有効にする」** をクリック
6. データベースの作成を待つ（約1分）

### 3-2. 初期管理者情報を Firestore に追加

1. 「データ」タブを選択
2. **「コレクションを開始」** をクリック
3. コレクションIDに `users` と入力
4. **「次へ」** をクリック
5. ドキュメントIDに **ステップ2-2でコピーしたUID** を貼り付け
6. フィールドを追加：

| フィールド | タイプ | 値 |
|----------|--------|-----|
| uid | string | （ステップ2-2のUID） |
| name | string | 管理者名（例: 山田太郎） |
| email | string | 管理者メールアドレス |
| role | string | admin |
| createdAt | timestamp | （現在時刻を選択） |

7. **「保存」** をクリック

### 3-3. セキュリティルールを設定

1. 「ルール」タブを選択
2. 既存のルールをすべて削除
3. プロジェクトの `firestore.rules` ファイルの内容をコピー
4. ルールエディタに貼り付け
5. **「公開」** をクリック

---

## ステップ4: Firebase設定情報の取得 (3分)

### 4-1. ウェブアプリを追加

1. プロジェクト概要（一番上の⚙️アイコン）をクリック
2. **「プロジェクトの設定」** を選択
3. 下にスクロールして **「アプリ」** セクションを表示
4. **「</>」（ウェブ）** アイコンをクリック
5. アプリのニックネームを入力
   - 例: `School Communication Web`
6. **「Firebase Hosting」** のチェックは外す
7. **「アプリを登録」** をクリック
8. 表示される `firebaseConfig` をコピー
   - **このコードは必ずメモしてください**

```javascript
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

9. **「コンソールに進む」** をクリック

---

## ステップ5: アプリケーションの設定 (5分)

### 5-1. index.html の設定

1. `index.html` をテキストエディタで開く
2. 以下の部分を探す（約35行目）：

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

3. ステップ4-1でコピーした値に置き換える
4. ファイルを保存

### 5-2. admin.html の設定

1. `admin.html` をテキストエディタで開く
2. 同じく約50行目の `firebaseConfig` を探す
3. ステップ4-1でコピーした値に置き換える
4. ファイルを保存

---

## ステップ6: Vercel へのデプロイ (10分)

### 6-1. Vercel アカウントの作成

1. [https://vercel.com/](https://vercel.com/) にアクセス
2. **「Sign Up」** をクリック
3. GitHubアカウントで登録（推奨）またはメールで登録

### 6-2. プロジェクトのデプロイ

#### 方法A: GitHub経由（推奨）

1. GitHubに新しいリポジトリを作成
2. プロジェクトファイルをプッシュ
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin <your-repo-url>
   git push -u origin main
   ```
3. Vercelで **「New Project」** をクリック
4. GitHubリポジトリを選択
5. **「Deploy」** をクリック

#### 方法B: 直接アップロード

1. Vercelで **「New Project」** をクリック
2. **「Import」** の下の **「Browse」** をクリック
3. プロジェクトフォルダを選択
4. **「Deploy」** をクリック

### 6-3. デプロイの確認

1. デプロイが完了するまで待つ（約1分）
2. **「Visit」** をクリックして動作確認
3. 提供されたURLをメモ
   - 例: `https://your-project.vercel.app`

---

## ステップ7: 動作確認 (5分)

### 7-1. 生徒向けアプリの確認

1. `https://your-project.vercel.app/` にアクセス
2. ステップ2-2で作成した管理者アカウントでログイン
   - メール: `admin@school.jp`
   - パスワード: （設定したパスワード）
3. ホーム画面が表示されることを確認

### 7-2. 管理者ツールの確認

1. `https://your-project.vercel.app/admin.html` にアクセス
2. 同じアカウントでログイン
3. ダッシュボードが表示されることを確認

### 7-3. 基本機能のテスト

#### 投稿を作成
1. 管理者ツールで「投稿管理」→「新規投稿」
2. タイトルと内容を入力して投稿
3. 生徒向けアプリで投稿が表示されることを確認

#### ユーザーを追加
1. 管理者ツールで「ユーザー管理」→「新規ユーザー」
2. テスト用生徒アカウントを作成
   - 名前: テスト生徒
   - メール: student1@school.jp
   - パスワード: test1234
   - 権限: 生徒
3. 生徒アカウントでログインできることを確認

#### イベントを作成
1. 管理者ツールで「イベント管理」→「新規イベント」
2. イベント情報を入力して登録
3. 生徒向けアプリのカレンダーで表示されることを確認

---

## 🎉 セットアップ完了！

おめでとうございます！学校コミュニケーションアプリの設定が完了しました。

### 次のステップ

1. **すべてのユーザーを登録**
   - 管理者ツールから生徒・職員のアカウントを作成
   - 各ユーザーにメールアドレスとパスワードを配布

2. **初期コンテンツを作成**
   - ウェルカムメッセージを投稿
   - 今月のイベントを登録
   - 使い方ガイドを投稿

3. **URLを共有**
   - 生徒・保護者: `https://your-project.vercel.app/`
   - 職員・管理者: `https://your-project.vercel.app/admin.html`

### 保存しておくべき情報

✅ Firebase プロジェクトID  
✅ 管理者のメールアドレスとパスワード  
✅ Vercel デプロイURL  
✅ firebaseConfig の内容

---

## 🆘 トラブルシューティング

### ログインできない

**原因**: Firebase設定が正しくない  
**解決**: `index.html` と `admin.html` の `firebaseConfig` を再確認

### データが表示されない

**原因**: Firestoreセキュリティルールが設定されていない  
**解決**: ステップ3-3を再実行

### 管理者ツールにアクセスできない

**原因**: ユーザーの権限が設定されていない  
**解決**: Firestoreの `users` コレクションで `role` が `admin` になっているか確認

---

## 📞 サポートが必要な場合

1. Firebase Console でエラーログを確認
2. ブラウザの開発者ツール（F12キー）でコンソールエラーを確認
3. README.md のトラブルシューティングセクションを参照

---

**最終更新**: 2025年12月17日