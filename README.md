# ちゃんと動くTypeScriptのサンプルプロジェクト
ChatVRM_testで使ったもので、vercelにデプロイして開発していた

## TypeScriptはクライアントサイドとサーバーサイドでできることが違う

`index.tsx`と`audioConverter.ts`の役割について説明する際に「クライアントサイド」と「サーバーサイド」という言葉を使用することは一般的ではありません。これらの用語は、アプリケーションのアーキテクチャにおける実行環境の違いを指すため、混乱を招く可能性があります。正しい用語使用について説明します。

### クライアントサイドとサーバーサイド

- **クライアントサイド**（フロントエンド）: ユーザーのブラウザ上で実行されるコードです。HTML、CSS、JavaScript（ReactやVue.jsなどのライブラリやフレームワークを含む）がこれに該当します。

- **サーバーサイド**（バックエンド）: サーバー上で実行されるコードで、Node.js、Python、Ruby、PHPなどの言語を使用して書かれます。このコードは、データベースの操作、認証、APIエンドポイントの提供などを担います。

### `index.tsx`と`audioConverter.ts`の役割

- **`index.tsx`**: これはReact（Next.jsを含む）コンポーネントファイルで、主にクライアントサイドで実行されるコードを含みます。ただし、Next.jsではサーバーサイドレンダリング（SSR）や静的サイト生成（SSG）の機能も提供するため、このファイルの一部のコードはサーバーで実行されることもあります。しかし、主な役割はフロントエンド、つまりクライアントサイドの機能を提供することです。

- **`audioConverter.ts`**: これは特定の機能（この場合はテキストを音声に変換する機能）を提供するためのユーティリティまたはサービスファイルです。このファイル自体はクライアントサイドで実行されるJavaScriptまたはTypeScriptコードを含む可能性がありますが、外部APIへのリクエストを行うために使用されることが多いです。そのため、直接サーバーサイドというわけではなく、APIリクエストを介してサーバーサイドのリソース（API）を利用するクライアントサイドのコードと考えるべきです。

この説明から、`index.tsx`と`audioConverter.ts`はどちらもクライアントサイドでの実行を目的としていることがわかります。`audioConverter.ts`が「サーバーサイド」と呼ばれることはありませんが、外部のサーバーサイドリソース（API）を利用するためのコードを含むことがあります。


### クライアントサイドでやるべき処理：ブラウザのAPI（例：window, document）やクライアントサイドでのみ利用可能なライブラリを使う場合。

- documentやその他のブラウザ固有のオブジェクトを使用するコードはクライアントサイドで実行する。
- これを実現する一般的な方法は、useEffectフックを使用する。
- たとえば、document.getElementByIdを使用している場合、それをuseEffect内に移動し、コンポーネントの初期レンダリング後にクライアントサイドで実行されるようにします。
- 一般原則は、ブラウザ固有のAPI（window、documentなど）を直接使用するコードはクライアントサイドでのみ実行する。


## クライアントサイドとサーバーサイドを見分ける方法(明確な見極め方はないのでその都度判断する)
ファイルの配置場所: 多くのフレームワークやプロジェクトでは、ファイルの配置場所に基づいてクライアントサイドとサーバーサイドのコードを区別します。例えば、Next.jsではpages/apiディレクトリ内のファイルがAPIルートとしてサーバーサイドで実行されることが期待されています。一方で、pagesディレクトリ内の他のファイルはページコンポーネントとして、サーバーサイドでのレンダリングやクライアントサイドでの動的なインタラクションの両方に使用されます。

コード内のAPI: 使用しているAPIやオブジェクトがブラウザ固有かどうかを確認します。例えば、windowやdocumentはブラウザ固有のオブジェクトであり、これらを使用しているコードはクライアントサイドで実行されることを意味します。一方、Node.js固有のAPIやモジュール（例えば、fsやprocess）を使用しているコードはサーバーサイドで実行されることを意味します。

---

実行環境のチェック: 実行時に環境をチェックすることで、コードがクライアントサイドかサーバーサイドのどちらで実行されているかを判断することができます。例えば、typeof window === 'undefined'をチェックすることで、コードがサーバーサイド環境で実行されているかどうかを確認できます。
