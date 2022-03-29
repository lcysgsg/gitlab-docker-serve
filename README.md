# Docker & Gitlab

目的是通过配置环境变量达到预配置目的

[预配置 Docker 容器](https://docs.gitlab.cn/jh/install/docker.html#%E9%A2%84%E9%85%8D%E7%BD%AE-docker-%E5%AE%B9%E5%99%A8)

[使用-docker-compose-安装极狐gitlab](https://docs.gitlab.cn/jh/install/docker.html#%E4%BD%BF%E7%94%A8-docker-compose-%E5%AE%89%E8%A3%85%E6%9E%81%E7%8B%90gitlab)

> 在设置其他所有内容之前，请配置一个新的环境变量 $GITLAB_HOME，指向配置、日志和数据文件所在的目录。 确保该目录存在并且已授予适当的权限。

> 对于 Linux 用户，将路径设置为 /srv/gitlab：

> `export GITLAB_HOME=/srv/gitlab`

## 设置中文

1. 选择settings

2. 选择Preferences

3. 往下拉，选择Localization---选择中文----保存即可

## Gitlab Runner

### 注册 runner

1. 进入 runner 容器
    - `$ docker exec -it 容器ID bash`

1. 注册 runner
    - `$ gitlab-runner register`

1. 输入 gitlab 示例的 url
    - Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com ):``
    - `$ http://192.168.1.10:8000/`

1. 输入用来注册 runner 的 token
    - Please enter the gitlab-ci token for this runner:
    - [项目指定Runner令牌/admin共享Runner令牌]

1. 输入 runner 的描述，随后可在 gitlab 界面中修改
    - Please enter the gitlab-ci description for this runner:

1. 输入与 runner 绑定的标签（可修改）
    - Please enter the gitlab-ci tags for this runner (comma separated):

1. 选择 runner 的执行方式（推荐docker）
    - Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
    - `$ docker`

1. 如果选择的执行方式是 docker，会要求填写默认的镜像
    - Please enter the Docker image (eg. ruby:2.1):
    - `$ alpine:latest`

注册成功后会在 runner 容器 ~/etc/gitlab-runner/ 目录下生成 config.toml 配置文件，这时候就可以在 gitlab 的管理页面中看到激活的 runner

共享runner需要使用 admin 的 token（管理中心 -> 概览 -> Runner）。

## 邮件服务

1. `$ gitlab-rails console`

2. `Notify.test_email('EMAIL', 'Message Subject', 'Message Body').deliver_now`

```
    gitlab_rails['smtp_enable'] = true
    gitlab_rails['smtp_address'] = "smtp.qq.com"
    gitlab_rails['smtp_domain'] = "smtp.qq.com"
    gitlab_rails['smtp_port'] = 465
    gitlab_rails['smtp_user_name'] = "My Email"
    gitlab_rails['smtp_password'] = "SMTP 授权码"
    gitlab_rails['smtp_authentication'] = "login"
    gitlab_rails['smtp_enable_starttls_auto'] = true
    gitlab_rails['smtp_tls'] = true
    gitlab_rails['gitlab_email_from'] = "My Email"
```
