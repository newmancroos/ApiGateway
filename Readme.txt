Api Gateway using Ocelot
------------------------

Feature of API Gateway : 
	- Routing
	- Request Aggregation
	- Authentication
	- Authorization
	- Rate Limiting
	- Caching
	- Load Balancing
	
Ocelot : 
	- ASP.Net Core Api Gateway
	- Nuget Package
	- Ocelot Api Gateway support all the featre that any standard Api gateways does.
	- ocelot.dev.json :
{
	"Routes" :[
	{
		"DownstreamPathTemplate" : "/weatherforecast",
		"DownstreamSchema": "https",
		DownstreamHostAndPorts":[
			{
				"Host" : "localhost",
				"Port" : 35001
			}
		],
		"UpstreamPathTemplate": "/api/weather",
		"UpstreamHttpMethod" : ["Get"]		
	}
  ],
  "GlobalConfiguration": {
	"BaseUrl" : "https://localhost:5021"
  }
}

In Program.cs
-------------
var env = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT");

webBuilder.ConfigureAppConfiguration(config =>
	config.AddJsonFile($"ocelot.{env}.json"));


We can have Rate limit. It is nothing but limiting the request per second or Minute. 
It is just configuration setiing in the Ocelot.json file

"RateLimitOptions":{
	"ClientWhitelist":[],
	"EnableRateLimiting": true,
	"Period": "5s",
	"PeriodTimespan": 1,
	"Limit": 1
}


We can also enable caching so that Api gateway hold the result for same request for the specified time.

- Add Nuget package "Ocelot.Cache.CacheManager"
-             services.AddOcelot().AddCacheManager(settings => {
                settings.WithDictionaryHandle();
            });
			
- In the Ocelot.json	
	"FileCacheOptions":  {"TtlSeconds" :  30}


- We can pass Bearertoken to the api thru Ocelot apigate way .Read https://stackoverflow.com/questions/65962957/adding-authorization-header-authorization-bearer-access-token-to-specific-r
