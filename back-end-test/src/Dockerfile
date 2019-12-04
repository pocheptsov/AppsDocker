# Useful commands
# $ docker build --rm -t interview:0.1 .; docker rm -f interview; docker run --env ASPNETCORE_ENVIRONMENT=Development --rm -p 8080:80 --name interview interview:0.1
# $ docker build --rm -t interview:0.1 .
# $ docker rm -f interview
# $ docker run --env ASPNETCORE_ENVIRONMENT=Development --rm -p 8080:80 --name interview interview:0.1
# Tests
# $ docker build --target testrunner -t interview:test .
# $ docker run --rm  interview:test
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY Interview/*.csproj ./
RUN DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER=0 dotnet restore

# Copy everything else and build
COPY Interview ./
RUN dotnet publish -c Debug -o out

FROM build-env AS testrunner
WORKDIR /tests
COPY Interview.Tests/. Interview.Tests/.
COPY Interview/. Interview/.
RUN dotnet build Interview.Tests/Interview.Tests.csproj
ENTRYPOINT ["dotnet", "test", "Interview.Tests/Interview.Tests.csproj"]

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "Interview.dll"]