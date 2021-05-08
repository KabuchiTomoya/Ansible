### Ansibleを使ってリモートサーバー（GoogleCloudEngine)に立ち上げたDockerコンテナの構成情報取得を行いました。
手順
--- 
sshのポートフォワーディングでローカルPCのportへのアクセスをGCE上のDocker-Deamonに自動転送します。<br>
```sudo ssh -i ~/.ssh/id_rsa -fNL localhost:22222:/var/run/docker.sock -4 vagrant@35.232.200.77``` <br>

```docker -H tcp://localhost:22222 ps``` (ローカルPCでGCE上の docker ps と同じ情報が出力されるのを確認します。）<br>

inventory.iniでansible_docker_extra_argsにポートフォワーディングしたポートを指定することで、AnsibleのDocker_Connection_PluginがローカルPCのdocker-deamonを使ってリモートのコンテナに docker exec を用いた操作を行います。<br>

```container ansible_connection=docker ansible_python_interpreter=/usr/bin/python3 ansible_docker_extra_args="-H=tcp://localhost:22222"```(コンテナにはpythonのインストールが必要です)

Ansibleはhostごとに、ファイル上で環境変数が定義できるので異なるホスト上のコンテナをローカルの環境変数を直接いじることなく操作できます。

参考
---
mitogen_plugin<br>
https://qiita.com/ya-man/items/ba521d014e180a484ccd <br>

docker_connction_plugin<br>
https://github.com/ansible/ansible/pull/11650

docker_deamon <br>
http://www.yasunaga-lab.bio.kyutech.ac.jp/EosJ/index.php/%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%AE%E4%B8%AD%E3%81%AEDocker%E3%81%AB%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E3%81%8B%E3%82%89%E6%8E%A5%E7%B6%9A%E3%81%99%E3%82%8B

追加
---
zabbixをローカルPCにてdockerで立ち上げて、リモートとローカルのdockerコンテナとリソースの情報の取得が出来ました。

以下を参考に行いました。<br>
https://qiita.com/ko-he-8/items/c88c00dbc4e4f8b868b8
