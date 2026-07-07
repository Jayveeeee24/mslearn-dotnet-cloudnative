# create a new webapi
dotnet new webapi

# dotnet publish as a container image
dotnet publish --os linux --arch x64 /t:PublishContainer -c Release

# Add a package
dotnet add package Microsoft.Extensions.Http.Resilience