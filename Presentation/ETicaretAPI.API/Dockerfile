#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Presentation/ETicaretAPI.API/ETicaretAPI.API.csproj", "Presentation/ETicaretAPI.API/"]
COPY ["Core/ETicaretAPI.Application/ETicaretAPI.Application.csproj", "Core/ETicaretAPI.Application/"]
COPY ["Core/ETicaretAPI.Domain/ETicaretAPI.Domain.csproj", "Core/ETicaretAPI.Domain/"]
COPY ["Infrastructure/ETicaretAPI.Infrastructure/ETicaretAPI.Infrastructure.csproj", "Infrastructure/ETicaretAPI.Infrastructure/"]
COPY ["Infrastructure/ETicaretAPI.Persistence/ETicaretAPI.Persistence.csproj", "Infrastructure/ETicaretAPI.Persistence/"]
RUN dotnet restore "./Presentation/ETicaretAPI.API/ETicaretAPI.API.csproj"
COPY . .
WORKDIR "/src/Presentation/ETicaretAPI.API"
RUN dotnet build "./ETicaretAPI.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ETicaretAPI.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ETicaretAPI.API.dll"]