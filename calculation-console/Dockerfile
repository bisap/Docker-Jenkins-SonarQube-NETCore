FROM microsoft/dotnet:2.1-sdk AS build
ARG PROJECT_VERSION
ENV DOTNET_CLI_TELEMETRY_OPTOUT = 1
WORKDIR /src

RUN apt-get update && apt-get install -y \
    openjdk-8-jre-headless

RUN apt-get clean
COPY . .

RUN dotnet restore calculation-console/calculation-console.csproj
RUN dotnet test calculation.tests/calculation.tests.csproj \
        --configuration Release \
        --logger:"trx;LogFileName=testresult.xml" \
        /p:CollectCoverage=true \
        /p:CoverletOutputFormat=opencover \ 
        /p:CoverletOutput="TestResults\opencover.xml"

RUN dotnet build-server shutdown

WORKDIR /src/calculation-console
RUN dotnet tool install --global dotnet-sonarscanner
ENV PATH="${PATH}:/root/.dotnet/tools"
RUN dotnet sonarscanner begin /k:"devops" /v:${PROJECT_VERSION} \
    /d:sonar.host.url=http://localhost:9000 \ 	
	/d:sonar.cs.opencover.reportsPaths="/src/**/TestResults/opencover.xml" \ 
	/d:sonar.cs.vstest.reportsPaths="/src/**/TestResults/testresult.xml" \ 
	/d:sonar.scm.disabled=true /d:sonar.coverage.dtdVerification=true \ 
	/d:sonar.coverage.exclusions="*Tests*.cs,*testresult*.xml,*opencover*.xml" \ 
	/d:sonar.test.exclusions="*Tests*.cs,*testresult*.xml,*opencover*.xml"
RUN dotnet build calculation-console.csproj -c Release -o /app --no-restore
RUN dotnet sonarscanner end

RUN dotnet publish calculation-console.csproj -c Release -o /app --no-restore

FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "calculation-console.dll"]

