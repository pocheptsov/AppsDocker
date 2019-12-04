# Useful commands
# $ docker build --rm -t interview:0.1 .; docker rm -f interview; docker run --env ASPNETCORE_ENVIRONMENT=Development --rm -p 8080:80 --name interview interview:0.1
# $ docker build --rm -t interview:0.1 .
# $ docker rm -f interview
# $ docker run --env ASPNETCORE_ENVIRONMENT=Development --rm -p 8080:80 --name interview interview:0.1

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY Interview/*.csproj ./
RUN DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER=0 dotnet restore

# Copy everything else and build
COPY Interview ./
RUN dotnet publish -c Debug -o out

# FROM build-env AS testrunner
# WORKDIR /tests
# COPY Interview.Tests/. .
# RUN dotnet build
# ENTRYPOINT ["dotnet", "test", "--logger:trx"]

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "Interview.dll"]