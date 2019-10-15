FROM mcr.microsoft.com/dotnet/core/sdk:2.2

RUN apt update && apt upgrade -y && \
  apt install -y lsb-release apt-transport-https ca-certificates

RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
  curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
  apt update && ACCEPT_EULA=Y apt install -y msodbcsql17 mssql-tools && \
  echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile && \
  echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc && \
  bash -c "source ~/.bashrc" && \
  apt install -y unixodbc-dev

RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
  echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php7.3.list && \
  apt update && apt install -y php7.3 php7.3-dom php7.3-mbstring php7.3-curl php7.3-zip php-pear php7.3-dev unzip

RUN curl -sS https://getcomposer.org/installer | php && \
  mv composer.phar /usr/local/bin/composer && \
  chmod +x /usr/local/bin/composer

RUN pecl install sqlsrv && \
   pecl install pdo_sqlsrv && \
  echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-pdo_sqlsrv.ini && \
  echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/20-sqlsrv.ini

WORKDIR /app

EXPOSE 5001

#CMD cp ./appsettings.json.test ./nfs-api/appsettings.json && \
#  dotnet restore && \
#  dotnet build && \
#  dotnet run --urls https://0.0.0.0:5001 --project /app/nfs-api

RUN apt autoremove && rm -rf /var/cache/apt/archives/* && rm -rf /tmp/*
