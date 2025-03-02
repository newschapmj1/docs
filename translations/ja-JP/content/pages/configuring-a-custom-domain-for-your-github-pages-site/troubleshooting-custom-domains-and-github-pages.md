---
title: カスタムドメインとGitHub Pages のトラブルシューティング
intro: '{% data variables.product.prodname_pages %} サイトのカスタムドメインまたは HTTPS について、よくあるエラーを確認して Issue を解決することができます。'
redirect_from:
  - /articles/my-custom-domain-isn-t-working
  - /articles/custom-domain-isn-t-working
  - /articles/troubleshooting-custom-domains
  - /articles/troubleshooting-custom-domains-and-github-pages
  - /github/working-with-github-pages/troubleshooting-custom-domains-and-github-pages
product: '{% data reusables.gated-features.pages %}'
versions:
  fpt: '*'
  ghec: '*'
topics:
  - Pages
shortTitle: カスタムドメインのトラブルシューティング
---

## _CNAME_ エラー

{% ifversion pages-custom-workflow %}カスタムの{% data variables.product.prodname_actions %}ワークフローから公開しているなら、いかなる_CNAME_ファイルも無視され、必要ではありません。{% endif %}

ブランチから公開しているなら、カスタムドメインは公開ソースのルートにある_CNAME_ファイルに保存されます。 このファイルは、リポジトリ設定を通じて、あるいは手動で追加または更新することができます。 詳しい情報については、「[{% data variables.product.prodname_pages %} サイト用のカスタムドメインを管理する](/articles/managing-a-custom-domain-for-your-github-pages-site)」を参照してください。

サイトが正しいドメインをレンダリングするには、_CNAME_ ファイルがまだリポジトリに存在していることを確認します。 たとえば、静的サイトジェネレータの多くはリポジトリへのプッシュを強制するので、カスタムドメインの設定時にリポジトリに追加された _CNAME_ ファイルを上書きすることができます。 ローカルでサイトをビルドし、生成されたファイルを {% data variables.product.product_name %} にプッシュする場合は、_CNAME_ ファイルをローカルリポジトリに追加したコミットを先にプルして、そのファイルがビルドに含まれるようにする必要があります。

次に、_CNAME_ のフォーマットが正しいことも確認します。

- _CNAME_ ファイル名は、すべて大文字である必要があります。
- _CNAME_ ファイルにはドメインを 1 つだけ含めることができます。 複数のドメインをサイトにポイントするには、DNSプロバイダ経由のリダイレクトを設定する必要があります。
- _CNAME_ ファイルにはドメイン名のみが含まれている必要があります。 たとえば、`www.example.com`、`blog.example.com`、`example.com` などです。
- ドメイン名は、すべての {% data variables.product.prodname_pages %} サイトで一意である必要があります。 たとえば、別のリポジトリの _CNAME_ ファイルに `example.com` が含まれている場合、自分のリポジトリの _CNAME_ ファイルに `example.com` を使用することはできません。

## DNS の設定ミス

サイトのデフォルトドメインをカスタムドメインを指すようにすることに問題がある場合は、DNS プロバイダに連絡してください。

カスタムドメインのDNSレコードが正しく設定されているかをテストするには、以下の方法のいずれかを使うこともできます。

- `dig`のようなCLIツール。 詳しい情報については「[{% data variables.product.prodname_pages %}サイトのカスタムドメインの管理](/articles/managing-a-custom-domain-for-your-github-pages-site)」を参照してください。
- オンラインのDNSルックアップツール。

## サポートされていないカスタムドメイン名

カスタムドメインがサポートされていない場合、使用しているドメインをサポートされているドメインに変更しなければならないかもしれません。 DNSプロバイダに問い合わせて、ドメイン名の転送サービスを提供しているかどうかを確認することもできます。

サイトが以下に当てはまっていないを確認してください。
- 複数の Apex ドメインを使用している。 たとえば、`example.com` と`anotherexample.com` の両方など。
- 複数の `www` サブドメインを使用している。 たとえば、`www.example.com` と`www.anotherexample.com` の両方など。
- Apex ドメインとカスタムサブドメインの両方を使用している。 たとえば、`example.com` と`docs.example.com` の両方など。

  例外は`www`サブドメインです。 正しく設定されていれば、`www`サブドメインは自動的にApexドメインにリダイレクトされます。 詳しい情報については、「[{% data variables.product.prodname_pages %} サイト用のカスタムドメインを管理する](/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain)」を参照してください。

{% data reusables.pages.wildcard-dns-warning %}

サポートされているカスタムサブドメインのリストは、「[カスタムドメインと {% data variables.product.prodname_pages %} について](/articles/about-custom-domains-and-github-pages/#supported-custom-domains)」を参照してください。

## HTTPS エラー

`CNAME`、`ALIAS`、`ANAME` や `A` DNS レコードで適切に設定されたカスタムドメインを使っている {% data variables.product.prodname_pages %} サイトは、HTTPS でアクセスできます。 詳しい情報については[HTTPSで{% data variables.product.prodname_pages %}サイトをセキュアにする](/articles/securing-your-github-pages-site-with-https)を参照してください。

カスタムドメインを設定した後、サイトが HTTPS 経由で利用可能になるには最長 1 時間かかります。 既存の DNS 設定をアップデートした後、HTTPS を有効化するプロセスを開始するには、カスタムドメインを削除してサイトのリポジトリに再追加しなければならない可能性があります。 詳しい情報については、「[{% data variables.product.prodname_pages %} サイト用のカスタムドメインを管理する](/articles/managing-a-custom-domain-for-your-github-pages-site)」を参照してください。

Certification Authority Authorization (CAA) レコードの使用を選択した場合、HTTPS 経由でサイトにアクセスするには、値が `letsencrypt.org` の CAA レコードが少なくとも 1 つ存在している必要があります。 詳しい情報については、Let's Encrypt ドキュメンテーションの「[Certificate Authority Authorization (CAA)](https://letsencrypt.org/docs/caa/)」を参照してください。

## Linux での URL フォーマット

サイトのURLに、先頭か最後がダッシュのユーザ名もしくは Organization 名が含まれていたり、連続するダッシュが含まれていたりすると、Linux でブラウズするユーザがそのサイトにアクセスしようとするとサーバーエラーを受け取ることになります。 これを修正するには、{% data variables.product.product_name %}のユーザ名から非英数字を取り除くよう変更してください。 詳細は「[{% data variables.product.prodname_dotcom %} ユーザ名を変更する](/articles/changing-your-github-username/)」を参照してください。

## ブラウザのキャッシュ

最近カスタムドメインを変更または削除し、ブラウザで新しい URL にアクセスできない場合は、ブラウザのキャッシュを削除してから新しい URL にアクセスすることが必要になる場合があります。 キャッシュの削除についての詳しい情報については、ブラウザのドキュメントを参照してください。
