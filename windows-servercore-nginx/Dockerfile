FROM mcr.microsoft.com/windows/servercore:ltsc2019

ARG NGINX_ARCHIVE=nginx-1.24.0.zip

WORKDIR C:\/
COPY ./$NGINX_ARCHIVE .
USER ContainerAdministrator

SHELL ["powershell", "-Command"]
# Make sure that Docker always uses default DNS servers which hosted by Dockerd.exe
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ServerPriorityTimeLimit -Value 0 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ScreenDefaultServers -Value 0 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ScreenUnreachableServers -Value 0 -Type DWord
	
# Shorten DNS cache times
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name MaxCacheTtl -Value 30 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name MaxNegativeCacheTtl -Value 30 -Type DWord

RUN tar -xf nginx-1.24.0.zip -C C:\/; \
    del nginx-1.24.0.zip; \
    ren nginx-1.24.0 nginx;

WORKDIR /nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]