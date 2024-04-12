FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["DrinkMenu/DrinkMenu.csproj", "DrinkMenu/"]
RUN dotnet restore "DrinkMenu/DrinkMenu.csproj"
COPY . .
WORKDIR "/src/DrinkMenu"
RUN dotnet build "DrinkMenu.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "DrinkMenu.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DrinkMenu.dll"]
