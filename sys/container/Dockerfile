FROM ubuntu

VOLUME /UOData
VOLUME /UO

RUN apt update; apt install -y gnupg ca-certificates; \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF; \
    echo "deb https://download.mono-project.com/repo/ubuntu stable-focal/snapshots/6.8.0.123 main" | tee /etc/apt/sources.list.d/ mono-official-stable.list; \
    apt update; \
    DEBIAN_FRONTEND="noninteractive" apt -y install mono-complete zlib1g-dev;
#ADD packages-microsoft-prod.deb packages-microsoft-prod.deb
#RUN dpkg -i packages-microsoft-prod.deb
#RUN rm packages-microsoft-prod.deb
RUN apt update; DEBIAN_FRONTEND="noninteractive" apt -y install dotnet-sdk-8.0
RUN groupadd uo
RUN useradd uo -g uo -m
WORKDIR /UO
COPY entrypoint.sh /
RUN chown 1000:1000 /entrypoint.sh; chmod +x /entrypoint.sh
USER 1000:1000

CMD ["/entrypoint.sh"]
