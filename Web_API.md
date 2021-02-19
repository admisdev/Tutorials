# Web API Best Practices

Framework: .NET 5.0

## Folder Structure

  -Controllers
  
  -Models
  
  -Services
  

## B2C Authentication & Authorization

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

## SQL Query

Should we recommend not to use entity framework?

    List<Result> results = new List<Result>();
    using (SqlConnection connection = new SqlConnection(this.configuration.GetSection("DBConnectionString").Value))
    {
        connection.Open();
        var queryString = "exec up_GetResults @Param1=@Param1, @Param2=@Param2";
        SqlCommand command = new SqlCommand(queryString, connection);
        command.Parameters.Add(new SqlParameter("@Param1", param1));
        command.Parameters.Add(new SqlParameter("@Param2", param2));
        using (SqlDataReader reader = await command.ExecuteReaderAsync())
        {
            while (reader.Read())
            {
                results.Add(new Result(reader));
            }
        }
        return results;
    }

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

### Azure Event Hub

    var eventHubConfig = Configuration.GetSection("Logging").GetSection("eventHub");
    string eventHubConnection = eventHubConfig.GetValue<string>("connectionString");
    Log.Logger = new LoggerConfiguration()
        .WriteTo.AzureEventHub(new JsonFormatter(),
            eventHubConnection.Replace(@"\", ""),
            eventHubConfig.GetValue<string>("entityName"))
        .CreateLogger();

## Error Handling

Return StatusCode(400, "Error message") or BadRequest(), Status(401, "Error message") or UnAuthorized()?
