#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 8090

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/BlazorApp.Server/BlazorApp.Server.csproj", "src/BlazorApp.Server/"]
RUN dotnet restore "src/BlazorApp.Server/BlazorApp.Server.csproj"
COPY . .
WORKDIR "/src/src/BlazorApp.Server"
RUN dotnet build "BlazorApp.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorApp.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS=http://+:8090
ENTRYPOINT ["dotnet", "BlazorApp.Server.dll","--server.urls","http://*:8090"]