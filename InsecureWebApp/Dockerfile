#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0.202 AS build
WORKDIR /src
COPY ["InsecureWebApp/InsecureWebApp.csproj", "InsecureWebApp/"]
COPY ["InsecureWebApp/wwwroot", "InsecureWebApp/wwwroot"]
COPY ["InsecureWebApp/Files", "InsecureWebApp/Files"]
RUN dotnet restore "InsecureWebApp/InsecureWebApp.csproj"
COPY . .
WORKDIR "/src/InsecureWebApp"
RUN dotnet build "InsecureWebApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "InsecureWebApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
RUN rm -rf appsettings.json
RUN cp appsettings.Docker.json appsettings.json
RUN mkdir -p Files/Prescriptions
ENTRYPOINT ["dotnet", "InsecureWebApp.dll"]
