# 电脑安装指南

## 如何判断当前shell 是bash 还是zsh
```shell
echo $SHELL
```
```shell
logan@B-WCV2MD6M-2006 4_script % echo $SHELL
/bin/zsh
```

## 文件目录配置

以下为基础文件安装脚本，取名install.sh，需要注意的是，不要使用root用户或者其他用户来执行这个脚本，就以当前用户来执行，否则会出现文件权限访问问题！

```shell
#!/bin/bash

# 创建box文件夹，所有编程相关的内容都放在这个文件夹下
mkdir ~/box

# 创建分类文件夹
# 存放代码，包括前后端、脚本等
mkdir ~/box/1_code
# 存放文档，用vscode编辑，包括博客的内容等
mkdir ~/box/2_doc
# 存放一些编程相关的文件，比如架构图ppt，xmind文件等
mkdir ~/box/3_file
# 存放一些必要的基础配置，包括maven配置等
mkdir ~/box/4_config
# 存放临时内容
mkdir ~/box/5_tmp

# 根据代码分类，创建对于的文件目录
mkdir ~/box/1_code/1_frontend
mkdir ~/box/1_code/2_backend
mkdir ~/box/1_code/3_client
mkdir ~/box/1_code/4_script

# 针对文件目录，创建一个不经常访问的角落，存放maven内容
mkdir ~/box/3_file/quiet_corner
mkdir ~/box/3_file/quiet_corner/maven


# 针对设置，创建对于的目录
mkdir ~/box/4_config/maven_setting
mkdir ~/box/4_config/idea_setting


# 针对下载内容，增加对于目录
mkdir ~/Downloads/chrome
```

## 软件安装配置
homebrew官网：https://brew.sh/


### homebrew安装脚本
```shell
# homebrew安装脚本
install_homebrew() {
    # 下载 Homebrew 安装脚本
    curl -v https://raw.githubusercontent.com/Homebrew/install/master/install.sh > install-brew.sh

    # 修改 BREW_REPO 为国内镜像站
    sed -i '' 's|https://github.com/Homebrew/brew|https://mirrors.ustc.edu.cn/brew.git|g' install-brew.sh

    # 设置 HOMEBREW_CORE_GIT_REMOTE 环境变量为国内镜像站
    export HOMEBREW_CORE_GIT_REMOTE=https://mirrors.ustc.edu.cn/homebrew-core.git

    # 运行安装脚本
    bash install-brew.sh

    # 删除安装脚本
    rm install-brew.sh
}

# 检查homebrew是否已经安装，如果未安装，则执行安装脚本
if ! command -v brew &> /dev/null; then
    echo "Homebrew not found. Installing..."
    install_homebrew
    echo "Homebrew is installed."
else
    echo "Homebrew is already installed."
fi

# 替换brew 源为中科大的数据源
# 替换 brew.git
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 替换 homebrew-core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# 使用brew安装基础的终端能力
check_and_install_dependencies() {
    dependency=$1
    if ! command -v $dependency &> /dev/null; then
        echo "$dependency is not installed, installing..."
        brew install $dependency
    else
        echo "$dependency is already installed"
    fi
}

check_and_install_dependencies wget
check_and_install_dependencies dpkg
```

### JDK安装脚本
```shell
#!/bin/bash

# 卸载JDK
brew uninstall openjdk@8

# 安装 JDK
brew install openjdk@8

# 获取安装地址，用户配置环境变量
jdk_directory=$(ls -d /usr/local/Cellar/openjdk@8/*)

# 配置 zsh 环境变量（如果使用的是 zsh）
echo "export JAVA_HOME=$jdk_directory" >> ~/.zshrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.zshrc

# 生效环境变量
source ~/.zshrc

# 验证安装
java -version
```

### MAVEN安装脚本
```shell
# 指定安装目录
install_dir="/Users/logan/box/3_file/quiet_corner/maven"

# 定义 Maven 版本号
maven_version="3.6.3"

# 下载 Maven 安装包
wget https://logan-download.oss-cn-hangzhou.aliyuncs.com/apache-maven-$maven_version-bin.tar.gz

# 解压安装包
tar xzf apache-maven-$maven_version-bin.tar.gz

# 将 Maven 移动到指定目录
mv apache-maven-$maven_version $install_dir

# 设置环境变量
echo "export PATH=\$PATH:$install_dir/apache-maven-$maven_version/bin" >> ~/.zshrc
source ~/.zshrc
```
### VSCODE下载脚本
```shell
#!/bin/bash

# 下载链接
echo "begin to install vscode"

extract_dir="/Users/logan/box/3_file/quiet_corner/vscode"
wget_url="https://vscode.cdn.azure.cn/stable/6445d93c81ebe42c4cbd7a60712e0b17d9463e97/VSCode-darwin-universal.zip"
file_name="VSCode-darwin-universal.zip"

check_and_create_folder() {
    folder=$1
    if [ ! -d "$folder" ]; then
        mkdir "$folder"
        echo "文件夹「$folder」已创建"
    else
        echo "文件夹「$folder」已存在"
    fi
}
check_and_downlaod_file() {
    file_path=$1
    url=$2
    if [ -f "$file_path" ]; then
        echo "文件「$file_path」存在"
        # 执行您的动作
    else
        echo "文件「$file_path」不存在"
        # 执行其他动作
        # 下载 VS Code
        wget $url

        # 解压文件
        unzip VSCode-darwin-universal.zip
    fi
}
check_and_downlaod_file $file_name $wget_url

# 移动文件到指定目录
check_and_create_folder $extract_dir
mv $file_name $extract_dir

# 输出完成信息
echo "$file_name 已下载并解压到目录：$extract_dir"
```

## 电脑软件配置
- [ ] Paste


## 快捷键设置
[唤醒Paste]：cmd+J


### 软件特定快捷键配置


**VSCODE**
  [查看: 切换 终端]: 「cmd+D」




