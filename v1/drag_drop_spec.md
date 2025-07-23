# ドラッグ&ドロップ機能仕様書 v1

## 📋 概要
TODOリストとメモ機能に看板を上下に動かすドラッグ&ドロップ機能を追加

## 🎯 対象アプリ
1. **📚 TODOアプリ** - タスクの順序変更
2. **📝 メモアプリ** - メモの順序変更
3. **🧮 電卓アプリ** - 機能維持（ドラッグ&ドロップ不要）

## 🏗️ 共通ドラッグ&ドロップ機能

### 基本仕様
- **操作方法**: マウス/タッチでアイテムをドラッグして上下移動
- **視覚フィードバック**: ドラッグ中はアイテムを半透明化・影付き
- **ドロップターゲット**: 他のアイテム間にドロップライン表示
- **データ保存**: 順序変更後にFirebaseへ自動保存

### 技術実装
```javascript
// 共通ドラッグ&ドロップクラス
class DragDropManager {
    constructor(containerSelector, itemSelector, onReorder) {
        this.container = document.querySelector(containerSelector);
        this.itemSelector = itemSelector;
        this.onReorder = onReorder; // コールバック関数
        this.draggedElement = null;
        this.setupEventListeners();
    }
    
    setupEventListeners() {
        // ドラッグイベントのセットアップ
    }
    
    handleDragStart(e) {
        // ドラッグ開始処理
    }
    
    handleDragOver(e) {
        // ドラッグオーバー処理（ドロップライン表示）
    }
    
    handleDrop(e) {
        // ドロップ処理（順序変更・保存）
    }
}
```

## 🎨 UI/UX設計

### TODOアプリ
- **ドラッグハンドル**: タスク左端に「≡」アイコン表示
- **優先度維持**: ドラッグ後も優先度カラーリング維持
- **完了状態維持**: チェック状態はドラッグ後も保持

### メモアプリ
- **ドラッグハンドル**: メモ右上に「≡」アイコン表示
- **タイムスタンプ更新**: 順序変更時は「移動日時」を記録
- **プレビュー維持**: ドラッグ中もメモ内容の一部を表示

## 📊 データ構造変更

### Firebase構造
```javascript
{
  "users": {
    "[uid]": {
      "todos": [
        {
          "id": "todo1",
          "content": "タスク内容",
          "completed": false,
          "priority": "high",
          "order": 0,  // 新規追加
          "createdAt": "2025-07-23T06:00:00.000Z",
          "movedAt": "2025-07-23T07:00:00.000Z"  // 新規追加
        }
      ],
      "memos": [
        {
          "id": "memo1",
          "title": "メモタイトル",
          "content": "メモ内容",
          "order": 0,  // 新規追加
          "createdAt": "2025-07-23T06:00:00.000Z",
          "movedAt": "2025-07-23T07:00:00.000Z"  // 新規追加
        }
      ]
    }
  }
}
```

## ✨ 機能削除
- **削除対象**: Good Morning, Good Afternoon ボタン
- **保持対象**: その他デバッグボタン（ログダウンロード等）

## 🧪 テスト項目
1. ドラッグ&ドロップ動作確認
2. 順序変更後のFirebase保存確認
3. ページリロード後の順序維持確認
4. モバイル環境でのタッチ操作確認
5. 複数タブでの同期確認