#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["src/server/AngularStore.Api/Directory.Build.props", "src/server/AngularStore.Api/"]
COPY ["src/server/AngularStore.Api/AngularStore.Api.csproj","src/server/AngularStore.Api/"]
COPY ["src/server/AngularStore.Application/AngularStore.Application.csproj","src/server/AngularStore.Application/"]
COPY ["src/server/AngularStore.Domain/AngularStore.Domain.csproj","src/server/AngularStore.Domain/"]
COPY ["src/server/AngularStore.Infrastructure/AngularStore.Infrastructure.csproj","src/server/AngularStore.Infrastructure/"]
COPY ["src/server/AngularStore.Persistence/AngularStore.Persistence.csproj","src/server/AngularStore.Persistence/"]
RUN dotnet restore "src/server/AngularStore.Api/AngularStore.Api.csproj"

COPY . .
WORKDIR "/src/src/server/AngularStore.Api"
RUN dotnet build "AngularStore.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AngularStore.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AngularStore.Api.dll"]