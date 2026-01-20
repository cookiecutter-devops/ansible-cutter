# kolla-test


## 配置目录结构

脚本默认从 `configs/` 目录中选择配置，每个配置目录通常包含：

- hosts：Ansible inventory 文件
- env：环境变量文件
- extra-vars.yml：额外的 Ansible 变量文件

通过 `-c` 选项可以指定使用哪个配置目录。


## 使用


```sh
# 初始化
cd kolla-test
mkdir -p /usr/share/kolla-test && cp -rf ./* /usr/share/kolla-test
cp -rf /usr/share/kolla-test/kolla-test /usr/bin/
cp -rf /usr/share/kolla-test/configs /etc/asb_config
cp -rf /usr/share/kolla-test/ansible.cfg /etc/ansible/

# 显示帮助信息
kolla-test help
# 或
kolla-test -h

# 回显命令 - 不实际运行
kolla-test -E deploy
# ansible-playbook -i configs/default/hosts -e @configs/default/extra-vars.yml -e _action=deploy ansible/site.yml

# 模拟运行配置命令，不实际更改任何内容：
kolla-test -n deploy

# 安装 Ansible 角色和依赖项：
kolla-test requirements

# 使用默认配置运行 Ansible：
kolla-test deploy


# 使用特定配置目录运行：
kolla-test -c /etc/configs/default deploy

# 增加详细程度：
kolla-test -v deploy


# 使用 Ansible ping 模块检查所有主机的连通性：
kolla-test -E ping
kolla-test ping


# 在 webservers 组上执行 df -h 命令：
kolla-test exec docker "df -h"
# 在所有主机上执行 uptime 命令：
kolla-test exec all "uptime"
#使用 Ansible 库存中的凭证 SSH 到 web01 主机：
kolla-test ssh server1


# SSH 到主机并执行命令：
kolla-test ssh web01 "ls -la /var/www"

# 将所有主机的 SSH 密钥重新扫描到 known_hosts 文件：
kolla-test rescan

# 查看当前配置的环境和环境变量：
kolla-test env



# 删除保存的配置文件：
kolla-test clean


# 运行自定义的 playbook 文件：
kolla-test -p ansible/custom-playbook.yml config


#运行时设置额外的 Ansible 变量：
kolla-test -E -e  "env=production debug=true" config


# 使用特定配置和变量运行 playbook
kolla-test -c configs/production -e "env=production" -p ansible/deploy.yml config

# 详细模式下的模拟运行
kolla-test -v -n -c configs/staging config

# 在多个主机上执行复杂命令
kolla-test exec docker "sudo systemctl restart httpd && sudo systemctl status httpd"
```
