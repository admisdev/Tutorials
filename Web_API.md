# Web API Tutorial

Framework: .NET 5.0

## Folder Structure

  -Controllers
  
  -Models
  
  -Services
  

## Authentication & Authorization

### appsettings.json

    "Authorization": {
        "azureAuthority": "https://#{LoginService}#.b2clogin.com/#{LoginService}#.onmicrosoft.com/#{LoginUserFlow}#/v2.0",
        "azureAudience": "#{WebAPIAppID}#"
    }

### ConfigureServices():

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
       
       options.Events = new JwtBearerEvents
       {
           OnTokenValidated = context =>
           {
               // Grab the raw value of the token, and store it as a claim so we can retrieve it again later in the request pipeline
               // Have a look at the ValuesController.UserInformation() method to see how to retrieve it and use it to retrieve the
               // user's information from the /userinfo endpoint
               if (context.SecurityToken is JwtSecurityToken token)
               {
                   if (context.Principal.Identity is ClaimsIdentity identity)
                   {
                       identity.AddClaim(new Claim("access_token", token.RawData));
                   }
               }

               return Task.FromResult(0);
           },
           OnAuthenticationFailed = AuthenticationFailed
       };
    });
    
### Configure():
    
    app.UseAuthentication();

    app.UseAuthorization();

## Security

### Configure():
  
    app.useHsts();

## CORS

Only enable CORS in development or if needed.

### ConfigureService():
  
    app.AddCors();
    
### Configure():
  
    app.UseCors(builder => builder
       .AllowAnyOrigin()
       .AllowAnyMethod()
       .AllowAnyHeader());

## Logging

### Serilog

    var config = new ConfigurationBuilder()
        .AddJsonFile("appsettings.json")
        .Build();

    Log.Logger = new LoggerConfiguration()
        .ReadFrom.Configuration(config)
        .CreateLogger();

## Exception Handling

