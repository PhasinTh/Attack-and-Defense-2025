FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-dotnet-checkers

WORKDIR /app/places
COPY checkers/places/src/ ./
RUN dotnet publish -c release -o ./publish

WORKDIR /app/spaces
COPY checkers/spaces/src/ ./
RUN dotnet publish -c release -o ./publish

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-dot-net-checkers

# From HITB 2022
WORKDIR /app
COPY checkers/smallword/src/ /app/
RUN dotnet publish -c release -o ./smallword-publish

FROM ghcr.io/hackerdom/ctf-scoreboard-client:hitbsecconfctf-2023 AS scoreboard


FROM ghcr.io/hackerdom/checksystem:master

COPY --from=scoreboard /scoreboard/build /scoreboard
ENV CS_STATIC=/scoreboard

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3 python3-pip nodejs npm software-properties-common && \
    apt-add-repository non-free && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        p7zip bzip2 gzip unrar-free zip

# Install dotnet runtime (https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian)
ENV DOTNET_CLI_TELEMETRY_OPTOUT=1
RUN wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && \ 
    dpkg -i packages-microsoft-prod.deb && \
    rm packages-microsoft-prod.deb && \
    apt-get update && \
    apt-get install -y --no-install-recommends dotnet-runtime-7.0

RUN wget https://packages.microsoft.com/config/debian/11/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && \
    dpkg -i packages-microsoft-prod.deb && \
    rm packages-microsoft-prod.deb && \
    apt-get update && apt-get install -y dotnet-sdk-6.0

# COPY checkers/funding/package*.json /app/checkers/funding/
# RUN /bin/bash -c "cd /app/checkers/funding; npm ci"

RUN pip3 install requests gornilo patool beautifulsoup4 --break-system-packages


COPY checkers/hyperborea-legends /app/checkers/hyperborea-legends
COPY checkers/memos /app/checkers/memos
COPY checkers/perune /app/checkers/perune
COPY checkers/rusi-trainer /app/checkers/rusi-trainer
COPY checkers/rustest /app/checkers/rustest
COPY checkers/werk /app/checkers/werk

RUN for f in /app/checkers/*/requirements.txt; do \
    pip install -r "$f" --break-system-packages; \
done

COPY cs.conf /app/cs.conf

COPY checkers/passmgr      /app/checkers/passmgr
COPY checkers/spaces       /app/checkers/spaces
COPY --from=build-dotnet-checkers /app/spaces/publish/ /app/checkers/spaces/src/bin/Release/net7.0/publish
COPY checkers/lockstone    /app/checkers/lockstone
COPY checkers/godeeper     /app/checkers/godeeper
COPY checkers/pure         /app/checkers/pure
RUN pip install -r /app/checkers/pure/requirements.txt --break-system-packages
# COPY checkers/funding      /app/checkers/funding
COPY checkers/keys         /app/checkers/keys
COPY checkers/places       /app/checkers/places
COPY --from=build-dotnet-checkers /app/places/publish/ /app/checkers/places/src/bin/Release/net7.0/publish
COPY checkers/docs         /app/checkers/docs
COPY checkers/tokenourcer  /app/checkers/tokenourcer
RUN pip install -r /app/checkers/tokenourcer/requirements.txt --break-system-packages
COPY checkers/notes        /app/checkers/notes
RUN pip install -r /app/checkers/notes/requirements.txt --break-system-packages


# Checkers from HITB 2022
COPY checkers/sh /app/checkers/sh
COPY checkers/crs /app/checkers/crs
COPY checkers/smallword /app/checkers/smallword
COPY checkers/mypack /app/checkers/mypack
COPY checkers/wallet /app/checkers/wallet
COPY checkers/n0tes /app/checkers/n0tes
RUN pip install -r /app/checkers/n0tes/requirements.txt --break-system-packages
COPY checkers/kv /app/checkers/kv
COPY checkers/obscurity /app/checkers/obscurity
COPY --from=build-dot-net-checkers /app/smallword-publish/ /app/checkers/smallword/src/bin/Release/net6.0/publish
COPY checkers/linkextractor /app/checkers/linkextractor
COPY checkers/issuecker /app/checkers/issuecker
RUN pip install -r /app/checkers/issuecker/requirements.txt --break-system-packages