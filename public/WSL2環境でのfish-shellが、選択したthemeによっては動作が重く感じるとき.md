---
title: WSL2環境でのfish-shellが、選択したpromptによっては動作が重く感じるとき
tags:
  - fish
  - fish-shell
  - WSL2
private: false
updated_at: '2023-08-23T16:22:25+09:00'
id: 4796d2c897fb1f4b1a10
organization_url_name: null
slide: false
---
## TL;DR

自分はMercurialでバージョン管理を行っていないので、その`hg`コマンドを実行しないようにしたことで動作を軽くしました。

1. `funced`コマンドで、`fish_vcs_prompt`を開く

   ```shell
   funced fish_vcs_prompt
   ```

2. `fish_vcs_prompt`の中身を以下`fish_hg_prompt`関数をコメントアウトする

   ```diff
   function fish_vcs_prompt --description "Print all vcs prompts"
       # If a prompt succeeded, we assume that it's printed the correct info.
       # This is so we don't try svn if git already worked.
       fish_git_prompt $argv
   -    or fish_hg_prompt $argv
   +    #or fish_hg_prompt $argv
       # The svn prompt is disabled by default because it's quite slow on common svn repositories.
       # To enable it uncomment it.
       # You can also only use it in specific directories by checking $PWD.
       # or fish_svn_prompt
   end
   ```

3. 保存するか聞かれるので、`y`を入力して保存する

## どういうときに重く感じたのか

ターミナル上でEnter(Return)キーを連打したとき、行移動が連打に追いついていない感じがしていました。
調査したところ、`fish_vcs_prompt`関数が原因でした。

もう少し具体的には、以下の条件のときに重く感じていました。

- `fish_config`で、git等の状態を表示するpromptを選択しているとき
- カレントディレクトリがバージョン管理されていないとき (rootディレクトリなど)

## 比較

`time fish_vcs_prompt %s`コマンドを実行して、実行時間を比較します。
コメントアウト前後と、リポジトリの有無で比較します。

| ファイルの編集 | リポジトリの有無 | 実行時間 |
| --- | --- | --- |
| 変更前 | あり | 26.54 millis |
| **変更後** | あり | 24.40 millis |
| 変更前 | **なし** | 117.09 millis |
| **変更後** | **なし** | **1.89 millis** |

## 確認環境

- WSL

  ```pwsh
  > wsl --version
  WSL バージョン: 1.2.5.0
  カーネル バージョン: 5.15.90.1
  WSLg バージョン: 1.0.51
  MSRDC バージョン: 1.2.3770
  Direct3D バージョン: 1.608.2-61064218
  DXCore バージョン: 10.0.25131.1002-220531-1700.rs-onecore-base2-hyp
  Windows バージョン: 10.0.22621.2134
  ```

- OS

  ```bash
  $ cat /etc/os-release
  NAME="Ubuntu"
  VERSION="20.04.6 LTS (Focal Fossa)"
  ID=ubuntu
  ID_LIKE=debian
  PRETTY_NAME="Ubuntu 20.04.6 LTS"
  VERSION_ID="20.04"
  HOME_URL="https://www.ubuntu.com/"
  SUPPORT_URL="https://help.ubuntu.com/"
  BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
  PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
  VERSION_CODENAME=focal
  UBUNTU_CODENAME=focal
  ```

- fish

  ```bash
  $ fish --version
  fish, version 3.6.1
  ```

## 参考リンク

- [fish_vcs_prompt - output version control system information for use in a prompt](https://fishshell.com/docs/current/cmds/fish_vcs_prompt.html)
- [funced - edit a function interactively](https://fishshell.com/docs/current/cmds/funced.html)
