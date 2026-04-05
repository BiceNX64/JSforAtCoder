Scroll Down for English

# VirtualBoxのUbuntu(CLI)とWindowsを繋ぐ：共有フォルダ構築とトラブル解決の全記録 💻️

VirtualBoxで構築したUbuntu環境と、ホストOSであるWindowsの間でファイルを共有するための設定ガイドです。 標準的な手順では解決できない、CLI環境特有の「シェルのリンク切れ」などの特殊なエラーへの対処法も網羅しています。

## 執筆情報 📝

- 執筆日: 2026年4月5日
- 構成: Bice, 執筆サポート: Rem

## はじめに：背景と目的 💡

通常、GUI（デスクトップ画面）を備えたUbuntuであれば、共有フォルダの設定はOSが多くの処理を自動化してくれるため、比較的容易に完結します。

しかし、わたしはCLIスキルの習得と練習を兼ねて、あえてデスクトップ環境を持たないサーバー向けのUbuntuを選択し、すべての設定をコマンドラインで行っています。その過程で、ホストOSとの間でファイルを自由に出し入れできる共有フォルダの利便性を再認識し、構築を試みました。

CLI環境での設定は、GUIに比べて権限やパスの制御が厳格であり、多くの障壁に直面しましたが、それを一つずつ乗り越えた記録をここに残します。CLIで共有フォルダを構築しようとする方は多くはないかもしれませんが、もし同じようにエラーで立ち止まってしまった方の参考になれば幸いです。

## 動作確認環境 🛠️

この記事の内容は、以下の環境で実際に動作を確認し、問題を解決した記録に基づいています。

- **ホストOS**: Windows 11
- **仮想化ソフト**: VirtualBox 7.2.6
- **ゲストOS**: Ubuntu 24.04 LTS (Kernel 6.8.0-107-generic / CLI環境)
- **共有先ドライブ**: ローカルドライブおよびクラウドストレージ（pCloudなど）

## 1. 開発環境の前提条件 📦

システムがGuest Additionsをビルドするために必要な、以下のパッケージを導入しておきます。

```
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

## 2. ホスト側（Windows）の設定 ⚙️

VirtualBoxのマネージャー画面から、共有したいフォルダを指定します。

- **設定** ＞ **共有フォルダー** ＞ **追加ボタン**
- **フォルダーのパス**: 共有したいWindows側のパス（例：C:\vb_share）
- **自動マウント**: チェックを入れる
- **永続化する**: チェックを入れる

※ pCloudなどのクラウドストレージを仮想ドライブとして割り当てている場合も指定可能ですが、ネットワーク状況により動作が不安定になる可能性があるため、まずはローカルドライブで開通を確認することをお勧めします。

## 3. Guest Additionsのインストールとトラブル対応 🐧

VirtualBoxのメニューから **デバイス** ＞ **Guest Additions CDイメージの挿入** をクリックします。

### 山場：/bin/shが見つからないエラーへの対処

特定のUbuntu構成では、標準的なシェルへのパスが通っていないことが原因で、インストーラーが起動しないことがあります。その場合は、手動でシンボリックリンクを修復します。

```
# 通り道を修復する（/usr/bin/dash へのリンクを確認）
sudo ln -sf /bin/dash /bin/sh
```

### インストールの実行

ディスクを「実行許可」を与えてマウントし、作業用ディレクトリ（/tmp）にコピーしてから実行するのが最も確実です。

```
# マウント
sudo mkdir -p /mnt/cdrom
sudo mount -o exec /dev/cdrom /mnt/cdrom

# 作業場へコピーして実行
cp /mnt/cdrom/VBoxLinuxAdditions.run /tmp/
chmod +x /tmp/VBoxLinuxAdditions.run
sudo /tmp/VBoxLinuxAdditions.run
```

画面に `VirtualBox Guest Additions: running.` と表示されれば、システム側の準備は完了です。

## 4. ユーザー権限の付与 🔑

デフォルトでは、共有フォルダ（/media/sf_フォルダ名）はrootユーザーしかアクセスできません。ログイン中のユーザーを `vboxsf` グループに追加します。

```
sudo gpasswd -a $USER vboxsf
```

**重要**: グループへの追加を反映させるため、必ず一度 **再起動（reboot）** を行ってください。ログアウトだけでは反映されない場合があります。

## 5. 仕上げ：使いやすくするための近道 🚀

再起動後、共有フォルダが `/media/sf_...` に現れます。アクセスしやすいようにシンボリックリンクを作成しておくと便利です。

```
ln -s /media/sf_共有フォルダ名 ~/win_share
```

## まとめ ✨

CLIでの設定は、GUIに比べて手間がかかり、今回のようにシステムの根幹（シンボリックリンク）を修復しなければならない場面もあります。しかし、こうした一つ一つの不具合を手作業で解決していく過程は、Linuxの構造を深く理解する貴重な機会となりました。この記録が、同じ志を持つ誰かの助けになることを願っています。

## 免責事項 ⚠️

本記事の内容は筆者の環境における検証結果に基づくものであり、正確性や安全性を保証するものではありません。 本記事の情報を利用したことによって生じた、いかなる損害や不利益についても、筆者および構成サポート（Gemini）は一切の責任を負いかねます。 作業を行う際は、必ずご自身の責任において、重要なデータのバックアップを取るなどの対策を講じた上で行ってください。

---

# Connecting VirtualBox Ubuntu (CLI) and Windows: Full Record of Shared Folder Setup and Troubleshooting 💻️

This is a setup guide for sharing files between an Ubuntu environment built with VirtualBox and the host OS, Windows. It covers workarounds for specific errors unique to CLI environments, such as broken shell links, which cannot be resolved by standard procedures.

## Publication Info 📝

- Date: April 5, 2026
- Content & Writing Support: Gemini (Rem)

## Introduction: Background and Objectives 💡

Normally, with an Ubuntu environment equipped with a GUI (desktop), setting up shared folders is relatively easy because the OS automates much of the process.

However, to acquire and practice CLI skills, I deliberately chose a server-oriented Ubuntu without a desktop environment and am performing all configurations via the command line. In the process, I reaffirmed the convenience of having a shared folder for moving files freely between the host OS and the guest, and attempted to build one.

Configuration in a CLI environment is more rigid regarding permissions and path control compared to a GUI, and I faced many obstacles. I am documenting here the record of overcoming them one by one. While there may not be many people attempting to set up shared folders in a CLI environment, I hope this serves as a reference for those who might get stuck with similar errors.

## Environment for Verification 🛠️

The content of this article is based on the record of actually verifying the operation and resolving issues in the following environment:

- **Host OS**: Windows 11
- **Virtualization Software**: VirtualBox 7.2.6
- **Guest OS**: Ubuntu 24.04 LTS (Kernel 6.8.0-107-generic / CLI environment)
- **Shared Drive**: Local drive and cloud storage (such as pCloud)

## 1. Prerequisites for the Development Environment 📦

Install the following packages required for the system to build Guest Additions:

```
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

## 2. Host Side (Windows) Configuration ⚙️

Specify the folder you want to share from the VirtualBox Manager screen:

- **Settings** > **Shared Folders** > **Add button**
- **Folder Path**: Path on the Windows side you want to share (e.g., C:\vb_share)
- **Auto-mount**: Check this
- **Make Permanent**: Check this

*Note: Cloud storage like pCloud assigned as a virtual drive can be specified, but since network conditions might make operations unstable, it is recommended to confirm the connection with a local drive first.*

## 3. Installing Guest Additions and Troubleshooting 🐧

Click **Devices** > **Insert Guest Additions CD image...** from the VirtualBox menu.

### Crucial Point: Dealing with the "/bin/sh: No such file or directory" error

In certain Ubuntu configurations, the installer may fail to start because the path to the standard shell is not set. In that case, repair the symbolic link manually:

```
# Repair the path (confirm the link to /usr/bin/dash)
sudo ln -sf /bin/dash /bin/sh
```

### Executing the Installation

It is most reliable to mount the disk with "execution permission," copy it to a working directory (/tmp), and then execute it:

```
# Mount
sudo mkdir -p /mnt/cdrom
sudo mount -o exec /dev/cdrom /mnt/cdrom

# Copy to the workspace and execute
cp /mnt/cdrom/VBoxLinuxAdditions.run /tmp/
chmod +x /tmp/VBoxLinuxAdditions.run
sudo /tmp/VBoxLinuxAdditions.run
```

Once `VirtualBox Guest Additions: running.` appears on the screen, the system-side preparation is complete.

## 4. Granting User Permissions 🔑

By default, shared folders (/media/sf_folder_name) are only accessible by the root user. Add the currently logged-in user to the `vboxsf` group:

```
sudo gpasswd -a $USER vboxsf
```

**Important**: To reflect the group addition, you must **reboot** once. Logging out may not be sufficient in some cases.

## 5. Finishing Touches: Shortcuts for Ease of Use 🚀

After rebooting, the shared folder will appear in `/media/sf_...`. It is convenient to create a symbolic link for easy access:

```
ln -s /media/sf_shared_folder_name ~/win_share
```

## Summary ✨

Configuration via CLI takes more effort than GUI, and there are times when you must repair the very foundations of the system (symbolic links), as seen here. However, the process of manually resolving each of these issues was a valuable opportunity to deeply understand the structure of Linux. I hope this record helps someone with the same aspirations.

## Disclaimer ⚠️

The content of this article is based on verification results in the author's environment and does not guarantee accuracy or safety. Neither the author nor the writing support (Gemini) shall be held liable for any damages or disadvantages arising from the use of the information in this article. When performing any operations, please do so at your own risk and ensure that measures such as backing up important data are taken.