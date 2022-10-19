# fio-cdm
[@buty4649 の fio-cdm](https://github.com/buty4649/fio-cdm) を修正し、BSD系OSで使えるようにしたものです。

TrueNASの簡易動作チェックに使うためにフォークしました。

現行のLinuxでも使えますが、エンジンがposixaioとなるため、Linuxの標準的なエンジンであるlibaioより若干遅く表示される可能性があります。

## 主な修正点

* 1GB/s以上の速度に対応し、NVMeや高速な商用ストレージでも測定可能にした（仮対策、有効桁数不足のため近々修正予定）
* エンジンをposixaioとした（BSD系OSで安定します）
* 現行のfioの出力を適切にパースするよう正規表現を修正

## 今後の修正予定（優先順位順）

* 表示と計算の有効桁数をそろえる（結果を正確にする）
* SI接頭辞と2進接頭辞を適切に計算する（結果を正確にする）
* IOPS表示機能を追加
* 読み書きサイズを指定可能にする
* 現在のCDMに合わせたチェック方法にもオプションで選択可能にする（ SEQ1M Q8T1 / SEQ128K Q32T1 / RND4K Q32T16 / RND4K Q1T1）
* エンジンを選択可能にする（マルチOS対応）

## Usage

```
# ローカルで使う場合
./fio-cdm <path>

# 直接参照する場合
curl -s https://raw.githubusercontent.com/tk-suzuki/fio-cdm4bsd/master/fio-cdm | sh /dev/stdin <path>

# 短縮URLでも利用できます
curl -s https://arpsrec.net/fio-cdm | sh /dev/stdin <path>
```

### sample

注: 1GB/s以上出ると小数点以下が無効になります（修正中）

```
# fio-cdm /mnt/zpool01/dataset
|      | Read(MB/s)|Write(MB/s)|
|------|-----------|-----------|
|  Seq |   5907.000|   8938.000|
| 512K |  17422.000|   6139.000|
|   4K |   2846.000|   1537.000|
|4KQD32|   3237.000|   1725.000|
```

## 参考
* [LinuxのI/OベンチマークでCrystalDiskMarkと同等の計測をfioで実現 - WinKey](http://www.winkey.jp/article.php/20110310142828679)
