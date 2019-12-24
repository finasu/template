# 設定ドキュメント https://www.virtualbox.org/manual/UserManual.html#vboxmanage
# Vagrantfileの日本語訳 https://qiita.com/78tch/items/6e95991d9783c8b330ee

Vagrant.configure("2") do |config|
  
  # BOXの指定 OSはubuntu-16.04
  config.vm.box = "bento/ubuntu-16.04"
  # config.vm.box = "hashicorp/bionic64"
  
  # 独自ドメインを設定
  # これを使う場合のコマンド $ vagrant plugin install vagrant-hostsupdater
  config.vm.hostname = "yokonoji.dev"
  
  # ホストPC-Vagrant間の通信IP
  # hostsupdater: "skip" はfrom C:/HashiCorp/Vagrant/embedded/～のエラーが出たから付けた
  config.vm.network "private_network", ip: "192.168.33.10", hostsupdater: "skip"
  
  # ホストの8080から仮想マシンの80ポートに接続 外部からのアクセスは許可しない
   config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  
  # ホストの対象ディレクトリを仮想マシンの/vagrantに共有 Virtualboxの機能を使って共有することを明示
  # $ cd /vagrant で共有したホスト側のフォルタ・ファイルが見れる 
  config.vm.synced_folder ".", "/home/vagrant/dev", type: "virtualbox"
  
  # 初回vagrant up時にDockerをインストール
  config.vm.provision :docker
  
  # Virtualboxの設定
  config.vm.provider "virtualbox" do |vb|
    
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # メモリの設定 MB
    vb.memory = "2048"

    vb.customize [
      "modifyvm", :id,
      "--vram", "256",                  # ビデオメモリ確保（フルスクリーンモードにするため）
      "--clipboard", "bidirectional",   # クリップボードの共有
      "--draganddrop", "bidirectional", # ドラッグアンドドロップ可能に
      "--cpus", "2",                    # CPUは2つ
      "--ioapic", "on"                  # 複数のCPUを指定する場合はonに
    ]
  end
end

# Guest Additionsバージョンに関するエラーが出る場合は、次のコマンドをホスト側で実行しておくと自動でアップデートをしてくれる
# $ vagrant plugin install vagrant-vbguest

# docker-composeをインストールする バージョンはここで確認 https://github.com/docker/compose/releases
# $ sudo curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
# $ sudo chmod +x /usr/local/bin/docker-compose
# $ docker-compose --version

# タイムゾーンの設定
# $ sudo timedatectl set-timezone Asia/Tokyo
# $ date

# http://192.168.33.10:8081/のようにしてDocker上で動くアプリケーションにつなぐ

# ★後日試してみる => docker-composeの自動インストール、タイムゾーン設定の自動
# 参考 https://qiita.com/kumanobori/items/edf112b3527f6cf78d46#vagrantfile%E3%81%AE%E4%BD%9C%E6%88%90
# $ config.vm.provision "shell", privileged: false, inline: <<-SHELL でシェルを動かしている
# $ vagrant plugin install vagrant-docker-compose を実行
# https://github.com/leighmcculloch/vagrant-docker-compose
# https://qiita.com/noobar/items/eedf1235c3fbd9744b76
