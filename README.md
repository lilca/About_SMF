# About_SMF

## 1. フォーマットの種類
|種類|説明|
|:-|:-|
|Format 0|1トラック|
|Format 1|マルチトラック対応|
|Format 2|不明|

## 2. チャンク
SMFは、ヘッダチャンクで始まり、それに続く１つ以上のトラックデータで構成される．

### 2.1. Format0の場合
<table>
  <thead>
    <tr>
      <th><span style="background-color : gray;">ファイル</span></th>
    </tr>
  </thead>
  <tbody>
    <tr><td>ヘッダチャンク</td></tr>
    <tr><td>トラックチャンク０</td></tr>
    <tr><td>トラックチャンク１</td></tr>
    <tr><td>:</td></tr>
    <tr><td>トラックチャンクＮ−１</td></tr>
  </tbody>
</table>
|<span style="background-color: gray;">ファイル</span>|
|:-:|
|ヘッダチャンク|
|トラックチャンク０|

### 2.2. Formatの場合(Nチャンネル使用時)
|<code style="color : name_color">ファイル</code>|
|:-:|
|ヘッダチャンク|
|トラックチャンク０|
|トラックチャンク１|
|:|
|トラックチャンクＮ−１|

### 2.3. Format2の場合
不明

## 3. ヘッダチャンク
では、SMFのフォーマット，トラック数や時間単位を設定．長さは固定．

|名称|バイト数|データ|説明|
|:--|:--:|:----------------|:--|
|チャンクタイプ|4|4D, 54, 68, 64|テキストデータ"MThd"で固定|
|データ長|4|00, 00, 00, 06|データ長以降の、チャンクの終わりまでのバイト数(6で固定)|
|フォーマット|2|ff, ff|フォーマットを設定|
|トラック数|2|nn, nn|ＳＭＦで使う全トラック数|
|時間単位|2|tt, tt|時間の最小の長さ． ***（＊１）***|

(データは１６進数表記でビックエンディアン)

|データ|詳細|
|:--|:--|
|ff,ff|Format0なら00,00．Format1なら00,01．
|nn,nn|Format0では、00,01で固定．Format1では最大00,0F？？？
|tt,tt|最上位ビットが"0"なら"四分音符"の分割数．"1"なら秒の分割数？ **（＊１）***

|15bit|上位バイト|下位バイト|分割数|説明|
|:-|:-:|:-|:-|:-|
|0|四分音符の分割数||
|1|-24<br>(24fps film)|
|^|-25<br>(25fps PAL)|
|^|-29<br>(30fps drop frame SMPTE)|
|^|-30<br>(30fps non-drop SMPTE(NTSC))|

<table>
  <thead>
    <tr>
      <th colspan="2">1</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3">1</td>
      <td rowspan="2">1</td>
      <td rowspan="2" colspan="2">2</td>
      <td>6</td>
    </tr>
    <tr>
      <td>7</td>
    </tr>
    <tr>
      <td>4</td>
      <td>3</td>
      <td colspan="2">5</td>
    </tr>
  </tbody>
</table>

***＊１*** 詳細調査中

## トラックチャンク
では、演奏を指示するデータや歌詞やテンポなどのデータが格納される．

|名称|バイト数|説明|
|:--|:----:|:--|
|デルタタイム|可変| データはリトルエンディアン．最上位ビットが"1"の場合次のデータが存在 |
|イベント|可変|  |

## デルタタイム

# イベント
「イベント」は、デルタタイムに続いて

## MIDIイベント
|メッセージ名|データ|説明|
|:--|:--:|:--|
|ノートオフ|8n, kk, vv|nチャンネルで、音階kkを、vvの速さで音を止める|
|ノートオン|9n, kk, vv|nチャンネルで、音階kkを、vvの速さ（＝音量）で音を鳴らす|
|コントロールチェンジ|Bn, cc, dd|nチャンネルで、各種コントローラを制御|
|プログラムチェンジ|Cn, pp|nチャンネルで、音色をppに設定|

n
kk
vv
cc
dd

## SysExイベント

|F0|データ長（可変長）|エクスクルーシブメッセージ|F7|

## メタイベント

|FF|メタイベントタイプ（1byte）|データ長（可変長）|メタデータ|


# キーノート
キーノートは、0から127の番号

    国際式
    キーノート番号 = 12 * (o + 1) + k  ---(a-1)
    440Hz => o = 4, k = 9

    ヤマハ式
    キーノート番号 = 12 * (o + 2) + k ----(a-2)
    440Hz => o = 3, k = 9

o...オクターブ,  k...音階

## 音階(k)
|C|C#|D|D#|E|F|F#|G|G#|A|#A|B|
|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|
|0|1|2|3|4|5|6|7|8|9|10|11|

# コントロールチェンジ
コントロールチェンジは、全部覚えるのは現実的でない。
ので、大事なものだけ。

## 参照
> SMF(Standard MIDI File)フォーマット解説
> http://maruyama.breadfish.jp/tech/smf/

> SMF(Standard MIDI Files)の構造
> https://sites.google.com/site/yyagisite/material/smfspec

> タイムコードの形式
> https://steinberg.help/cubase_elements_le_ai/v9.5/ja/cubase_nuendo/topics/synchronization/synchronization_timecode_standards_c.html
