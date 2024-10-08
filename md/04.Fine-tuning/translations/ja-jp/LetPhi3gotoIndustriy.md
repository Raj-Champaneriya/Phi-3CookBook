# **Phi-3を業界の専門家にする**

Phi-3モデルを業界に導入するには、業界のビジネスデータをPhi-3モデルに追加する必要があります。私たちには2つの異なる選択肢があります。1つ目はRAG（Retrieval Augmented Generation）で、2つ目はファインチューニングです。

## **RAG vs Fine-Tuning**

### **Retrieval Augmented Generation**

RAGはデータ検索+テキスト生成です。企業の構造化データと非構造化データはベクトルデータベースに保存されます。関連するコンテンツを検索する際に、関連する要約とコンテンツを見つけてコンテキストを形成し、LLM/SLMのテキスト補完機能と組み合わせてコンテンツを生成します。

### **ファインチューニング**

ファインチューニングは特定のモデルの改良に基づいています。モデルアルゴリズムから始める必要はありませんが、データを継続的に蓄積する必要があります。業界アプリケーションでより正確な用語と言語表現を求める場合、ファインチューニングがより良い選択です。しかし、データが頻繁に変わる場合、ファインチューニングは複雑になる可能性があります。

### **どちらを選ぶか**

1. 回答に外部データの導入が必要な場合、RAGが最適な選択です。

2. 安定して正確な業界知識を出力する必要がある場合、ファインチューニングが良い選択です。RAGは関連するコンテンツを優先的に引き出しますが、専門的なニュアンスを常に把握できるわけではありません。

3. ファインチューニングには高品質なデータセットが必要であり、少量のデータでは大きな違いはありません。RAGはより柔軟です。

4. ファインチューニングはブラックボックスであり、内部メカニズムを理解するのが難しいです。しかし、RAGはデータの出所を見つけやすく、幻覚やコンテンツエラーを効果的に調整し、より良い透明性を提供します。

### **シナリオ**

1. 垂直業界では特定の専門用語や表現が必要であり、***ファインチューニング***が最適な選択です。

2. QAシステムでは、異なる知識点の統合が必要であり、***RAG***が最適な選択です。

3. 自動化されたビジネスフローの組み合わせでは、***RAG + ファインチューニング***が最適な選択です。

## **RAGの使用方法**

![rag](../../../../imgs/04/01/RAG.png)

ベクトルデータベースは、数学的な形式でデータを保存するコレクションです。ベクトルデータベースは、機械学習モデルが以前の入力を記憶するのを容易にし、検索、推薦、テキスト生成などのユースケースをサポートするために使用されます。データは正確な一致ではなく、類似性メトリクスに基づいて識別されるため、コンピュータモデルがデータのコンテキストを理解できるようになります。

ベクトルデータベースはRAGを実現するための鍵です。データをベクトルモデル（例：text-embedding-3、jina-ai-embeddingなど）を使用してベクトルストレージに変換できます。

RAGアプリケーションの作成について詳しくは、[https://github.com/microsoft/Phi-3CookBook](https://github.com/microsoft/Phi-3CookBook?WT.mc_id=aiml-138114-kinfeylo)をご覧ください。

## **ファインチューニングの使用方法**

ファインチューニングでよく使用されるアルゴリズムはLoraとQLoraです。どちらを選ぶか？
- [このサンプルノートブックで詳細を学ぶ](../../../../code/04.Finetuning/Phi_3_Inference_Finetuning.ipynb)
- [Pythonファインチューニングサンプルの例](../../../../code/04.Finetuning/FineTrainingScript.py)

### **LoraとQLora**

![lora](../../../../imgs/04/01/qlora.png)

LoRA（低ランク適応）とQLoRA（量子化低ランク適応）は、どちらもパラメータ効率の高いファインチューニング（PEFT）技術を使用して大規模言語モデル（LLM）をファインチューニングする技術です。PEFT技術は、従来の方法よりも効率的にモデルをトレーニングするように設計されています。
LoRAは、重み更新行列に低ランク近似を適用することでメモリフットプリントを削減する独立したファインチューニング技術です。トレーニング時間が短く、従来のファインチューニング方法に近いパフォーマンスを維持します。

QLoRAは、量子化技術を組み込んでメモリ使用量をさらに削減するLoRAの拡張バージョンです。QLoRAは、事前トレーニングされたLLMの重みパラメータを4ビット精度に量子化し、LoRAよりもメモリ効率が高いです。ただし、追加の量子化および非量子化ステップがあるため、QLoRAのトレーニングはLoRAのトレーニングよりも約30％遅くなります。

QLoRAは、量子化エラー中に導入されたエラーを修正するためにLoRAを補助として使用します。QLoRAは、数十億のパラメータを持つ大規模モデルを比較的小さく、利用可能なGPUでファインチューニングすることを可能にします。たとえば、QLoRAは、36個のGPUが必要な70Bパラメータモデルをわずか2個のGPUでファインチューニングできます。
