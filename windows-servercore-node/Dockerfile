FROM mcr.microsoft.com/windows/servercore:ltsc2019 AS installer

ENV NODEJS_INSTALLER=node-v18.17.1-x64.msi

WORKDIR /archive
COPY ./$NODEJS_INSTALLER .

SHELL ["powershell", "-Command"]
RUN Start-Process -FilePath msiexec -ArgumentList /q, /i, ${env:NODEJS_INSTALLER} -Wait ; \
    Remove-Item -Path ${env:NODEJS_INSTALLER};

RUN Copy-Item -Path 'C:/Program Files/nodejs' -Destination "./nodejs" -Recurse

# Build final image
FROM mcr.microsoft.com/windows/servercore:ltsc2019 AS final

ARG SYSTEM_TOKEN
ENV ACCESS_TOKEN=${SYSTEM_TOKEN}

WORKDIR C:\/
COPY --from=installer ["C:/archive/nodejs/", "C:/Windows/System32/"]

USER ContainerAdministrator
RUN setx /M PATH "%PATH%;C:\Users\ContainerUser\AppData\Roaming\npm\;"
USER ContainerUser

COPY "/.npmrc" "./"

SHELL ["powershell", "-Command"]
RUN cp .npmrc ~/.npmrc

# Install global tools here. ie.:
# RUN npm install pnpm@7.29.1 -g;

CMD ["node.exe"]