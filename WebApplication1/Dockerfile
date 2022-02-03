#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["WebApplication1/WebApplication1.csproj", "WebApplication1/"]
RUN dotnet restore "WebApplication1/WebApplication1.csproj"
COPY . .
WORKDIR "/src/WebApplication1"
RUN dotnet build "WebApplication1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# <wkhtmltopdf>
# Install dependencies for wkhtmltopdf
RUN apt-get update

RUN apt-get install wget libgdiplus -y

RUN wget -P /app/publish https://github.com/rdvojmoc/DinkToPdf/raw/master/v0.12.4/64%20bit/libwkhtmltox.dll

RUN wget -P /app/publish https://github.com/rdvojmoc/DinkToPdf/raw/master/v0.12.4/64%20bit/libwkhtmltox.dylib

RUN wget -P /app/publish https://github.com/rdvojmoc/DinkToPdf/raw/master/v0.12.4/64%20bit/libwkhtmltox.so

#RUN wget -P /app https://github.com/rdvojmoc/DinkToPdf/raw/master/v0.12.4/64%20bit/libwkhtmltox.dll
#
#RUN wget -P /app https://github.com/rdvojmoc/DinkToPdf/raw/master/v0.12.4/64%20bit/libwkhtmltox.dylib
#
#RUN wget -P /app https://github.com/rdvojmoc/DinkToPdf/raw/master/v0.12.4/64%20bit/libwkhtmltox.so


ENTRYPOINT ["dotnet", "WebApplication1.dll"]