# .pro ファイル フォーマット解析

## ファイル概要

`.pro` ファイルは NiZ 専用ソフトウェアが使用する XML 形式の設定ファイル。
キーボードの全キーマッピング設定を保持する。

---

## XML 構造

```xml
<?xml version="1.0" encoding="UTF-8"?>
<66EC(S)>
  <PredefinedCombo>...</PredefinedCombo>     <!-- コンボキー定義 -->
  <PredefinedMacro>...</PredefinedMacro>     <!-- マクロ定義 -->
  <PredefinedEmu>...</PredefinedEmu>         <!-- エミュレーション定義 -->
  <BackgroundLightSettings>...</BackgroundLightSettings>  <!-- LED色設定 -->
  <CurrentSettings>
    <KEY ID="..." Level="..." Mode="..." HWCode="...">
      <ComboKey>
        <List .../>
      </ComboKey>
    </KEY>
    ...
  </CurrentSettings>
</66EC(S)>
```

---

## KEY 要素の属性

### ID（1〜66）
物理キーの位置番号。ATOM66は66キーなので ID=1〜66。

キーボード上の物理的な配置順で番号が振られている:

| 行 | KEY ID 範囲 | キー |
|----|------------|------|
| 最上段 | 1〜15 | ESC, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, -, =, Del, \ |
| 2段目 | 16〜30 | Tab, Q, W, E, R, T, Y, U, I, O, P, [, ], BS, ` |
| 3段目 | 31〜42 | A, S, D, F, G, H, J, K, L, ;, ', Enter |
| 4段目 | 43〜55 | L-Shift, Z, X, C, V, B, N, M, , , . , / , R-Shift, Up |
| 最下段 | 56〜66 | L-Ctrl, L-Alt, L-Super, LFn, Space, CapsLock, R-Alt, RFn, Left, Down, Right |

### Level（0〜8）
モードとレイヤーの組み合わせ（前述の通り）。

### Mode
キーの割り当てタイプを示す:

| Mode値 | 意味 |
|--------|------|
| 0 | 未設定（デフォルト値を使用） |
| 1 | 単一キー（通常のキー割り当て） |
| 6 | マウス操作/特殊制御 |
| 7 | システム機能/特殊キー |

### HWCode（0〜255）
NiZ 独自のハードウェアコード。USB HID スキャンコードとは異なる独自の体系。

---

## HWCode 一覧表（主要なもの）

| HWCode | キー名 |
|--------|--------|
| 0 | (未割当) |
| 1 | ESC |
| 2〜13 | F1〜F12 |
| 14 | ` ~ (バッククォート) |
| 15〜24 | 1〜0 |
| 25 | - _ |
| 26 | = + |
| 27 | Backspace |
| 28 | Tab |
| 29〜41 | Q W E R T Y U I O P [ ] \ |
| 42 | Caps Lock |
| 43〜54 | A S D F G H J K L ; ' Enter |
| 55 | Left Shift |
| 56〜66 | Z X C V B N M , . / Right Shift |
| 67 | Left Ctrl |
| 68 | Left Super (Win) |
| 69 | Left Alt |
| 70 | Space |
| 71 | Right Alt |
| 74 | (Right Super / App等) |
| 78 | Insert |
| 79 | Home |
| 80 | Page Up |
| 81 | Delete (文字削除) |
| 82 | End |
| 83 | Page Down |
| 84 | Print Screen |
| 85 | Scroll Lock |
| 86 | Pause |
| 87 | Up Arrow |
| 88 | Left Arrow |
| 89 | Down Arrow |
| 90 | Right Arrow |
| 91 | Num Lock |
| 92〜107 | テンキー関連 |
| 108 | Media Next Track |
| 109 | Media Previous Track |
| 110 | Media Stop |
| 111 | Media Play/Pause |
| 112 | Media Mute |
| 113 | Media Volume Up |
| 114 | Media Volume Down |
| 115〜125 | メディア・WWW 制御 |
| 126 | Mouse Left |
| 127 | Mouse Right |
| 128 | Mouse Up |
| 129 | Mouse Down |
| 130 | Mouse Left Click |
| 131 | Mouse Right Click |
| 132 | Mouse Wheel Click |
| 133〜134 | Mouse 追加機能 |
| 135 | バックライトスイッチ |
| 136〜143 | バックライト制御 |
| 144 | RFN (Right Fn) |
| 145 | (特殊制御) |
| 149〜151 | (特殊/システム機能) |
| 152 | (特殊制御) |
| 156 | (特殊制御) |
| 157〜158 | Mouse速度/ピクセル調整 |
| 159 | (特殊制御) |
| 166 | LFN (Left Fn) |
| 173 | Mouse First Delay |
| 174 | Key Repeat Rate |
| 175 | Key Response Delay |
| 179 | (特殊制御) |
| 198 | (特殊キー) |
| 207 | (特殊制御) |

---

## 現在の設定（現在.pro）の解析

### Program 1 - Base レイヤー (Level 0)
全66キーが Mode=1 で設定済み。標準的なQWERTYレイアウト。
- KEY 59（通常 LFn の位置）: Mode=7, HWCode=166 → LFN キーとして機能

### Program 1 - Left FN レイヤー (Level 1)
FN併用時の拡張機能が設定済み:
- 数字行(ID 2-13): F1〜F12 にマッピング
- WASD(ID 18,31,32,33): マウスカーソル移動
- ZXC(ID 44,45,46): マウスクリック
- I,O,P(ID 24,25,26): Insert, Home, Page Up
- K,L,;(ID 38,39,40): Delete, End, Page Down
- カンマ,ドット,スラッシュ(ID 51,52,53): Print Screen, Scroll Lock, Pause

### Program 1 - Right FN レイヤー (Level 2)
Left FNとほぼ同一の設定（一部差異あり）

### Program 2/3 (Level 3-8)
全て Mode=0（未設定）。カスタマイズされていない。
→ ここに独自のレイヤー設定を作り込むことで、Program 2 や Program 3 として使い分けが可能。
