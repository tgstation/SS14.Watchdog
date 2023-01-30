# Use alpine to save space
# FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine AS base
FROM mcr.microsoft.com/dotnet/aspnet:7.0-bullseye-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
EXPOSE 1212 # Game port
VOLUME ["/app/instances"]
RUN apt-get update && apt-get install -y git python3 python-is-python3

# FROM mcr.microsoft.com/dotnet/sdk:7.0-alpine AS build
FROM mcr.microsoft.com/dotnet/sdk:7.0-bullseye-slim AS build
WORKDIR /src
COPY ["SS14.Watchdog/SS14.Watchdog.csproj", "SS14.Watchdog/"]
RUN dotnet restore "SS14.Watchdog/SS14.Watchdog.csproj"
COPY . .
WORKDIR "/src/SS14.Watchdog"
RUN dotnet build "SS14.Watchdog.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SS14.Watchdog.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SS14.Watchdog.dll"]