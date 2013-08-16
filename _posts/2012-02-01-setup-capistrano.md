---
layout: post
title: Capistranoをやってみよう
tags: Capistrano
categories: CI
---
{page.title}
-----------------

<a href="https://github.com/capistrano/capistrano"><img title="capistrano/capistrano - GitHub" src="http://capture.heartrails.com/300x300/cool?https://github.com/capistrano/capistrano" alt="https://github.com/capistrano/capistrano" width="300" height="300" /></a>

Capistranoをやってみました。
[こちら](http://builder.japan.zdnet.com/virtualization/sp_open-source-software-moonlinx-2009/20396188/)と[こちら](http://doruby.kbmj.com/trinityt_on_rails/20080325/__Capistrano___1)を参考にしました。

##Capistranoって？
Capistranoは複数サーバにアプリケーションを配備できるデプロイツールなのですが、厳密にいえば「複数のサーバ上で同時に並行してコマンドを実行できるツール」だそうです。

##インストール
{% highlight sh %}
> gem install -y capistrano
INFO:  `gem install -y` is now default and will be removed
INFO:  use --ignore-dependencies to install only the gems you list
Fetching: highline-1.6.11.gem (100%)
Fetching: net-ssh-2.3.0.gem (100%)
Fetching: net-sftp-2.0.5.gem (100%)
Fetching: net-scp-1.0.4.gem (100%)
Fetching: net-ssh-gateway-1.1.0.gem (100%)
Fetching: capistrano-2.9.0.gem (100%)
Successfully installed highline-1.6.11
Successfully installed net-ssh-2.3.0
Successfully installed net-sftp-2.0.5
Successfully installed net-scp-1.0.4
Successfully installed net-ssh-gateway-1.1.0
Successfully installed capistrano-2.9.0
6 gems installed
Installing ri documentation for highline-1.6.11...
Installing ri documentation for net-ssh-2.3.0...
Installing ri documentation for net-sftp-2.0.5...
Installing ri documentation for net-scp-1.0.4...
Installing ri documentation for net-ssh-gateway-1.1.0...
Installing ri documentation for capistrano-2.9.0...
Installing RDoc documentation for highline-1.6.11...
Installing RDoc documentation for net-ssh-2.3.0...
Installing RDoc documentation for net-sftp-2.0.5...
Installing RDoc documentation for net-scp-1.0.4...
Installing RDoc documentation for net-ssh-gateway-1.1.0...
Installing RDoc documentation for capistrano-2.9.0...
{% endhighlight %}
これでインストール完了。gemって便利だ

{% highlight sh %}
>cap -h
Usage: cap [options] action ...
    -d, --debug                      Prompts before each remote command execution.
    -e, --explain TASK               Displays help (if available) for the task.
    -F, --default-config             Always use default config, even with -f.
    -f, --file FILE                  A recipe file to load. May be given more than once.
    -H, --long-help                  Explain these options and environment variables.
    -h, --help                       Display this help message.
    -l [STDERR|STDOUT|file]          Choose logger method. STDERR used by default.
        --logger
    -n, --dry-run                    Prints out commands without running them.
    -p, --password                   Immediately prompt for the password.
    -q, --quiet                      Make the output as quiet as possible.
    -r, --preserve-roles             Preserve task roles
    -S, --set-before NAME=VALUE      Set a variable before the recipes are loaded.
    -s, --set NAME=VALUE             Set a variable after the recipes are loaded.
    -T, --tasks [PATTERN]            List all tasks (matching optional PATTERN) in the loaded recipe files.
    -t, --tool                       Abbreviates the output of -T for tool integration.
    -V, --version                    Display the Capistrano version, and exit.
    -v, --verbose                    Be more verbose. May be given more than once.
    -X, --skip-system-config         Don't load the system config file (capistrano.conf)
    -x, --skip-user-config           Don't load the user config file (.caprc)
{% endhighlight %}
インストールされてることを確認。


##capifyの実行
アプリケーションのカレントディレクトリに移動して、capifyコマンドを実行
{% highlight sh %}
> capify . #.を忘れないこと
[add] writing './Capfile'
[add] making directory './config'
[add] writing './config/deploy.rb'
[done] capified!
{% endhighlight %}
カレントディレクトリ直下に、Capfileファイルとconfigディレクトリ
config配下に、deploy.rbが作成される。

##deploy.rbの編集
/cofig/deploy.rbを編集してみる。
とりあえずHelloWorldするために、作成されたdeploy.rbの内容を以下のように修正。
{% highlight sh %}
set :application, "hoge"
set :repository,  "huga"

set :scm, :subversion

role :test, "127.0.0.1"                          # Your HTTP server, Apache/etc
role :app, "127.0.0.1"                          # This may be the same as your `Web` server

namespace :deploy do
task :hw, :roles => [:test, :ap] do
  run "echo HelloWorld! $HOSTNAME"
end
end
{% endhighlight %}


##デプロイの実行
コマンドを実行してデプロイ
{% highlight sh %}
> cap deploy:hw
  * executing `deploy:hw'
  * executing "echo HelloWorld! $HOSTNAME"
    servers: ["127.0.0.1"]
  connection failed for: 127.0.0.1 (Errno::ECONNREFUSED: 対象のコンピューターによって拒否されたため、接続できませんでした。 - connect(2))
{% endhighlight %}

タスクが実行される。
ローカルにSSH接続できなかったので、タスクに失敗したことを確認。

※Railsのプロジェクトを用意してなかったので、これは想定通りの動きなのでとりあえず今はOK

想定の動作としては、↓のような感じ
{% highlight sh %}
> cap deploy:hw
  * executing `deploy:hw'
  * executing "echo HelloWorld! $HOSTNAME"
  servers: ["127.0.0.1","127.0.0.1"]
  ["127.0.0.1"] executing command
  **[out :: 127.0.0.1]HelloWorld! test
  **[out :: 127.0.0.1]HelloWorld! app
{% endhighlight %}

##最後に
なんとなく動かして、Capistranoからコマンドを実行する手前までは確認。
あとは、デプロイ対象のサーバとSSH接続して、どうコマンドを実行させるかって感じでしょうか。
もっといろいろ触ってみてよう