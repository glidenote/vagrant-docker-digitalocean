# vagrant-docker-digitalocean

![](http://blog.glidenote.com/images/2013/12/vagrant-docker-digital-ocean.png)

## これは何

 * [Vagrant](http://www.vagrantup.com/)を利用して、[DigitalOcean](https://www.digitalocean.com/?refcode=510a665458a3)にCentOS 6.5+Dockerな環境をコマンド一発(`vagrant up --provider=digital_ocean --provision`)で構築します。
 * CentOS 6.5より[Docker](https://www.docker.io/)がサポートされていますが、2014年02月18日現在DigitalOceanで提供されているCentOS 6.5のimageには`rsync(1)`が入っていないため、インスタンス作成時にprovisionを走らせる事が出来ません。これを利用することで簡単にCentOS6.5+Dockerが利用可能になります。
 * 安く、SSDで高速な開発環境を簡単に構築できます。

## 事前準備

Macでの利用を想定した手順になっています。

### Digital Oceanに申し込んでAPIの`client_id`と`api_key`を取得する

 1. [Digital Ocean](https://www.digitalocean.com/?refcode=510a665458a3)に申し込む
 1. [https://cloud.digitalocean.com/ssh_keys](https://cloud.digitalocean.com/ssh_keys)で作成したインスタンスに接続するための`ssh key`を登録
 1. [https://cloud.digitalocean.com/api_access](https://cloud.digitalocean.com/api_access)から`client_id`と`api_key`を取得しておく

[ここ経由](https://www.digitalocean.com/?refcode=510a665458a3)で申し込んで頂けると私の懐が潤います!

### Vagrantをインストール

[Vagrant](http://www.vagrantup.com/)からファイルをダウンロードしてインストール。2014/02/18現在最新の`1.4.3`を利用してます。

###  [vagrant-digitalocean](https://github.com/smdahlen/vagrant-digitalocean)のインストール

vagrantコマンドでvagrant-digitaloceanをインストール。vagrant-digitaloceanは2014/02/18現在最新の`0.5.3`を利用

``` sh
vagrant plugin install vagrant-digitalocean
```

### curl-ca-bundleのインストール

apiを叩くのに必要なので、brewで導入

``` sh
brew install curl-ca-bundle
```

## 使い方

1. このリポジトリをclone

    ``` sh
    git clone https://github.com/glidenote/vagrant-docker-digitalocean.git
    ```

1. `Vagrantfile`を下記部分を自分の環境に合わせて修正。
    
    * `config.vm.hostname`には付けたいホスト名を設定
    * `provider.client_id`には準備で作成した`client_id`を設定
    * `provider.api_key`には準備で作成した`api_key`を設定
    * `provider.ssh_key_name`には準備で作成したssh鍵名を指定
    * regionは日本からのレスポンスが良い`Singapore 1`、sizeはDockerを利用するのに快適な`1GB`に設定しています。2014/02/18現在size`1GB`のインスタンス(Droplet)は1時間2円弱(0.015ドル)です。売り切れの場合などは必要に応じて適時変更してください

1. `Vagrant up`でインスタンス(Droplet)の作成

    ``` sh
    vagrant up --provider=digital_ocean --provision
    ```

1. `Vagrant ssh`でインスタンス(Droplet)にログイン

    ``` sh
    vagrant ssh
    ```

1. Dockerを動かす

    ``` sh
    sudo docker run -i -t mattdm/fedora /bin/bash
    ```

1. 不要になったらインスタンス(Droplet)を削除。(停止状態だと`halt`だと課金がかかるのでご注意ください。)

    ``` sh
    vagrant destroy
    ```

