# escape=`
FROM microsoft/dotnet:1.0.7-sdk-nanoserver-10.0.14393.1715

# set up environment
ENV ASPNETCORE_URLS http://+:80
ENV NODE_VERSION 6.11.3

# Install node
RUN Invoke-WebRequest -UseBasicParsing https://nodejs.org/dist/v${env:NODE_VERSION}/node-v${env:NODE_VERSION}-win-x64.zip -outfile node.zip; `
    Expand-Archive node.zip -DestinationPath ${env:TEMP}/nodejs-tmp; `
    Move-Item ${env:TEMP}/nodejs-tmp/node-v${env:NODE_VERSION}-win-x64 ${env:ProgramFiles}/nodejs; `
    Remove-Item -Force node.zip; `
    $env:PATH += \";${env:ProgramFiles}/nodejs\"; `
    & npm install -g gulp bower; `
    Invoke-WebRequest -UseBasicParsing https://github.com/git-for-windows/git/releases/download/v2.14.1.windows.1/MinGit-2.14.1-64-bit.zip -outfile git.zip; `
    Expand-Archive git.zip -DestinationPath $Env:ProgramFiles/git; `
    Remove-Item -Force git.zip

RUN setx /M PATH $($Env:PATH + ';' + $Env:ProgramFiles + '/nodejs' + ';' + $Env:ProgramFiles + '/git/cmd')

# warmup up NuGet package cache
COPY packagescache.csproj C:/warmup/packagescache.csproj
RUN dotnet restore C:/warmup/packagescache.csproj `
        --source https://api.nuget.org/v3/index.json `
        --verbosity quiet; `
    Remove-Item -Recurse -Force C:/warmup
