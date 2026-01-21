# {{cookiecutter.appname}}


## 配置目录结构

脚本默认从 `configs/` 目录中选择配置，每个配置目录通常包含：

- hosts：Ansible inventory 文件
- env：环境变量文件
- extra-vars.yml：额外的 Ansible 变量文件

通过 `-c` 选项可以指定使用哪个配置目录。


## 使用


```sh
# 初始化
cd {{cookiecutter.appname}}
pip install -r requirements.txt

git init .
git add .
git commit -m "init project"
git tag -a v0.1.0 -m "initial release"

# pip install .
# 或者
python setup.py install

# 拷贝配置文件
cp -rf /usr/share/{{cookiecutter.appname}}/etc_examples/{{cookiecutter.appname}}/ /etc/{{cookiecutter.appname}}/


# 显示帮助信息
{{cookiecutter.appname}} help
# 或
{{cookiecutter.appname}} -h

# 回显命令 - 不实际运行
{{cookiecutter.appname}} -E deploy
# ansible-playbook -i /etc/kolla_k8s/default/hosts -e @/etc/kolla_k8s/default/extra-vars.yml -e _action=deploy /usr/share/kolla_k8s/ansible/site.yml

# 模拟运行配置命令，不实际更改任何内容：
{{cookiecutter.appname}} -n deploy

# 使用 -i 指定 inventory 文件：
{{cookiecutter.appname}} -E -i /etc/kolla_k8s/my_inventory.yml deploy

# 安装 Ansible 角色和依赖项：
{{cookiecutter.appname}} requirements

# 使用默认配置运行 Ansible：
{{cookiecutter.appname}} deploy


# 使用特定配置目录运行：
{{cookiecutter.appname}} -c /etc/configs/default deploy

# 增加详细程度：
{{cookiecutter.appname}} -v deploy


# 使用 Ansible ping 模块检查所有主机的连通性：
{{cookiecutter.appname}} -E ping
{{cookiecutter.appname}} ping


# 在 webservers 组上执行 df -h 命令：
{{cookiecutter.appname}} exec docker "df -h"
# 在所有主机上执行 uptime 命令：
{{cookiecutter.appname}} exec all "uptime"
#使用 Ansible 库存中的凭证 SSH 到 web01 主机：
{{cookiecutter.appname}} ssh server1
# SSH 到主机并执行命令：
{{cookiecutter.appname}} ssh server1 "ls -la /etc/ssh"


# 将所有主机的 SSH 密钥重新扫描到 known_hosts 文件：
{{cookiecutter.appname}} rescan

# 查看当前配置的环境和环境变量：
{{cookiecutter.appname}} env


# 列出所有的主机
{{cookiecutter.appname}} list

# 删除保存的配置文件：
{{cookiecutter.appname}} clean


# 运行自定义的 playbook 文件：
{{cookiecutter.appname}} -p ansible/custom-playbook.yml config


#运行时设置额外的 Ansible 变量：
{{cookiecutter.appname}} -E -e  "env=production debug=true" config


# 使用特定配置和变量运行 playbook
{{cookiecutter.appname}} -c configs/production -e "env=production" -p ansible/deploy.yml config

# 详细模式下的模拟运行
{{cookiecutter.appname}} -v -n -c configs/staging config

# 在多个主机上执行复杂命令
{{cookiecutter.appname}} exec docker "ps aux |grep sshd"
```
