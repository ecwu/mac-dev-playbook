<img src="https://raw.githubusercontent.com/geerlingguy/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Development Ansible Playbook

[![CI][badge-gh-actions]][link-gh-actions]

这个 playbook 会安装和配置我在 Mac 上使用的大部分 web 和软件开发所需的软件。macOS 中的一些东西有点难以自动化，所以还有一些手动安装步骤，但至少所有内容都记录在这里了。

这个文档源自 [https://github.com/geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)。为了我自己的实际需求和避免使用时阅读理解错误可能带来的问题，我选择将源文档进行筛选翻译并在未来以此为基础更新。

## 安装

  1. 确保Apple的命令行工具已安装(运行 `xcode-select --install` 来启动安装)。
  2. [安装 Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

     1. 运行以下命令将 Python 3 添加到 `$PATH` 环境变量: `export PATH="$HOME/Library/Python/3.9/bin:/opt/homebrew/bin:$PATH"`
     2. 更新 Pip: `sudo pip3 install --upgrade pip`
     3. 安装 Ansible: `pip3 install ansible`

  3. 将此仓库克隆或下载到本地。
  4. 在此目录内运行 `ansible-galaxy install -r requirements.yml` 来安装所需的 Ansible 角色。
  5. 在此目录内运行 `ansible-playbook main.yml --ask-become-pass`，当提示成为 root 用户密码时输入您的 Mac 账号密码。

> 注意：如果某些 Homebrew 命令失败,可能需要同意 Xcode 的授权协议或者解决其他 Brew 问题。运行 `brew doctor` 查看是否是这种情况。

### 运行指定任务集

您可以使用 `ansible-playbook` 的 `--tags` 参数过滤运行配置过程中的某部分任务。可用的标签有 `dotfiles`、`homebrew`、`mas`、`extra-packages` 和 `osx`。

```bash
ansible-playbook main.yml -K --tags "dotfiles,homebrew"
```

## 覆盖默认值

每个人的开发环境和软件配置偏好不同。

您可以通过创建 `config.yml` 文件，在其中设置 `default.config.yml` 中默认配置的覆盖值。比如自定义安装的包和应用:

```yaml
homebrew_installed_packages:
  - cowsay
  - git
  - go

mas_installed_apps:
  - { id: 443987910, name: "1Password" }
  - { id: 498486288, name: "Quick Resizer" }
  - { id: 557168941, name: "Tweetbot" }
  - { id: 497799835, name: "Xcode" }

composer_packages:
  - name: hirak/prestissimo
  - name: drush/drush
    version: '^8.1'

gem_packages:
  - name: bundler
    state: latest

npm_packages:
  - name: webpack

pip_packages:
  - name: mkdocs

configure_dock: true
dockitems_remove:
  - Launchpad
  - TV
dockitems_persist:
  - name: "Sublime Text"
    path: "/Applications/Sublime Text.app/"
    pos: 5
```

任何变量都可以在 `config.yml` 中被覆盖；有关每个支持角色可用变量的完整列表，请参考其文档。

## 包含的应用 / 配置（默认）

应用 (用Homebrew Cask安装):

  - [ChromeDriver](https://sites.google.com/chromium.org/driver/)
  - [Docker](https://www.docker.com/)
  - [Dropbox](https://www.dropbox.com/)
  - [Firefox](https://www.mozilla.org/en-US/firefox/new/)
  - [Google Chrome](https://www.google.com/chrome/)
  - [Handbrake](https://handbrake.fr/)
  - [Homebrew](http://brew.sh/)
  - [LICEcap](http://www.cockos.com/licecap/)
  - [nvALT](http://brettterpstra.com/projects/nvalt/)
  - [Sequel Ace](https://sequel-ace.com) (MySQL client)
  - [Slack](https://slack.com/)
  - [Sublime Text](https://www.sublimetext.com/)
  - [Transmit](https://panic.com/transmit/) (S/FTP client)

包(用Homebrew安装):

  - autoconf
  - bash-completion
  - doxygen
  - gettext
  - gifsicle
  - git
  - gh
  - go
  - gpg
  - httpie
  - iperf
  - libevent
  - sqlite
  - mcrypt
  - nmap
  - node
  - nvm
  - php
  - ssh-copy-id
  - cowsay
  - readline
  - openssl
  - pv
  - wget
  - wrk
  - zsh-history-substring-search

geerlingguy 的 [dotfiles](https://github.com/geerlingguy/dotfiles) 也会安装到当前用户的主目录,包括用于优化 macOS 性能和易用性的 `.osx` dotfile。通过设置 `configure_dotfiles: no` 可以禁用 `dotfiles` 管理。

此外，还为各种应用和服务添加了一些其他首选项和设置。

## 完全 / 从头设置指南

由于我使用这个 playbook 设置过大约 20 台不同的 Mac,所以写了一个 100% 从头开始的详细安装指南，便于自己参考(每个人的细节安装可能会有点不同)。
可以在这里查看完全从头设置文档: [full-mac-setup.md](full-mac-setup.md).

## 作者

这个项目是由 [Jeff Geerling](https://www.jeffgeerling.com/) 创建（原始灵感来源于 [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)）.

[badge-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/workflows/CI/badge.svg?event=push
[link-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/actions?query=workflow%3ACI
