#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0-preview AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0-preview AS build
WORKDIR /src
COPY ["Playground/Server/Playground.Server.csproj", "Playground/Server/"]
COPY ["Playground/Client/Playground.Client.csproj", "Playground/Client/"]
COPY ["Playground/Shared/Playground.Shared.csproj", "Playground/Shared/"]
RUN dotnet restore "Playground/Server/Playground.Server.csproj"
COPY . .
WORKDIR "/src/Playground/Server"
RUN dotnet build "Playground.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Playground.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Playground.Server.dll"]