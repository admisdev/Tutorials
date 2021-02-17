# Web API Tutorial

Framework: .NET 5.0

## Folder Structure

  Controllers
  Models
  Services

## Azure B2C Authentication

    services.AddAuthentication(options =>
    {
        options.DefaultScheme = JwtBearerDefaults.AuthenticationScheme;
        options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
    })
    .AddJwtBearer(options =>
    {
       options.Authority = Configuration.GetSection("Authorization").GetSection("azureAuthority").Value;
       options.Audience = Configuration.GetSection("Authorization").GetSection("azureAudience").Value;
       options.IncludeErrorDetails = true;
    });

## Security

## CORS

## Logging
