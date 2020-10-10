FROM debian:latest 

LABEL version="0.1" maintainer="Rodrigo Braga Prado <docker@quati.com.br>"

################
# Base install #
################
RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y \
    curl  && \
    apt-get clean

###############
# Set locales #
###############
RUN echo -e "en_US.UTF-8 UTF-8 \
    pt_BR.UTF-8 UTF-8" > /etc/locale.gen

RUN echo -e " \
    export HISTTIMEFORMAT=\"%Y/%m/%d %T => \" ; \
    HISTSIZE=10000000000000000 ;\
    HISTFILESIZE=2000000000000000000000 ;\
    alias ..='cd ..' ;\
    alias ...='cd ../..' ;\
    alias ....='cd ../../..' \
    alias now='date \"+%Y-%m-%d-%H:%M:%S\"' ;\
    alias egrep='egrep --color=auto'; \
    alias fgrep='fgrep --color=auto' ;\
    alias grep='grep --color=auto'; \
    alias l='ls -CF'; \
    alias la='ls -A'; \
    alias lf='ls -lahFcips --full-time | awk '\''{k=0;for(i=0;i<=8;i++)k+=((substr(\$3,i+2,1)~/[rwx]/)*2^(8-i));if(k)printf(\" %0o \",k);print}'\''' ; \
    alias lg='ls -l | grep --color=auto ;' \
    alias ll='ls -alF' \
    alias llm='ls -l | less' \
    alias lo='ls -lah --color | awk '\''{k=0;for(i=0;i<=8;i++)k+=((substr(\$1,i+2,1)~/[rwx]/)*2^(8-i));if(k)printf(\" %0o \",k);print}'\''' \
    alias ls='ls --color=auto'; \
    alias nocomment='grep -Ev '\''^(#|\$|;)'\'''; \
    alias psg='ps ax | grep -v grep | grep --color=auto ; ' \
    alias psram='ps aux | sort -rnk 4 | head'; \
    alias ssh='ssh -X -C -v' ;\
    alias wtf='man' ; " >> /etc/bash.bashrc

