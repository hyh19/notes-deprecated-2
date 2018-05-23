# Shell 安装脚本模板

```bash
#!/usr/bin/env bash

# 工作目录
WORKING_DIR=/tmp

# 软件名称
SOFTWARE_NAME=git

# 软件版本
SOFTWARE_VERSION=2.15.0

# 压缩包名称
ARCHIVE_FILE_NAME="${SOFTWARE_NAME}-${SOFTWARE_VERSION}.tar.gz"

# 压缩包下载地址
ARCHIVE_FILE_DOWNLOAD_URL="https://www.kernel.org/pub/software/scm/git/${ARCHIVE_FILE_NAME}"

# 压缩包解压后的目录名称
UNARCHIVE_DIR_NAME="${SOFTWARE_NAME}-${SOFTWARE_VERSION}"

# 压缩包下载后的完整路径
ARCHIVE_FILE_SAVE_PATH="${WORKING_DIR}/${ARCHIVE_FILE_NAME}"

# 压缩包解压后的完整路径
UNARCHIVE_DIR_PATH="${WORKING_DIR}/${UNARCHIVE_DIR_NAME}"

# 软件各个版本的安装根目录
INSTALL_DIR_ROOT_PATH=/usr/local/${SOFTWARE_NAME}

# 软件当前版本的安装目录
INSTALL_DIR_CURRENT_PATH="${INSTALL_DIR_ROOT_PATH}/${SOFTWARE_NAME}-${SOFTWARE_VERSION}"

# 软件当前版本的符号链接
SYM_LINK="${INSTALL_DIR_ROOT_PATH}/current"

# PATH 配置文件
PATH_PROFILE="/etc/profile.d/${SOFTWARE_NAME}.sh"

# 判断 Linux 发行版本的脚本
CHECK_SYS_SCRIPT_NAME="check_sys.sh"
CHECK_SYS_SCRIPT_DOWNLOAD_URL="https://github.com/mrhuangyuhui/shell/raw/snippets/${CHECK_SYS_SCRIPT_NAME}"
CHECK_SYS_SCRIPT_SAVE_PATH="${WORKING_DIR}/${CHECK_SYS_SCRIPT_NAME}"

# 使用 YUM 安装依赖
function install_dependencies_with_yum() {
    yum install epel-release -y
    yum install autoconf automake libtool -y
    yum install dh-autoreconf libcurl-devel expat-devel gettext-devel openssl-devel perl-devel zlib-devel -y
    yum install asciidoc xmlto docbook2X -y
    ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
}

# 使用 APT 安装依赖
function install_dependencies_with_apt() {
    apt-get update
    apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev  gettext zlib1g-dev libssl-dev -y
    apt-get install asciidoc xmlto docbook2x -y
    ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
}

# 编译和安装
function make_and_install() {
    # 创建安装目录
    mkdir -p $INSTALL_DIR_CURRENT_PATH
    # 进入解压目录
    cd $UNARCHIVE_DIR_PATH

    make configure
    ./configure --prefix=$INSTALL_DIR_CURRENT_PATH
    make all doc info
    make install install-doc install-html install-info
}

# PATH 配置
function config_binary_path() {
    echo "export PATH=\${PATH}:${SYM_LINK}/bin" > $PATH_PROFILE
}

# 进入工作目录
cd $WORKING_DIR

# 下载判断 Linux 发行版本的脚本
rm -f $CHECK_SYS_SCRIPT_SAVE_PATH
wget -O $CHECK_SYS_SCRIPT_SAVE_PATH $CHECK_SYS_SCRIPT_DOWNLOAD_URL

if [ -e "$CHECK_SYS_SCRIPT_SAVE_PATH" ]; then
    . $CHECK_SYS_SCRIPT_SAVE_PATH
else
    echo "[ERROR] Download ${CHECK_SYS_SCRIPT_NAME} failed."
    exit 1
fi

# 安装依赖
if check_sys "packageManager" "yum"; then
    install_dependencies_with_yum
elif check_sys "packageManager" "apt"; then
    install_dependencies_with_apt
else
    echo "[ERROR] Not supported distro."
    exit 1
fi

# 下载压缩包
if [ ! -e "$ARCHIVE_FILE_SAVE_PATH" ]; then
    wget -O $ARCHIVE_FILE_SAVE_PATH $ARCHIVE_FILE_DOWNLOAD_URL
fi

# 下载失败，不再继续。
if [ ! -e "$ARCHIVE_FILE_SAVE_PATH" ]; then
    echo "[ERROR] Download ${ARCHIVE_FILE_NAME} failed."
    exit 1
fi

# 备份旧的解压目录
if [ -d "$UNARCHIVE_DIR_PATH" ]; then
    mv $UNARCHIVE_DIR_PATH "${UNARCHIVE_DIR_PATH}-$(date +%Y%m%d%H%M%S)"
fi

# 备份旧的安装目录
if [ -d "$INSTALL_DIR_CURRENT_PATH" ]; then
    mv $INSTALL_DIR_CURRENT_PATH "${INSTALL_DIR_CURRENT_PATH}-$(date +%Y%m%d%H%M%S)"
fi

# 解压压缩包
tar zxvf $ARCHIVE_FILE_SAVE_PATH

# 开始编译和安装
make_and_install

# 创建符号链接
if [ -L "$SYM_LINK" ]; then
    rm -f $SYM_LINK
fi

ln -s $INSTALL_DIR_CURRENT_PATH $SYM_LINK

# PATH 配置
config_binary_path

echo "################################################################################"
echo "# Open a new terminal or enter: source ${PATH_PROFILE}"
echo "################################################################################"
```