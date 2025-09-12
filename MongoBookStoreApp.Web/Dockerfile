FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
ENV ASPNETCORE_URLS=http://+:80
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt install -y nodejs

WORKDIR /src

# copy csproj files
COPY ["./MongoBookStoreApp.Contracts/MongoBookStoreApp.Contracts.csproj", "src/MongoBookStoreApp.Contracts/"]
COPY ["./MongoBookStoreApp.Core/MongoBookStoreApp.Core.csproj", "src/MongoBookStoreApp.Core/"]
COPY ["./MongoBookStoreApp.Web/MongoBookStoreApp.Web.csproj", "src/MongoBookStoreApp.Web/"]

# restore
RUN dotnet restore "src/MongoBookStoreApp.Web/MongoBookStoreApp.Web.csproj"

COPY . .

# build
WORKDIR "/src/MongoBookStoreApp.Web/"
RUN dotnet build -c Release -o /app/build

# publish
FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS runtime
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MongoBookStoreApp.Web.dll"]

FROM base AS runtime
WORKDIR /app

COPY --from=publish /app/publish .
RUN ls -l
ENTRYPOINT [ "dotnet", "MongoBookStoreApp.Web.dll" ]
