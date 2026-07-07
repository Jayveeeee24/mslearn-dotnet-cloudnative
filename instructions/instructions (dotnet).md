# create a new webapi
dotnet new webapi

# dotnet publish as a container image
dotnet publish --os linux --arch x64 /t:PublishContainer -c Release

# Add a package
dotnet add package Microsoft.Extensions.Http.Resilience
dotnet add package OpenTelemetry.Exporter.Console
dotnet add package OpenTelemetry.Extensions.Hosting
dotnet add package OpenTelemetry.Instrumentation.AspNetCore
dotnet add package OpenTelemetry.Instrumentation.EventCounters --prerelease
dotnet add package OpenTelemetry.Instrumentation.Runtime
dotnet add package OpenTelemetry.Instrumentation.SqlClient --prerelease
dotnet add package OpenTelemetry.Instrumentation.Http
dotnet add package OpenTelemetry.Exporter.Prometheus.AspNetCore
dotnet add package OpenTelemetry.Exporter.Zipkin
dotnet add package Azure.Monitor.OpenTelemetry.AspNetCore

# build for development
dotnet build

# grafana dashboard import
https://github.com/microsoft/aspire/blob/main/src/Grafana/dashboards/aspnetcore.json