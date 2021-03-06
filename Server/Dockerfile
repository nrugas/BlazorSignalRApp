#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY Server/BlazorSignalRApp.Server.csproj Server/
COPY Client/BlazorSignalRApp.Client.csproj Client/
COPY Shared/BlazorSignalRApp.Shared.csproj Shared/
RUN dotnet restore "Server/BlazorSignalRApp.Server.csproj"
COPY . .
WORKDIR "/src/Server"
RUN dotnet build "BlazorSignalRApp.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorSignalRApp.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorSignalRApp.Server.dll"]
