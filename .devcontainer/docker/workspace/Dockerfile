# ベースイメージとして Ubuntu 22.04 を使用
FROM ubuntu:22.04

# ビルド時の引数を定義。デフォルトのユーザー情報やパッケージリストも設定
ARG USERNAME=vscode
ARG USER_GROUP_NAME=workspace
ARG USER_UID=1000
ARG USER_GID=1000
ARG PKG="git vim curl unzip zip sudo"

# 以降の RUN コマンドで bash を使用
SHELL ["/bin/bash", "-c"]

# パッケージの更新と必要なパッケージのインストール、ユーザーとグループの作成、sudo 設定
RUN apt-get update \
    # OS のパッケージリストを更新
    && apt-get install -y ${PKG} \
    # PKG 変数で指定されたパッケージ (git, vim, curl など) をインストール
    && groupadd --gid ${USER_GID} ${USER_GROUP_NAME} \
    # USER_GROUP_NAME という名前のグループを作成し、USER_GID という ID を割り当てる
    && useradd --uid ${USER_UID} --shell /bin/bash --gid ${USER_GID} -m ${USERNAME} \
    # USERNAME という名前のユーザーを作成し、USER_UID という ID と USER_GID というグループ ID を割り当てる。また、そのユーザーのシェルとして /bin/bash を設定し、ホームディレクトリを作成する
    && echo %${USER_GROUP_NAME} ALL=\(ALL\) NOPASSWD:ALL > /etc/sudoers.d/${USER_GROUP_NAME} \
    # sudo 設定を作成し、USER_GROUP_NAME グループのメンバーがパスワードなしで sudo コマンドを実行できるようにする
    && chmod 0440 /etc/sudoers.d/${USER_GROUP_NAME}
    # /etc/sudoers.d/ 内の設定ファイルのパーミッションを 0440 に設定して、セキュリティを確保


# Java と Gradle のバージョンを指定するための引数
ARG JAVA_VERSION=18.0.2-amzn
ARG GRADLE_VERSION=7.5

# sdkman を使用して指定したバージョンの Java と Gradle をインストール
RUN su ${USERNAME} --command \
    'curl -s "https://get.sdkman.io" | bash \
    && source "${HOME}/.sdkman/bin/sdkman-init.sh" \
    && sdk install java "${JAVA_VERSION}" \
    && sdk install gradle "${GRADLE_VERSION}"'
