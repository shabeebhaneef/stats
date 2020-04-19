#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 5001 80 443
EXPOSE 5000

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["DemoApplication.Api/DemoApplication.Api.csproj", "DemoApplication.Api/"]
COPY ["DemoApplication.Interfaces/DemoApplication.Interfaces.csproj", "DemoApplication.Interfaces/"]
COPY ["DemoApplication.IoC/DemoApplication.IoC.csproj", "DemoApplication.IoC/"]
COPY ["DemoApplication.Application/DemoApplication.Application.csproj", "DemoApplication.Application/"]
COPY ["DemoApplication.Domain/DemoApplication.Domain.csproj", "DemoApplication.Domain/"]
RUN dotnet restore "DemoApplication.Api/DemoApplication.Api.csproj"
COPY . .
WORKDIR "/src/DemoApplication.Api"
RUN dotnet build "DemoApplication.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DemoApplication.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DemoApplication.Api.dll"]
