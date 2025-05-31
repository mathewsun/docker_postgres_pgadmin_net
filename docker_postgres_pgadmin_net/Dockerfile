FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["docker_postgres_pgadmin_net/docker_postgres_pgadmin_net.csproj", "docker_postgres_pgadmin_net/"]
RUN dotnet restore "./docker_postgres_pgadmin_net/docker_postgres_pgadmin_net.csproj"
COPY . .
WORKDIR "/src/docker_postgres_pgadmin_net"
RUN dotnet build "./docker_postgres_pgadmin_net.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./docker_postgres_pgadmin_net.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "docker_postgres_pgadmin_net.dll"]