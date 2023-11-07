# Introduction 
nginx Windows ServerCore Docker image

Base image: mcr.microsoft.com/windows/servercore:ltsc2019
(Have to use ServerCore instead of Nano, because the distributed nginx binary is x86-64 and Nanoserver only supports x64 binaries. See: https://serverfault.com/questions/848211/how-do-i-run-nginx-in-a-windows-nano-server-based-container)

Download your binary from here:
https://nginx.org/en/download.html