#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build

# Install Node
RUN apt-get update -yq \
    && apt-get install curl gnupg -yq \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash \
    && apt-get install nodejs -yq

RUN echo "NODE Version:" && node --version
RUN echo "NPM Version:" && npm --version

WORKDIR /src
COPY ["AspNetCore.Angular/AspNetCore.Angular.csproj", "AspNetCore.Angular/"]
RUN dotnet restore "AspNetCore.Angular/AspNetCore.Angular.csproj"

# NPM Cache
COPY ["AspNetCore.Angular/ClientApp/package.json", "AspNetCore.Angular/ClientApp/"]
COPY ["AspNetCore.Angular/ClientApp/package-lock.json", "AspNetCore.Angular/ClientApp/"]
RUN npm --prefix AspNetCore.Angular/ClientApp install 

COPY AspNetCore.Angular/ AspNetCore.Angular/
WORKDIR "/src/AspNetCore.Angular"
RUN dotnet build "AspNetCore.Angular.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AspNetCore.Angular.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AspNetCore.Angular.dll"]