---
title: Docker コンテナのアクションを作成する
intro: このガイドでは、Docker コンテナのアクションを作成するために最低限必要なステップを案内します。
redirect_from:
  - /articles/creating-a-docker-container-action
  - /github/automating-your-workflow-with-github-actions/creating-a-docker-container-action
  - /actions/automating-your-workflow-with-github-actions/creating-a-docker-container-action
  - /actions/building-actions/creating-a-docker-container-action
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: tutorial
topics:
  - Action development
  - Docker
shortTitle: Docker container action
---

{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}

## はじめに

このガイドでは、パッケージ化されたDockerコンテナのアクションを作成して使うために必要な、基本的コンポーネントについて学びます。 アクションのパッケージ化に必要なコンポーネントのガイドに焦点を当てるため、アクションのコードの機能は最小限に留めます。 このアクションは、ログに "Hello World" を出力するものです。また、カスタム名を指定した場合は、"Hello [who-to-greet]" を出力します。

このプロジェクトを完了すると、あなたの Docker コンテナのアクションをビルドして、ワークフローでテストする方法が理解できます。

{% data reusables.actions.self-hosted-runner-reqs-docker %}

{% data reusables.actions.context-injection-warning %}

## 必要な環境

{% data variables.product.prodname_actions %}の環境変数及びDockerコンテナのファイルシステムに関する基本的な理解があれば役立つでしょう。

- [環境変数の利用](/actions/automating-your-workflow-with-github-actions/using-environment-variables)
{% ifversion ghae %}
- 「[Docker コンテナファイルシステム](/actions/using-github-hosted-runners/about-ae-hosted-runners#docker-container-filesystem)」
{% else %}
- "[About {% data variables.product.prodname_dotcom %}-hosted runners](/actions/using-github-hosted-runners/about-github-hosted-runners#docker-container-filesystem)"
{% endif %}

始める前に、{% data variables.product.prodname_dotcom %} リポジトリを作成する必要があります。

1. {% data variables.product.product_location %} に新しいリポジトリを作成します。 リポジトリ名は任意です。この例のように "hello-world-docker-action" を使ってもいいでしょう。 詳しい情報については、「[新しいリポジトリの作成](/articles/creating-a-new-repository)」を参照してください。

1. リポジトリをお手元のコンピューターにクローンします。 詳しい情報については[リポジトリのクローン](/articles/cloning-a-repository)を参照してください。

1. ターミナルから、ディレクトリを新しいリポジトリに変更します。

  ```shell{:copy}
  cd hello-world-docker-action
  ```

## Dockerfileの作成

新しい`hello-world-docker-action`ディレクトリ内に、新たに`Dockerfile`というファイルを作成してください。 Make sure that your filename is capitalized correctly (use a capital `D` but not a capital `f`) if you're having issues. 詳しい情報については、「[{% data variables.product.prodname_actions %} のための Dockerfile サポート](/actions/creating-actions/dockerfile-support-for-github-actions)」を参照してください。

**Dockerfile**
```Dockerfile{:copy}
# コードを実行するコンテナイメージ
FROM alpine:3.10

# アクションのリポジトリからコードファイルをコンテナのファイルシステムパス `/`にコピー
COPY entrypoint.sh /entrypoint.sh

# dockerコンテナが起動する際に実行されるコードファイル (`entrypoint.sh`)
ENTRYPOINT ["/entrypoint.sh"]
```

## アクションのメタデータファイルの作成

新しい `action.yml` ファイルを、上で作成した `hello-world-docker-action` ディレクトリの中に作成します。 詳しい情報については、「[{% data variables.product.prodname_actions %} のメタデータ構文](/actions/creating-actions/metadata-syntax-for-github-actions)」を参照してください。

{% raw %}
**action.yml**
```yaml{:copy}
# action.yml
name: 'Hello World'
description: 'Greet someone and record the time'
inputs:
  who-to-greet:  # id of input
    description: 'Who to greet'
    required: true
    default: 'World'
outputs:
  time: # id of output
    description: 'The time we greeted you'
runs:
  using: 'docker'
  image: 'Dockerfile'
  args:
    - ${{ inputs.who-to-greet }}
```
{% endraw %}

このメタデータは、1 つの `who-to-greet` 入力と 1 つの `time` 出力パラメータを定義しています。 To pass inputs to the Docker container, you should declare the input using `inputs` and pass the input in the `args` keyword. Everything you include in `args` is passed to the container, but for better discoverability for users of your action, we recommended using inputs.

{% data variables.product.prodname_dotcom %} は `Dockerfile` からイメージをビルドし、このイメージを使用して新しいコンテナでコマンドを実行します。

## アクションのコードの記述

任意のベース Docker イメージを選択できるので、アクションに任意の言語を選択できます。 次のシェルスクリプトの例では、`who-to-greet` 入力変数を使って、ログファイルに "Hello [who-to-greet]" と出力します。

次に、スクリプトは現在の時刻を取得し、それをジョブ内で後に実行するアクションが利用できる出力変数に設定します。 {% data variables.product.prodname_dotcom %}に出力変数を認識させるには、`echo "::set-output name=<output name>::<value>"`という構文でワークフローコマンドを使わなければなりません。 詳しい情報については「[{% data variables.product.prodname_actions %}のワークフローコマンド](/actions/reference/workflow-commands-for-github-actions#setting-an-output-parameter)」を参照してください。

1. `hello-world-docker-action` ディレクトリに、新しい `entrypoint.sh` を作成します。

1. `entrypoint.sh`ファイルに次のコードを追加します。

  **entrypoint.sh**
  ```shell{:copy}
  #!/bin/sh -l

  echo "Hello $1"
  time=$(date)
  echo "::set-output name=time::$time"
  ```
  `entrypoint.sh`がエラーなく実行できたら、アクションのステータスは`success`に設定されます。 アクションのコード中で明示的に終了コードを設定して、アクションのステータスを提供することもできます。 詳しい情報については「[アクションの終了コードの設定](/actions/creating-actions/setting-exit-codes-for-actions)」を参照してください。

1. 以下のコマンドをシステムで実行して、`entrypoint.sh`ファイルを実行可能にしてください。

  ```shell{:copy}
  $ chmod +x entrypoint.sh
  ```

## READMEの作成

アクションの使用方法を説明するために、README ファイルを作成できます。 README はアクションの公開を計画している時に非常に役立ちます。また、アクションの使い方をあなたやチームが覚えておく方法としても優れています。

`hello-world-docker-action` ディレクトリの中に、以下の情報を記述した `README.md` ファイルを作成してください。

- アクションが実行する内容の詳細
- 必須の入力引数と出力引数
- オプションの入力引数と出力引数
- アクションが使用するシークレット
- アクションが使用する環境変数
- ワークフローでアクションを使う使用方法の例

**README.md**
```markdown{:copy}
# Hello world docker action

このアクションは"Hello World"もしくは"Hello" + ログに挨拶する人物名を出力します。

## Inputs

## `who-to-greet`

**Required** The name of the person to greet. デフォルトは `"World"`。

## Outputs

## `time`

The time we greeted you.

## 使用例

uses: actions/hello-world-docker-action@v1
with:
  who-to-greet: 'Mona the Octocat'
```

## アクションの{% data variables.product.product_name %}へのコミットとタグ、プッシュ

ターミナルから、`action.yml`、`entrypoint.sh`、`Dockerfile`、および `README.md` ファイルをコミットします。

アクションのリリースにはバージョンタグを加えることもベストプラクティスです。 アクションのバージョン管理の詳細については、「[アクションについて](/actions/automating-your-workflow-with-github-actions/about-actions#using-release-management-for-actions)」を参照してください。

```shell{:copy}
git add action.yml entrypoint.sh Dockerfile README.md
git commit -m "My first action is ready"
git tag -a -m "My first action release" v1
git push --follow-tags
```

## ワークフローでアクションをテストする

これで、ワークフローでアクションをテストできるようになりました。 プライベートリポジトリにあるアクションは、同じリポジトリのワークフローでしか使用できません。 パブリックアクションは、どのリポジトリのワークフローでも使用できます。

{% data reusables.actions.enterprise-marketplace-actions %}

### パブリックアクションを使用する例

以下のワークフローのコードは、パブリックの[`actions/hello-world-docker-action`](https://github.com/actions/hello-world-docker-action)リポジトリ内の完成した_hello world_アクションを使います。 次のワークフローサンプルコードを `.github/workflows/main.yml` にコピーし、`actions/hello-world-docker-action` をあなたのリポジトリとアクション名に置き換えてください。 `who-to-greet`の入力を自分の名前に置き換えることもできます。 {% ifversion fpt or ghec %}パブリックなアクションは、{% data variables.product.prodname_marketplace %}に公開されていなくても使うことができます。 詳しい情報については「[アクションの公開](/actions/creating-actions/publishing-actions-in-github-marketplace#publishing-an-action)」を参照してください。 {% endif %}

{% raw %}
**.github/workflows/main.yml**
```yaml{:copy}
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      - name: Hello world action step
        id: hello
        uses: actions/hello-world-docker-action@v1
        with:
          who-to-greet: 'Mona the Octocat'
      # `hello` ステップからの出力を使用する
      - name: Get the output time
        run: echo "The time was ${{ steps.hello.outputs.time }}"
```
{% endraw %}

### プライベートアクションを使用する例

次のワークフローコードサンプルを、あなたのアクションのリポジトリの `.github/workflows/main.yml` ファイルにコピーします。 `who-to-greet`の入力を自分の名前に置き換えることもできます。 {% ifversion fpt or ghec %}このプライベートのアクションは{% data variables.product.prodname_marketplace %}に公開する事はできず、このリポジトリ内でのみ利用できます。{% endif %}

**.github/workflows/main.yml**
```yaml{:copy}
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      # To use this repository's private action,
      # you must check out the repository
      - name: Checkout
        uses: {% data reusables.actions.action-checkout %}
      - name: Hello world action step
        uses: ./ # Uses an action in the root directory
        id: hello
        with:
          who-to-greet: 'Mona the Octocat'
      # Use the output from the `hello` step
      - name: Get the output time
        run: echo "The time was {% raw %}${{ steps.hello.outputs.time }}"{% endraw %}
```

リポジトリから [**Actions**] タブをクリックして、最新のワークフロー実行を選択します。 Under **Jobs** or in the visualization graph, click **A job to say hello**. "Hello Mona the Octocat"、または`who-to-greet` 入力に指定した名前とタイムスタンプがログに出力されます。

![ワークフローでアクションを使用しているスクリーンショット](/assets/images/help/repository/docker-action-workflow-run-updated.png)

