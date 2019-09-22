# React公式チュートリアルをTypeScriptでやってみる

チュートリアル  
https://ja.reactjs.org/tutorial/tutorial.html

Vimを使うのでTypeScript関連のプラグインを入れておく。  
https://github.com/Quramy/tsuquyomi  
https://github.com/leafgarland/typescript-vim  

進め方は以下を参考。  
https://note.mu/tkugimot/n/nf7fe751298b1  

TypeScriptで記述する場合も `npm start` するだけでアプリケーションは動作する。

データをProps経由で渡す場合、TypeScriptではInterfaceでの定義が必要。

しっかり開発するならReact Devtools拡張は必須。

状態を子が持つべきか、親が持つべきかを意識する。  
複数の子コンポーネントからデータを集めたい、ある2つの子コンポーネントを相互にやりとりさせたい、と思ったら、基本的に親コンポーネントに状態を持たせる。  

状態を更新するとき、例えばその状態が配列なら、 `slice()` を使用して配列をコピーし、それを変更すると良い。  
イミュータビリティは重要な考え方である。

クラスのコンポーネントは常にpropsを引数として親クラスのコンストラクタを呼び出す必要がある。

```js
constructor(props) {
    super(props);
    this.state = {date: new Date()};
}
```

```ts
interface GameStateInterface {
    // オブジェクトの配列。そのオブジェクトはsquaresをキーに持ち、値はstringの配列。
    history: {squares: string[]}[];
    // ...
}
```

# React.jsを途中からTypeScript化する on Laravel

https://engineering.mobalab.net/2019/07/01/react-jsのコードをtypescript化する/

### インストール済みパッケージのリストを確認してみる

インストール済みパッケージ表示（トップ階層のみ）

```
$ npm ls --depth=0
```

グローバルのインストール済みパッケージ表示

```
$ npm ls -g --depth=0
```

一度 `npm` についてしっかり学びたい。

`typescript` と `ts-loader` は必要。

`@types/react` と `@types/react-dom` も必要っぽい。

redux使ってるなら `@types/react-redux` も必要。

### 変更のポイント

- propsはinterfaceの定義が必要
- jsxを記述しているコードの拡張子はtsx

TypeScriptでFetch APIを使用する場合  
https://stackoverflow.com/questions/47754183/typescript-cannot-add-headers-to-a-fetch-api-using-react-native?rq=1