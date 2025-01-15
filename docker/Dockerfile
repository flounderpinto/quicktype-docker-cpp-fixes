FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
RUN apt update && \
    apt install -y \
       curl \
       git \
       build-essential \
       libboost-dev && \
#Install nodejs
    cd "$HOME" && \
    curl -sL https://deb.nodesource.com/setup_20.x -o ./nodesource_setup.sh && \
    chmod 755 ./nodesource_setup.sh && \
    ./nodesource_setup.sh && \
    apt install -y \
       nodejs && \
    rm ./nodesource_setup.sh && \
#Install nvm
    cd "$HOME" && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash && \
    export NVM_DIR="$HOME/.nvm" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \
    . "$NVM_DIR/nvm.sh" && \
    [ -s "$NVM_DIR/bash_completion" ] && \
    . "$NVM_DIR/bash_completion" && \
#Clone quicktype and modify file
    cd "$HOME" && \
    git clone --depth 1 https://github.com/glideapps/quicktype.git && \
    cd "$HOME/quicktype" && \
    git checkout 29bb8160660a7daa2a837ac0cb1e92f99729ad8e && \
    CPP_FILE="$HOME/quicktype/packages/quicktype-core/src/language/CPlusPlus/CPlusPlusRenderer.ts" && \
    sed -i 's|                    (property.isOptional && property.type.kind !== "null" && property.type.kind !== "any")|                    false|' "$CPP_FILE" && \
#Build
    npm install && \
    npm audit fix | true && \
    npm run build && \
#Test
    FIXTURE=cplusplus npm test && \
#Install
    npm link ../quicktype && \
#Cleanup
    apt remove -y \
       curl \
       git \ 
       build-essential \
       libboost-dev && \
    apt autoremove -y && \
    rm -rf /var/lib/apt/lists/*
