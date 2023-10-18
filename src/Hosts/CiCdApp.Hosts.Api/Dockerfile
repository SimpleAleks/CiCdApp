FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["src/Hosts/CiCdApp.Hosts.Api/CiCdApp.Hosts.Api.csproj", "src/Hosts/CiCdApp.Hosts.Api/"]
RUN dotnet restore "src/Hosts/CiCdApp.Hosts.Api/CiCdApp.Hosts.Api.csproj"
COPY . .
WORKDIR "/src/src/Hosts/CiCdApp.Hosts.Api"
RUN dotnet build "CiCdApp.Hosts.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CiCdApp.Hosts.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CiCdApp.Hosts.Api.dll"]
