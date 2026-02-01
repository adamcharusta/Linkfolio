FROM mcr.microsoft.com/dotnet/runtime:10.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Src/Email/Linkfolio.Email.WorkerService/Linkfolio.Email.WorkerService.csproj", "Src/Email/Linkfolio.Email.WorkerService/"]
RUN dotnet restore "Src/Email/Linkfolio.Email.WorkerService/Linkfolio.Email.WorkerService.csproj"
COPY . .
WORKDIR "/src/Src/Email/Linkfolio.Email.WorkerService"
RUN dotnet build "Linkfolio.Email.WorkerService.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Linkfolio.Email.WorkerService.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Linkfolio.Email.WorkerService.dll"]
