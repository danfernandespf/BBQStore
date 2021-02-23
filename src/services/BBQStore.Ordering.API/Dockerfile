#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/services/BBQStore.Ordering.API/BBQStore.Ordering.API.csproj", "src/services/BBQStore.Ordering.API/"]
RUN dotnet restore "src/services/BBQStore.Ordering.API/BBQStore.Ordering.API.csproj"
COPY . .
WORKDIR "/src/src/services/BBQStore.Ordering.API"
RUN dotnet build "BBQStore.Ordering.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BBQStore.Ordering.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BBQStore.Ordering.API.dll"]