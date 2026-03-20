# {{cookiecutter.appname}}

Ansible 部署工具，类似 kolla-ansible，用于快速部署容器化应用。

## 安装

### 方式1：从 PyPI 安装（待发布）

```sh
pip install {{cookiecutter.appname}}
```

### 方式2：从源码安装

```sh
# 克隆仓库
git clone https://github.com/your-repo/{{cookiecutter.appname}}.git
cd {{cookiecutter.appname}}

# 安装
pip install .

# 或者构建 wheel 后安装
# 构建 wheel
pip install wheel check-wheel-contents
python setup.py sdist bdist_wheel
# 构建完成后，安装 wheel
pip install dist/{{cookiecutter.appname}}-*.whl
```

### 方式3：直接运行（无需安装）

```sh
cd {{cookiecutter.appname}}
./tools/{{cookiecutter.appname}}/{{cookiecutter.appname}} deploy
```

## 配置

### 方式1：从源码运行时的配置

```sh
# 从源码目录直接运行
cd {{cookiecutter.appname}}
./tools/{{cookiecutter.appname}}/{{cookiecutter.appname}} -i /path/to/inventory deploy
```

### 方式2：全局安装后的配置

安装后需要复制配置文件：

```sh
# 复制 Ansible playbook 和配置文件到全局目录
sudo mkdir -p /usr/share/{{cookiecutter.appname}}
sudo cp -r ansible /usr/share/{{cookiecutter.appname}}/
sudo cp -r etc/{{cookiecutter.appname}} /etc/

# 或者使用环境变量指定目录
export {{cookiecutter.appname}}_DIR=/path/to/{{cookiecutter.appname}}
export {{cookiecutter.appname}}_CONFIG_DIR=/path/to/config
```

## 使用

```sh
# 显示帮助
{{cookiecutter.appname}} --help

# 查看版本
{{cookiecutter.appname}} --version

# 回显命令（不实际运行）
{{cookiecutter.appname}} -E deploy

# 模拟运行
{{cookiecutter.appname}} -n deploy

# 使用自定义 inventory
{{cookiecutter.appname}} -i /path/to/inventory deploy

# 限制主机
{{cookiecutter.appname}} -l compute deploy
{{cookiecutter.appname}} --limit control,compute prechecks

# 安装 Ansible 角色依赖
{{cookiecutter.appname}} requirements

# 部署操作
{{cookiecutter.appname}} deploy              # 部署
{{cookiecutter.appname}} config              # 配置
{{cookiecutter.appname}} remove              # 移除
{{cookiecutter.appname}} bootstrap-servers    # 初始化服务器
{{cookiecutter.appname}} prechecks           # 部署前检查
{{cookiecutter.appname}} postchecks          # 部署后检查
{{cookiecutter.appname}} pull                # 拉取镜像
{{cookiecutter.appname}} stop                # 停止服务
{{cookiecutter.appname}} reconfigure         # 重新配置
{{cookiecutter.appname}} restart             # 重启服务

# 增加详细程度
{{cookiecutter.appname}} -v deploy

# 设置额外的 Ansible 变量
{{cookiecutter.appname}} -e "env=production" -E deploy
```

## 组件操作

```sh
# chrony 操作
{{cookiecutter.appname}} -t chrony deploy    # 部署 chrony
{{cookiecutter.appname}} -t chrony config    # 重新配置 chrony
{{cookiecutter.appname}} -t chrony remove    # 移除 chrony

# prechecks 操作
{{cookiecutter.appname}} prechecks           # 运行所有预检查
```

## 运维命令

```sh
# 检查主机连通性
{{cookiecutter.appname}} ping
{{cookiecutter.appname}} ping docker

# 在主机组上执行命令
{{cookiecutter.appname}} exec all "uptime"
{{cookiecutter.appname}} exec docker "df -h"

# SSH 到主机
{{cookiecutter.appname}} ssh server1
# SSG 到主机执行命令
{{cookiecutter.appname}} ssh server1 "uptime"

# 列出所有主机
{{cookiecutter.appname}} list

# 查看环境变量
{{cookiecutter.appname}} env
```

## 环境变量

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `{{cookiecutter.appname}}_DIR` | 安装目录 | `/usr/share/{{cookiecutter.appname}}` |
| `{{cookiecutter.appname}}_CONFIG_DIR` | 配置目录 | `/etc/{{cookiecutter.appname}}` |

## 目录结构

```sh
/usr/share/{{cookiecutter.appname}}/          # 安装目录（需手动创建）
├── ansible/                   # Ansible playbooks 和 roles
│   ├── site.yml              # 主 playbook
│   ├── gather-facts.yml      # 收集主机信息
│   └── roles/                # Ansible roles
│       ├── chrony/           # 时间同步
│       ├── prechecks/        # 部署前检查
│       └── common/           # 通用角色
└── etc_examples/             # 配置示例
    └── {{cookiecutter.appname}}/            # 配置目录模板
        ├── hosts             # inventory 文件
        ├── env               # 环境变量
        └── extra-vars.yml    # 额外变量
```

## 示例

```sh
# 详细模式下的模拟部署
{{cookiecutter.appname}} -v -n deploy

# 生产环境部署
{{cookiecutter.appname}} -e "env=production" -v deploy

# 详细输出检查
{{cookiecutter.appname}} -v prechecks
```
