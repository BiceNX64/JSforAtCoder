**Scroll down for English!**

# 🗺️ JS for AtCoder: JSで緑を目指す旅の記録

**JavaScriptで「緑」を目指す、Web系独学エンジニアの記録**

## 🖋 はじめに

私は現在、非IT系で趣味としてWebフルスタックエンジニアを目指して独学中です（2025年6月学習開始）。

初めて学んだプログラミング言語はJavaScriptで、Udemyの「Web Developer Bootcamp」を中心に基礎を学びました。HTML/CSS/JSから始まって、最終的にはMERNスタックで簡単なアプリの模写をする長尺のコースでした。競技プログラミングをはじめようと思ったのは、SNSでたまに見かけていておもしろそうだなと興味を持ったからです。

### なぜ、JavaScriptで競技プログラミングなのか？

世間ではPythonが推奨されることが多い中、あえてJSを選んだのには理由があります。

- **学習の集約:** JSはブラウザで動く唯一無二の言語であり、フロントエンドからバックエンドまで一貫して書くことができます。まずは1つ目の言語として効率よく深く広く習得したいという思いがありました。
- **敢えて逆を行く:** 「競プロならC++/Python」という定説の逆を行き、道がない場所に道を作ってみたいという思いもありました。趣味なので自由が利きます。

「何か書きたいけれど、何を書けばいいかわからない」という独学初期の課題を解決するため、目的が明確なパズルであるAtCoderに挑戦し、まずは1人前と言える「緑レーティング」への到達を目標としています。

------

## 🚧 最初の壁：Node.js 入出力の洗礼

最初に直面したのは、ブラウザ環境とNode.js環境における入出力の圧倒的なギャップでした。 普段の学習では意識することのなかった、標準入力を受け取るという作業が、これほどまでに高い壁になるとは思いませんでした。

当初はVS Codeのスニペット機能や、引数にtxtファイルを指定して読み込むスクリプトを自作して対応していました。 しかし、実戦の緊張感の中では、テスト用と提出用のコードを間違えてエラーを出してしまうといったミスも経験しました。

この無駄なやり方を解消するため、最終的に**「ローカルテスト（input.txt）と提出コードを両立させるテンプレート」**を構築しました。いま振り返ってみれば、このようなテンプレートがなかなかJSでは手に入らなかったということが、このプロジェクトの、JSで競技プログラミングに挑戦するひとのための情報を提供したい、ということの原点かもしれません。

👉 詳細は Basement/ ディレクトリを参照してください。

------

## ⚔️ 次の壁：アルゴリズムという「共通言語」

コンテスト（ABC）に参加する中で、文法を知っているだけでは越えられない「C問題以降の壁」を感じました。SNSで飛び交う「累積和」「スタック」「ランレングス圧縮」といった言葉は、競技プログラマーにおける共通言語でした。

- **本質:** その場で解法を発明するのではなく、先人の知恵（アルゴリズム）を武器として装備し、適切に選択すること。
- **課題:** 言語の壁はないはずのアルゴリズムも、実用的なコード例はC++やPythonに偏っており、JSユーザーは常に翻訳を強いられる現実があります。

このリポジトリでは、学習したアルゴリズムを自分専用の**JSライブラリ**として蓄積し、JSで戦うための武器庫として整備していきます。

------

## 📉 現状分析：JavaScript勢の立ち位置

AI（Grok）による2025年時点の推定データによれば、JS勢は極めて少数派です。

| **項目**     | **状況**                        | **備考**                                                     |
| ------------ | ------------------------------- | ------------------------------------------------------------ |
| **参加率**   | **0.3 〜 0.8%**                 | ABC提出数ベース。上位層ほど減少。                            |
| **主な障壁** | 🐢 実行速度 / 📚 情報の圧倒的不足 | 64bit整数の扱いなど、言語仕様上の課題。                      |
| **結論**     | **絶滅危惧種**                  | ただし、緑レートまでは言語差よりも「情報の有無」が勝敗を分ける。 |

------

## 🎯 このリポジトリの目的

「情報がないなら、自分で作ればいい」

開発の現場ではメジャーともいえるJavaScriptを主戦場とする競技プログラマーは非常に少ないのが現実ですが、その最大の理由は言語の競技プログラミングに対する適性の差よりも圧倒的な「情報の不足」にあると考えています。

このリポジトリは、荒野を開拓する私のベースキャンプであり、以下のようなコンテンツを公開・更新していきます。

- 🛠 **Basement:** 便利なテンプレートやチートシートなど初期の必須装備
- 📚 **Vault:** JSで実装する競プロの武器たち（アルゴリズム解説+代表的な過去問）
- 📝 **Travelogue:** ほぼ毎週参加するABCの実戦記

これからJSでAtCoderに挑戦する人たちにとって、この記録が誰かの「道標」になれば幸いです。



---

# 🗺️ JS for AtCoder: A Journey to Reach "Green" with JavaScript

**The records of a self-taught Web engineer aiming for the "Green" rank in AtCoder using JavaScript.**

## 🖋 Introduction

I am currently teaching myself to become a full-stack Web engineer as a hobby while working in a non-IT field (started learning in June 2025).

The first programming language I learned was JavaScript, and I mastered the basics primarily through the "Web Developer Bootcamp" on Udemy. It was a long-form course that started with HTML/CSS/JS and eventually involved cloning simple apps using the MERN stack. I became interested in competitive programming after seeing it occasionally on social media and thinking it looked fascinating.

### Why Competitive Programming with JavaScript?

While Python is often recommended for competitive programming, I chose JS for several specific reasons:

- **Consolidation of Learning:** JS is the one and only language that runs in the browser, allowing for consistent development from the front-end to the back-end. I wanted to deepen and broaden my mastery of my first language efficiently.
- **Taking the Path Less Traveled:** I wanted to go against the conventional wisdom that says competitive programming equals C++ or Python, and try to blaze a trail where none exists. Since this is a hobby, I have the freedom to choose my own path.

To solve the common problem for beginners of "knowing how to write but not knowing what to write," I decided to take on AtCoder—a platform of puzzles with clear objectives. My current goal is to reach the "Green" rating, which is considered a mark of a competent coder.

------

## 🚧 The First Barrier: The Ritual of Node.js I/O

The first obstacle I faced was the overwhelming gap between the browser environment and the Node.js environment. I never realized in my daily studies that the simple task of receiving standard input could be such a high wall to climb.

At first, I managed by using VS Code snippets or custom scripts that read input from a txt file. However, in the heat of actual competition, I experienced mistakes like submitting test code by accident and getting errors.

To eliminate this inefficient workflow, I eventually built a **"Template that works for both local testing (input.txt) and official submission."** Looking back, the fact that such templates were hard to find for JS was the starting point for this project: I want to provide information for others challenging competitive programming with JavaScript.

👉 See the `Basement/` directory for details.

------

## ⚔️ The Next Barrier: Algorithms as a "Lingua Franca"

As I participated in contests (ABC), I felt the "Wall of Problem C and beyond," which cannot be overcome by knowing syntax alone. Terms like "Cumulative Sum," "Stack," and "Run-Length Encoding" flying around on social media are the common language of competitive programmers.

- **The Essence:** It's not about inventing a solution on the spot, but about equipping yourself with the wisdom of predecessors (algorithms) and choosing the right one for the job.
- **The Challenge:** Even though algorithms themselves have no language barrier, practical code examples are heavily biased toward C++ and Python. JS users are constantly forced to "translate" these concepts.

In this repository, I will accumulate the algorithms I've learned as my own **JS Library**, maintaining it as an armory for fighting with JavaScript.

------

## 📉 Current Status: The Position of JavaScript Users

According to estimated data for 2025 by AI (Grok), the JS contingent is an extreme minority.

| **Item**               | **Status**                                 | **Notes**                                                    |
| ---------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| **Participation Rate** | **0.3% – 0.8%**                            | Based on ABC submissions. Decreases in higher ranks.         |
| **Main Obstacles**     | 🐢 Execution Speed / 📚 Extreme Lack of Info | Issues with language specs, such as handling 64-bit integers. |
| **Conclusion**         | **Endangered Species**                     | However, up to the Green rate, the "availability of information" matters more than language performance differences. |

------

## 🎯 Purpose of This Repository

"If the information doesn't exist, I'll create it myself."

I believe the main reason why JavaScript—so dominant in professional development—is not mainstream in competitive programming is more about the "lack of information" than the difference in performance.

This repository is my basecamp as I cultivate this wilderness. I will publish and update the following content:

- 🛠 **Basement:** Essential initial equipment, including useful templates and cheat sheets.
- 📚 **Vault:** Weapons for competitive programming implemented in JS (Algorithm explanations + representative past problems).
- 📝 **Travelogue:** Real-world battle records of the ABC contests I participate in almost every week.

I hope these records serve as a "guidepost" for others who choose to challenge AtCoder with JavaScript in the future.