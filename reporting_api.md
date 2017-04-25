# Reporting API for Publishers v.1.0.3

### v1.0.3
Add the "region" request parameter. If you use “region” statistics, those data will be classified by countries.

usage:

region=all

The media statistics are returned as json lists, each containing a certain country / region information:

region=SPECIFIC_REGION_CODE

You can use this method to obtain specific country statistics. You can find the regoin code map with ISO_3166-3(https://zh.wikipedia.org/wiki/ISO_3166-3), and you can search the region code in https://countrycode.org/

### v1.0.2
Add USD($) currency type for reporting. you can add a "cur=usd" query param to get reporting currency type as USD, "cur" param is optional, default currency type is RMB. (using bid exchange rate from Yahoo! and updated by day, this number should be only used for reference, actual exchange rate may different.)

Add v2 version for reporting(revenue unit change to *1 rather than v1's *1000000) to use v2 version , query URL changed from /api/applications to /api_v2/applications.
    
### v1.0.1
Add hr section support for specify time info.

#### 1.
| Resource |  Param(s) | Description |
| ---------- | -----------: | :------------: |
|GET /api/applications |key=[API TOKEN]|	return all apps info|

```php
[
    {  
        "deverid": "62555", //Domob developer id
        "pubid" : "XXXXX", //publisher id  of the app
        "name" : "割绳子", //name of the app
        "platform" : "iOS", //platform
        "status" : "1" //app status，0 for running,  1 for paused
    },
    … …
]
```
#### 2.
|Request|Description|Remark|
| -----|-----|----|
|GET /api/applications/{PUBLISHER ID}?{key=API TOKEN}&{dt=yyyymmdd]}&[cur=usd]&[region=[all &brvbar; region code]]|return statistic data by dt| cur and region are optional parameters|
|GET /api/applications/{PUBLISHER ID}?{key=API TOKEN}&{dt=yyyymmdd}&{hr=hh}&[cur=usd]&[region=[all &brvbar; region code]]|return statistic data by dt and hr | cur and region are optional parameters|
|GET /api/applications/{PUBLISHER ID}?{key=API TOKEN}&{dt_start=yyyymmdd}&{dt_end=yyyymmdd}&[cur=usd]&[region=[all &brvbar; region code]]|return statistic data between dt_start and dt_end | cur and region are optional parameters|

##### Example
```php
{
    "date" : “2012-08-16",  //date of the data
    "request" : 7275,  //numbers of user's requests
    "imp_start" : 7201, //video start
    "imp_finish" : 7102, //video finish
    "eCPM" : 40.636, //eCPM in RMB
    "revenue" : 288,600,000,  //revenue*1000000 in RMB
    ......
}

{
    "date" : "012-08-16",  //date of the data
    "hr" : "12" , //time of the data
    "request" : 7275,  //numbers of user’s requests
    "imp_start" : 7201, //video start
    "imp_finish" : 7102, //video finish
    "eCPM": 40.636, //eCPM in RMB
    "revenue" : 288,600,000,  //revenue*1000000 in RMB
    ......
}

{
    "dt_start" : "2012-08-16",  //start date
    "dt_end": "2012-9-16",   //end date
    "request" : 14065,   //numbers of user’s requests
    "imp_start" : 13950,  //video start
    "imp_finish" : 13901, //video finish
    "eCPM": 43.062, //eCPM in RMB
    "revenue" : 598,600,000,  //revenue*1000000 in RMB
    ......
}
```

#### 3.
    1) Get all applications.
``` php
 bash: curl -X GET \
        -G 'http://dvx.domob.cn/api/applications' \
        -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm'

 return:
[
    {
        "deverid": "5...9",
        "pubid": "96Z...-edY/wTBSK",
        "name": "XXXX Run",
        "platform": 2,
        "status": 0
    },
    {
        "deverid": "5...9",
        "pubid": "96Z...zedY/wTPrJ",
        "name": "XXX Legend",
        "platform": 2,
        "status": 0
    },
    {
        "deverid": "5...9",
        "pubid": "96ZJ...edY/wTPrI",
        "name": "XXX Farm",
        "platform": 2,
        "status": 0
    },
    {
        "deverid": "5...9",
        "pubid": "96ZJ...edY/wTPrL",
        "name": "XXX 3D",
        "platform": 2,
        "status": 0
    }
]
```

    2) Get specific date statistics of one application.

```php
 bash: curl -X GET \
            -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
            -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
            -d 'dt=20160504'

return:
{
    "dt": "20160504",
    "request": "7504",
    "imp_start": "5835",
    "imp_finish": "4413",
    "clk": "140",
    "revenue": "264780000",
    "ecpm": 60
}
```
    3) Get specific date range statistics of one application.
```php
bash:curl -X GET \
          G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
          -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
          -d 'dt_start=20160504' \
          -d 'dt_end=20160604'

return:
{
    "dt_start": "20160504",
    "dt_end": "20160604",
    "request": "258869",
    "imp_start": "201774",
    "imp_finish": "152308",
    "clk": "7856",
    "revenue": "11115880000",
    "ecpm": 111.69
}
```
    4) Get specific date statistics of one application.
```php
bash: curl -X GET \
           -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
           -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
           -d 'dt=20160504' \
           -d 'hr=12'

return:
{
    "dt": "20160504",
    "hr": "12",
    "request": "7504",
    "imp_start": "5835",
    "imp_finish": "1213",
    "clk": "56",
    "revenue": "264780000",
    "ecpm": 72
}
```
    5) Using "cur=usd" when get report.
```php
bash: curl -X GET \
           -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
           -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
           -d 'dt=20160504' \
           -d 'hr=12' \
           -d 'cur=usd'

return:
{
    "dt": "20160504",
    "hr": "12",
    "request": "7504",
    "imp_start": "5835",
    "imp_finish": "1213",
    "clk": "56",
    "revenue": "38472534",
    "ecpm": 10.46
}
```

	6) Using "region=all" each object contain a certain country info：
```php
bash: curl -X GET \
            -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
            -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
            -d 'dt=20160401' \
            -d 'region=all'

return:

[
	{
		"dt":"20170401",
		"request":"1081428",
		"imp_start":"420",
		"imp_finish":"315",
		"clk":"83",
		"revenue":11912120,
		"ecpm":37.82, 
		"region" :cn
	},
	{
		"dt":"20170401",
		"request":"1081428",
		"imp_start":"420",
		"imp_finish":"315",
		"clk":"83",
		"revenue":11912120,
		"ecpm”:37.82,
		"region" :aq
	}
	......
]
```

	7) Using "region=SPECIFIC_REGION_CODE" you can get specific country statistics by this method.
```php
bash: curl -X GET \
            -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
            -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
            -d 'dt=20160401' \
            -d 'region=cn'

return:
{
	"dt":"20170401",
	"request":"1081428",
	"imp_start":"420",
	"imp_finish":"315",
	"clk":"83",
	"revenue":11912120,
	"ecpm":37.82, 
	"region" :cn
}

```
