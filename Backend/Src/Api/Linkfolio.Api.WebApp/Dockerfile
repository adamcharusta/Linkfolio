FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Src/Api/Linkfolio.Api.WebApp/Linkfolio.Api.WebApp.csproj", "Src/Api/Linkfolio.Api.WebApp/"]
RUN dotnet restore "Src/Api/Linkfolio.Api.WebApp/Linkfolio.Api.WebApp.csproj"
COPY . .
WORKDIR "/src/Src/Api/Linkfolio.Api.WebApp"
RUN dotnet build "Linkfolio.Api.WebApp.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Linkfolio.Api.WebApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Linkfolio.Api.WebApp.dll"]
