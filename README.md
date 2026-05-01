# rk3188-repo
RK3188が手元にあったので色々遊んでみたレポート＆リポジトリ

## 1. 現状確認と絶望

まずは動作確認を試みるも、沈黙。
「ハードウェアさえ生きていれば、システムイメージを焼き直せばなんとかなる」と信じ、作業を開始する。

とはいったものの１０年前の機材。

メーカー公式サイトらしきものも見当たらない。。
ネット上を漂うのは、正体不明の怪しいファームウェアばかり。

とはいえまずは信頼しないと始まらないので落として入れてみる。

## 2. 開発環境の構築

ファームウェア書き込み用のツールとして、OSSの [rkdeveloptool](https://github.com/rockchip-linux/rkdeveloptool.git
) を採用。
以下の手順でビルド環境を構築する。

### セットアップ手順

```bash
# リポジトリの準備
git clone https://github.com/misogihagi/rk3188-repo
cd rk3188-repo
git clone https://github.com/rockchip-linux/rkdeveloptool.git

# 依存関係のインストール
sudo apt-get update
sudo apt-get install libudev-dev libusb-1.0-0-dev dh-autoreconf

# rkdeveloptoolのビルド
cd rkdeveloptool
./autogen.sh
./configure
make
```

## 3. 実装と試行錯誤

一応使い方は書いてある。が、具体的にどうすればいいのかはその機種次第。

**基本的なコマンドフロー:**
1.  **ブートローダーのロード:** `sudo ./rkdeveloptool db RKXXLoader.bin`
2.  **イメージの書き込み:** `sudo ./rkdeveloptool wl 0x8000 kernel.img` (セクタ指定)
3.  **再起動:** `sudo ./rkdeveloptool rd`

筐体横のピンホールを押し込み、リカバリーモード（あるいはブートローダーモード）での認識を狙ってPCと接続する。
これがリカバリーモードなのかもわからない。

 `sudo ./rkdeveloptool db assets/TV3Q_RK3188_AP6330_Android4.2.2_20140423/rockdev/RK3188Loader(L)_V2.13.bin`がいつまでたっても終わらないなあ。
 これであってるのか


## 補足

巨大なファイルはGitHubに弾かれるため、分割して管理しておく。

```bash
7z a -v100m system.img.7z system.img
```

---

**考察:** 10年前のデバイスは、現行のライブラリ（libusb等）との相性もシビアな場合があります。果たして完走できるか。運命のデバッグは続きます。
