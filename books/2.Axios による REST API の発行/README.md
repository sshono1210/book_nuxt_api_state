# Axios による REST API の発行

Nuxt.js などの フロントアプリケーションでは、
サーバとのデータの受け渡しは、REST API を利用して行うのが一般的です。

JavaScript では REST API の通信に際し、 axios というライブラリを用いる事が多く、
Nuxt.js でも これを利用するための axios モジュールが提供されています。

まずは Nuxt.js 上で axios モジュールを利用するために、以下のコマンドでモジュールのインストールをを粉います。

```
$ npm i @nuxtjs/axios
```

モジュールのインストールが終わったら、axios モジュールを有効化するために`nuxt.config.js`を修正します。

```js
module.exports = {
  modules: [
    '@nuxtjs/axios',
  ],
  axios: {
    baseURL: "https://api.github.com"
  }
}
```

`modules` に `@nuxtjs/axios` を追加するのに加え、
`axios` セクションを追加して、今回主に通信する Github API の baseURL を設定しておきます。

## APIを発行する

準備ができたら実際に API を発行してみましょう。

Github の Issue 一覧ページを作成するために、 Github の REST API を実行して、
Issue 一覧 のデータを取得します。

Github の REST API の一覧は以下の公式ドキュメントからその使いかたを確認する事ができます。

https://developer.github.com/v3/

API の実行には Github の Personal Access Token が必要になるため、以下のURLから事前に準備しておいてください。

https://github.com/settings/tokens

### Issue の一覧を取得する

Github から Issue の 一覧を取得する場合 `GET /repos/:owner/:repo/issues` の API を使用します。

[Issues \| GitHub Developer Guide](https://developer.github.com/v3/issues/#list-issues-for-a-repository)

Issue 一覧画面にて、ページ読み込み時にIssue の一覧を取得する場合、
以下のような形で mounted 内で APIの発行処理を記述します。

```
export default {
  data(){
    return {
      issues: []
    }
  },
  async mounted(){
    const url = "/repos/lec-cafe/book_nuxt_api_state/issues"
    const response = await this.$axios.get(url, {
      headers: {
        Authorization: "token YOUR_GITHUB_PRESONAL_ACCESS_TOKEN"
      }
    })
    console.log(response)
    this.issues = response.data
  }
}
```

`YOUR_GITHUB_PRESONAL_ACCESS_TOKEN` の箇所は Github から取得した Personal Access Token の値で置き換えてください。

response のデータは `response.data` に格納されているため、以下のように記述するケースも多いでしょう。

```
export default {
  async mounted(){
    const url = "/repos/lec-cafe/book_nuxt_api_state/issues"
    const {data} = await this.$axios.get(url, {
      headers: {
        Authorization: "token YOUR_GITHUB_PRESONAL_ACCESS_TOKEN"
      }
    })
    console.log(data)
    this.issues = data
  }
}
```

::: tip 
async await の使いかたについては [こちら](/9.1.Promise%20と%20async%20await/) のドキュメントを確認してください。
:::

::: tip 
axios モジュールの詳細な使いかたについては [こちら](/9.2.axios%20モジュールの使いかた/) のドキュメントを確認してください。
:::

一覧のデータが取得が REST API 経由で実施できたら、リポジトリ名を変更するなどして、
様々なリポジトリ上の Issue 一覧を画面に表示してみましょう。

自分の所有するリポジトリの Issue 一覧が確認できるようになったら、以下の Issue の作成 API を使用して
Issue 作成画面から Issue を作成できるように変更してみましょう。

https://developer.github.com/v3/issues/#create-an-issue
