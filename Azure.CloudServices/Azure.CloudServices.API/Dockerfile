#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Azure.CloudServices.API/Azure.CloudServices.API.csproj", "Azure.CloudServices.API/"]
RUN dotnet restore "Azure.CloudServices.API/Azure.CloudServices.API.csproj"
COPY . .
WORKDIR "/src/Azure.CloudServices.API"
RUN dotnet build "Azure.CloudServices.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Azure.CloudServices.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Azure.CloudServices.API.dll"]