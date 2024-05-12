FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["Directory.Build.props", "/"]
COPY ["src/PdfRenderer.Api/PdfRenderer.Api.csproj", "src/PdfRenderer.Api/"]
RUN dotnet restore "src/PdfRenderer.Api/PdfRenderer.Api.csproj"

COPY . .
WORKDIR "/src/src/PdfRenderer.Api"
RUN dotnet build "PdfRenderer.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "PdfRenderer.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PdfRenderer.Api.dll"]
