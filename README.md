# TrussForce Webアプリ Web連携体験談🍵（クラウドにデプロイしていないから正式にはローカルで動かせて満足している状態）

トラス構造の**部材力を自動計算**してくれるシンプルで使いやすいWebアプリ（にするつもり）です。  
設計した接点と部材のデータを入力すると、反力や部材ごとの軸力（引張・圧縮）がすぐにわかります。  
最初は手元の.jarで動かして満足していましたが、フロントもバックも触りたくなって挑戦したら、あれよあれよと沼に沈みました。


## 背景

トラス構造の部材力を自動で計算するWebアプリを作成しました。  
最初はローカル環境で動作確認をしていたものの、友人や他の人に使ってもらうためにはWeb上に公開する必要がありました。


![画面1](スクリーンショット%202025-06-21%20213143.png)
![画面2](スクリーンショット%202025-06-21%20213159.png)


## 🔥 特徴

- Reactでサクッと動くシングルページアプリ
- ノード（接点）とメンバー（部材）を簡単に追加・削除可能
- 荷重や支点条件も直感的に設定できる
- 計算結果はリアルタイムで見やすく表示
- 部材力は自動でソートして分かりやすく提示
- Web API連携でバックエンドのJava計算ロジックと繋げている


## 🚀 使い方

1. 接点と部材を入力  
2. 支点や荷重を設定  
3. 「送信」ボタンを押すと計算結果が表示される  


## 🛠️ 技術スタック

- フロントエンド：React  
- バックエンド：Java（Spring Boot + 計算ロジック）  
- API通信：fetchでJSONデータの送受信  


# 🌐 TrussForce 処理の流れまとめ（Web版）

このドキュメントは、TrussForceのフロントエンドからバックエンド、計算ロジック、結果の表示までの一連の流れを説明します。


## 🔁 処理フロー図（ざっくり）

```text
[React画面]
    ↓
fetch POST (JSON: nodes, members)
    ↓
Spring Controller (@RestController)
    ↓
InputDto にパース (@RequestBody)
    ↓
Serviceクラスが Solver を呼ぶ
    ↓
TrussForceSolver（反力・部材力計算）
    ↓
ResultDto に格納
    ↓
JSON形式でフロントへ返す
    ↓
Reactで結果を描画  

```

## 🧱 コンポーネント詳細  
  
1. React UI（App.jsx or App.js）  
ユーザーがノード・部材・荷重・支点条件を入力  

送信ボタンで fetch("/api/solve", { method: "POST", body: JSON })  

結果は setResult() で保存 → 表示  

2. Spring Boot Controller（TrussForceController.java）  
java


```  
@PostMapping("/api/solve")
public ResultDto solve(@RequestBody InputDto input) {
    return trussForceService.solve(input);
}

```

APIの受け口  

@RequestBody でJSONを InputDto に変換

計算処理は Service に丸投げ  


3. Service層（TrussForceService.java）
java

```
public ResultDto solve(InputDto inputDto) {
    // DTOからモデル生成（必要なら）
    return solver.solve(inputDto);
}

```
必要があれば DTO → Entity に変換

Solver（ビジネスロジック）に処理を依頼

4. Solver（TrussForceSolver.java）
java

```
public ResultDto solve(InputDto dto) {
    // 反力計算
    // 部材力計算
    // 結果まとめて return new ResultDto(...)
}

```

Javaで実装されたトラス構造の力学計算

静定条件の確認、反力解法、部材軸力計算を実行


5. ResultDto（ResultDto.java）
計算結果をJSON形式でまとめるためのオブジェクト

含まれる項目:

Map<Integer, Double> reactionsX

Map<Integer, Double> reactionsY

Map<String, Double> memberForces  

## 🧪 補足Tips
Controller は「APIの玄関」。

Service は「交通整理係」。

Solver が「頭脳」。

DTO は「データの箱」。

React は「見た目と操作」。

fetch は「橋渡し役」。  


## 💡 こんな人におすすめ

- 構造力学を勉強中の学生
- 力学大好きな高校生  
- トラス設計の初歩を手軽に確認したいエンジニア  
- Webで手軽に部材力計算したい人
- 部材力を手計算で求めろとか言われた大学生  


## 苦戦したポイント

- **ローカルでしか動かない問題**  
  ローカルのPCでサーバーを立ててAPIを叩いていたため、他人からはアクセスできませんでした。  
- **クラウドのハードル**  
  AWSやAzureは名前を聞いたことはあったものの、設定の複雑さやクレジットカードの問題でなかなか手を出せませんでした（まだ出せてません）。  


## 学んだこと

- GitHubにコードを置くだけでは、Webアプリとしては使えません。  
- クラウドサービスでのデプロイが必要不可欠。
- ローカル開発だけで満足せず、公開までセットで考えるべき（最初からクラウドに上げたいって思ってるならローカルでテストする必要ないと思われた）。  


## まとめ

最初は「ローカルだけでいいや」と思っていても、実際に誰かに使ってもらうならクラウドへのデプロイは必須です。    
これからWebアプリを作る人は、早めにクラウド公開まで進めることをおすすめします。


## 質問受け付け中

2Dトラスの部材力・反力をどうしてもすぐ計算したい大学生はここに接点座標、部材番号、支点情報（ピン、ローラー固定）を書いてください。気が向いたら計算結果送ります。  
https://github.com/Airi20/TrussForce-Web-/issues/new

