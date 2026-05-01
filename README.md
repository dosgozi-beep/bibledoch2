# ✝ バイブルドッヂ - ダビデ vs ゴリアテ vs サウロ

聖句の真偽を見抜いて戦う、ドッジボール×聖書クイズゲーム。

## 遊び方

1. `index.html` をブラウザで開く（GitHub Pagesでアクセス）
2. 🔊ボタンをタップして音楽ON
3. 「戦いを始める」で対戦開始

## ルール

- ターン開始時、ダビデと敵が掛け合いをする
- ボールを投げる時、聖句を叫ぶ
- 受ける側は **〇(本物)** か **×(偽物)** で判定
- **正解**でボールを避ける → 相手に **10ダメージ** (反撃)
- **判定ミス** → 自分が **CRITICAL 40ダメージ** を受ける
- 先に相手のHPをゼロにしたら勝利

## ストーリー

1. **第1幕**：少年ダビデ vs 巨人ゴリアテ
2. **第2幕**：嫉妬に狂ったサウル王が立ちはだかる（高難易度）

## 🏆 ランキング機能のセットアップ（Firebase）

ランキングを使うには **Firebase Realtime Database**（無料）の設定が必要です。

### 手順

#### 1. Firebaseプロジェクト作成
1. [Firebase Console](https://console.firebase.google.com/) を開く
2. 「**プロジェクトを追加**」をクリック
3. プロジェクト名（例: `bible-dodge`）を入力 → 続行
4. Google Analytics は不要なのでオフにできます → 続行 → プロジェクト作成

#### 2. Realtime Database を有効化
1. 左メニュー「**ビルド**」→「**Realtime Database**」を選択
2. 「**データベースを作成**」をクリック
3. リージョン（**asia-southeast1** 推奨、または近い場所）を選択 → 次へ
4. **「テストモードで開始」** を選択 → 有効にする
   - ⚠️ テストモードは30日後に書き込み不可になります。後述の「ルール設定」で本番モードに切り替え可能

#### 3. Webアプリを登録して設定情報を取得
1. 左上の歯車⚙️ → **プロジェクトの設定**
2. 下部の「**マイアプリ**」セクション → **`</>` Webアイコン**をクリック
3. アプリのニックネーム（例: `bible-dodge-web`）を入力 → 「アプリを登録」
4. 表示される `firebaseConfig` の中身をコピー

例：
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyA...",
  authDomain: "bible-dodge.firebaseapp.com",
  databaseURL: "https://bible-dodge-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "bible-dodge",
  storageBucket: "bible-dodge.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123def456"
};
```

#### 4. `index.html` に貼り付け

`index.html` をエディタで開き、`FIREBASE_CONFIG` で検索して見つかる部分：

```javascript
const FIREBASE_CONFIG = {
  apiKey: "YOUR_API_KEY_HERE",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  ...
};
```

ここに**手順3でコピーした自分の設定値**を上書きして保存してください。

#### 5. Realtime Database のセキュリティルールを設定（推奨）

テストモードの30日制限を解除し、不正書き込みを防ぐため：

1. Firebase Console → Realtime Database → 「**ルール**」タブ
2. 以下を貼り付けて「**公開**」：

```json
{
  "rules": {
    "rankings": {
      ".read": true,
      ".write": "newData.child('nickname').isString() && newData.child('nickname').val().length <= 20 && newData.child('score').isNumber() && newData.child('score').val() <= 999999"
    }
  }
}
```

これで誰でもランキング閲覧/登録できるが、不正な値の書き込みは拒否されます。

#### 6. GitHub Pagesにアップロード
書き換えた `index.html` を上書きコミット → 数分でランキング機能が動作します。

### 🎮 スコア計算

| 要素 | 点数 |
|---|---|
| 正解1問 | +100点 |
| 不正解1問 | -30点 |
| 残りHP | ×10 |
| ゴリアテ撃破 | +500点 |
| サウロ撃破 | +1500点 |
| 正答率90%以上 | +500点 |
| 正答率75%以上 | +200点 |

## ファイル構成

```
.
├── index.html              # ゲーム本体
├── README.md
├── david.png               # ダビデのキャラクター画像
├── goliath.png             # ゴリアテのキャラクター画像
├── saul.png                # サウル王のキャラクター画像
├── background.jpg          # エラの谷の戦場背景
├── title.mp3               # タイトル画面BGM
├── battle.mp3              # バトルBGM
├── victory.mp3             # 勝利賛美ラップ
└── defeat.mp3              # 敗北ジングル
```

## 特徴

- 🎭 ミッドジャーニー製のキャラクター画像
- 💬 ターン毎に異なる掛け合いセリフ + 投擲前のスパロボ風カットイン
- 🎵 Sunoで作曲したオリジナルBGM + ダビデのラップ賛美歌
- 🔥 ドラゴンボール風のチャージオーラ演出
- 💥 CRITICAL時は派手に吹っ飛ぶ + 雷光の必殺技演出
- 🪨 ダビデは石投げ名手なので投石のように飛ぶ
- 📖 **口語訳聖書**を使用、本物43種・偽物35種を収録
- 👥 背景にイスラエル軍 vs ペリシテ軍が応援
- 📊 対戦後の成績発表とダビデのエピソード紹介
- 🏆 Firebase連動のランキング機能（要セットアップ）

## クレジット

- キャラクターデザイン: Midjourney
- BGM/賛美歌: Suno AI
- ゲーム開発: Claude (Anthropic)
