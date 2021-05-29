#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["AspNetCore.BlazorWebAssembly/Server/AspNetCore.BlazorWebAssembly.Server.csproj", "AspNetCore.BlazorWebAssembly/Server/"]
COPY ["AspNetCore.BlazorWebAssembly/Shared/AspNetCore.BlazorWebAssembly.Shared.csproj", "AspNetCore.BlazorWebAssembly/Shared/"]
COPY ["AspNetCore.BlazorWebAssembly/Client/AspNetCore.BlazorWebAssembly.Client.csproj", "AspNetCore.BlazorWebAssembly/Client/"]
RUN dotnet restore "AspNetCore.BlazorWebAssembly/Server/AspNetCore.BlazorWebAssembly.Server.csproj"
COPY AspNetCore.BlazorWebAssembly/ AspNetCore.BlazorWebAssembly/
WORKDIR "/src/AspNetCore.BlazorWebAssembly/Server"
RUN dotnet build "AspNetCore.BlazorWebAssembly.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AspNetCore.BlazorWebAssembly.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AspNetCore.BlazorWebAssembly.Server.dll"]