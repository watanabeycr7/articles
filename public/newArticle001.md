---
title: Gemma4をローカルで動かしてclaude codeをタダで使い倒したかった
tags:
  - Gemma
  - ollama
  - ClaudeCode
private: false
updated_at: '2026-04-17T23:10:28+09:00'
id: 794b5ebfbd2cc0c44cd8
organization_url_name: null
slide: false
ignorePublish: false
---
# 読んで欲しい人

- 有名なコーディングエージェントを使ったことはあるけどローカル環境でのAIは動かしたことがない人
- claude codeへの課金を少しでもケチりたい人

# なぜローカルLLMを試そうとしたのか？

仕事では普段からclaude codeやgemini cliにお世話になっていました。
ただ、別に個人開発とかをガッツリしているわけでもないので個人アカウントで課金するのは「もったいないかなー」とためらってました。
そんな時に[Can I Run AI locally?](https://www.canirun.ai/)というサイトを知り、小型モデルであれば動かせそうと思ったので試してみました。

gemini cliも個人利用で学習に使われてOKであれば無料で使えますが上限に達してしまうのは早い印象ですし、claude codeがデファクトスタンダードになりつつあるので普段から慣れておいた方が利点を多そうです。
CLAUDE.mdなどの恩恵を受けつつ、あわよくば完全無課金でclaude codeを使い倒したい算段です。

# 環境

- ハードウェア: mac mini m4 メモリ16GB
- OS: macOS 26.3.1
- ターミナル: Warp
- パッケージマネージャー: Homebrew
- LLM: Gemma4

mac mini m4の最安モデルを使っての検証です。メモリ増量モデルは枯渇しつつあるらしいので、最安モデルでどの程度動くのか気になる方の参考になればありがたいです。
LLMは2026年4月にリリースされたばかりのGemma4を使用していきます。

# 検証

## Ollamaのインストール

ローカル環境でLLMを動作させたいので、今回は[Ollama](https://ollama.com/download)を使用していきます。

公式だとcurlコマンドが例示されてますが、homebrewでのインストールもできるので今回はそちらを使います。

```sh
brew install --cask ollama
```

以下のように成功するのを待ちましょう。

```sh
✔︎ JSON API cask.jws.json                                                                                                  Downloaded   15.4MB/ 15.4MB
✔︎ JSON API formula.jws.json                                                                                               Downloaded   32.0MB/ 32.0MB
==> Fetching downloads for: ollama-app
✔︎ Cask ollama-app (0.20.7)                                                                                                Verified    163.7MB/163.7MB
==> Installing Cask ollama-app
==> Moving App 'Ollama.app' to '/Applications/Ollama.app'
==> Linking Binary 'ollama' to '/opt/homebrew/bin/ollama'
🍺  ollama-app was successfully installed!
```

## モデルのダウンロード

OllamaがインストールできたらLLMをダウンロードします。
今回はGemma4のうち2番目に軽いe4bのモデルを使用してみます。

```sh
ollama pull gemma4:e4b
```

ダウンロードを待ちましょう。

```sh
pulling manifest 
pulling 4c27e0f5b5ad: 100% ▕███████████████████████████████████████████████████████████████████████████████████████▏ 9.6 GB                         
pulling 7339fa418c9a: 100% ▕███████████████████████████████████████████████████████████████████████████████████████▏  11 KB                         
pulling 56380ca2ab89: 100% ▕███████████████████████████████████████████████████████████████████████████████████████▏   42 B                         
pulling f0988ff50a24: 100% ▕███████████████████████████████████████████████████████████████████████████████████████▏  473 B                         
verifying sha256 digest 
writing manifest 
success 
```

ダウンロードできたらモデルを指定してollamaを実行します。

```sh
ollama run gemma4:e4b
```

そうしたらプロンプト入力が可能になるので応答が返ってくるか試してみましょう。

```sh
>>> hello, there.
Hello! How can I help you today?
```

推論している様子も見れますし、日本語もできるそうです。

```sh
>>> can you speak japanese?
Thinking...
Thinking Process:

1.  **Analyze the request:** The user asked, "can you speak japanese?"
2.  **Determine capability:** As an AI language model, I have been trained on a massive dataset that includes Japanese. Therefore, I can 
understand, generate, and translate Japanese text.
3.  **Formulate the answer (English):** Confirm the ability clearly.
4.  **Provide demonstration/proof (Japanese and English):** It's best practice to demonstrate the capability immediately to satisfy the user. I 
should say yes, and then provide a simple greeting or phrase in Japanese, followed by its translation.
5.  **Review Identity/Constraints:** I am Gemma 4. This interaction is neutral and does not violate any constraints.

*Drafting the response.*
...done thinking.

はい、できます！(Hai, dekimasu!)

Yes, I can communicate in Japanese. I have been trained on a wide range of languages, including Japanese, so I can understand, write, and 
translate the language for you.

Would you like me to write something in Japanese, or would you like to practice a conversation?

---
**Example:**
How do you say "Hello, how are you?" in Japanese?
*Konnichiwa, ogenki desu ka?* (こんにちは、お元気ですか？)
```

ナレッジのカットオフも確認したいので日本の首相が誰か聞いてみます。

```sh
>>> 日本の首相を教えて欲しい。
Thinking...
Thinking Process:

1.  **Identify the core request:** The user asked, "日本の首相を教えて欲しい" (Nihon no shushō o oshiete hoshii), which translates to "I want 
you to tell me the Prime Minister of Japan."
2.  **Determine the current/most relevant information:** I need to provide the name of the current Prime Minister of Japan.
3.  **Identify the current PM:** As of my knowledge cutoff (January 2025) and general current knowledge, the PM is Fumio Kishida.
4.  **Formulate the response in Japanese (as requested):**
    *   Start with a polite confirmation.
    *   State the current Prime Minister's name.
    *   Add supporting details (e.g., their title, their party, or when they took office, if necessary, but keeping it natural).
5.  **Review and refine the Japanese phrasing:**

    *   *Current PM:* 岸田文雄 (Kishida Fumio)
    *   *Formal response:* 日本の現在の首相は、岸田文雄氏です。
    *   *Adding context:* 彼は自由民主党（自民党）の総裁を務めています。

6.  **Final Output Generation.** (Ensure the tone remains polite and conversational, as requested by the context.)
...done thinking.

はい、承知いたしました。

現在、日本の首相は**岸田文雄（きしだ ふみお）**氏です。

彼は自由民主党（自民党）の総裁を務めています。

他に日本の政治や文化について知りたいことはありますか？ 😊
```

カットオフは2025年の1月とのこと。
ちなみに、2025年1月時点での首相は石破茂ですね。

正誤は置いておいて、ひとまずローカル環境で動作させることができました。

:::note warn
`ollama launch`で立ち上げた場合、1回目の応答の後で以下のエラーになります。

```
Error: error running model: flag accessed but not defined: verbose
```
この問題については[issue](https://github.com/ollama/ollama/issues/14654)は起票されているのでPRがマージされるのを待ちましょう。
:::


## Ollama経由でclaude codeを動かす

さて、ここから本題のclaude codeを動かしていきます。
といっても特別な設定は必要なく、ollamaからclaude codeを呼び出すだけです。
claude codeのインストールは終わっている前提で進めます。
まずollamaを立ち上げます。

```sh
ollama
```

「Launch Claude Code」を選択しましょう。これだけです。

```sh
$ ollama
Ollama 0.20.7                                       
                                                    
  Chat with a model                                 
    Start an interactive chat with a model          
                                                    
  Launch OpenClaw (install)                         
    Personal AI with 100+ skills                    
                                                    
▸ Launch Claude Code (gemma4)                       
    Anthropic's coding tool with subagents          
                                                    
  Launch OpenCode (not installed)                   
    Anomaly's open-source coding agent              
                                                    
  More...                                           
    Show additional integrations   
```

そうしたらいつものcluade codeが現れます。
モデルが「gemma4」になっていれば成功です。

```sh
$ ollama
╭─── Claude Code v2.1.92 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│                                │ Tips for getting started                                                                                         │
│          Welcome back!         │ Run /init to create a CLAUDE.md file with instructions for Claude                                                │
│                                │ Note: You have launched claude in your home directory. For the best experience, launch it in a project director… │
│             ▐▛███▜▌            │ ──────────────────────────────────────────────────────────────────────────────────────────────────────────────── │
│            ▝▜█████▛▘           │ Recent activity                                                                                                  │
│              ▘▘ ▝▝             │ No recent activity                                                                                               │
│                                │                                                                                                                  │
│   gemma4 · API Usage Billing   │                                                                                                                  │
│           /Users/someuser      │                                                                                                                  │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
               
  ↑ Opus now defaults to 1M context · 5x more room, same pricing
```

試しに今回の記事の構成を考えてもらいました。

```sh
❯ 技術記事を書きたいのでその構成を手伝って。                                                                                                         
内容はollamaをインストールしてローカルモデルを使ってclaude codeを動かすという内容。                                                                  
ollamaのインストールとモデルのダウンロード、そのモデルの応答確認まで済んだら実際にollama経由でclaudeを動かすという内容。                             
mac miniで動かした時のcpu使用率とメモリプレッシャーを共有することでただのコピペ記事ではなく一つのサンプルとして有意義な記事としたい。                
                                                                                                                                                     
∴ Thinking…                                                                                                                                          
                                                                                                                                                     
  Here's a thinking process to construct the suggested plan:                                                                                         
                                                                                                                                                     
  1. Analyze the Request:                                                                                                                            
    - Goal: Write a guide/article about using Claude locally via Ollama.                                                                             
    - Target Audience: Users who want to run AI models locally, specifically integrating with a tool/workflow that uses Claude's capabilities        
  (implied integration with the system/tool that uses claude commands).                                                                              
    - Key Steps/Content:                                                                                                                             
        i. Introduce the concept (running LLMs locally, why Ollama).                                                                                 
      ii. Guide installation (Ollama setup).                                                                                                         
      iii. Guide model setup (Pulling the desired model, e.g., a Claude alternative or a general LLM).                                               
      iv. Demonstrate usage (Running the model via command line).                                                                                    
      v. Crucial Requirement: Provide specific performance metrics/diagnostics (CPU/Memory usage while running the LLM).                             
      vi. Tone: Informative, technical, step-by-step guide format.                                                                                   
  2. Determine the Structure (Article Outline): A good technical guide needs a clear flow.                                                           
    - Title/Introduction (Hook the reader).                                                                                                          
    - Prerequisites (What they need before starting).                                                                                                
    - Section 1: Installing Ollama (The foundational step).                                                                                          
    - Section 2: Running the Model (Making it functional).                                                                                           
    - Section 3: Advanced Usage & Performance Monitoring (Meeting the diagnostic requirement).                                                       
    - Conclusion/Next Steps.                                                                                                                         
  3. Flesh out the Content for Each Section (Self-Correction/Refinement):                                                                            

    - Introduction: Needs to address the "why local AI?" angle and mention that Ollama is the bridge.
    - Prerequisites: Mention macOS/Linux compatibility (since performance monitoring often implies Unix-like tools like top or htop).
    - Installation: Keep it simple: Download/Run the installer.
    - Model Selection: Since the user mentioned "Claude," I should advise them on finding a strong, local alternative (like Llama 3, Mistral, etc.)
  that performs well for complex tasks.
    - Performance Monitoring (The hard part): How do I show this? I need to wrap the execution command (ollama run model) with system monitoring
  commands (top, htop, or scripting the process to check /proc/cpuinfo equivalents). I must provide the commands they need to run.
    - Integration: How does this relate to the system using the claude context? I need a bridging paragraph explaining that they might need a wrapper
   script (e.g., alias or a Python function) to call ollama run instead of the direct claude SDK call.
  4. Review against User Requirements:
    - Guide on local Claude usage? Yes (via Ollama proxy).
    - Covers setup? Yes.
    - Covers usage? Yes.
    - Includes performance diagnostics? Yes (dedicated section with top/htop guidance).
  5. Final Polish (Tone and Language): Use clear headings, markdown formatting, and Japanese phrasing suitable for a technical Japanese audience
  (polite but direct). (Since the request didn't specify language, I'll default to English structure but ensure the tone is highly professional,
  assuming Japanese translation capability is needed). Self-Correction: I will frame the output as a detailed article outline/draft, making it ready
  for the user to fill in the actual writing. (This is the safest and most helpful approach.)

⏺ This is a detailed technical guide draft, structured like a high-quality blog post or documentation article.
                                                                                          
  Since the goal is to replicate the functionality of interacting with Claude locally using a tool like Ollama, the article needs to be highly
  practical, step-by-step, and address the required performance monitoring aspect.                                                                   
                                                                                                                                                     
  ---                                                                                                                                                
  🤖 ローカル環境でClaude級の性能を再現する方法：Ollama完全ガイド                                                                                    
                                                                                                                                                     
  （タイトル案：ローカルAIの最前線：Ollamaを使って、クローズドモデルを自分だけの環境に！）                                                           
                                                                                                                                                     
  🎯 はじめに：なぜローカルLLMなのか？ (Introduction)                                                                                                
                                                                                                                                                     
  （目的: 読者の共感を得る。APIコスト、プライバシー、オフライン利用のメリットを強調する。）                                                          
                                                                                                                                                     
  近年、大規模言語モデル（LLM）の利用は爆発的に増えています。AnthropicのClaudeをはじめとする高性能なモデルは魅力的ですが、API利用には継続的なコストが
  発生し、データが外部サーバーに送信されるプライバシーの懸念も無視できません。                                                                       
                                                                                                                                                     
  そこで注目されているのが**「ローカル実行」**です。ローカル環境でモデルを動かすことで、これらの懸念を解消し、高い自由度とプライバシーを確保できます 
  。                                                                                                                                                 
                                                                                                                                                     
  この記事では、業界標準のローカルLLM実行環境であるOllamaを導入し、Claudeに匹敵する性能を持つモデルを自分のPC上で動かし、さらにその際のハードウェア負
  荷（CPU/GPU/メモリ使用率）まで詳細に診断する方法を解説します。                                                                                     
                                                                                                                                                     
  🛠️  準備するもの (Prerequisites)                                                                                                                    
                                                                                                                                                     
  始める前に、以下の環境が整っていることを確認してください。                                                                                         
                                                                                                                                                     
  1. OS: macOS (Apple Silicon推奨) または Linux ディストリビューション。                                                                             
  2. ハードウェア: 十分な空きディスク容量（モデルサイズによる）、そして可能であれば専用のGPU（NVIDIA/AMD）。                                         
  3. ターミナル: コマンドライン操作ができる環境。                                                                                                    
                                                                                                                                                     
  🚀 ステップ1：Ollamaのインストール (Installation)                                                                                                  
                                                                                                                                                     
  Ollamaは、様々なオープンソースLLMを簡単にダウンロードし、実行するためのフレームワークです。                                                        
                                                                                                                                                     
  1. インストーラーの実行:                                                                                                                           
    - （ここに公式サイトへのリンクを挿入）からインストーラーをダウンロードし、実行します。                                                           
  2. 基本動作確認:                                                                                                                                   
    - ターミナルを再起動し、以下のコマンドを実行して、Ollamaが正しく認識されているか確認します。                                                     
  ollama --version                                                                                                                                   
                                                                                                                                                     
  🧠 ステップ2：モデルの選択とダウンロード (Model Selection & Pulling)                                                                               
                                                                                                                                                     
  「Claude並み」の性能を目指すため、現在トップクラスと評価されているモデルを選択します。一般的に、Llama 3やMistralの最新バージョンが推奨されます。   
                                                                                                                                                     
  1. モデルの検索:                                                                                                                                   
    - 利用可能なモデル一覧を確認し、目的のモデルを決定します。（例: llama3）                                                                         
  2. モデルのダウンロード:                                                                                                                           
    - 以下のコマンドを実行することで、モデルファイル（重み）がローカルにダウンロードされます。                                                       
  ollama pull llama3 # 例としてLlama 3をプルする                                                                                                     
    - 補足: モデルサイズには通常、7b (7Bパラメータ) や 8b があります。性能と動作速度のバランスを見て選択してください。                               
                                                                                                                                                     
  ⚙️  ステップ3：モデルの実行と基本対話 (Basic Usage)                                                                                                 
                                                                                                                                                     
  ダウンロードしたモデルを起動し、対話を開始します。                                                                                                 
                                                                                                                                                     
  ollama run llama3                                                                                                                                  
                                                                                                                                                     
  - プロンプトが表示されたら、質問を投げかけてみてください。                                                                                         
  - 対話終了時は /bye または Ctrl + D でセッションを終了します。                                                                                     
                                                                                                                                                     
  🔬 ステップ4：【重要】パフォーマンス診断：負荷計測 (Performance Monitoring)                                                                        
                                                                                                                                                     
  単に動かすだけでなく、「どれだけ負荷がかかっているか」を計測することが、プロのガイドラインでは必須です。システムリソースの使用状況を監視する方法を 
  解説します。                                                                                                                                       
                                                                                                                                                     
  🔥 実行手順:                                                                                                                                       
  ターミナルを2つ開きます。                                                                                                                          
                                                                                                                                                     
  1. 【ウィンドウ A：監視用】 システムリソース監視コマンドを実行します。                                                                             
    - macOS: top または htop を実行します。（htopがより直感的です）                                                                                  
    - Linux: htop または top を実行します。                                                                                                          
  2. 【ウィンドウ B：実行用】 モデルをロードし、負荷をかけるプロンプトを実行します。                                                                 
  ollama run llama3 "以下のテーマについて、専門家として詳細なレポートを作成してください。レポートの構成案を提示してください。"                       
  3. 診断ポイントの記録:                                                                                                                             
    - GPU使用率 (GPU Utilization): 描画負荷が最もかかり、高いほど処理にGPUが貢献している証拠です。                                                   
    - VRAM使用量 (Video RAM): モデルがロードされているメモリ量です。                                                                                 
    - CPU使用率 (CPU %): モデルの推論処理中にどの程度CPUリソースが使われているか。                                                                   
    - メモリ（RAM）使用量: 全体的なメモリ消費量。                                                                                                    
                                                                                                                                                     
  💡 診断のポイント:                                                                                                                                 
  この計測を通じて、「どのモデルサイズ（例: 8b vs                                                                                                    
  70b）が、自分のPCのGPUとCPUのバランスを最も良く利用できているか」という、実用的なベンチマークデータを収集できます。                                
                                                                                                                                                     
  🔗 ステップ5：システムの組み込み（APIラッパーの構築） (Integration)                                                                                
                                                                                                                                                     
  もし、あなたが使用しているシステムがClaudeのAPIエンドポイントを想定している場合、ollama runを直接呼び出すのではなく、**ラッパー関数（Wrapper       
  Function）**を作成する必要があります。                                                                                                             
                                                                                                                                                     
  Pythonでの実装例 (概念図):                                                                                                                         
                                                                                                                                                     
  import subprocess                                                                                                                                  
  import json                                                                                                                                        
                                                                                                                                                     
  def call_local_llm(prompt: str) -> str:                                                                                                            
      """ローカルで実行されるLLM（ollama経由）を呼び出す関数"""                                                                                      
      try:                                                                                                                                           
          # ollama CLIを呼び出すコマンド例 (実際の引数は使用するモデルによる)                                                                        
          result = subprocess.run(                                                                                                                   
              ['ollama', 'run', 'llama3', prompt],                                                                                                   
              capture_output=True, text=True, check=True                                                                                             
          )                                                                                                                                          
          return result.stdout.strip()                                                                                                               
      except subprocess.CalledProcessError as e:                                                                                                     
          return f"エラーが発生しました: {e.stderr}"                                                                                                 
                                                                                                                                                     
  # 以降のコードで、この関数を呼び出すことで、APIコールと同様の体験を実現できる                                                                      
                                                                                                                                                     
  まとめと次のステップ                                                                                                                               
                                                                                                                                                     
  1. 導入の成功: ollama（または使用するモデルのランナー）をインストールし、基本的なモデルが動作することを確認してください。                          
  2. 性能ベンチマーク: 複数のモデル（例: Llama 3 vs                                                                                                  
  Mixtral）を同じプロンプトで動かし、どのモデルが最も高速か、または最も高品質な出力をするかを比較してください。                                      
  3. 安定性の確保: 長時間の連続利用を行う場合は、システムリソース（特にGPUメモリ）を監視しながら運用することが重要です。                             
                                                                                                                                                     
  ---【補足】                                                                                                                                        
  - Ollamaの利用: 本記事の記述では、ローカルLLMの実行にollamaという非常に人気のあるCLIツールを利用する前提で解説しています。これにより、モデルの管理 
  と実行が非常に容易になります。                                                                                                                     
  - モデルの選択: 「Claude 3 Opus」レベルの知性をローカルで再現するには、高性能なGPU（例: 12GB以上のVRAM）と、最適化された大規模モデル（例: Llama 3  
  70Bなどの量子化版）の導入が必要です。                                                                                                              
                                                                                                                                                     
✻ Crunched for 2m 11s
```

出力まで2分11秒かかりました。
sonnet4.6と比べると体感ではありますがやはり出力に時間はかかる印象です。
しかし、求めていたものと方向性は合っていたので意外と使えるなという印象です。

今回はAPIラッパーは扱うつもりはないのでそれは消してもらいましょう。

```sh
❯ pythonの項目はけして。                                                                                                                             
あくまでollama経由でclaude codeを動かすのが目的なので。                                                                                              
                                                                                                                                                     
⏺ どのような「項目」を指しているのか、もう少し詳しい情報を提供していただけますでしょうか？                                                           
                                                                                                                                                     
  例えば、以下のようないくつかの状況が考えられます。                                                                                                 
                                                                                                                                                     
  1. 以前の会話の内容について： 特定のトピックや機能に関する記述を消去したい。                                                                       
  2. コードやファイルの内容について： 特定のコードブロックやファイルの内容から記述を削除したい。                                                     
  3. 設定や指示の内容について： システムプロンプトや前提条件から何かを削除したい。                                                                   
                                                                                                                                                     
  どのような文脈で「項目を消したい」のかを教えていただければ、適切な対応ができます。                                                                 
                                                                                                                                                     
❯ ステップ5がいらないんじゃないかという話。                                                                                                          
                                                                                                                                                     
⏺ 「ステップ5が不要ではないか」というご意見ですね。                                                                                                  
                                                                                                                                                     
  どのような文脈（例えば、どのプロセス、どのドキュメント、どのような手順の話）でステップ5について話されているか教えていただけますでしょうか？        
                                                                                                                                                     
  文脈が分かれば、ステップ5をスキップすることによる影響や、代わりにどのようなアプローチが適切かなど、より具体的なご意見を差し上げることができます。  
                                                                                                                                                     
  お手数ですが、詳細を教えていただけますでしょうか。
```

うーん、忘れられてますね...。

ちなみにこの時点でのトークンは4.1kとのこと。
[公式](https://ai.google.dev/gemma/docs/core/model_card_4?hl=ja#dense_models)曰く128kまで大丈夫らしいですがなぜでしょう？

```sh
❯ /context                                                                                                                                           
  ⎿  Context Usage                                                                                                                                   
     ⛁ ⛁ ⛁ ⛁ ⛁ ⛁ ⛁ ⛀ ⛁ ⛁   gemma4                                                                                                                    
                           4.1k/200k tokens (2%)                                                                                                     
     ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶                                                                                                                             
                           Estimated usage by category                                                                                               
     ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶   ⛁ System prompt: 6k tokens (3.0%)                                                                                         
                           ⛁ System tools: 7.7k tokens (3.9%)                                                                                        
     ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶   ⛁ Skills: 476 tokens (0.2%)                                                                                               
                           ⛁ Messages: 3.9k tokens (2.0%)                                                                                            
     ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶   ⛶ Free space: 148.8k (74.4%)                                                                                           
                           ⛝ Autocompact buffer: 33k tokens (16.5%)
     ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶

     ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶

     ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶ ⛶

     ⛶ ⛶ ⛶ ⛝ ⛝ ⛝ ⛝ ⛝ ⛝ ⛝

     ⛝ ⛝ ⛝ ⛝ ⛝ ⛝ ⛝ ⛝ ⛝ ⛝

     Skills · /skills
```

`/clear`をして別のプロンプトを投げましたがそれでも直前の記憶を失ってしまっていました。
~~[公式の推奨モデル](https://docs.ollama.com/integrations/claude-code#recommended-models)にgemma4は含まれてないので他のモデルだったら解決するかも？~~

:::note info
追記（2026/04/18）

会話が忘れられる問題の原因はollama起動時のトークン設定の上限に達したことが原因でした。
[ollama公式](https://docs.ollama.com/context-length)によると、認識したVRAM容量に合わせてトークンの上限を設定するようです。
使用したmac miniの場合、24GiBに満たないので上限が4kに設定されていました。
claude codeでやりとり1往復した後のcontext量とほぼ同じなので次の会話になるとすぐ忘れてしまうようです。


`OLLAMA_CONTEXT_LENGTH=64000 ollama serve`で起動した後別ウィンドウでollamaからclaude codeを呼び出せば前の会話を引き継いでくれました。
:::

### cpuとメモリ使用率

実際に上記のプロンプトを処理しているときのアクティビティモニタのキャプチャです。

- cpu
![スクリーンショット 2026-04-17 19.22.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2630405/c52529f0-8d93-4793-9441-6cecb9b388b3.png)

- メモリ
![スクリーンショット 2026-04-17 19.02.43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2630405/d8364593-20fc-4ec9-8d87-48dfc48cde5f.png)

![スクリーンショット 2026-04-17 19.02.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2630405/2ca756b8-c33a-4cdb-846c-98e905e02b44.png)

cpuは全く問題なかったんですがメモリが約10GB使用されていました。
ブラウザやらメーラーやら普段仕事で使うであろうアプリも立ち上げているとおよそ23GBもメモリを消費することになりました。

ollama起動時とかにはマウスカーソルが動かなくなるくらいには負荷がかかります。
ただ、一度読み込みが完了してしまえばフリーズするような様子はなかったです。

# まとめ
- ollamaでモデルをダウンロードするだけで簡単にローカル環境でLLMを動かせる。
- ollama経由でclaude codeも動かせる。
  - ~~しかし、**直前の記憶を失ってしまっていた。** これがgemma4によるものなのかclaude codeの特定のバージョンによるものかは不明。~~
- mac mini m4 であればgemma4:e4bは起動時以外は問題なく動作する。

# 感想

初めてローカル環境でAIを動かしたわけですが、あっという間に動かせるのはありがたかったです。
先人たちが色々と環境を整えてくれたおかげですね。
当然使うモデルに左右はされますが10万円以下のマシンで動かせてしまうわけですし、小型モデルが出た時には色々試してみて自分の環境に合うものを探したいですね。

~~claude codeで会話を続けられなかったのは残念でしたが、同じような話はweb検索する限りヒットせず。~~
~~どうも「おま環」っぽいので原因わかったらまた記事書きます。~~
~~同じ問題起きたけどこうしたら治るという情報お持ちであればコメントください 🙇~~
