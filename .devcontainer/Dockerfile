FROM debian:bullseye
USER 0:0

RUN apt-get -y update && \
    apt-get -y upgrade
RUN apt-get -y install --no-install-recommends ca-certificates gpg curl wget xz-utils gzip lz4 libfontconfig1 fontconfig && \
    apt-get -y install perl-modules git sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# texlive
WORKDIR /tmp
COPY texlive.profile /tmp/texlive.profile
RUN wget https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz && \
    zcat < install-tl-unx.tar.gz | tar xf - && \
    cd install-tl-* && \
    ./install-tl --no-interaction --no-doc-install --no-src-install --profile=/tmp/texlive.profile
ENV PATH="$PATH:/usr/local/texlive/bin/x86_64-linux"

# ltex-ls
RUN wget https://github.com/valentjn/ltex-ls/releases/download/15.2.0/ltex-ls-15.2.0-linux-x64.tar.gz && \
    tar xfz ltex-ls-15.2.0-linux-x64.tar.gz && \
    cp -arv ltex-ls-15.2.0 /opt/ltex-ls && \
    chmod 777 /tmp

# tex live additional packages
RUN tlmgr init-usertree && \
    tlmgr update --self --all
RUN tlmgr install import texcount latexmk babel-hungarian glossaries glossaries-magyar glossaries-extra tagging \
        pgfplots lipsum relsize bigfoot \
        xstring environ cprotect textpos todonotes mfirstuc physics && \
    texhash

# magyar.ldf
RUN wget http://www.math.bme.hu/latex/lakk/texmf/tex/generic/magyar/magyar.ldf && \
    mv magyar.ldf $(find /usr/local/texlive/ -name magyar.ldf)

RUN groupadd wheel && useradd -Ums /bin/bash -G wheel latex && \
    sh -c 'echo "%wheel ALL=(ALL:ALL) NOPASSWD: ALL" > /etc/sudoers'
USER latex
WORKDIR /home/latex
