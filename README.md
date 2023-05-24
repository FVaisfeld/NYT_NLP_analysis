
Open In Colab
New York Times headline regression analysis with LLM
The Archive API returns an array of New York Times (NYT) articles for a specific month, spanning all the way back to 1851. I aim to evaluate the potential of a large language model for regression tasks, specifically predicting the publication date solely based on the article headlines. The resulting model could be utilized for exploratory analysis, examining the evolution of language over time. For instance, it could be employed to investigate how historical events influence language patterns.


# let's get the data from the NYT api
import time 
yourkey = 'RAqO2c4Hdk6iFp0RoanDHPGzXppednzR'
months = range(1,2) # month range
years = range(1,101) #year range
first_year = 1922
for month in months:
  for y in years:
    
    year = first_year+y 
    date = f'{year}/{month*6}'
    url = f'https://api.nytimes.com/svc/archive/v1/{date}.json?api-key={yourkey}'
    !wget -O '{year}{month}.json' {url}
    time.sleep(10) # add some sleep for not running into api rate limits 

     
--2023-05-24 09:53:10--  https://api.nytimes.com/svc/archive/v1/1923/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 8701326 (8.3M) [application/json]
Saving to: ‘19231.json’

19231.json          100%[===================>]   8.30M  26.9MB/s    in 0.3s    

2023-05-24 09:53:10 (26.9 MB/s) - ‘19231.json’ saved [8701326/8701326]

--2023-05-24 09:53:20--  https://api.nytimes.com/svc/archive/v1/1924/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9214166 (8.8M) [application/json]
Saving to: ‘19241.json’

19241.json          100%[===================>]   8.79M  30.2MB/s    in 0.3s    

2023-05-24 09:53:21 (30.2 MB/s) - ‘19241.json’ saved [9214166/9214166]

--2023-05-24 09:53:31--  https://api.nytimes.com/svc/archive/v1/1925/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10134569 (9.7M) [application/json]
Saving to: ‘19251.json’

19251.json          100%[===================>]   9.67M  30.2MB/s    in 0.3s    

2023-05-24 09:53:32 (30.2 MB/s) - ‘19251.json’ saved [10134569/10134569]

--2023-05-24 09:53:42--  https://api.nytimes.com/svc/archive/v1/1926/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13117305 (13M) [application/json]
Saving to: ‘19261.json’

19261.json          100%[===================>]  12.51M  30.2MB/s    in 0.4s    

2023-05-24 09:53:42 (30.2 MB/s) - ‘19261.json’ saved [13117305/13117305]

--2023-05-24 09:53:53--  https://api.nytimes.com/svc/archive/v1/1927/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15954414 (15M) [application/json]
Saving to: ‘19271.json’

19271.json          100%[===================>]  15.21M  30.2MB/s    in 0.5s    

2023-05-24 09:53:53 (30.2 MB/s) - ‘19271.json’ saved [15954414/15954414]

--2023-05-24 09:54:03--  https://api.nytimes.com/svc/archive/v1/1928/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15464583 (15M) [application/json]
Saving to: ‘19281.json’

19281.json          100%[===================>]  14.75M  28.5MB/s    in 0.5s    

2023-05-24 09:54:04 (28.5 MB/s) - ‘19281.json’ saved [15464583/15464583]

--2023-05-24 09:54:14--  https://api.nytimes.com/svc/archive/v1/1929/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16037606 (15M) [application/json]
Saving to: ‘19291.json’

19291.json          100%[===================>]  15.29M  31.3MB/s    in 0.5s    

2023-05-24 09:54:15 (31.3 MB/s) - ‘19291.json’ saved [16037606/16037606]

--2023-05-24 09:54:25--  https://api.nytimes.com/svc/archive/v1/1930/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17015284 (16M) [application/json]
Saving to: ‘19301.json’

19301.json          100%[===================>]  16.23M  29.6MB/s    in 0.5s    

2023-05-24 09:54:26 (29.6 MB/s) - ‘19301.json’ saved [17015284/17015284]

--2023-05-24 09:54:36--  https://api.nytimes.com/svc/archive/v1/1931/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16553374 (16M) [application/json]
Saving to: ‘19311.json’

19311.json          100%[===================>]  15.79M  29.2MB/s    in 0.5s    

2023-05-24 09:54:37 (29.2 MB/s) - ‘19311.json’ saved [16553374/16553374]

--2023-05-24 09:54:47--  https://api.nytimes.com/svc/archive/v1/1932/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14376925 (14M) [application/json]
Saving to: ‘19321.json’

19321.json          100%[===================>]  13.71M  31.6MB/s    in 0.4s    

2023-05-24 09:54:48 (31.6 MB/s) - ‘19321.json’ saved [14376925/14376925]

--2023-05-24 09:54:58--  https://api.nytimes.com/svc/archive/v1/1933/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 09:54:58 ERROR 429: Too Many Requests.

--2023-05-24 09:55:08--  https://api.nytimes.com/svc/archive/v1/1934/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16784545 (16M) [application/json]
Saving to: ‘19341.json’

19341.json          100%[===================>]  16.01M  31.5MB/s    in 0.5s    

2023-05-24 09:55:09 (31.5 MB/s) - ‘19341.json’ saved [16784545/16784545]

--2023-05-24 09:55:19--  https://api.nytimes.com/svc/archive/v1/1935/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16083258 (15M) [application/json]
Saving to: ‘19351.json’

19351.json          100%[===================>]  15.34M  28.5MB/s    in 0.5s    

2023-05-24 09:55:20 (28.5 MB/s) - ‘19351.json’ saved [16083258/16083258]

--2023-05-24 09:55:30--  https://api.nytimes.com/svc/archive/v1/1936/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17266562 (16M) [application/json]
Saving to: ‘19361.json’

19361.json          100%[===================>]  16.47M  27.4MB/s    in 0.6s    

2023-05-24 09:55:31 (27.4 MB/s) - ‘19361.json’ saved [17266562/17266562]

--2023-05-24 09:55:41--  https://api.nytimes.com/svc/archive/v1/1937/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16608370 (16M) [application/json]
Saving to: ‘19371.json’

19371.json          100%[===================>]  15.84M  31.3MB/s    in 0.5s    

2023-05-24 09:55:42 (31.3 MB/s) - ‘19371.json’ saved [16608370/16608370]

--2023-05-24 09:55:52--  https://api.nytimes.com/svc/archive/v1/1938/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15677425 (15M) [application/json]
Saving to: ‘19381.json’

19381.json          100%[===================>]  14.95M  30.8MB/s    in 0.5s    

2023-05-24 09:55:53 (30.8 MB/s) - ‘19381.json’ saved [15677425/15677425]

--2023-05-24 09:56:03--  https://api.nytimes.com/svc/archive/v1/1939/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14896024 (14M) [application/json]
Saving to: ‘19391.json’

19391.json          100%[===================>]  14.21M  30.0MB/s    in 0.5s    

2023-05-24 09:56:03 (30.0 MB/s) - ‘19391.json’ saved [14896024/14896024]

--2023-05-24 09:56:13--  https://api.nytimes.com/svc/archive/v1/1940/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15707790 (15M) [application/json]
Saving to: ‘19401.json’

19401.json          100%[===================>]  14.98M  31.3MB/s    in 0.5s    

2023-05-24 09:56:14 (31.3 MB/s) - ‘19401.json’ saved [15707790/15707790]

--2023-05-24 09:56:24--  https://api.nytimes.com/svc/archive/v1/1941/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16276443 (16M) [application/json]
Saving to: ‘19411.json’

19411.json          100%[===================>]  15.52M  28.8MB/s    in 0.5s    

2023-05-24 09:56:25 (28.8 MB/s) - ‘19411.json’ saved [16276443/16276443]

--2023-05-24 09:56:35--  https://api.nytimes.com/svc/archive/v1/1942/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14401480 (14M) [application/json]
Saving to: ‘19421.json’

19421.json          100%[===================>]  13.73M  29.9MB/s    in 0.5s    

2023-05-24 09:56:36 (29.9 MB/s) - ‘19421.json’ saved [14401480/14401480]

--2023-05-24 09:56:46--  https://api.nytimes.com/svc/archive/v1/1943/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13033395 (12M) [application/json]
Saving to: ‘19431.json’

19431.json          100%[===================>]  12.43M  30.0MB/s    in 0.4s    

2023-05-24 09:56:47 (30.0 MB/s) - ‘19431.json’ saved [13033395/13033395]

--2023-05-24 09:56:57--  https://api.nytimes.com/svc/archive/v1/1944/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 09:56:57 ERROR 429: Too Many Requests.

--2023-05-24 09:57:07--  https://api.nytimes.com/svc/archive/v1/1945/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11675507 (11M) [application/json]
Saving to: ‘19451.json’

19451.json          100%[===================>]  11.13M  30.4MB/s    in 0.4s    

2023-05-24 09:57:08 (30.4 MB/s) - ‘19451.json’ saved [11675507/11675507]

--2023-05-24 09:57:18--  https://api.nytimes.com/svc/archive/v1/1946/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14420652 (14M) [application/json]
Saving to: ‘19461.json’

19461.json          100%[===================>]  13.75M  30.2MB/s    in 0.5s    

2023-05-24 09:57:19 (30.2 MB/s) - ‘19461.json’ saved [14420652/14420652]

--2023-05-24 09:57:29--  https://api.nytimes.com/svc/archive/v1/1947/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14973143 (14M) [application/json]
Saving to: ‘19471.json’

19471.json          100%[===================>]  14.28M  29.7MB/s    in 0.5s    

2023-05-24 09:57:30 (29.7 MB/s) - ‘19471.json’ saved [14973143/14973143]

--2023-05-24 09:57:40--  https://api.nytimes.com/svc/archive/v1/1948/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14262401 (14M) [application/json]
Saving to: ‘19481.json’

19481.json          100%[===================>]  13.60M  27.0MB/s    in 0.5s    

2023-05-24 09:57:40 (27.0 MB/s) - ‘19481.json’ saved [14262401/14262401]

--2023-05-24 09:57:50--  https://api.nytimes.com/svc/archive/v1/1949/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14470764 (14M) [application/json]
Saving to: ‘19491.json’

19491.json          100%[===================>]  13.80M  31.5MB/s    in 0.4s    

2023-05-24 09:57:51 (31.5 MB/s) - ‘19491.json’ saved [14470764/14470764]

--2023-05-24 09:58:01--  https://api.nytimes.com/svc/archive/v1/1950/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14669078 (14M) [application/json]
Saving to: ‘19501.json’

19501.json          100%[===================>]  13.99M  29.1MB/s    in 0.5s    

2023-05-24 09:58:02 (29.1 MB/s) - ‘19501.json’ saved [14669078/14669078]

--2023-05-24 09:58:12--  https://api.nytimes.com/svc/archive/v1/1951/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14615331 (14M) [application/json]
Saving to: ‘19511.json’

19511.json          100%[===================>]  13.94M  30.5MB/s    in 0.5s    

2023-05-24 09:58:13 (30.5 MB/s) - ‘19511.json’ saved [14615331/14615331]

--2023-05-24 09:58:23--  https://api.nytimes.com/svc/archive/v1/1952/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14534927 (14M) [application/json]
Saving to: ‘19521.json’

19521.json          100%[===================>]  13.86M  27.9MB/s    in 0.5s    

2023-05-24 09:58:23 (27.9 MB/s) - ‘19521.json’ saved [14534927/14534927]

--2023-05-24 09:58:34--  https://api.nytimes.com/svc/archive/v1/1953/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14081336 (13M) [application/json]
Saving to: ‘19531.json’

19531.json          100%[===================>]  13.43M  28.0MB/s    in 0.5s    

2023-05-24 09:58:34 (28.0 MB/s) - ‘19531.json’ saved [14081336/14081336]

--2023-05-24 09:58:44--  https://api.nytimes.com/svc/archive/v1/1954/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13101548 (12M) [application/json]
Saving to: ‘19541.json’

19541.json          100%[===================>]  12.49M  30.8MB/s    in 0.4s    

2023-05-24 09:58:45 (30.8 MB/s) - ‘19541.json’ saved [13101548/13101548]

--2023-05-24 09:58:55--  https://api.nytimes.com/svc/archive/v1/1955/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 09:58:55 ERROR 429: Too Many Requests.

--2023-05-24 09:59:05--  https://api.nytimes.com/svc/archive/v1/1956/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13635427 (13M) [application/json]
Saving to: ‘19561.json’

19561.json          100%[===================>]  13.00M  29.8MB/s    in 0.4s    

2023-05-24 09:59:06 (29.8 MB/s) - ‘19561.json’ saved [13635427/13635427]

--2023-05-24 09:59:16--  https://api.nytimes.com/svc/archive/v1/1957/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12999314 (12M) [application/json]
Saving to: ‘19571.json’

19571.json          100%[===================>]  12.40M  30.2MB/s    in 0.4s    

2023-05-24 09:59:17 (30.2 MB/s) - ‘19571.json’ saved [12999314/12999314]

--2023-05-24 09:59:27--  https://api.nytimes.com/svc/archive/v1/1958/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13311484 (13M) [application/json]
Saving to: ‘19581.json’

19581.json          100%[===================>]  12.69M  28.8MB/s    in 0.4s    

2023-05-24 09:59:28 (28.8 MB/s) - ‘19581.json’ saved [13311484/13311484]

--2023-05-24 09:59:38--  https://api.nytimes.com/svc/archive/v1/1959/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12970209 (12M) [application/json]
Saving to: ‘19591.json’

19591.json          100%[===================>]  12.37M  27.1MB/s    in 0.5s    

2023-05-24 09:59:39 (27.1 MB/s) - ‘19591.json’ saved [12970209/12970209]

--2023-05-24 09:59:49--  https://api.nytimes.com/svc/archive/v1/1960/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13159859 (13M) [application/json]
Saving to: ‘19601.json’

19601.json          100%[===================>]  12.55M  31.0MB/s    in 0.4s    

2023-05-24 09:59:49 (31.0 MB/s) - ‘19601.json’ saved [13159859/13159859]

--2023-05-24 10:00:00--  https://api.nytimes.com/svc/archive/v1/1961/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12585665 (12M) [application/json]
Saving to: ‘19611.json’

19611.json          100%[===================>]  12.00M  27.6MB/s    in 0.4s    

2023-05-24 10:00:00 (27.6 MB/s) - ‘19611.json’ saved [12585665/12585665]

--2023-05-24 10:00:10--  https://api.nytimes.com/svc/archive/v1/1962/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12648485 (12M) [application/json]
Saving to: ‘19621.json’

19621.json          100%[===================>]  12.06M  30.3MB/s    in 0.4s    

2023-05-24 10:00:11 (30.3 MB/s) - ‘19621.json’ saved [12648485/12648485]

--2023-05-24 10:00:21--  https://api.nytimes.com/svc/archive/v1/1963/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13350531 (13M) [application/json]
Saving to: ‘19631.json’

19631.json          100%[===================>]  12.73M  31.6MB/s    in 0.4s    

2023-05-24 10:00:22 (31.6 MB/s) - ‘19631.json’ saved [13350531/13350531]

--2023-05-24 10:00:32--  https://api.nytimes.com/svc/archive/v1/1964/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14794918 (14M) [application/json]
Saving to: ‘19641.json’

19641.json          100%[===================>]  14.11M  26.2MB/s    in 0.5s    

2023-05-24 10:00:33 (26.2 MB/s) - ‘19641.json’ saved [14794918/14794918]

--2023-05-24 10:00:43--  https://api.nytimes.com/svc/archive/v1/1965/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11738384 (11M) [application/json]
Saving to: ‘19651.json’

19651.json          100%[===================>]  11.19M  30.7MB/s    in 0.4s    

2023-05-24 10:00:43 (30.7 MB/s) - ‘19651.json’ saved [11738384/11738384]

--2023-05-24 10:00:53--  https://api.nytimes.com/svc/archive/v1/1966/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 10:00:54 ERROR 429: Too Many Requests.

--2023-05-24 10:01:04--  https://api.nytimes.com/svc/archive/v1/1967/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11574576 (11M) [application/json]
Saving to: ‘19671.json’

19671.json          100%[===================>]  11.04M  30.8MB/s    in 0.4s    

2023-05-24 10:01:04 (30.8 MB/s) - ‘19671.json’ saved [11574576/11574576]

--2023-05-24 10:01:14--  https://api.nytimes.com/svc/archive/v1/1968/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11619696 (11M) [application/json]
Saving to: ‘19681.json’

19681.json          100%[===================>]  11.08M  27.9MB/s    in 0.4s    

2023-05-24 10:01:15 (27.9 MB/s) - ‘19681.json’ saved [11619696/11619696]

--2023-05-24 10:01:25--  https://api.nytimes.com/svc/archive/v1/1969/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10111742 (9.6M) [application/json]
Saving to: ‘19691.json’

19691.json          100%[===================>]   9.64M  31.7MB/s    in 0.3s    

2023-05-24 10:01:26 (31.7 MB/s) - ‘19691.json’ saved [10111742/10111742]

--2023-05-24 10:01:36--  https://api.nytimes.com/svc/archive/v1/1970/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12161288 (12M) [application/json]
Saving to: ‘19701.json’

19701.json          100%[===================>]  11.60M  29.1MB/s    in 0.4s    

2023-05-24 10:01:37 (29.1 MB/s) - ‘19701.json’ saved [12161288/12161288]

--2023-05-24 10:01:47--  https://api.nytimes.com/svc/archive/v1/1971/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11644530 (11M) [application/json]
Saving to: ‘19711.json’

19711.json          100%[===================>]  11.10M  30.7MB/s    in 0.4s    

2023-05-24 10:01:48 (30.7 MB/s) - ‘19711.json’ saved [11644530/11644530]

--2023-05-24 10:01:58--  https://api.nytimes.com/svc/archive/v1/1972/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 10:01:58 ERROR 429: Too Many Requests.

--2023-05-24 10:02:08--  https://api.nytimes.com/svc/archive/v1/1973/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13179387 (13M) [application/json]
Saving to: ‘19731.json’

19731.json          100%[===================>]  12.57M  30.3MB/s    in 0.4s    

2023-05-24 10:02:09 (30.3 MB/s) - ‘19731.json’ saved [13179387/13179387]

--2023-05-24 10:02:19--  https://api.nytimes.com/svc/archive/v1/1974/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12464736 (12M) [application/json]
Saving to: ‘19741.json’

19741.json          100%[===================>]  11.89M  28.3MB/s    in 0.4s    

2023-05-24 10:02:19 (28.3 MB/s) - ‘19741.json’ saved [12464736/12464736]

--2023-05-24 10:02:29--  https://api.nytimes.com/svc/archive/v1/1975/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12561817 (12M) [application/json]
Saving to: ‘19751.json’

19751.json          100%[===================>]  11.98M  29.6MB/s    in 0.4s    

2023-05-24 10:02:30 (29.6 MB/s) - ‘19751.json’ saved [12561817/12561817]

--2023-05-24 10:02:40--  https://api.nytimes.com/svc/archive/v1/1976/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11986172 (11M) [application/json]
Saving to: ‘19761.json’

19761.json          100%[===================>]  11.43M  30.2MB/s    in 0.4s    

2023-05-24 10:02:41 (30.2 MB/s) - ‘19761.json’ saved [11986172/11986172]

--2023-05-24 10:02:51--  https://api.nytimes.com/svc/archive/v1/1977/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10949645 (10M) [application/json]
Saving to: ‘19771.json’

19771.json          100%[===================>]  10.44M  27.3MB/s    in 0.4s    

2023-05-24 10:02:52 (27.3 MB/s) - ‘19771.json’ saved [10949645/10949645]

--2023-05-24 10:03:02--  https://api.nytimes.com/svc/archive/v1/1978/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11312359 (11M) [application/json]
Saving to: ‘19781.json’

19781.json          100%[===================>]  10.79M  30.3MB/s    in 0.4s    

2023-05-24 10:03:02 (30.3 MB/s) - ‘19781.json’ saved [11312359/11312359]

--2023-05-24 10:03:13--  https://api.nytimes.com/svc/archive/v1/1979/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10526730 (10M) [application/json]
Saving to: ‘19791.json’

19791.json          100%[===================>]  10.04M  28.1MB/s    in 0.4s    

2023-05-24 10:03:13 (28.1 MB/s) - ‘19791.json’ saved [10526730/10526730]

--2023-05-24 10:03:23--  https://api.nytimes.com/svc/archive/v1/1980/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7505954 (7.2M) [application/json]
Saving to: ‘19801.json’

19801.json          100%[===================>]   7.16M  30.0MB/s    in 0.2s    

2023-05-24 10:03:24 (30.0 MB/s) - ‘19801.json’ saved [7505954/7505954]

--2023-05-24 10:03:34--  https://api.nytimes.com/svc/archive/v1/1981/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15534439 (15M) [application/json]
Saving to: ‘19811.json’

19811.json          100%[===================>]  14.81M  28.5MB/s    in 0.5s    

2023-05-24 10:03:35 (28.5 MB/s) - ‘19811.json’ saved [15534439/15534439]

--2023-05-24 10:03:45--  https://api.nytimes.com/svc/archive/v1/1982/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14825513 (14M) [application/json]
Saving to: ‘19821.json’

19821.json          100%[===================>]  14.14M  28.9MB/s    in 0.5s    

2023-05-24 10:03:45 (28.9 MB/s) - ‘19821.json’ saved [14825513/14825513]

--2023-05-24 10:03:56--  https://api.nytimes.com/svc/archive/v1/1983/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 10:03:56 ERROR 429: Too Many Requests.

--2023-05-24 10:04:06--  https://api.nytimes.com/svc/archive/v1/1984/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15024768 (14M) [application/json]
Saving to: ‘19841.json’

19841.json          100%[===================>]  14.33M  30.4MB/s    in 0.5s    

2023-05-24 10:04:07 (30.4 MB/s) - ‘19841.json’ saved [15024768/15024768]

--2023-05-24 10:04:17--  https://api.nytimes.com/svc/archive/v1/1985/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16335313 (16M) [application/json]
Saving to: ‘19851.json’

19851.json          100%[===================>]  15.58M  30.7MB/s    in 0.5s    

2023-05-24 10:04:18 (30.7 MB/s) - ‘19851.json’ saved [16335313/16335313]

--2023-05-24 10:04:28--  https://api.nytimes.com/svc/archive/v1/1986/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17212780 (16M) [application/json]
Saving to: ‘19861.json’

19861.json          100%[===================>]  16.42M  31.3MB/s    in 0.5s    

2023-05-24 10:04:29 (31.3 MB/s) - ‘19861.json’ saved [17212780/17212780]

--2023-05-24 10:04:39--  https://api.nytimes.com/svc/archive/v1/1987/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13694463 (13M) [application/json]
Saving to: ‘19871.json’

19871.json          100%[===================>]  13.06M  29.5MB/s    in 0.4s    

2023-05-24 10:04:39 (29.5 MB/s) - ‘19871.json’ saved [13694463/13694463]

--2023-05-24 10:04:49--  https://api.nytimes.com/svc/archive/v1/1988/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13744343 (13M) [application/json]
Saving to: ‘19881.json’

19881.json          100%[===================>]  13.11M  30.6MB/s    in 0.4s    

2023-05-24 10:04:50 (30.6 MB/s) - ‘19881.json’ saved [13744343/13744343]

--2023-05-24 10:05:00--  https://api.nytimes.com/svc/archive/v1/1989/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13423673 (13M) [application/json]
Saving to: ‘19891.json’

19891.json          100%[===================>]  12.80M  25.5MB/s    in 0.5s    

2023-05-24 10:05:01 (25.5 MB/s) - ‘19891.json’ saved [13423673/13423673]

--2023-05-24 10:05:11--  https://api.nytimes.com/svc/archive/v1/1990/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12012703 (11M) [application/json]
Saving to: ‘19901.json’

19901.json          100%[===================>]  11.46M  30.2MB/s    in 0.4s    

2023-05-24 10:05:12 (30.2 MB/s) - ‘19901.json’ saved [12012703/12012703]

--2023-05-24 10:05:22--  https://api.nytimes.com/svc/archive/v1/1991/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14602402 (14M) [application/json]
Saving to: ‘19911.json’

19911.json          100%[===================>]  13.93M  28.2MB/s    in 0.5s    

2023-05-24 10:05:23 (28.2 MB/s) - ‘19911.json’ saved [14602402/14602402]

--2023-05-24 10:05:33--  https://api.nytimes.com/svc/archive/v1/1992/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14166227 (14M) [application/json]
Saving to: ‘19921.json’

19921.json          100%[===================>]  13.51M  31.2MB/s    in 0.4s    

2023-05-24 10:05:33 (31.2 MB/s) - ‘19921.json’ saved [14166227/14166227]

--2023-05-24 10:05:44--  https://api.nytimes.com/svc/archive/v1/1993/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13354541 (13M) [application/json]
Saving to: ‘19931.json’

19931.json          100%[===================>]  12.74M  30.7MB/s    in 0.4s    

2023-05-24 10:05:44 (30.7 MB/s) - ‘19931.json’ saved [13354541/13354541]

--2023-05-24 10:05:54--  https://api.nytimes.com/svc/archive/v1/1994/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 10:05:54 ERROR 429: Too Many Requests.

--2023-05-24 10:06:05--  https://api.nytimes.com/svc/archive/v1/1995/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13636454 (13M) [application/json]
Saving to: ‘19951.json’

19951.json          100%[===================>]  13.00M  25.2MB/s    in 0.5s    

2023-05-24 10:06:05 (25.2 MB/s) - ‘19951.json’ saved [13636454/13636454]

--2023-05-24 10:06:15--  https://api.nytimes.com/svc/archive/v1/1996/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13873167 (13M) [application/json]
Saving to: ‘19961.json’

19961.json          100%[===================>]  13.23M  29.3MB/s    in 0.5s    

2023-05-24 10:06:16 (29.3 MB/s) - ‘19961.json’ saved [13873167/13873167]

--2023-05-24 10:06:26--  https://api.nytimes.com/svc/archive/v1/1997/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13778192 (13M) [application/json]
Saving to: ‘19971.json’

19971.json          100%[===================>]  13.14M  30.5MB/s    in 0.4s    

2023-05-24 10:06:27 (30.5 MB/s) - ‘19971.json’ saved [13778192/13778192]

--2023-05-24 10:06:37--  https://api.nytimes.com/svc/archive/v1/1998/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14635112 (14M) [application/json]
Saving to: ‘19981.json’

19981.json          100%[===================>]  13.96M  28.0MB/s    in 0.5s    

2023-05-24 10:06:38 (28.0 MB/s) - ‘19981.json’ saved [14635112/14635112]

--2023-05-24 10:06:48--  https://api.nytimes.com/svc/archive/v1/1999/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16197776 (15M) [application/json]
Saving to: ‘19991.json’

19991.json          100%[===================>]  15.45M  29.1MB/s    in 0.5s    

2023-05-24 10:06:49 (29.1 MB/s) - ‘19991.json’ saved [16197776/16197776]

--2023-05-24 10:06:59--  https://api.nytimes.com/svc/archive/v1/2000/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 10:06:59 ERROR 429: Too Many Requests.

--2023-05-24 10:07:09--  https://api.nytimes.com/svc/archive/v1/2001/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17690914 (17M) [application/json]
Saving to: ‘20011.json’

20011.json          100%[===================>]  16.87M  30.6MB/s    in 0.6s    

2023-05-24 10:07:10 (30.6 MB/s) - ‘20011.json’ saved [17690914/17690914]

--2023-05-24 10:07:20--  https://api.nytimes.com/svc/archive/v1/2002/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 18159964 (17M) [application/json]
Saving to: ‘20021.json’

20021.json          100%[===================>]  17.32M  30.0MB/s    in 0.6s    

2023-05-24 10:07:21 (30.0 MB/s) - ‘20021.json’ saved [18159964/18159964]

--2023-05-24 10:07:31--  https://api.nytimes.com/svc/archive/v1/2003/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17849434 (17M) [application/json]
Saving to: ‘20031.json’

20031.json          100%[===================>]  17.02M  30.1MB/s    in 0.6s    

2023-05-24 10:07:32 (30.1 MB/s) - ‘20031.json’ saved [17849434/17849434]

--2023-05-24 10:07:42--  https://api.nytimes.com/svc/archive/v1/2004/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16867714 (16M) [application/json]
Saving to: ‘20041.json’

20041.json          100%[===================>]  16.09M  30.1MB/s    in 0.5s    

2023-05-24 10:07:43 (30.1 MB/s) - ‘20041.json’ saved [16867714/16867714]

--2023-05-24 10:07:53--  https://api.nytimes.com/svc/archive/v1/2005/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 19797688 (19M) [application/json]
Saving to: ‘20051.json’

20051.json          100%[===================>]  18.88M  31.2MB/s    in 0.6s    

2023-05-24 10:07:54 (31.2 MB/s) - ‘20051.json’ saved [19797688/19797688]

--2023-05-24 10:08:04--  https://api.nytimes.com/svc/archive/v1/2006/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 29769945 (28M) [application/json]
Saving to: ‘20061.json’

20061.json          100%[===================>]  28.39M  30.0MB/s    in 0.9s    

2023-05-24 10:08:05 (30.0 MB/s) - ‘20061.json’ saved [29769945/29769945]

--2023-05-24 10:08:15--  https://api.nytimes.com/svc/archive/v1/2007/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14448502 (14M) [application/json]
Saving to: ‘20071.json’

20071.json          100%[===================>]  13.78M  29.7MB/s    in 0.5s    

2023-05-24 10:08:16 (29.7 MB/s) - ‘20071.json’ saved [14448502/14448502]

--2023-05-24 10:08:26--  https://api.nytimes.com/svc/archive/v1/2008/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15859788 (15M) [application/json]
Saving to: ‘20081.json’

20081.json          100%[===================>]  15.12M  27.1MB/s    in 0.6s    

2023-05-24 10:08:27 (27.1 MB/s) - ‘20081.json’ saved [15859788/15859788]

--2023-05-24 10:08:37--  https://api.nytimes.com/svc/archive/v1/2009/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 23391960 (22M) [application/json]
Saving to: ‘20091.json’

20091.json          100%[===================>]  22.31M  30.7MB/s    in 0.7s    

2023-05-24 10:08:38 (30.7 MB/s) - ‘20091.json’ saved [23391960/23391960]

--2023-05-24 10:08:48--  https://api.nytimes.com/svc/archive/v1/2010/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16633276 (16M) [application/json]
Saving to: ‘20101.json’

20101.json          100%[===================>]  15.86M  29.7MB/s    in 0.5s    

2023-05-24 10:08:49 (29.7 MB/s) - ‘20101.json’ saved [16633276/16633276]

--2023-05-24 10:08:59--  https://api.nytimes.com/svc/archive/v1/2011/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 10:08:59 ERROR 429: Too Many Requests.

--2023-05-24 10:09:09--  https://api.nytimes.com/svc/archive/v1/2012/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 19031410 (18M) [application/json]
Saving to: ‘20121.json’

20121.json          100%[===================>]  18.15M  30.7MB/s    in 0.6s    

2023-05-24 10:09:10 (30.7 MB/s) - ‘20121.json’ saved [19031410/19031410]

--2023-05-24 10:09:20--  https://api.nytimes.com/svc/archive/v1/2013/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 18438685 (18M) [application/json]
Saving to: ‘20131.json’

20131.json          100%[===================>]  17.58M  30.8MB/s    in 0.6s    

2023-05-24 10:09:21 (30.8 MB/s) - ‘20131.json’ saved [18438685/18438685]

--2023-05-24 10:09:31--  https://api.nytimes.com/svc/archive/v1/2014/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 19988874 (19M) [application/json]
Saving to: ‘20141.json’

20141.json          100%[===================>]  19.06M  30.4MB/s    in 0.6s    

2023-05-24 10:09:32 (30.4 MB/s) - ‘20141.json’ saved [19988874/19988874]

--2023-05-24 10:09:42--  https://api.nytimes.com/svc/archive/v1/2015/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 19649980 (19M) [application/json]
Saving to: ‘20151.json’

20151.json          100%[===================>]  18.74M  30.0MB/s    in 0.6s    

2023-05-24 10:09:43 (30.0 MB/s) - ‘20151.json’ saved [19649980/19649980]

--2023-05-24 10:09:53--  https://api.nytimes.com/svc/archive/v1/2016/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17563293 (17M) [application/json]
Saving to: ‘20161.json’

20161.json          100%[===================>]  16.75M  29.4MB/s    in 0.6s    

2023-05-24 10:09:54 (29.4 MB/s) - ‘20161.json’ saved [17563293/17563293]

--2023-05-24 10:10:04--  https://api.nytimes.com/svc/archive/v1/2017/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16239005 (15M) [application/json]
Saving to: ‘20171.json’

20171.json          100%[===================>]  15.49M  28.5MB/s    in 0.5s    

2023-05-24 10:10:05 (28.5 MB/s) - ‘20171.json’ saved [16239005/16239005]

--2023-05-24 10:10:15--  https://api.nytimes.com/svc/archive/v1/2018/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16256868 (16M) [application/json]
Saving to: ‘20181.json’

20181.json          100%[===================>]  15.50M  28.0MB/s    in 0.6s    

2023-05-24 10:10:16 (28.0 MB/s) - ‘20181.json’ saved [16256868/16256868]

--2023-05-24 10:10:26--  https://api.nytimes.com/svc/archive/v1/2019/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14807464 (14M) [application/json]
Saving to: ‘20191.json’

20191.json          100%[===================>]  14.12M  31.1MB/s    in 0.5s    

2023-05-24 10:10:27 (31.1 MB/s) - ‘20191.json’ saved [14807464/14807464]

--2023-05-24 10:10:37--  https://api.nytimes.com/svc/archive/v1/2020/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15168937 (14M) [application/json]
Saving to: ‘20201.json’

20201.json          100%[===================>]  14.47M  28.4MB/s    in 0.5s    

2023-05-24 10:10:38 (28.4 MB/s) - ‘20201.json’ saved [15168937/15168937]

--2023-05-24 10:10:48--  https://api.nytimes.com/svc/archive/v1/2021/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14436575 (14M) [application/json]
Saving to: ‘20211.json’

20211.json          100%[===================>]  13.77M  31.0MB/s    in 0.4s    

2023-05-24 10:10:49 (31.0 MB/s) - ‘20211.json’ saved [14436575/14436575]

--2023-05-24 10:10:59--  https://api.nytimes.com/svc/archive/v1/2022/6.json?api-key=RAqO2c4Hdk6iFp0RoanDHPGzXppednzR
Resolving api.nytimes.com (api.nytimes.com)... 34.73.242.132
Connecting to api.nytimes.com (api.nytimes.com)|34.73.242.132|:443... connected.
HTTP request sent, awaiting response... 429 Too Many Requests
2023-05-24 10:10:59 ERROR 429: Too Many Requests.


# open the json files and extract the date and headers into a list 
import json
headline = []
pub_date = []
for month in months:
  for y in years:  #range of the years 
    year = 1922+y
    try:
      with open(f'{year}{month}.json') as f:
          d = json.load(f)
          n_articles = len(d['response']['docs'])
          [ headline.append(d['response']['docs'][i]['headline']['main']) for i in range(n_articles) if len(d['response']['docs'][i]['headline']['main'])>0]
          [ pub_date.append(d['response']['docs'][i]['pub_date']) for i in range(n_articles) if len(d['response']['docs'][i]['headline']['main'])>0]
    except:
      continue


     

# example of headline 
print(headline[10])
print(len(headline))
     
SWISS LOSE AT TENNIS.; Czechoslovakia Wins Doubles Match in Davis Cup Play.
880722

# get some libraries for plotting 
import pandas as pd
import numpy as np
import seaborn as sns
import nltk
nltk.download('punkt')
nltk.download('stopwords')

     
[nltk_data] Downloading package punkt to /root/nltk_data...
[nltk_data]   Unzipping tokenizers/punkt.zip.
[nltk_data] Downloading package stopwords to /root/nltk_data...
[nltk_data]   Unzipping corpora/stopwords.zip.
True

# remove time from date since we only care about the day 
# fomat looks like this: '2004-01-16T14:18:00+0000'
for i,d in enumerate(pub_date):
  pub_date[i] = d[:d.find('T')]

pub_date[0]
     
'1923-06-01'

# concatenate headlines for same dates
import datetime

#let's sort the dates 
sorted_date = sorted(pub_date, key=lambda x: datetime.datetime.strptime(x, '%Y-%m-%d'))

#get difference dates
dates = sorted(set(sorted_date), key=lambda x: datetime.datetime.strptime(x, '%Y-%m-%d'))

# concatenate same dates
art2date = []
for d in dates:
  day_ind = [i for i, x in enumerate(sorted_date) if x == d]
  st = {"date": d, "indices": day_ind}
  art2date.append(st)  
     

# plot how many articles we have per date 
import matplotlib.pyplot as plt
n_articles = [len(a['indices']) for a in art2date]
x = np.arange(len(n_articles))
fig, ax = plt.subplots()
plt.plot(x, n_articles)
plt.xticks(x, dates, rotation=90)  # Set text labels and properties.
ax.locator_params(nbins=10, axis='x')
plt.title('amount of articles')
plt.xlabel('dates') 
plt.ylabel('n_articles')
plt.show()
     


# check min number of articles per date
print(f'minimum number of articles per date: {np.min(n_articles)}')
print(f'average number of articles per date: {np.mean(n_articles)}')

     
minimum number of articles per date: 4
average number of articles per date: 325.9518874907476

# merge articles per date 
headDate = []
for i,a in enumerate(art2date):
  headDate.append([headline[i] for i in a['indices'] if len(headline[i])>0])
     

headDate[0]
     
['FOR THE HONDURAN BARBER; American Finds the New York Product Savage in Comparison.',
 "WHITMAN'S FAME GROWING.; Out of 200 Writers Only Three Re- port Poet's Influence Waning.",
 'STRENGTH OF WHISKY NEED NOT BE PROVED; Conviction of Seven Men Up- held, Although Alcoholic Con- tent Was Not Shown.',
 'FOREST FIRES SUBSIDING.; Up-State, Michigan and Canadian Blazes Checked by Hard Fighting.',
 "HAYWARD LEADS 'DRYS'; Federal Attorney Tells Governor He Must Keep His Oath. THOMAS CHIEF WET SPEAKER Playwright Declares Minority Has Right to Resist Law Invading Rights. SMITH SILENT AT THE END But Drys' Showing in Crowded Meeting Leaves Repeal Advocates Shaky. Assembly Chamber Crowded as Governor Hears Wet and Dry Pleas G0V. SMITH HEARS WET AND DRY APPEAL",
 'POCAHONTAS SEARCH ANGERS ENGLISHMEN; Loud Protests Are Voiced Over Disturbing 50 Bodies in an Old Churchyard. WANT PROCEEDINGS HALTED Exhumed Bodies Are Reburled With Prayers, but Some Skulls Are Held for Examination.']

# check how long the concatenated headers per day in average are
# tokenization of the sentences
from nltk.tokenize import word_tokenize  
l_words = [len(word_tokenize(''.join(headers))) for headers in headDate]
x = np.arange(len(l_words))
fig, ax = plt.subplots()

plt.plot(x, np.array(l_words)/np.array(n_articles))
plt.xticks(x, dates,
       rotation=90)  # Set text labels and properties.

ax.locator_params(nbins=10, axis='x')
plt.title('average length of headers')
plt.xlabel('dates') 
plt.ylabel('n words')
plt.show()
     

Since the 1930s, the length of headers has generally decreased with a few exceptions until around 2013. However, since then, there appears to have been a continuous increase in header length.


# for creating a word cloud remove of the irrelevant stopwords and punctuation 
import string
string.punctuation
j_headline = ' '.join(headline)
tokens = word_tokenize(j_headline)
tokens = [char for char in tokens if char not in string.punctuation]
from nltk.corpus import stopwords
# remove stopwords
stop = stopwords.words('english')
tokens = [token for token in tokens if token not in stop]
# remove words less than three letters
tokens = [word for word in tokens if len(word) >= 3]
# remove article and title from tokens
tokens = [word for word in tokens if word not in ['Article', 'Title']]

     

print(tokens[:10])
     
['FOR', 'THE', 'HONDURAN', 'BARBER', 'American', 'Finds', 'New', 'York', 'Product', 'Savage']

# creation of a word cloud of all headers over all years
!pip install WordCloud
from wordcloud import WordCloud
from PIL import Image
plt.figure(figsize=(20,20))
wc = WordCloud(max_font_size=50, max_words=100, background_color='white')
wordcloud_jan = wc.generate_from_text(' '.join(tokens))
plt.imshow(wordcloud_jan, interpolation='bilinear')
plt.title('word cloud of the most used words in headers')
plt.axis('off')
plt.show()
     
Looking in indexes: https://pypi.org/simple, https://us-python.pkg.dev/colab-wheels/public/simple/
Requirement already satisfied: WordCloud in /usr/local/lib/python3.10/dist-packages (1.8.2.2)
Requirement already satisfied: numpy>=1.6.1 in /usr/local/lib/python3.10/dist-packages (from WordCloud) (1.22.4)
Requirement already satisfied: pillow in /usr/local/lib/python3.10/dist-packages (from WordCloud) (8.4.0)
Requirement already satisfied: matplotlib in /usr/local/lib/python3.10/dist-packages (from WordCloud) (3.7.1)
Requirement already satisfied: contourpy>=1.0.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->WordCloud) (1.0.7)
Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.10/dist-packages (from matplotlib->WordCloud) (0.11.0)
Requirement already satisfied: fonttools>=4.22.0 in /usr/local/lib/python3.10/dist-packages (from matplotlib->WordCloud) (4.39.3)
Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->WordCloud) (1.4.4)
Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.10/dist-packages (from matplotlib->WordCloud) (23.1)
Requirement already satisfied: pyparsing>=2.3.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->WordCloud) (3.0.9)
Requirement already satisfied: python-dateutil>=2.7 in /usr/local/lib/python3.10/dist-packages (from matplotlib->WordCloud) (2.8.2)
Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.10/dist-packages (from python-dateutil>=2.7->matplotlib->WordCloud) (1.16.0)


# Usage of an embedding to make the sentences interpretable for a transfomrer model
!pip install transformers
from transformers import BertTokenizer as Tokenizer
tokenizer = Tokenizer.from_pretrained("bert-base-uncased")

maximum_length = 300 #here define number of input tokens
all_encoded_corpus = []

# iterate and tokenize
for i,h in enumerate(headDate):
  encoded_corpus = tokenizer(text=h,
                              add_special_tokens=True,
                              padding='max_length',
                              truncation='longest_first',
                              max_length=maximum_length,
                              return_attention_mask=True)

  all_encoded_corpus.append(encoded_corpus)
  
     
Looking in indexes: https://pypi.org/simple, https://us-python.pkg.dev/colab-wheels/public/simple/
Collecting transformers
  Downloading transformers-4.29.2-py3-none-any.whl (7.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.1/7.1 MB 53.9 MB/s eta 0:00:00
Requirement already satisfied: filelock in /usr/local/lib/python3.10/dist-packages (from transformers) (3.12.0)
Collecting huggingface-hub<1.0,>=0.14.1 (from transformers)
  Downloading huggingface_hub-0.14.1-py3-none-any.whl (224 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 224.5/224.5 kB 31.0 MB/s eta 0:00:00
Requirement already satisfied: numpy>=1.17 in /usr/local/lib/python3.10/dist-packages (from transformers) (1.22.4)
Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.10/dist-packages (from transformers) (23.1)
Requirement already satisfied: pyyaml>=5.1 in /usr/local/lib/python3.10/dist-packages (from transformers) (6.0)
Requirement already satisfied: regex!=2019.12.17 in /usr/local/lib/python3.10/dist-packages (from transformers) (2022.10.31)
Requirement already satisfied: requests in /usr/local/lib/python3.10/dist-packages (from transformers) (2.27.1)
Collecting tokenizers!=0.11.3,<0.14,>=0.11.1 (from transformers)
  Downloading tokenizers-0.13.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (7.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.8/7.8 MB 104.8 MB/s eta 0:00:00
Requirement already satisfied: tqdm>=4.27 in /usr/local/lib/python3.10/dist-packages (from transformers) (4.65.0)
Requirement already satisfied: fsspec in /usr/local/lib/python3.10/dist-packages (from huggingface-hub<1.0,>=0.14.1->transformers) (2023.4.0)
Requirement already satisfied: typing-extensions>=3.7.4.3 in /usr/local/lib/python3.10/dist-packages (from huggingface-hub<1.0,>=0.14.1->transformers) (4.5.0)
Requirement already satisfied: urllib3<1.27,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests->transformers) (1.26.15)
Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests->transformers) (2022.12.7)
Requirement already satisfied: charset-normalizer~=2.0.0 in /usr/local/lib/python3.10/dist-packages (from requests->transformers) (2.0.12)
Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests->transformers) (3.4)
Installing collected packages: tokenizers, huggingface-hub, transformers
Successfully installed huggingface-hub-0.14.1 tokenizers-0.13.3 transformers-4.29.2
Downloading (…)solve/main/vocab.txt:   0%|          | 0.00/232k [00:00<?, ?B/s]
Downloading (…)okenizer_config.json:   0%|          | 0.00/28.0 [00:00<?, ?B/s]
Downloading (…)lve/main/config.json:   0%|          | 0.00/570 [00:00<?, ?B/s]

# the Bert model uses at the beginning the CLS token, between sentences the SEP token and at the end the PAD tokens
spezial_token = ['[CLS]', '[SEP]', '[PAD]']
CLS = tokenizer(spezial_token[0],add_special_tokens=False)['input_ids']
SEP = tokenizer(spezial_token[1],add_special_tokens=False)['input_ids']
PAD = tokenizer(spezial_token[2],add_special_tokens=False)['input_ids']
print(f'[CLS] token: {CLS}')
print(f'[SEP] token: {SEP}')
print(f'[PAD] token: {PAD}')
     
[CLS] token: [101]
[SEP] token: [102]
[PAD] token: [0]

# Concatenate multiple headers of the same date together as the later input for the transformer 

n_headers = 7 #number of headers per date
total_length = maximum_length #number of tokens input
min_articles_per_date = 50 #number of articles per date that will be used. The same amount is important for avoiding a bias towards dates with more headers. 
conc_corpus = []
conc_attention_mask = []
inputs_per_date = int(np.floor(min_articles_per_date/n_headers)) #number of trainable inputs per date
for _ in range(inputs_per_date): #iterate to get multiple inputs per date
  for dd, day_headers in enumerate(all_encoded_corpus):
      l_headers = len(day_headers['input_ids']) #n_headers per day
      ind = np.random.randint(0, l_headers ,n_headers)
      conc_headers = []
      for i in ind:
        zind = np.where(np.array(day_headers['input_ids'][i]) == 0) #find end == first 0
        try: 
          zind[0][0] #if there is no 0, then skip this one
        except:
          continue
        while float(zind[0][0])>float(maximum_length/n_headers): #check that header are not too long to fit together as input
          ind = np.random.randint(0, l_headers ,1)
          zind = np.where(np.array(day_headers['input_ids'][ind[0]]) == 0) #find end == first 0
        conc_headers = np.concatenate((conc_headers, day_headers['input_ids'][i][1:zind[0][0]]))
      conc_headers = np.concatenate((conc_headers[:-1], np.zeros(total_length-len(conc_headers[:-1]))))
      eind = np.where(conc_headers == 0) #find where last header ends
      attention_mask = np.zeros(total_length) 
      attention_mask[:eind[0][n_headers-1]] = 1
      conc_corpus.append(conc_headers)
      conc_attention_mask.append(attention_mask)


conc_corpus = np.array(conc_corpus).tolist()
conc_attention_mask = np.array(conc_attention_mask).tolist()

print(len(conc_corpus))
print(len(conc_attention_mask))

     
18914
18914

print('This is how an embedded input looks like: \n')
print(conc_corpus[1])
print('This is how the mask looks like: \n')
print(conc_attention_mask[1])

     
This is how an embedded input looks like: 

[2496.0, 10234.0, 28619.0, 3736.0, 6155.0, 1012.0, 1025.0, 3335.0, 6249.0, 14434.0, 1998.0, 1044.0, 1012.0, 3779.0, 1997.0, 25540.0, 26580.0, 21981.0, 2012.0, 6440.0, 1012.0, 102.0, 2496.0, 10234.0, 28619.0, 3736.0, 6155.0, 1012.0, 1025.0, 3335.0, 6249.0, 14434.0, 1998.0, 1044.0, 1012.0, 3779.0, 1997.0, 25540.0, 26580.0, 21981.0, 2012.0, 6440.0, 1012.0, 102.0, 2496.0, 10234.0, 28619.0, 3736.0, 6155.0, 1012.0, 1025.0, 3335.0, 6249.0, 14434.0, 1998.0, 1044.0, 1012.0, 3779.0, 1997.0, 25540.0, 26580.0, 21981.0, 2012.0, 6440.0, 1012.0, 102.0, 2392.0, 3931.0, 1016.0, 1011.0, 1011.0, 2053.0, 2516.0, 102.0, 13730.0, 16356.0, 2545.0, 1999.0, 3190.0, 1012.0, 1025.0, 6278.0, 2036.0, 4152.0, 3006.0, 1005.0, 1055.0, 2034.0, 2326.0, 2648.0, 2047.0, 2259.0, 1012.0, 102.0, 13730.0, 16356.0, 2545.0, 1999.0, 3190.0, 1012.0, 1025.0, 6278.0, 2036.0, 4152.0, 3006.0, 1005.0, 1055.0, 2034.0, 2326.0, 2648.0, 2047.0, 2259.0, 1012.0, 102.0, 3720.0, 1018.0, 1011.0, 1011.0, 2053.0, 2516.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
This is how the mask looks like: 

[1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]

print(headDate[0][:2])
print(encoded_corpus['input_ids'])
print(len(encoded_corpus['input_ids'][0]))
     
['FOR THE HONDURAN BARBER; American Finds the New York Product Savage in Comparison.', "WHITMAN'S FAME GROWING.; Out of 200 Writers Only Three Re- port Poet's Influence Waning."]
[[101, 1059, 1012, 1044, 1012, 1051, 1012, 23876, 29361, 2058, 7160, 8349, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 28513, 1521, 3021, 22264, 10887, 9891, 3086, 2000, 4424, 6101, 19238, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 11562, 26068, 2102, 5016, 2041, 10024, 2378, 6764, 2005, 2019, 1045, 1012, 1052, 1012, 1051, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2311, 28421, 2426, 1018, 4727, 1999, 10611, 2543, 2012, 4639, 2729, 2188, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2053, 24265, 2005, 2280, 4108, 28224, 2040, 2915, 1998, 2730, 1037, 2304, 5013, 2923, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 16837, 1998, 1037, 16371, 11818, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2698, 9252, 15516, 1521, 3319, 1024, 6620, 1998, 6536, 5613, 2369, 3221, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 20563, 3508, 1997, 1996, 2154, 1024, 2859, 23394, 2489, 22920, 2075, 4291, 4290, 2000, 2049, 2097, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2160, 14379, 9268, 8055, 11342, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 20983, 1024, 2238, 2382, 1010, 25682, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2167, 4420, 4311, 1037, 1520, 2307, 5325, 1521, 1999, 2049, 7865, 3433, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2129, 2000, 2994, 4658, 1998, 3647, 1999, 1037, 3684, 4400, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2009, 1521, 1055, 2204, 2000, 2022, 17963, 4027, 1010, 1999, 1996, 1050, 1012, 1038, 1012, 1037, 1012, 1998, 1999, 3598, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2115, 9317, 27918, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 2381, 1997, 1037, 14412, 10450, 2389, 3309, 1998, 2049, 3297, 6368, 1010, 2013, 5465, 1998, 16794, 2000, 11274, 1998, 9736, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2642, 3163, 2003, 2746, 2000, 2019, 2203, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 7349, 8499, 15288, 2004, 14200, 9466, 1998, 4584, 5981, 2925, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2024, 2017, 14436, 2075, 1037, 2709, 2000, 1520, 3671, 1521, 1029, 2017, 1521, 2128, 2025, 2894, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2723, 2011, 2723, 1010, 1996, 4394, 2111, 1998, 2439, 3268, 2379, 5631, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2144, 2043, 2031, 3628, 5839, 2069, 2005, 4138, 4841, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2057, 1521, 2222, 2131, 2247, 1529, 2043, 14695, 4875, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2003, 4419, 2739, 2428, 2035, 2008, 3928, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 2190, 2126, 2000, 5660, 11546, 1024, 2659, 1998, 4030, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2054, 2079, 16080, 1010, 3598, 1998, 8101, 2031, 1999, 2691, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2000, 4468, 2770, 6441, 1010, 2123, 1521, 1056, 6073, 2039, 2115, 9410, 2205, 2172, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 6203, 3003, 1024, 1037, 2379, 1011, 3819, 3661, 2013, 1037, 8398, 25353, 3597, 21890, 3372, 1010, 5754, 17287, 3064, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2047, 2259, 2103, 2003, 1037, 2088, 19662, 2993, 1012, 1521, 2021, 2009, 2089, 2425, 2149, 2073, 8037, 2024, 3753, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2043, 2508, 10970, 2001, 1037, 1520, 2038, 1011, 2042, 1521, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2043, 1996, 3606, 1010, 1998, 2026, 18087, 1011, 2000, 1011, 2022, 1010, 2768, 6248, 2077, 2033, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2007, 1520, 2621, 1997, 3969, 1010, 1521, 8795, 14301, 2063, 4122, 2000, 6039, 1037, 3451, 11675, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2478, 1037, 1012, 1045, 1012, 2000, 2424, 13827, 1999, 1037, 1012, 1045, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 13336, 1997, 1037, 13314, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2024, 2017, 14436, 2075, 1037, 2709, 2000, 1520, 3671, 1521, 1029, 2017, 1521, 2128, 2025, 2894, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2129, 2116, 4268, 2024, 2057, 2183, 2000, 4558, 1029, 1521, 2176, 27928, 3713, 2055, 1996, 2627, 2095, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 5522, 2758, 2009, 2003, 3201, 2005, 2522, 17258, 1011, 2539, 1012, 2054, 2055, 17932, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 5631, 1011, 2181, 2311, 7859, 5694, 1024, 2040, 2027, 2020, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 3462, 16742, 7462, 2014, 3117, 2006, 18901, 20619, 2015, 1012, 2009, 2165, 2125, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3517, 2000, 2022, 17183, 5397, 1010, 2900, 1521, 1055, 3057, 2227, 9561, 18608, 2000, 5188, 5544, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2859, 2038, 13763, 1012, 1521, 1998, 2009, 2003, 7501, 2005, 2971, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1045, 4339, 2055, 1996, 2375, 1012, 2021, 2071, 1045, 2428, 2393, 2489, 1037, 7267, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2129, 4080, 8675, 2253, 2013, 2392, 1011, 5479, 2000, 2959, 2173, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 5093, 7395, 1998, 27554, 4511, 1024, 2054, 2017, 4149, 2044, 1996, 2522, 17258, 2915, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 4539, 3573, 5494, 2015, 2265, 6537, 1997, 16393, 2000, 2304, 4279, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 6178, 3089, 1998, 4013, 6895, 2850, 1024, 1037, 6925, 1997, 2048, 3470, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 4291, 4290, 9667, 4036, 2166, 1521, 1055, 3574, 1012, 2085, 2002, 7879, 2493, 1999, 7173, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2542, 1999, 1012, 1012, 1012, 2899, 7535, 1010, 7128, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2899, 7535, 1024, 1996, 1520, 2197, 26829, 1997, 8984, 8010, 1521, 1999, 7128, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 7160, 8349, 1999, 1050, 1012, 1061, 1012, 1039, 1012, 1024, 2054, 2000, 2113, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2503, 1996, 1057, 1012, 1042, 1012, 1051, 1012, 3189, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 12188, 4019, 2013, 1996, 3016, 2181, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2027, 2357, 1996, 1520, 1057, 25394, 4355, 2448, 7698, 25805, 1521, 2046, 1037, 12188, 3959, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 3167, 2466, 1997, 3425, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2637, 1024, 1996, 4367, 3861, 1521, 3319, 1024, 1999, 10243, 2057, 3404, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 14076, 6265, 1024, 1996, 2162, 2003, 2196, 2058, 1521, 3319, 1024, 1037, 7196, 4013, 6767, 16280, 3126, 18094, 2015, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2044, 14419, 3766, 2003, 5229, 1010, 13411, 6985, 2015, 2457, 3785, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1059, 1012, 1044, 1012, 1051, 1012, 2758, 2859, 2038, 5892, 19132, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3901, 2040, 13377, 1996, 25805, 2020, 4986, 2005, 1996, 18551, 2027, 2187, 2369, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 2035, 1011, 2732, 2208, 1521, 1055, 3768, 1997, 2806, 2097, 2022, 2081, 2039, 1999, 9415, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 16080, 1011, 3141, 6441, 1998, 6677, 5598, 2076, 6090, 3207, 7712, 1010, 3189, 3065, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 2502, 1052, 1012, 1054, 1012, 3291, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2058, 1043, 1012, 1051, 1012, 1052, 1012, 4559, 1010, 1996, 2160, 3596, 1037, 7276, 2837, 2000, 8556, 1996, 5553, 1012, 1020, 11421, 2012, 1996, 9424, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2004, 1996, 7865, 7385, 2015, 1010, 22072, 13956, 12513, 2000, 2131, 1037, 1006, 2845, 1007, 17404, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2396, 2000, 15476, 3672, 2260, 13194, 5822, 2105, 1996, 2088, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2054, 1521, 1055, 2279, 2005, 29168, 13957, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 7226, 2368, 16393, 2015, 2976, 4681, 2004, 1037, 2501, 3684, 4400, 1998, 3748, 26332, 25223, 2140, 1996, 2225, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2735, 2115, 3042, 2046, 1037, 10516, 2873, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2054, 2017, 2131, 2005, 1002, 20317, 1010, 2199, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1002, 20317, 1010, 2199, 5014, 1999, 2047, 2259, 1010, 2047, 3933, 1998, 2662, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2024, 15806, 2746, 2067, 1029, 1996, 7160, 8349, 2038, 2070, 2367, 4584, 2128, 15222, 8950, 2075, 29361, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 8672, 17838, 1997, 1996, 13391, 2003, 5496, 1997, 6101, 2075, 1037, 2450, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 7996, 3689, 3632, 15413, 1011, 2489, 2007, 1002, 5018, 2454, 5592, 2013, 2585, 16216, 18032, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 2093, 1011, 11536, 2160, 2006, 1996, 7139, 3023, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2160, 5933, 2006, 2358, 1012, 12337, 1024, 22704, 1999, 1996, 6770, 2239, 4020, 2005, 1002, 1015, 1012, 1023, 2454, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2827, 4584, 2024, 4055, 2058, 1996, 2004, 6494, 10431, 19281, 17404, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2009, 19982, 7666, 1996, 3239, 1521, 1024, 3329, 6198, 1521, 1055, 3435, 5829, 8840, 5358, 2058, 3190, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 2110, 29466, 1012, 2097, 2053, 2936, 5478, 2966, 5491, 13946, 5907, 2005, 1057, 1012, 1055, 1012, 12293, 17362, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 1050, 1012, 1061, 1012, 1039, 1012, 3864, 2604, 2003, 1037, 7071, 1012, 2023, 2003, 1996, 2197, 13137, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 2047, 2126, 2000, 18651, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2317, 2006, 2317, 1521, 3319, 1024, 18636, 4871, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1999, 5631, 2181, 7859, 1010, 17659, 1997, 1023, 1013, 2340, 1999, 1996, 9940, 1998, 17538, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 12152, 24948, 2280, 6514, 4584, 1997, 6997, 1999, 17581, 5233, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2160, 2778, 1064, 29475, 3240, 3779, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2503, 29475, 3240, 3779, 1521, 1055, 29047, 3959, 2160, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2058, 3263, 2454, 2086, 3283, 1010, 3267, 2170, 1012, 2009, 2001, 2440, 1997, 14538, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3422, 2444, 1024, 3516, 25805, 7859, 10651, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 1520, 3313, 9346, 1521, 1024, 2339, 2070, 22437, 2111, 5998, 2007, 5177, 2740, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 5863, 9021, 2003, 16981, 1002, 3963, 2454, 2058, 22369, 6304, 1998, 2291, 2041, 13923, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 12436, 14693, 23854, 1998, 5457, 1029, 6998, 2055, 15806, 1010, 1996, 7160, 8349, 1998, 12687, 15245, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 7733, 2038, 13158, 2015, 1998, 1037, 1057, 1012, 1037, 1012, 1041, 1012, 12076, 4110, 2047, 4620, 1997, 2068, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 5347, 10509, 2000, 2604, 1997, 3864, 7561, 1024, 1520, 13718, 1010, 2009, 2003, 5263, 2000, 2022, 4527, 1012, 1521, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2292, 1521, 1055, 2377, 17974, 2674, 8571, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2154, 1997, 7385, 1024, 2019, 1999, 1011, 5995, 2298, 2012, 2129, 1037, 11240, 16201, 1996, 9424, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2503, 1996, 9424, 11421, 1024, 2019, 7262, 2678, 4812, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 24350, 2004, 2019, 2396, 2471, 2351, 2125, 1012, 2064, 9618, 4572, 2562, 2009, 4142, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2621, 2082, 2003, 2182, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 1050, 1012, 1061, 1012, 1039, 1012, 2604, 1997, 3864, 2038, 1037, 2146, 2381, 1997, 14154, 11563, 2015, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 8825, 2304, 3702, 13783, 2039, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2610, 6545, 2778, 2139, 2605, 5470, 2040, 2027, 2360, 3303, 5823, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3233, 1011, 11139, 2131, 6388, 1999, 2274, 29118, 2047, 19247, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1050, 1012, 1061, 1012, 1039, 1012, 6561, 3066, 2006, 2501, 1002, 5585, 4551, 5166, 2000, 4681, 6090, 3207, 7712, 7233, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 6733, 3192, 27791, 2062, 2084, 1002, 1016, 4551, 2000, 5907, 9945, 4073, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3725, 1998, 1996, 1041, 1012, 1057, 1012, 13366, 2121, 2895, 2006, 2642, 3163, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 7726, 4487, 11493, 14192, 3370, 3947, 2253, 2235, 2000, 2994, 2104, 2502, 6627, 1521, 1055, 7217, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 26002, 2006, 5934, 1024, 4760, 2149, 2129, 2116, 3268, 2057, 5383, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 9733, 2758, 1996, 2047, 1042, 1012, 1056, 1012, 1039, 1012, 3242, 1010, 27022, 4967, 1010, 2323, 28667, 8557, 2841, 2013, 9751, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2043, 3606, 11463, 5104, 2007, 4349, 1024, 2437, 1520, 1045, 4287, 2017, 2007, 2033, 1521, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2339, 1996, 4274, 2134, 1521, 1056, 14899, 2091, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 6677, 9997, 2004, 3684, 4400, 22953, 12146, 2530, 2710, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 1039, 1012, 1040, 1012, 1039, 1012, 2472, 2128, 10354, 27972, 2015, 2008, 12436, 14693, 23854, 2111, 1999, 1996, 1057, 1012, 1055, 1012, 2123, 1521, 1056, 2342, 15806, 1999, 2087, 8146, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2106, 2072, 1010, 1996, 2822, 4536, 1011, 16889, 2075, 5016, 1010, 3084, 2049, 2834, 2006, 2813, 2395, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3021, 2522, 14478, 10650, 2004, 2457, 2058, 22299, 2015, 2010, 3348, 6101, 10652, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2129, 2000, 4652, 16356, 15424, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 29168, 13957, 1521, 1055, 2269, 4455, 2005, 9934, 2046, 3220, 1521, 1055, 2491, 4447, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3883, 2040, 8733, 2308, 2005, 1050, 9048, 2615, 2213, 7331, 2000, 1017, 2086, 1999, 3827, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 4907, 15159, 2320, 21236, 2058, 2336, 1521, 1055, 2694, 1012, 2054, 3047, 1029, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 5343, 2869, 9033, 6199, 2083, 11385, 6575, 2005, 5694, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 28117, 23743, 2099, 17031, 2006, 1520, 1062, 6030, 1010, 1521, 4519, 1998, 2108, 2785, 2121, 2000, 2841, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2757, 4176, 9378, 16145, 1999, 5185, 7252, 2044, 2911, 14437, 2015, 12141, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 8292, 2571, 10024, 7062, 1998, 15121, 7685, 13407, 1006, 1998, 7592, 1007, 1999, 8603, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1520, 2057, 1521, 2128, 2652, 4608, 2039, 1010, 1521, 7226, 2368, 2758, 2006, 5081, 1997, 3748, 26332, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1017, 2396, 3916, 3065, 2000, 2156, 2157, 2085, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 4388, 5922, 6764, 1037, 9870, 2000, 7969, 2010, 2157, 2000, 4119, 1996, 2602, 3463, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2331, 9565, 9466, 2000, 2385, 1999, 3516, 2311, 7859, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2054, 1037, 2300, 15843, 2003, 2725, 2000, 2070, 1997, 2637, 1521, 1055, 2190, 16439, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 11530, 2225, 20279, 2098, 1998, 14217, 1012, 2016, 2036, 25590, 1037, 4446, 2057, 1521, 2128, 2145, 2206, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2019, 1050, 6199, 1997, 1996, 2088, 2898, 4773, 15187, 2005, 1002, 1019, 1012, 1018, 2454, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2054, 1521, 1055, 1999, 2256, 24240, 1029, 1520, 13323, 7856, 16446, 1521, 1998, 2062, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 14052, 8259, 2005, 12620, 5852, 7226, 2368, 1521, 1055, 2529, 2916, 3343, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 8037, 2031, 1037, 2095, 2000, 3828, 1996, 4774, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 7226, 2368, 4227, 2007, 8777, 1998, 4603, 6830, 2967, 1010, 2047, 2951, 3065, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2990, 22501, 1010, 1039, 1012, 1045, 1012, 1037, 1012, 2708, 1999, 3147, 2162, 15433, 1010, 8289, 2012, 3770, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 17889, 2884, 4059, 1010, 1520, 5268, 1521, 2005, 8398, 1010, 5344, 5571, 1998, 3231, 1997, 2010, 9721, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3188, 25090, 6774, 2111, 2000, 3857, 2067, 1996, 4610, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 5731, 2006, 7733, 4604, 2047, 5328, 2000, 3011, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 6221, 19379, 22747, 14273, 1010, 3639, 3187, 2104, 1016, 11274, 1010, 2003, 2757, 2012, 6070, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1037, 3025, 3820, 2007, 5503, 3459, 2953, 1010, 8398, 1521, 1055, 17727, 5243, 22729, 4905, 1010, 2003, 2012, 1996, 2540, 1997, 2522, 14478, 1521, 1055, 2713, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 1059, 1012, 1044, 1012, 1051, 1012, 4455, 2041, 16655, 26426, 3229, 2000, 28896, 2005, 3763, 2137, 1998, 7139, 3032, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 2012, 2188, 1998, 2185, 2621, 2377, 9863, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 4587, 2688, 1999, 2047, 2259, 2003, 9186, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2024, 1520, 3684, 15856, 1521, 1996, 3437, 2000, 3684, 5975, 1029, 2070, 3655, 2228, 2061, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2051, 2000, 2131, 7823, 2007, 1996, 4895, 24887, 28748, 3064, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2715, 2293, 16110, 1024, 2043, 2048, 2330, 12743, 8902, 24198, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 1996, 8398, 2808, 2024, 2746, 1012, 16091, 1996, 2162, 1997, 1996, 24962, 1012, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 8519, 5981, 8161, 1996, 4259, 2457, 1521, 1055, 2373, 2000, 4894, 2091, 4277, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 3021, 2522, 14478, 1521, 1055, 10652, 2003, 17068, 1024, 3191, 1996, 2457, 1521, 1055, 5448, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], [101, 2280, 6373, 2732, 7648, 21402, 5338, 2007, 6016, 13216, 7696, 2000, 1037, 3576, 102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]
300

# the output label (date) needs to be transformed into a continuous number 
# create a mapping from date to float that range from -1 to 1 for uniformly distributed labels

dtoi = { d:i for i,d in enumerate(dates) }
itod = { i:d for i,d in enumerate(dates) }
encode = lambda s: dtoi[s]/(0.5*len(dates))-1 # encoder: take a date, output a float scaled in the range from -1 to 1
decode = lambda l: itod[round((l+1)*0.5*len(dates))] # decoder: take a float scaled in the range from -1 to 1, output the corresponding date

print(f'date:  {dates[1]}')
print(f'encoded date:  {encode(dates[1])}')
print(f'decoded encoded date:  {decode(encode(dates[1]))}')
     
date:  1851-09-22
encoded date:  -0.999259807549963
decoded encoded date:  1851-09-22

# meake sure we have got uniformly distributed labels
labels = []
for _ in range(inputs_per_date):
  labels = labels + [encode(d) for d in dates]
fig1, ax1 = plt.subplots()
ax1.set_title('range labels')
plt.boxplot(labels)

assert len(conc_attention_mask)==len(labels)
print(f'num labels: {len(labels)}')

     
num labels: 18914


# use sklearn for the train test split 
from sklearn.model_selection import train_test_split

test_size = 0.1 #test size
seed = 42 #for reproduzability
train_inputs, test_inputs, train_labels, test_labels = \
            train_test_split(conc_corpus, labels, test_size=test_size, 
                             random_state=seed)
train_masks, test_masks, _, _ = train_test_split(conc_attention_mask, 
                                        labels, test_size=test_size, 
                                        random_state=seed)
     

# create torch data loaders for creating trainable batches of the inputs 
import torch
from torch.utils.data import TensorDataset, DataLoader

def create_dataloaders(inputs, masks, labels, batch_size):
    input_tensor = torch.tensor(inputs)
    mask_tensor = torch.tensor(masks)
    labels_tensor = torch.tensor(labels)
    dataset = TensorDataset(input_tensor, mask_tensor, 
                            labels_tensor)
    dataloader = DataLoader(dataset, batch_size=batch_size, 
                            shuffle=True)
    return dataloader
     

batch_size = 16 #batch size, this is hyperparameter that could be optimized
train_dataloader = create_dataloaders(train_inputs, train_masks, 
                                      train_labels, batch_size)

test_dataloader = create_dataloaders(test_inputs, test_masks, 
                                     test_labels, batch_size)
     
Model architecture
I contend that existing techniques place limitations on the potential of pre-trained representations, particularly in the case of fine-tuning approaches. The primary limitation arises from the unidirectional nature of standard language models, which constrains the choice of architectures during pre-training. For instance, in OpenAI GPT, the authors adopt a left-to-right architecture, where each token can only attend to preceding tokens in the self-attention layers of the Transformer (Vaswani et al., 2017). These restrictions prove suboptimal for sentence-level tasks and can be detrimental when applying fine-tuning to token-level tasks, such as question answering, where the incorporation of context from both directions is critical.

Now, let's delve into the actual architecture of the model. BERT consists of an embedding layer followed by 12 stacked transformers.

For every input sequence, BERT produces an output sequence of vectors of the same size. These vectors represent the final hidden states corresponding to each input token, with each vector comprising 768 floats.

The BERT paper specifies that only the final hidden state of the first token in the output sequence should be utilized for classification tasks. In other words, only the vector representing the "[CLS]" token mentioned earlier should be employed.

For the regression task, I will follow the same approach, but instead of adding a dense pooling layer for classification, I will introduce a dense linear layer with dropout. This layer will serve as our final regression layer.

bert with regression layer NY.png


# get the Bert model from the hugging face library and add a fully connected layer for regression to the CLS output together with a dropout layer  
import torch.nn as nn
from transformers import AutoModel

class BertRegressor(nn.Module):
    
    def __init__(self, drop_rate=0.2, freeze_bert=False):
        
        super(BertRegressor, self).__init__()
        D_in, D_out = 768, 1 #the first token of vector dim 768 is used for the regression task 
        
        self.bert = AutoModel.from_pretrained('distilbert-base-uncased')
                                                 
        self.regressor = nn.Sequential(
            nn.Dropout(drop_rate),
            nn.Linear(D_in, D_out))
        

    def forward(self, input_ids, attention_masks):
        
        outputs = self.bert(input_ids, attention_masks)
        outputs = torch.swapaxes(outputs[0], 0, 1) 
        class_label_output = outputs[0]
        outputs = self.regressor(class_label_output)
        return outputs
        

model = BertRegressor(drop_rate=0.2)
     
Downloading (…)lve/main/config.json:   0%|          | 0.00/483 [00:00<?, ?B/s]
Downloading pytorch_model.bin:   0%|          | 0.00/268M [00:00<?, ?B/s]
Some weights of the model checkpoint at distilbert-base-uncased were not used when initializing DistilBertModel: ['vocab_layer_norm.bias', 'vocab_transform.bias', 'vocab_projector.weight', 'vocab_transform.weight', 'vocab_layer_norm.weight', 'vocab_projector.bias']
- This IS expected if you are initializing DistilBertModel from the checkpoint of a model trained on another task or with another architecture (e.g. initializing a BertForSequenceClassification model from a BertForPreTraining model).
- This IS NOT expected if you are initializing DistilBertModel from the checkpoint of a model that you expect to be exactly identical (initializing a BertForSequenceClassification model from a BertForSequenceClassification model).

#bring the model to GPU 
import torch
if torch.cuda.is_available():       
    device = torch.device("cuda")
    print("Using GPU.")
else:
    print("No GPU available, using the CPU instead.")
    device = torch.device("cpu")
model.to(device)
     
Using GPU.
BertRegressor(
  (bert): DistilBertModel(
    (embeddings): Embeddings(
      (word_embeddings): Embedding(30522, 768, padding_idx=0)
      (position_embeddings): Embedding(512, 768)
      (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
      (dropout): Dropout(p=0.1, inplace=False)
    )
    (transformer): Transformer(
      (layer): ModuleList(
        (0-5): 6 x TransformerBlock(
          (attention): MultiHeadSelfAttention(
            (dropout): Dropout(p=0.1, inplace=False)
            (q_lin): Linear(in_features=768, out_features=768, bias=True)
            (k_lin): Linear(in_features=768, out_features=768, bias=True)
            (v_lin): Linear(in_features=768, out_features=768, bias=True)
            (out_lin): Linear(in_features=768, out_features=768, bias=True)
          )
          (sa_layer_norm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
          (ffn): FFN(
            (dropout): Dropout(p=0.1, inplace=False)
            (lin1): Linear(in_features=768, out_features=3072, bias=True)
            (lin2): Linear(in_features=3072, out_features=768, bias=True)
            (activation): GELUActivation()
          )
          (output_layer_norm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
        )
      )
    )
  )
  (regressor): Sequential(
    (0): Dropout(p=0.2, inplace=False)
    (1): Linear(in_features=768, out_features=1, bias=True)
  )
)

# choose optimizer and learning rate for fine tuning (recommended in Bert paper is 5e-5)
from transformers import AdamW
optimizer = AdamW(model.parameters(),
                  lr=2e-5, #5e-5, 3e-5, 2e-5
                  eps=1e-8)
     
/usr/local/lib/python3.10/dist-packages/transformers/optimization.py:407: FutureWarning: This implementation of AdamW is deprecated and will be removed in a future version. Use the PyTorch implementation torch.optim.AdamW instead, or set `no_deprecation_warning=True` to disable this warning
  warnings.warn(

# choose epochs and schedule a linear learning rate decrease for reducing the probability of overfitting 
from transformers import get_linear_schedule_with_warmup
epochs = 5 #recommended in paper is 1-2, but I see still improvment on the test data with more iterations
total_steps = len(train_dataloader) * epochs
scheduler = get_linear_schedule_with_warmup(optimizer,       
                 num_warmup_steps=0, num_training_steps=total_steps)
     

# define mse loss for the regression task in the loss function 
loss_function = nn.MSELoss()
     

# train the model 
from torch.nn.utils.clip_grad import clip_grad_norm 
import torch

def train(model, optimizer, scheduler, loss_function, epochs,       
          train_dataloader, device, clip_value=2):
    training_stats = []
    for epoch in range(epochs):
        print(epoch)
        print("-----")
        total_train_loss = 0
        model.train()
        # iterate batches
        for step, batch in enumerate(train_dataloader): 
            print(step)  
            batch_inputs, batch_masks, batch_labels = \
                               tuple(b.to(device) for b in batch)
            model.zero_grad()
            batch_inputs = batch_inputs.to(torch.int64)
            batch_masks = batch_masks.to(torch.int64)
            outputs = model(batch_inputs, batch_masks)
            loss = loss_function(outputs.T[0], 
                             batch_labels)
            #accumulate train loss
            total_train_loss += loss.item()
            print(f'train loss: {loss}')
            print("-----")

            #backpropagate train loss 
            loss.backward()
            clip_grad_norm(model.parameters(), clip_value) #mitigate the problem of exploding gradients 
            optimizer.step()
            scheduler.step()


        # Calculate the average loss over all of the batches.
        avg_train_loss = total_train_loss / len(train_dataloader)   
        print("")
        print("  Average training loss: {0:.2f}".format(avg_train_loss))


        # Put the model in evaluation mode--the dropout layers behave differently
        # during evaluation.
        model.eval()

        # Tracking variables 
        total_eval_accuracy = 0
        total_eval_loss = 0
        nb_eval_steps = 0

        # Evaluate data for one epoch
        for batch in test_dataloader:

          batch_inputs, batch_masks, batch_labels = \
                               tuple(b.to(device) for b in batch)
          batch_inputs = batch_inputs.to(torch.int64)
          batch_masks = batch_masks.to(torch.int64)
    
          # Tell pytorch not to bother with constructing the compute graph during
          # the forward pass, since this is only needed for backprop (training).
          with torch.no_grad():        

              outputs = model(batch_inputs, batch_masks)
              loss = loss_function(outputs.T[0],  batch_labels)
                             
              
          # Accumulate the validation loss.
          total_eval_loss += loss.item()


        # Calculate the average loss over all of the batches.
        avg_val_loss = total_eval_loss / len(test_dataloader)
        
        print("  Validation Loss: {0:.2f}\n".format(avg_val_loss))

        # Record all statistics from this epoch.
        training_stats.append(
            {
                'epoch': epoch + 1,
                'Training Loss': avg_train_loss,
                'Valid. Loss': avg_val_loss
            }
        )

                
    return model, training_stats
    
model, training_stats = train(model, optimizer, scheduler, loss_function, epochs, 
              train_dataloader, device, clip_value=2)
     
0
-----
0
train loss: 0.2702253758907318
-----
<ipython-input-30-943a67e0ea0d>:31: UserWarning: torch.nn.utils.clip_grad_norm is now deprecated in favor of torch.nn.utils.clip_grad_norm_.
  clip_grad_norm(model.parameters(), clip_value) #mitigate the problem of exploding gradients
Streaming output truncated to the last 5000 lines.
-----
465
train loss: 0.008334474638104439
-----
466
train loss: 0.007282600272446871
-----
467
train loss: 0.004850280471146107
-----
468
train loss: 0.00508474325761199
-----
469
train loss: 0.009127136319875717
-----
470
train loss: 0.005021387245506048
-----
471
train loss: 0.010758303105831146
-----
472
train loss: 0.004980510100722313
-----
473
train loss: 0.008560271933674812
-----
474
train loss: 0.005310220643877983
-----
475
train loss: 0.0068405079655349255
-----
476
train loss: 0.007973744533956051
-----
477
train loss: 0.006137901917099953
-----
478
train loss: 0.007264714688062668
-----
479
train loss: 0.00564879784360528
-----
480
train loss: 0.005210919305682182
-----
481
train loss: 0.002888960065320134
-----
482
train loss: 0.009908308275043964
-----
483
train loss: 0.006633973680436611
-----
484
train loss: 0.008472389541566372
-----
485
train loss: 0.00818187277764082
-----
486
train loss: 0.006839610170572996
-----
487
train loss: 0.0035204272717237473
-----
488
train loss: 0.007313713431358337
-----
489
train loss: 0.009770266711711884
-----
490
train loss: 0.0057448772713541985
-----
491
train loss: 0.006624434143304825
-----
492
train loss: 0.00809235405176878
-----
493
train loss: 0.005770695395767689
-----
494
train loss: 0.0033616777509450912
-----
495
train loss: 0.00436844676733017
-----
496
train loss: 0.0032788310199975967
-----
497
train loss: 0.005133694503456354
-----
498
train loss: 0.00372572080232203
-----
499
train loss: 0.0033139644656330347
-----
500
train loss: 0.00441938079893589
-----
501
train loss: 0.012784738093614578
-----
502
train loss: 0.005755459424108267
-----
503
train loss: 0.009386311285197735
-----
504
train loss: 0.0036879810504615307
-----
505
train loss: 0.007750052027404308
-----
506
train loss: 0.0032499469816684723
-----
507
train loss: 0.006663228385150433
-----
508
train loss: 0.006456353701651096
-----
509
train loss: 0.005685873795300722
-----
510
train loss: 0.010048838332295418
-----
511
train loss: 0.0026280616875737906
-----
512
train loss: 0.004966397769749165
-----
513
train loss: 0.004422544967383146
-----
514
train loss: 0.006278508342802525
-----
515
train loss: 0.00779702328145504
-----
516
train loss: 0.004338528029620647
-----
517
train loss: 0.00288205873221159
-----
518
train loss: 0.00443994952365756
-----
519
train loss: 0.004662707913666964
-----
520
train loss: 0.0076274629682302475
-----
521
train loss: 0.005681026726961136
-----
522
train loss: 0.0036782720126211643
-----
523
train loss: 0.006042521446943283
-----
524
train loss: 0.006352215074002743
-----
525
train loss: 0.007578066550195217
-----
526
train loss: 0.009589064866304398
-----
527
train loss: 0.00589017104357481
-----
528
train loss: 0.00574050098657608
-----
529
train loss: 0.00757010979577899
-----
530
train loss: 0.005824600346386433
-----
531
train loss: 0.018795285373926163
-----
532
train loss: 0.007790929637849331
-----
533
train loss: 0.0060150763019919395
-----
534
train loss: 0.01286730170249939
-----
535
train loss: 0.006304651033133268
-----
536
train loss: 0.005502466578036547
-----
537
train loss: 0.005509341601282358
-----
538
train loss: 0.012678774073719978
-----
539
train loss: 0.00527395261451602
-----
540
train loss: 0.005020469427108765
-----
541
train loss: 0.006353872828185558
-----
542
train loss: 0.004245336167514324
-----
543
train loss: 0.007705935277044773
-----
544
train loss: 0.009635034948587418
-----
545
train loss: 0.0161599088460207
-----
546
train loss: 0.007492723874747753
-----
547
train loss: 0.009685593657195568
-----
548
train loss: 0.004287128336727619
-----
549
train loss: 0.0012232218869030476
-----
550
train loss: 0.0046210698783397675
-----
551
train loss: 0.004275583196431398
-----
552
train loss: 0.008345525711774826
-----
553
train loss: 0.009025335311889648
-----
554
train loss: 0.0039585912600159645
-----
555
train loss: 0.006073798984289169
-----
556
train loss: 0.014258982613682747
-----
557
train loss: 0.011133793741464615
-----
558
train loss: 0.008444637060165405
-----
559
train loss: 0.006513976491987705
-----
560
train loss: 0.0075288680382072926
-----
561
train loss: 0.0033249473199248314
-----
562
train loss: 0.00467302231118083
-----
563
train loss: 0.008142145350575447
-----
564
train loss: 0.005898024421185255
-----
565
train loss: 0.01176268421113491
-----
566
train loss: 0.009251520968973637
-----
567
train loss: 0.009150780737400055
-----
568
train loss: 0.004067990463227034
-----
569
train loss: 0.005313621833920479
-----
570
train loss: 0.0069471606984734535
-----
571
train loss: 0.005011474713683128
-----
572
train loss: 0.0057424334809184074
-----
573
train loss: 0.004709710367023945
-----
574
train loss: 0.006053307093679905
-----
575
train loss: 0.009827902540564537
-----
576
train loss: 0.0035367170348763466
-----
577
train loss: 0.005992750637233257
-----
578
train loss: 0.0031318683177232742
-----
579
train loss: 0.008472641929984093
-----
580
train loss: 0.005299742799252272
-----
581
train loss: 0.006509518250823021
-----
582
train loss: 0.0038710806984454393
-----
583
train loss: 0.00513946358114481
-----
584
train loss: 0.00686996802687645
-----
585
train loss: 0.004578460939228535
-----
586
train loss: 0.006954505108296871
-----
587
train loss: 0.007447210606187582
-----
588
train loss: 0.005865554325282574
-----
589
train loss: 0.005370217841118574
-----
590
train loss: 0.01673668995499611
-----
591
train loss: 0.003731214441359043
-----
592
train loss: 0.004394450690597296
-----
593
train loss: 0.003776534926146269
-----
594
train loss: 0.004697614815086126
-----
595
train loss: 0.0031850249506533146
-----
596
train loss: 0.0044024307280778885
-----
597
train loss: 0.0036136838607490063
-----
598
train loss: 0.005324577912688255
-----
599
train loss: 0.004568242933601141
-----
600
train loss: 0.004265609662979841
-----
601
train loss: 0.003799277124926448
-----
602
train loss: 0.002345126587897539
-----
603
train loss: 0.006998459342867136
-----
604
train loss: 0.008434436284005642
-----
605
train loss: 0.004692976363003254
-----
606
train loss: 0.003658958012238145
-----
607
train loss: 0.012735548429191113
-----
608
train loss: 0.0065275453962385654
-----
609
train loss: 0.009850765578448772
-----
610
train loss: 0.003913531079888344
-----
611
train loss: 0.008587978780269623
-----
612
train loss: 0.004879072308540344
-----
613
train loss: 0.005292325746268034
-----
614
train loss: 0.005812286399304867
-----
615
train loss: 0.005305057391524315
-----
616
train loss: 0.00740412063896656
-----
617
train loss: 0.008885927498340607
-----
618
train loss: 0.005184418521821499
-----
619
train loss: 0.00324320700019598
-----
620
train loss: 0.009371702559292316
-----
621
train loss: 0.005668340250849724
-----
622
train loss: 0.016904858872294426
-----
623
train loss: 0.006812305189669132
-----
624
train loss: 0.004542458802461624
-----
625
train loss: 0.007068542297929525
-----
626
train loss: 0.003145129419863224
-----
627
train loss: 0.0063158199191093445
-----
628
train loss: 0.0012373580830171704
-----
629
train loss: 0.003546545747667551
-----
630
train loss: 0.005959782749414444
-----
631
train loss: 0.010830914601683617
-----
632
train loss: 0.0066871317103505135
-----
633
train loss: 0.005257456097751856
-----
634
train loss: 0.006887954659759998
-----
635
train loss: 0.004008344374597073
-----
636
train loss: 0.005398616194725037
-----
637
train loss: 0.007668203208595514
-----
638
train loss: 0.0043252198956906796
-----
639
train loss: 0.0018696966581046581
-----
640
train loss: 0.005945751443505287
-----
641
train loss: 0.007036302704364061
-----
642
train loss: 0.015793194994330406
-----
643
train loss: 0.004822300747036934
-----
644
train loss: 0.004962538834661245
-----
645
train loss: 0.004650203511118889
-----
646
train loss: 0.007880760356783867
-----
647
train loss: 0.0036994696129113436
-----
648
train loss: 0.005670513026416302
-----
649
train loss: 0.008237611502408981
-----
650
train loss: 0.006474994122982025
-----
651
train loss: 0.004997977986931801
-----
652
train loss: 0.006267762277275324
-----
653
train loss: 0.0046106018126010895
-----
654
train loss: 0.006883897818624973
-----
655
train loss: 0.0039473953656852245
-----
656
train loss: 0.004474152810871601
-----
657
train loss: 0.005926760379225016
-----
658
train loss: 0.00582015560939908
-----
659
train loss: 0.004083629231899977
-----
660
train loss: 0.006280964240431786
-----
661
train loss: 0.008306551724672318
-----
662
train loss: 0.0035862354561686516
-----
663
train loss: 0.009735647588968277
-----
664
train loss: 0.00628060195595026
-----
665
train loss: 0.006496301852166653
-----
666
train loss: 0.002846374176442623
-----
667
train loss: 0.0054818373173475266
-----
668
train loss: 0.005238936748355627
-----
669
train loss: 0.011464051902294159
-----
670
train loss: 0.008562388829886913
-----
671
train loss: 0.003976087551563978
-----
672
train loss: 0.005010239779949188
-----
673
train loss: 0.005018996074795723
-----
674
train loss: 0.005002210382372141
-----
675
train loss: 0.009810477495193481
-----
676
train loss: 0.004332046024501324
-----
677
train loss: 0.004227137193083763
-----
678
train loss: 0.004142626654356718
-----
679
train loss: 0.006696769967675209
-----
680
train loss: 0.005921605508774519
-----
681
train loss: 0.006837551947683096
-----
682
train loss: 0.005188050679862499
-----
683
train loss: 0.004607685375958681
-----
684
train loss: 0.005096639506518841
-----
685
train loss: 0.007967514917254448
-----
686
train loss: 0.005376816727221012
-----
687
train loss: 0.004990104585886002
-----
688
train loss: 0.009028359316289425
-----
689
train loss: 0.004355315119028091
-----
690
train loss: 0.006346275098621845
-----
691
train loss: 0.0026263538748025894
-----
692
train loss: 0.006907298229634762
-----
693
train loss: 0.0070946235209703445
-----
694
train loss: 0.008964134380221367
-----
695
train loss: 0.0033432054333388805
-----
696
train loss: 0.006349143572151661
-----
697
train loss: 0.0049215042963624
-----
698
train loss: 0.005798935424536467
-----
699
train loss: 0.010259581729769707
-----
700
train loss: 0.007789324037730694
-----
701
train loss: 0.005305892787873745
-----
702
train loss: 0.0037754252552986145
-----
703
train loss: 0.007328834384679794
-----
704
train loss: 0.004275636747479439
-----
705
train loss: 0.007810740731656551
-----
706
train loss: 0.00860312208533287
-----
707
train loss: 0.004411259200423956
-----
708
train loss: 0.008449917659163475
-----
709
train loss: 0.00616579269990325
-----
710
train loss: 0.0038821615744382143
-----
711
train loss: 0.005572495050728321
-----
712
train loss: 0.012718366459012032
-----
713
train loss: 0.00790509581565857
-----
714
train loss: 0.004910677671432495
-----
715
train loss: 0.00477145379409194
-----
716
train loss: 0.007879478856921196
-----
717
train loss: 0.00599384680390358
-----
718
train loss: 0.003981391433626413
-----
719
train loss: 0.007789758499711752
-----
720
train loss: 0.006851437501609325
-----
721
train loss: 0.0035645850002765656
-----
722
train loss: 0.01007673516869545
-----
723
train loss: 0.003275799099355936
-----
724
train loss: 0.002559472806751728
-----
725
train loss: 0.008904283866286278
-----
726
train loss: 0.007393225096166134
-----
727
train loss: 0.00787175539880991
-----
728
train loss: 0.005167500115931034
-----
729
train loss: 0.006940317340195179
-----
730
train loss: 0.006227854639291763
-----
731
train loss: 0.0028474675491452217
-----
732
train loss: 0.0057662129402160645
-----
733
train loss: 0.003487521782517433
-----
734
train loss: 0.0043525793589651585
-----
735
train loss: 0.0042041754350066185
-----
736
train loss: 0.0035855111200362444
-----
737
train loss: 0.004427668638527393
-----
738
train loss: 0.005345185287296772
-----
739
train loss: 0.007039578631520271
-----
740
train loss: 0.0036993466783314943
-----
741
train loss: 0.006757904309779406
-----
742
train loss: 0.003078525885939598
-----
743
train loss: 0.004403899423778057
-----
744
train loss: 0.00422483729198575
-----
745
train loss: 0.002955697476863861
-----
746
train loss: 0.005092198960483074
-----
747
train loss: 0.011062012985348701
-----
748
train loss: 0.0042286948300898075
-----
749
train loss: 0.005060754716396332
-----
750
train loss: 0.003543141297996044
-----
751
train loss: 0.0025955159217119217
-----
752
train loss: 0.011498650535941124
-----
753
train loss: 0.007036381401121616
-----
754
train loss: 0.0042905560694634914
-----
755
train loss: 0.0058789681643247604
-----
756
train loss: 0.007500786334276199
-----
757
train loss: 0.005475271493196487
-----
758
train loss: 0.01281730830669403
-----
759
train loss: 0.005806709174066782
-----
760
train loss: 0.0036020700354129076
-----
761
train loss: 0.004211418330669403
-----
762
train loss: 0.005048704799264669
-----
763
train loss: 0.008135058917105198
-----
764
train loss: 0.006150612607598305
-----
765
train loss: 0.008327484130859375
-----
766
train loss: 0.006915932986885309
-----
767
train loss: 0.005469569470733404
-----
768
train loss: 0.003602121025323868
-----
769
train loss: 0.003626743331551552
-----
770
train loss: 0.002537766005843878
-----
771
train loss: 0.0032547255977988243
-----
772
train loss: 0.004932635463774204
-----
773
train loss: 0.008283236064016819
-----
774
train loss: 0.003249229397624731
-----
775
train loss: 0.00494132936000824
-----
776
train loss: 0.00416353065520525
-----
777
train loss: 0.0038166185840964317
-----
778
train loss: 0.005710097495466471
-----
779
train loss: 0.00555966654792428
-----
780
train loss: 0.009803742170333862
-----
781
train loss: 0.003906168043613434
-----
782
train loss: 0.0038934245239943266
-----
783
train loss: 0.006622254848480225
-----
784
train loss: 0.002848029136657715
-----
785
train loss: 0.01085483469069004
-----
786
train loss: 0.005267101805657148
-----
787
train loss: 0.00485820509493351
-----
788
train loss: 0.0021757923532277346
-----
789
train loss: 0.004665712360292673
-----
790
train loss: 0.006475304253399372
-----
791
train loss: 0.004666915629059076
-----
792
train loss: 0.007235451601445675
-----
793
train loss: 0.007493644021451473
-----
794
train loss: 0.008551104925572872
-----
795
train loss: 0.004945864900946617
-----
796
train loss: 0.0033475374802947044
-----
797
train loss: 0.00240268069319427
-----
798
train loss: 0.006928163580596447
-----
799
train loss: 0.006747527979314327
-----
800
train loss: 0.009853935800492764
-----
801
train loss: 0.007721171248704195
-----
802
train loss: 0.0024411953054368496
-----
803
train loss: 0.006355241872370243
-----
804
train loss: 0.0036139998119324446
-----
805
train loss: 0.007524565793573856
-----
806
train loss: 0.007282148580998182
-----
807
train loss: 0.009707000106573105
-----
808
train loss: 0.005258234217762947
-----
809
train loss: 0.007521342020481825
-----
810
train loss: 0.003979570232331753
-----
811
train loss: 0.005404677242040634
-----
812
train loss: 0.0042231082916259766
-----
813
train loss: 0.007990586571395397
-----
814
train loss: 0.009495450183749199
-----
815
train loss: 0.004685874097049236
-----
816
train loss: 0.008461827412247658
-----
817
train loss: 0.010194504633545876
-----
818
train loss: 0.004223786760121584
-----
819
train loss: 0.012922877445816994
-----
820
train loss: 0.00839269533753395
-----
821
train loss: 0.0045287651009857655
-----
822
train loss: 0.00401614373549819
-----
823
train loss: 0.004478235729038715
-----
824
train loss: 0.004196294583380222
-----
825
train loss: 0.004660702310502529
-----
826
train loss: 0.0037935208529233932
-----
827
train loss: 0.006954350508749485
-----
828
train loss: 0.006410314701497555
-----
829
train loss: 0.0017924780258908868
-----
830
train loss: 0.0038928152061998844
-----
831
train loss: 0.004331853706389666
-----
832
train loss: 0.003932264633476734
-----
833
train loss: 0.004325333517044783
-----
834
train loss: 0.0036082742735743523
-----
835
train loss: 0.0009352680644951761
-----
836
train loss: 0.007181230932474136
-----
837
train loss: 0.00410793162882328
-----
838
train loss: 0.0042040711268782616
-----
839
train loss: 0.0092439204454422
-----
840
train loss: 0.013655580580234528
-----
841
train loss: 0.004318538587540388
-----
842
train loss: 0.005530379246920347
-----
843
train loss: 0.006878374144434929
-----
844
train loss: 0.008055015467107296
-----
845
train loss: 0.00868188962340355
-----
846
train loss: 0.006095717661082745
-----
847
train loss: 0.005172122269868851
-----
848
train loss: 0.005910069681704044
-----
849
train loss: 0.012807709164917469
-----
850
train loss: 0.011376295238733292
-----
851
train loss: 0.002538464032113552
-----
852
train loss: 0.003922213800251484
-----
853
train loss: 0.004469590727239847
-----
854
train loss: 0.0098509406670928
-----
855
train loss: 0.0057682618498802185
-----
856
train loss: 0.003970229998230934
-----
857
train loss: 0.0068944343365728855
-----
858
train loss: 0.002958024386316538
-----
859
train loss: 0.0058762566186487675
-----
860
train loss: 0.008900808170437813
-----
861
train loss: 0.004730229265987873
-----
862
train loss: 0.006282748654484749
-----
863
train loss: 0.009956195950508118
-----
864
train loss: 0.005250497721135616
-----
865
train loss: 0.007771460805088282
-----
866
train loss: 0.006427203770726919
-----
867
train loss: 0.010554708540439606
-----
868
train loss: 0.006119015160948038
-----
869
train loss: 0.007414036430418491
-----
870
train loss: 0.001977053936570883
-----
871
train loss: 0.0046988422982394695
-----
872
train loss: 0.007220158353447914
-----
873
train loss: 0.004044465720653534
-----
874
train loss: 0.0065744658932089806
-----
875
train loss: 0.005329063627868891
-----
876
train loss: 0.003840472549200058
-----
877
train loss: 0.010892498306930065
-----
878
train loss: 0.006483215838670731
-----
879
train loss: 0.004645070526748896
-----
880
train loss: 0.003555618692189455
-----
881
train loss: 0.007748844102025032
-----
882
train loss: 0.006589221302419901
-----
883
train loss: 0.0020082779228687286
-----
884
train loss: 0.0027353595942258835
-----
885
train loss: 0.0046776775270700455
-----
886
train loss: 0.007807530928403139
-----
887
train loss: 0.013592083007097244
-----
888
train loss: 0.0051785921677947044
-----
889
train loss: 0.003946264740079641
-----
890
train loss: 0.006324059795588255
-----
891
train loss: 0.004298684187233448
-----
892
train loss: 0.006090576760470867
-----
893
train loss: 0.012823546305298805
-----
894
train loss: 0.007915807887911797
-----
895
train loss: 0.004192518536001444
-----
896
train loss: 0.006046745460480452
-----
897
train loss: 0.0031169122084975243
-----
898
train loss: 0.00427192822098732
-----
899
train loss: 0.007988843135535717
-----
900
train loss: 0.007228800095617771
-----
901
train loss: 0.005984034389257431
-----
902
train loss: 0.0035742907784879208
-----
903
train loss: 0.008982239291071892
-----
904
train loss: 0.006562712602317333
-----
905
train loss: 0.005069653503596783
-----
906
train loss: 0.005984539166092873
-----
907
train loss: 0.00654897466301918
-----
908
train loss: 0.005832788068801165
-----
909
train loss: 0.005799232050776482
-----
910
train loss: 0.017582938075065613
-----
911
train loss: 0.0038003649096935987
-----
912
train loss: 0.01352118793874979
-----
913
train loss: 0.006345267873257399
-----
914
train loss: 0.005496941041201353
-----
915
train loss: 0.0110214464366436
-----
916
train loss: 0.00465383380651474
-----
917
train loss: 0.005283988080918789
-----
918
train loss: 0.004600421991199255
-----
919
train loss: 0.009777725674211979
-----
920
train loss: 0.005984757095575333
-----
921
train loss: 0.004507339559495449
-----
922
train loss: 0.007043751422315836
-----
923
train loss: 0.007238709833472967
-----
924
train loss: 0.0031444875057786703
-----
925
train loss: 0.006588096264749765
-----
926
train loss: 0.0020271241664886475
-----
927
train loss: 0.008458343334496021
-----
928
train loss: 0.00427587516605854
-----
929
train loss: 0.010790358297526836
-----
930
train loss: 0.006347875576466322
-----
931
train loss: 0.009978607296943665
-----
932
train loss: 0.004311525262892246
-----
933
train loss: 0.003230836009606719
-----
934
train loss: 0.004357488825917244
-----
935
train loss: 0.004612891469150782
-----
936
train loss: 0.007883235812187195
-----
937
train loss: 0.00574785191565752
-----
938
train loss: 0.0075513203628361225
-----
939
train loss: 0.00801809597760439
-----
940
train loss: 0.0036309973802417517
-----
941
train loss: 0.0037210413720458746
-----
942
train loss: 0.006135768257081509
-----
943
train loss: 0.0038867080584168434
-----
944
train loss: 0.006273400969803333
-----
945
train loss: 0.004028088878840208
-----
946
train loss: 0.007487769238650799
-----
947
train loss: 0.006169288419187069
-----
948
train loss: 0.010240761563181877
-----
949
train loss: 0.006338093429803848
-----
950
train loss: 0.005966597236692905
-----
951
train loss: 0.004159857984632254
-----
952
train loss: 0.003213853808119893
-----
953
train loss: 0.0037142287474125624
-----
954
train loss: 0.004491799511015415
-----
955
train loss: 0.006359169725328684
-----
956
train loss: 0.0063542090356349945
-----
957
train loss: 0.00747089134529233
-----
958
train loss: 0.00386491185054183
-----
959
train loss: 0.009750638157129288
-----
960
train loss: 0.0035731082316488028
-----
961
train loss: 0.004485898185521364
-----
962
train loss: 0.006014405749738216
-----
963
train loss: 0.0025559356436133385
-----
964
train loss: 0.0039754691533744335
-----
965
train loss: 0.007863912731409073
-----
966
train loss: 0.010380883701145649
-----
967
train loss: 0.005824967287480831
-----
968
train loss: 0.011450877413153648
-----
969
train loss: 0.009445229545235634
-----
970
train loss: 0.007333890534937382
-----
971
train loss: 0.005530240945518017
-----
972
train loss: 0.007229934446513653
-----
973
train loss: 0.003467041766270995
-----
974
train loss: 0.00751673337072134
-----
975
train loss: 0.0038263064343482256
-----
976
train loss: 0.00787409394979477
-----
977
train loss: 0.005263352766633034
-----
978
train loss: 0.005468639545142651
-----
979
train loss: 0.005028534214943647
-----
980
train loss: 0.00726031418889761
-----
981
train loss: 0.006163433194160461
-----
982
train loss: 0.009527888149023056
-----
983
train loss: 0.0058151851408183575
-----
984
train loss: 0.004826649092137814
-----
985
train loss: 0.008189762942492962
-----
986
train loss: 0.005966844502836466
-----
987
train loss: 0.004525538068264723
-----
988
train loss: 0.004006111528724432
-----
989
train loss: 0.004889661446213722
-----
990
train loss: 0.008834334090352058
-----
991
train loss: 0.013672830536961555
-----
992
train loss: 0.004083964973688126
-----
993
train loss: 0.0046338955871760845
-----
994
train loss: 0.0061143748462200165
-----
995
train loss: 0.004243527539074421
-----
996
train loss: 0.007394552230834961
-----
997
train loss: 0.007948791608214378
-----
998
train loss: 0.002262965776026249
-----
999
train loss: 0.0069724940694868565
-----
1000
train loss: 0.007522108033299446
-----
1001
train loss: 0.007697036024183035
-----
1002
train loss: 0.004601011984050274
-----
1003
train loss: 0.007940713316202164
-----
1004
train loss: 0.008799724280834198
-----
1005
train loss: 0.004335848614573479
-----
1006
train loss: 0.00474170409142971
-----
1007
train loss: 0.0036951531656086445
-----
1008
train loss: 0.005082633346319199
-----
1009
train loss: 0.009700620546936989
-----
1010
train loss: 0.00686864135786891
-----
1011
train loss: 0.008479489013552666
-----
1012
train loss: 0.0066994233056902885
-----
1013
train loss: 0.006819065194576979
-----
1014
train loss: 0.006790143437683582
-----
1015
train loss: 0.00798107124865055
-----
1016
train loss: 0.004410067107528448
-----
1017
train loss: 0.007836820557713509
-----
1018
train loss: 0.002550981007516384
-----
1019
train loss: 0.00770318228751421
-----
1020
train loss: 0.0030930526554584503
-----
1021
train loss: 0.006118883844465017
-----
1022
train loss: 0.005707293748855591
-----
1023
train loss: 0.006263395771384239
-----
1024
train loss: 0.0048110634088516235
-----
1025
train loss: 0.009019317105412483
-----
1026
train loss: 0.008123107254505157
-----
1027
train loss: 0.005081293173134327
-----
1028
train loss: 0.003770899260416627
-----
1029
train loss: 0.004115336108952761
-----
1030
train loss: 0.00390615570358932
-----
1031
train loss: 0.005005905870348215
-----
1032
train loss: 0.00484803132712841
-----
1033
train loss: 0.012241815216839314
-----
1034
train loss: 0.007361759897321463
-----
1035
train loss: 0.006198210641741753
-----
1036
train loss: 0.0030532565433532
-----
1037
train loss: 0.010285131633281708
-----
1038
train loss: 0.00689137727022171
-----
1039
train loss: 0.003937601577490568
-----
1040
train loss: 0.008390502072870731
-----
1041
train loss: 0.006261544767767191
-----
1042
train loss: 0.0048258546739816666
-----
1043
train loss: 0.0050168102607131
-----
1044
train loss: 0.0029727101791650057
-----
1045
train loss: 0.00335692148655653
-----
1046
train loss: 0.010674498975276947
-----
1047
train loss: 0.0033904449082911015
-----
1048
train loss: 0.006147253327071667
-----
1049
train loss: 0.004953218158334494
-----
1050
train loss: 0.0062086135149002075
-----
1051
train loss: 0.010676256380975246
-----
1052
train loss: 0.005690068006515503
-----
1053
train loss: 0.007873177528381348
-----
1054
train loss: 0.005727717652916908
-----
1055
train loss: 0.0031061838380992413
-----
1056
train loss: 0.007201100699603558
-----
1057
train loss: 0.002330644754692912
-----
1058
train loss: 0.0055107492953538895
-----
1059
train loss: 0.0027186141815036535
-----
1060
train loss: 0.010295321233570576
-----
1061
train loss: 0.0022261596750468016
-----
1062
train loss: 0.006697855889797211
-----
1063
train loss: 0.006057197693735361
-----

  Average training loss: 0.01
  Validation Loss: 0.01

4
-----
0
train loss: 0.0074537028558552265
-----
1
train loss: 0.007220666389912367
-----
2
train loss: 0.0038041698280721903
-----
3
train loss: 0.0033007878810167313
-----
4
train loss: 0.007111699786037207
-----
5
train loss: 0.0044804769568145275
-----
6
train loss: 0.0027714716270565987
-----
7
train loss: 0.006829763762652874
-----
8
train loss: 0.004957706667482853
-----
9
train loss: 0.00393525930121541
-----
10
train loss: 0.004782978445291519
-----
11
train loss: 0.0045335907489061356
-----
12
train loss: 0.005957695655524731
-----
13
train loss: 0.003869270905852318
-----
14
train loss: 0.0026561995036900043
-----
15
train loss: 0.004516467917710543
-----
16
train loss: 0.00428323308005929
-----
17
train loss: 0.004190485924482346
-----
18
train loss: 0.0034542125649750233
-----
19
train loss: 0.007201645523309708
-----
20
train loss: 0.0037977206520736217
-----
21
train loss: 0.005602110177278519
-----
22
train loss: 0.005412091966718435
-----
23
train loss: 0.00422879122197628
-----
24
train loss: 0.006537967827171087
-----
25
train loss: 0.0038842977955937386
-----
26
train loss: 0.009427199140191078
-----
27
train loss: 0.003814251162111759
-----
28
train loss: 0.0035474731121212244
-----
29
train loss: 0.0028155120089650154
-----
30
train loss: 0.011507303453981876
-----
31
train loss: 0.006388590205460787
-----
32
train loss: 0.0069677638821303844
-----
33
train loss: 0.0050540221855044365
-----
34
train loss: 0.0034442152827978134
-----
35
train loss: 0.0040056826546788216
-----
36
train loss: 0.009487309493124485
-----
37
train loss: 0.005401165224611759
-----
38
train loss: 0.006393272429704666
-----
39
train loss: 0.004742050543427467
-----
40
train loss: 0.0064061833545565605
-----
41
train loss: 0.006444439757615328
-----
42
train loss: 0.004796906840056181
-----
43
train loss: 0.00537153659388423
-----
44
train loss: 0.007599742151796818
-----
45
train loss: 0.0032490380108356476
-----
46
train loss: 0.003608015365898609
-----
47
train loss: 0.005093507468700409
-----
48
train loss: 0.004544030874967575
-----
49
train loss: 0.005941377952694893
-----
50
train loss: 0.0033946398179978132
-----
51
train loss: 0.00432521291077137
-----
52
train loss: 0.001347262179479003
-----
53
train loss: 0.004478090908378363
-----
54
train loss: 0.0029716892167925835
-----
55
train loss: 0.004315313883125782
-----
56
train loss: 0.006451497785747051
-----
57
train loss: 0.0047971345484256744
-----
58
train loss: 0.007961523719131947
-----
59
train loss: 0.0036467162426561117
-----
60
train loss: 0.004974076058715582
-----
61
train loss: 0.0029795877635478973
-----
62
train loss: 0.003874939400702715
-----
63
train loss: 0.004753366112709045
-----
64
train loss: 0.007667736615985632
-----
65
train loss: 0.005233262199908495
-----
66
train loss: 0.004109940491616726
-----
67
train loss: 0.007115816697478294
-----
68
train loss: 0.005794418975710869
-----
69
train loss: 0.009374493733048439
-----
70
train loss: 0.008672467432916164
-----
71
train loss: 0.005043752957135439
-----
72
train loss: 0.006442311219871044
-----
73
train loss: 0.0031830286607146263
-----
74
train loss: 0.006761323194950819
-----
75
train loss: 0.0029326248914003372
-----
76
train loss: 0.004293281584978104
-----
77
train loss: 0.00434090755879879
-----
78
train loss: 0.002853803802281618
-----
79
train loss: 0.004418914206326008
-----
80
train loss: 0.002779320813715458
-----
81
train loss: 0.004328818991780281
-----
82
train loss: 0.005297170951962471
-----
83
train loss: 0.004070834256708622
-----
84
train loss: 0.0043631549924612045
-----
85
train loss: 0.006991335656493902
-----
86
train loss: 0.003888955805450678
-----
87
train loss: 0.006082750856876373
-----
88
train loss: 0.004053047858178616
-----
89
train loss: 0.006500099785625935
-----
90
train loss: 0.005868166219443083
-----
91
train loss: 0.00723752798512578
-----
92
train loss: 0.004811776801943779
-----
93
train loss: 0.00623503141105175
-----
94
train loss: 0.0070065222680568695
-----
95
train loss: 0.002861208515241742
-----
96
train loss: 0.005075477529317141
-----
97
train loss: 0.005247969180345535
-----
98
train loss: 0.003802729304879904
-----
99
train loss: 0.003974933177232742
-----
100
train loss: 0.003807368455454707
-----
101
train loss: 0.0032400041818618774
-----
102
train loss: 0.0036341287195682526
-----
103
train loss: 0.0065051596611738205
-----
104
train loss: 0.004431800451129675
-----
105
train loss: 0.0061814566142857075
-----
106
train loss: 0.00412295525893569
-----
107
train loss: 0.005879979580640793
-----
108
train loss: 0.005048936232924461
-----
109
train loss: 0.006425126455724239
-----
110
train loss: 0.006112453527748585
-----
111
train loss: 0.004594302736222744
-----
112
train loss: 0.005170459393411875
-----
113
train loss: 0.0031730937771499157
-----
114
train loss: 0.0042741610668599606
-----
115
train loss: 0.0044965585693717
-----
116
train loss: 0.005467190407216549
-----
117
train loss: 0.004433454480022192
-----
118
train loss: 0.005450525786727667
-----
119
train loss: 0.003531558671966195
-----
120
train loss: 0.004353820346295834
-----
121
train loss: 0.006033912301063538
-----
122
train loss: 0.003679924178868532
-----
123
train loss: 0.007102688774466515
-----
124
train loss: 0.0066282786428928375
-----
125
train loss: 0.0035038217902183533
-----
126
train loss: 0.006178588606417179
-----
127
train loss: 0.0048868232406675816
-----
128
train loss: 0.0033167661167681217
-----
129
train loss: 0.005110281519591808
-----
130
train loss: 0.0021673748269677162
-----
131
train loss: 0.003045240417122841
-----
132
train loss: 0.005944998934864998
-----
133
train loss: 0.003278707154095173
-----
134
train loss: 0.007494955789297819
-----
135
train loss: 0.004433730151504278
-----
136
train loss: 0.0035399338230490685
-----
137
train loss: 0.004324115347117186
-----
138
train loss: 0.0076924217864871025
-----
139
train loss: 0.004241349175572395
-----
140
train loss: 0.005336353555321693
-----
141
train loss: 0.002970689907670021
-----
142
train loss: 0.007920493371784687
-----
143
train loss: 0.007892432622611523
-----
144
train loss: 0.0034218267537653446
-----
145
train loss: 0.0028944029472768307
-----
146
train loss: 0.00484158331528306
-----
147
train loss: 0.007403974421322346
-----
148
train loss: 0.00690868403762579
-----
149
train loss: 0.0027678143233060837
-----
150
train loss: 0.0033868271857500076
-----
151
train loss: 0.006671970710158348
-----
152
train loss: 0.006216912530362606
-----
153
train loss: 0.009152472950518131
-----
154
train loss: 0.0027164402417838573
-----
155
train loss: 0.0013027588138356805
-----
156
train loss: 0.00396244041621685
-----
157
train loss: 0.006094727665185928
-----
158
train loss: 0.006219352129846811
-----
159
train loss: 0.004858884960412979
-----
160
train loss: 0.0033752541057765484
-----
161
train loss: 0.0035484391264617443
-----
162
train loss: 0.0038360622711479664
-----
163
train loss: 0.003895529080182314
-----
164
train loss: 0.00384115194901824
-----
165
train loss: 0.0038450611755251884
-----
166
train loss: 0.005601107142865658
-----
167
train loss: 0.006739875301718712
-----
168
train loss: 0.0057614510878920555
-----
169
train loss: 0.0045785848051309586
-----
170
train loss: 0.003928507678210735
-----
171
train loss: 0.0041076671332120895
-----
172
train loss: 0.0039027156308293343
-----
173
train loss: 0.005150198936462402
-----
174
train loss: 0.008108237758278847
-----
175
train loss: 0.003605482168495655
-----
176
train loss: 0.002947496483102441
-----
177
train loss: 0.0064733000472188
-----
178
train loss: 0.012060162611305714
-----
179
train loss: 0.008913099765777588
-----
180
train loss: 0.006494145840406418
-----
181
train loss: 0.005785313434898853
-----
182
train loss: 0.002363775623962283
-----
183
train loss: 0.0042649684473872185
-----
184
train loss: 0.007860250771045685
-----
185
train loss: 0.00351572223007679
-----
186
train loss: 0.0049763089045882225
-----
187
train loss: 0.006145030725747347
-----
188
train loss: 0.0036638067103922367
-----
189
train loss: 0.0036658707540482283
-----
190
train loss: 0.006892087869346142
-----
191
train loss: 0.008329598233103752
-----
192
train loss: 0.00409587100148201
-----
193
train loss: 0.002420948352664709
-----
194
train loss: 0.008137281984090805
-----
195
train loss: 0.0058077555149793625
-----
196
train loss: 0.004277962259948254
-----
197
train loss: 0.004328531213104725
-----
198
train loss: 0.003111487254500389
-----
199
train loss: 0.005514855496585369
-----
200
train loss: 0.00738892424851656
-----
201
train loss: 0.0013833222910761833
-----
202
train loss: 0.003153040772303939
-----
203
train loss: 0.005531135480850935
-----
204
train loss: 0.0013444803189486265
-----
205
train loss: 0.004601828288286924
-----
206
train loss: 0.002564221154898405
-----
207
train loss: 0.005991704761981964
-----
208
train loss: 0.005183418281376362
-----
209
train loss: 0.0015193111030384898
-----
210
train loss: 0.009912244975566864
-----
211
train loss: 0.004684602841734886
-----
212
train loss: 0.006445326842367649
-----
213
train loss: 0.005375183653086424
-----
214
train loss: 0.005309492349624634
-----
215
train loss: 0.005849206820130348
-----
216
train loss: 0.003124828916043043
-----
217
train loss: 0.0015779442619532347
-----
218
train loss: 0.002552345395088196
-----
219
train loss: 0.003205173183232546
-----
220
train loss: 0.006284730974584818
-----
221
train loss: 0.005068347789347172
-----
222
train loss: 0.0030800611712038517
-----
223
train loss: 0.0022497379686683416
-----
224
train loss: 0.005174159538000822
-----
225
train loss: 0.0031170970760285854
-----
226
train loss: 0.004761469550430775
-----
227
train loss: 0.004730436950922012
-----
228
train loss: 0.0036101890727877617
-----
229
train loss: 0.004804779775440693
-----
230
train loss: 0.004434898495674133
-----
231
train loss: 0.005036810413002968
-----
232
train loss: 0.00621220376342535
-----
233
train loss: 0.006583084352314472
-----
234
train loss: 0.004080763086676598
-----
235
train loss: 0.0027330159209668636
-----
236
train loss: 0.005665551405400038
-----
237
train loss: 0.0022779146675020456
-----
238
train loss: 0.0037676796782761812
-----
239
train loss: 0.008460525423288345
-----
240
train loss: 0.00798814371228218
-----
241
train loss: 0.006624077446758747
-----
242
train loss: 0.005177085287868977
-----
243
train loss: 0.0021235093008726835
-----
244
train loss: 0.00475462106987834
-----
245
train loss: 0.006055584643036127
-----
246
train loss: 0.004194410517811775
-----
247
train loss: 0.0038254321552813053
-----
248
train loss: 0.005584212951362133
-----
249
train loss: 0.004346425645053387
-----
250
train loss: 0.006981986574828625
-----
251
train loss: 0.005571623332798481
-----
252
train loss: 0.005588953383266926
-----
253
train loss: 0.0046104369685053825
-----
254
train loss: 0.004242252092808485
-----
255
train loss: 0.002664944389835
-----
256
train loss: 0.0054782978259027
-----
257
train loss: 0.005451525561511517
-----
258
train loss: 0.003046745667234063
-----
259
train loss: 0.004452722612768412
-----
260
train loss: 0.004125648178160191
-----
261
train loss: 0.004524766467511654
-----
262
train loss: 0.003324724268168211
-----
263
train loss: 0.0047093965113162994
-----
264
train loss: 0.005767943803220987
-----
265
train loss: 0.004269307479262352
-----
266
train loss: 0.001867199083790183
-----
267
train loss: 0.0020874247420579195
-----
268
train loss: 0.0035127862356603146
-----
269
train loss: 0.0024886340834200382
-----
270
train loss: 0.0021749534644186497
-----
271
train loss: 0.004038454499095678
-----
272
train loss: 0.00467760069295764
-----
273
train loss: 0.003018449991941452
-----
274
train loss: 0.0024418719112873077
-----
275
train loss: 0.0020879358053207397
-----
276
train loss: 0.0026277676224708557
-----
277
train loss: 0.0028834822587668896
-----
278
train loss: 0.002571273595094681
-----
279
train loss: 0.006997791118919849
-----
280
train loss: 0.004446038976311684
-----
281
train loss: 0.002541216788813472
-----
282
train loss: 0.004542526789009571
-----
283
train loss: 0.0044539812952280045
-----
284
train loss: 0.007249172776937485
-----
285
train loss: 0.007068266160786152
-----
286
train loss: 0.00228498550131917
-----
287
train loss: 0.004908738657832146
-----
288
train loss: 0.004027922172099352
-----
289
train loss: 0.0020992157515138388
-----
290
train loss: 0.0035843648947775364
-----
291
train loss: 0.0038110590539872646
-----
292
train loss: 0.004785947035998106
-----
293
train loss: 0.005003257654607296
-----
294
train loss: 0.0031115382444113493
-----
295
train loss: 0.00503868144005537
-----
296
train loss: 0.0020361526403576136
-----
297
train loss: 0.0033080787397921085
-----
298
train loss: 0.006983551196753979
-----
299
train loss: 0.004323928151279688
-----
300
train loss: 0.007871127687394619
-----
301
train loss: 0.003559420583769679
-----
302
train loss: 0.005813300609588623
-----
303
train loss: 0.004081593826413155
-----
304
train loss: 0.004256468266248703
-----
305
train loss: 0.004159367643296719
-----
306
train loss: 0.004421037156134844
-----
307
train loss: 0.005981079768389463
-----
308
train loss: 0.004139569588005543
-----
309
train loss: 0.004053171724081039
-----
310
train loss: 0.00449037179350853
-----
311
train loss: 0.0055120522156357765
-----
312
train loss: 0.003416365012526512
-----
313
train loss: 0.0046413373202085495
-----
314
train loss: 0.005196899641305208
-----
315
train loss: 0.00351989408954978
-----
316
train loss: 0.0034041821490973234
-----
317
train loss: 0.004863933194428682
-----
318
train loss: 0.0034169687423855066
-----
319
train loss: 0.006358488462865353
-----
320
train loss: 0.004298040643334389
-----
321
train loss: 0.0026129204779863358
-----
322
train loss: 0.004820562899112701
-----
323
train loss: 0.004706596024334431
-----
324
train loss: 0.004988798871636391
-----
325
train loss: 0.008173979818820953
-----
326
train loss: 0.005387407261878252
-----
327
train loss: 0.004859788343310356
-----
328
train loss: 0.005058261565864086
-----
329
train loss: 0.006073447875678539
-----
330
train loss: 0.005110329948365688
-----
331
train loss: 0.005115144420415163
-----
332
train loss: 0.007309682667255402
-----
333
train loss: 0.006658099591732025
-----
334
train loss: 0.003228149376809597
-----
335
train loss: 0.0052506206557154655
-----
336
train loss: 0.0027770954184234142
-----
337
train loss: 0.004856463987380266
-----
338
train loss: 0.007082005031406879
-----
339
train loss: 0.003474089317023754
-----
340
train loss: 0.0023277071304619312
-----
341
train loss: 0.006994597148150206
-----
342
train loss: 0.0065478072501719
-----
343
train loss: 0.00876547209918499
-----
344
train loss: 0.007938563823699951
-----
345
train loss: 0.007058389019221067
-----
346
train loss: 0.005795660428702831
-----
347
train loss: 0.004313092213124037
-----
348
train loss: 0.0020438088104128838
-----
349
train loss: 0.004614229314029217
-----
350
train loss: 0.0024458428379148245
-----
351
train loss: 0.004903682507574558
-----
352
train loss: 0.007292464375495911
-----
353
train loss: 0.005143395625054836
-----
354
train loss: 0.005113507620990276
-----
355
train loss: 0.005678943358361721
-----
356
train loss: 0.004057429730892181
-----
357
train loss: 0.010131118819117546
-----
358
train loss: 0.003676452673971653
-----
359
train loss: 0.005473569966852665
-----
360
train loss: 0.0031439089216291904
-----
361
train loss: 0.003928667865693569
-----
362
train loss: 0.0023600049316883087
-----
363
train loss: 0.0041090683080255985
-----
364
train loss: 0.003978664055466652
-----
365
train loss: 0.003441148903220892
-----
366
train loss: 0.004346065688878298
-----
367
train loss: 0.00616104481741786
-----
368
train loss: 0.0039739483036100864
-----
369
train loss: 0.0026802527718245983
-----
370
train loss: 0.0031947444658726454
-----
371
train loss: 0.0031456612050533295
-----
372
train loss: 0.005224660504609346
-----
373
train loss: 0.00488433800637722
-----
374
train loss: 0.0029739036690443754
-----
375
train loss: 0.004069603979587555
-----
376
train loss: 0.0047041261568665504
-----
377
train loss: 0.002162264660000801
-----
378
train loss: 0.0034596561454236507
-----
379
train loss: 0.0055803172290325165
-----
380
train loss: 0.0037448755465447903
-----
381
train loss: 0.006726209074258804
-----
382
train loss: 0.00399960670620203
-----
383
train loss: 0.005281037651002407
-----
384
train loss: 0.005995518993586302
-----
385
train loss: 0.0048021478578448296
-----
386
train loss: 0.006111358292400837
-----
387
train loss: 0.0027577767614275217
-----
388
train loss: 0.005247908644378185
-----
389
train loss: 0.005082791205495596
-----
390
train loss: 0.005519133992493153
-----
391
train loss: 0.006221732124686241
-----
392
train loss: 0.0034584312234073877
-----
393
train loss: 0.005220132879912853
-----
394
train loss: 0.005988711025565863
-----
395
train loss: 0.0033038314431905746
-----
396
train loss: 0.005870127119123936
-----
397
train loss: 0.005339421331882477
-----
398
train loss: 0.004256931599229574
-----
399
train loss: 0.0037203123793005943
-----
400
train loss: 0.006373905576765537
-----
401
train loss: 0.007654211483895779
-----
402
train loss: 0.004686649888753891
-----
403
train loss: 0.005990911740809679
-----
404
train loss: 0.0050967070274055
-----
405
train loss: 0.0017268648371100426
-----
406
train loss: 0.005745762959122658
-----
407
train loss: 0.008233001455664635
-----
408
train loss: 0.00397932855412364
-----
409
train loss: 0.010511917062103748
-----
410
train loss: 0.0030195866711437702
-----
411
train loss: 0.0067428057081997395
-----
412
train loss: 0.005385612603276968
-----
413
train loss: 0.0018861026037484407
-----
414
train loss: 0.00553599139675498
-----
415
train loss: 0.00453495979309082
-----
416
train loss: 0.003290219232439995
-----
417
train loss: 0.004504679702222347
-----
418
train loss: 0.0045133912935853004
-----
419
train loss: 0.009647390805184841
-----
420
train loss: 0.003018169431015849
-----
421
train loss: 0.009142149239778519
-----
422
train loss: 0.003311718348413706
-----
423
train loss: 0.004672147333621979
-----
424
train loss: 0.009421205148100853
-----
425
train loss: 0.004898839630186558
-----
426
train loss: 0.005398091860115528
-----
427
train loss: 0.0032667284831404686
-----
428
train loss: 0.004893493372946978
-----
429
train loss: 0.006469179876148701
-----
430
train loss: 0.005559843964874744
-----
431
train loss: 0.0036684973165392876
-----
432
train loss: 0.001810664776712656
-----
433
train loss: 0.005066810175776482
-----
434
train loss: 0.004506534896790981
-----
435
train loss: 0.0045596156269311905
-----
436
train loss: 0.0030925367027521133
-----
437
train loss: 0.006984600331634283
-----
438
train loss: 0.0031211243476718664
-----
439
train loss: 0.0024527418427169323
-----
440
train loss: 0.005849616136401892
-----
441
train loss: 0.006606264039874077
-----
442
train loss: 0.004914466291666031
-----
443
train loss: 0.003994216211140156
-----
444
train loss: 0.0066761053167283535
-----
445
train loss: 0.004373346921056509
-----
446
train loss: 0.007832487113773823
-----
447
train loss: 0.005748562049120665
-----
448
train loss: 0.005253645591437817
-----
449
train loss: 0.004640602506697178
-----
450
train loss: 0.0016416856087744236
-----
451
train loss: 0.011470792815089226
-----
452
train loss: 0.006708728615194559
-----
453
train loss: 0.0055019063875079155
-----
454
train loss: 0.003451590659096837
-----
455
train loss: 0.004521499387919903
-----
456
train loss: 0.0048796939663589
-----
457
train loss: 0.0037778662517666817
-----
458
train loss: 0.004935663193464279
-----
459
train loss: 0.004090779460966587
-----
460
train loss: 0.003627320285886526
-----
461
train loss: 0.002692669630050659
-----
462
train loss: 0.004940928891301155
-----
463
train loss: 0.0031232363544404507
-----
464
train loss: 0.007536463439464569
-----
465
train loss: 0.0038057565689086914
-----
466
train loss: 0.003801557468250394
-----
467
train loss: 0.0065515604801476
-----
468
train loss: 0.003673079190775752
-----
469
train loss: 0.008549544960260391
-----
470
train loss: 0.0039738621562719345
-----
471
train loss: 0.0036556716077029705
-----
472
train loss: 0.007094452157616615
-----
473
train loss: 0.002644835039973259
-----
474
train loss: 0.004129102453589439
-----
475
train loss: 0.0022030023392289877
-----
476
train loss: 0.0034327791072428226
-----
477
train loss: 0.00710078701376915
-----
478
train loss: 0.0017041403334587812
-----
479
train loss: 0.004555218853056431
-----
480
train loss: 0.004658563993871212
-----
481
train loss: 0.0066039832308888435
-----
482
train loss: 0.003584454534575343
-----
483
train loss: 0.007466425187885761
-----
484
train loss: 0.005788293667137623
-----
485
train loss: 0.008235013112425804
-----
486
train loss: 0.006671513430774212
-----
487
train loss: 0.0022235750220716
-----
488
train loss: 0.004213140811771154
-----
489
train loss: 0.006093104835599661
-----
490
train loss: 0.0036998989526182413
-----
491
train loss: 0.00777960941195488
-----
492
train loss: 0.004344635177403688
-----
493
train loss: 0.005001885816454887
-----
494
train loss: 0.0042897844687104225
-----
495
train loss: 0.004097968805581331
-----
496
train loss: 0.005001713987439871
-----
497
train loss: 0.0073662083595991135
-----
498
train loss: 0.0019462895579636097
-----
499
train loss: 0.008895276114344597
-----
500
train loss: 0.005454406142234802
-----
501
train loss: 0.00408867746591568
-----
502
train loss: 0.00665572052821517
-----
503
train loss: 0.0031318452674895525
-----
504
train loss: 0.003514709882438183
-----
505
train loss: 0.006401675753295422
-----
506
train loss: 0.0036111490335315466
-----
507
train loss: 0.004681717604398727
-----
508
train loss: 0.00982316117733717
-----
509
train loss: 0.00532402191311121
-----
510
train loss: 0.006077781319618225
-----
511
train loss: 0.008416766300797462
-----
512
train loss: 0.0046227434650063515
-----
513
train loss: 0.0027015781961381435
-----
514
train loss: 0.006680460646748543
-----
515
train loss: 0.007327895611524582
-----
516
train loss: 0.002406852087005973
-----
517
train loss: 0.005695108789950609
-----
518
train loss: 0.0036623654887080193
-----
519
train loss: 0.00384390726685524
-----
520
train loss: 0.00983818992972374
-----
521
train loss: 0.007400629110634327
-----
522
train loss: 0.006142350845038891
-----
523
train loss: 0.006244133226573467
-----
524
train loss: 0.005830245092511177
-----
525
train loss: 0.00811782106757164
-----
526
train loss: 0.005331356078386307
-----
527
train loss: 0.002793285995721817
-----
528
train loss: 0.0057973298244178295
-----
529
train loss: 0.0031726821325719357
-----
530
train loss: 0.0034607304260134697
-----
531
train loss: 0.005319058429449797
-----
532
train loss: 0.001757632358931005
-----
533
train loss: 0.002363711129873991
-----
534
train loss: 0.004072125535458326
-----
535
train loss: 0.0023357300087809563
-----
536
train loss: 0.006730289198458195
-----
537
train loss: 0.004485937766730785
-----
538
train loss: 0.004597427323460579
-----
539
train loss: 0.004273037426173687
-----
540
train loss: 0.0058693308383226395
-----
541
train loss: 0.00916454941034317
-----
542
train loss: 0.0032935021445155144
-----
543
train loss: 0.007893258705735207
-----
544
train loss: 0.0026318610180169344
-----
545
train loss: 0.005123019218444824
-----
546
train loss: 0.005244356580078602
-----
547
train loss: 0.005370587110519409
-----
548
train loss: 0.0027500095311552286
-----
549
train loss: 0.006141066085547209
-----
550
train loss: 0.003893646877259016
-----
551
train loss: 0.004476177506148815
-----
552
train loss: 0.004474593326449394
-----
553
train loss: 0.006687335669994354
-----
554
train loss: 0.0024723520036786795
-----
555
train loss: 0.0041244542226195335
-----
556
train loss: 0.0026660114526748657
-----
557
train loss: 0.0036269607953727245
-----
558
train loss: 0.007918054237961769
-----
559
train loss: 0.00259709102101624
-----
560
train loss: 0.0064369505271315575
-----
561
train loss: 0.0045290677808225155
-----
562
train loss: 0.005028606858104467
-----
563
train loss: 0.004993900656700134
-----
564
train loss: 0.007631049957126379
-----
565
train loss: 0.004153006710112095
-----
566
train loss: 0.006404950749129057
-----
567
train loss: 0.004244687967002392
-----
568
train loss: 0.006191630847752094
-----
569
train loss: 0.005941783078014851
-----
570
train loss: 0.0027476795949041843
-----
571
train loss: 0.004162254277616739
-----
572
train loss: 0.004904689732939005
-----
573
train loss: 0.008016359061002731
-----
574
train loss: 0.006427893880754709
-----
575
train loss: 0.0030371530447155237
-----
576
train loss: 0.005640833638608456
-----
577
train loss: 0.01003083772957325
-----
578
train loss: 0.0017926874570548534
-----
579
train loss: 0.0034506344236433506
-----
580
train loss: 0.004289922304451466
-----
581
train loss: 0.008736941032111645
-----
582
train loss: 0.006590402685105801
-----
583
train loss: 0.007673677522689104
-----
584
train loss: 0.00646806787699461
-----
585
train loss: 0.0024212943390011787
-----
586
train loss: 0.004208230413496494
-----
587
train loss: 0.008305469527840614
-----
588
train loss: 0.005190779455006123
-----
589
train loss: 0.006253247614949942
-----
590
train loss: 0.003486510831862688
-----
591
train loss: 0.006219789385795593
-----
592
train loss: 0.006380942650139332
-----
593
train loss: 0.002752589760348201
-----
594
train loss: 0.004412130452692509
-----
595
train loss: 0.0035625994205474854
-----
596
train loss: 0.006381236016750336
-----
597
train loss: 0.005854391027241945
-----
598
train loss: 0.004625500179827213
-----
599
train loss: 0.0064709968864917755
-----
600
train loss: 0.006617593113332987
-----
601
train loss: 0.003689975943416357
-----
602
train loss: 0.004919749218970537
-----
603
train loss: 0.0045022666454315186
-----
604
train loss: 0.0063803354278206825
-----
605
train loss: 0.0029861517250537872
-----
606
train loss: 0.012148026376962662
-----
607
train loss: 0.0033162469044327736
-----
608
train loss: 0.00467868335545063
-----
609
train loss: 0.004451191518455744
-----
610
train loss: 0.004133520182222128
-----
611
train loss: 0.001491750474087894
-----
612
train loss: 0.004572304431349039
-----
613
train loss: 0.002959061414003372
-----
614
train loss: 0.0047541409730911255
-----
615
train loss: 0.0030014622025191784
-----
616
train loss: 0.0037537289317697287
-----
617
train loss: 0.0028833625838160515
-----
618
train loss: 0.005614349152892828
-----
619
train loss: 0.004006821662187576
-----
620
train loss: 0.0034362399019300938
-----
621
train loss: 0.0044461400248110294
-----
622
train loss: 0.0043873353861272335
-----
623
train loss: 0.0070771086029708385
-----
624
train loss: 0.0031992485746741295
-----
625
train loss: 0.0026144864968955517
-----
626
train loss: 0.005335361696779728
-----
627
train loss: 0.005720300599932671
-----
628
train loss: 0.00676381029188633
-----
629
train loss: 0.0035523674450814724
-----
630
train loss: 0.005459403619170189
-----
631
train loss: 0.0035871472209692
-----
632
train loss: 0.002955016680061817
-----
633
train loss: 0.003739729057997465
-----
634
train loss: 0.002174138789996505
-----
635
train loss: 0.002428911393508315
-----
636
train loss: 0.0026778073515743017
-----
637
train loss: 0.0055037555284798145
-----
638
train loss: 0.007210855837911367
-----
639
train loss: 0.0020269169472157955
-----
640
train loss: 0.00639559468254447
-----
641
train loss: 0.0026027073618024588
-----
642
train loss: 0.0037614605389535427
-----
643
train loss: 0.01106029748916626
-----
644
train loss: 0.006014641374349594
-----
645
train loss: 0.007265433203428984
-----
646
train loss: 0.0068018389865756035
-----
647
train loss: 0.006213491782546043
-----
648
train loss: 0.0025745355524122715
-----
649
train loss: 0.003415302839130163
-----
650
train loss: 0.0028276368975639343
-----
651
train loss: 0.0038985577411949635
-----
652
train loss: 0.0068828952498734
-----
653
train loss: 0.005665513686835766
-----
654
train loss: 0.0049765221774578094
-----
655
train loss: 0.002365067135542631
-----
656
train loss: 0.0029201835859566927
-----
657
train loss: 0.004000754561275244
-----
658
train loss: 0.004503927193582058
-----
659
train loss: 0.004096080549061298
-----
660
train loss: 0.0064182160422205925
-----
661
train loss: 0.00551146175712347
-----
662
train loss: 0.003717769868671894
-----
663
train loss: 0.004224322270601988
-----
664
train loss: 0.0027027386240661144
-----
665
train loss: 0.00458179647102952
-----
666
train loss: 0.006079255603253841
-----
667
train loss: 0.005098168272525072
-----
668
train loss: 0.005199635401368141
-----
669
train loss: 0.007188962306827307
-----
670
train loss: 0.004792193882167339
-----
671
train loss: 0.006378652527928352
-----
672
train loss: 0.001936056767590344
-----
673
train loss: 0.0031377365812659264
-----
674
train loss: 0.007093358319252729
-----
675
train loss: 0.003215186996385455
-----
676
train loss: 0.006958186160773039
-----
677
train loss: 0.0041035539470613
-----
678
train loss: 0.00214413832873106
-----
679
train loss: 0.007969575002789497
-----
680
train loss: 0.0032875933684408665
-----
681
train loss: 0.006731076166033745
-----
682
train loss: 0.0033328384160995483
-----
683
train loss: 0.003942294977605343
-----
684
train loss: 0.0019497668836265802
-----
685
train loss: 0.005071419291198254
-----
686
train loss: 0.00358887342736125
-----
687
train loss: 0.004694016650319099
-----
688
train loss: 0.0036039219703525305
-----
689
train loss: 0.0032359303440898657
-----
690
train loss: 0.008315911516547203
-----
691
train loss: 0.002938956953585148
-----
692
train loss: 0.0035309582017362118
-----
693
train loss: 0.003666092175990343
-----
694
train loss: 0.005639358423650265
-----
695
train loss: 0.004856898449361324
-----
696
train loss: 0.006308635231107473
-----
697
train loss: 0.0028590471483767033
-----
698
train loss: 0.004544573836028576
-----
699
train loss: 0.004970972891896963
-----
700
train loss: 0.006455389317125082
-----
701
train loss: 0.0060365404933691025
-----
702
train loss: 0.006981103681027889
-----
703
train loss: 0.0037405029870569706
-----
704
train loss: 0.0049410006031394005
-----
705
train loss: 0.005795593839138746
-----
706
train loss: 0.00521331001073122
-----
707
train loss: 0.003250837791711092
-----
708
train loss: 0.00592909986153245
-----
709
train loss: 0.006577749270945787
-----
710
train loss: 0.005346945021301508
-----
711
train loss: 0.007959075272083282
-----
712
train loss: 0.012894367799162865
-----
713
train loss: 0.001999668311327696
-----
714
train loss: 0.006164310500025749
-----
715
train loss: 0.005340655334293842
-----
716
train loss: 0.004714892245829105
-----
717
train loss: 0.0029187710024416447
-----
718
train loss: 0.004420341923832893
-----
719
train loss: 0.004736102651804686
-----
720
train loss: 0.0068547772243618965
-----
721
train loss: 0.003771753516048193
-----
722
train loss: 0.0035947663709521294
-----
723
train loss: 0.0046706050634384155
-----
724
train loss: 0.002919235732406378
-----
725
train loss: 0.005600136239081621
-----
726
train loss: 0.00442872429266572
-----
727
train loss: 0.0037114189472049475
-----
728
train loss: 0.0038321830797940493
-----
729
train loss: 0.002075567841529846
-----
730
train loss: 0.006579347420483828
-----
731
train loss: 0.0033459258265793324
-----
732
train loss: 0.0022006300278007984
-----
733
train loss: 0.00597735121846199
-----
734
train loss: 0.003008616389706731
-----
735
train loss: 0.003530085552483797
-----
736
train loss: 0.0027567052748054266
-----
737
train loss: 0.005199698731303215
-----
738
train loss: 0.004841177724301815
-----
739
train loss: 0.00507355947047472
-----
740
train loss: 0.005360919516533613
-----
741
train loss: 0.005401451140642166
-----
742
train loss: 0.004249531775712967
-----
743
train loss: 0.004144635517150164
-----
744
train loss: 0.005986526608467102
-----
745
train loss: 0.0026896451599895954
-----
746
train loss: 0.006149603519588709
-----
747
train loss: 0.004595584701746702
-----
748
train loss: 0.0051145413890480995
-----
749
train loss: 0.004077787511050701
-----
750
train loss: 0.005063394550234079
-----
751
train loss: 0.004923724569380283
-----
752
train loss: 0.006677188910543919
-----
753
train loss: 0.006665973924100399
-----
754
train loss: 0.009044196456670761
-----
755
train loss: 0.005037097260355949
-----
756
train loss: 0.008642058819532394
-----
757
train loss: 0.006259790156036615
-----
758
train loss: 0.007042079232633114
-----
759
train loss: 0.004232669249176979
-----
760
train loss: 0.003121535759419203
-----
761
train loss: 0.003659958951175213
-----
762
train loss: 0.0062833866104483604
-----
763
train loss: 0.0030702147632837296
-----
764
train loss: 0.006869279779493809
-----
765
train loss: 0.003566452767699957
-----
766
train loss: 0.004269793163985014
-----
767
train loss: 0.005493322852998972
-----
768
train loss: 0.009446115233004093
-----
769
train loss: 0.012038091197609901
-----
770
train loss: 0.0062115369364619255
-----
771
train loss: 0.005786250811070204
-----
772
train loss: 0.005443211179226637
-----
773
train loss: 0.003008713945746422
-----
774
train loss: 0.006674110423773527
-----
775
train loss: 0.005769498646259308
-----
776
train loss: 0.0024503879249095917
-----
777
train loss: 0.009453722275793552
-----
778
train loss: 0.00826770905405283
-----
779
train loss: 0.0035149245522916317
-----
780
train loss: 0.004735368769615889
-----
781
train loss: 0.004401414655148983
-----
782
train loss: 0.008136575110256672
-----
783
train loss: 0.002544393762946129
-----
784
train loss: 0.006572387181222439
-----
785
train loss: 0.005426385439932346
-----
786
train loss: 0.005848631728440523
-----
787
train loss: 0.005076038651168346
-----
788
train loss: 0.0036596048157662153
-----
789
train loss: 0.00530341686680913
-----
790
train loss: 0.0049200281500816345
-----
791
train loss: 0.0030372762121260166
-----
792
train loss: 0.0033917122054845095
-----
793
train loss: 0.0025367478374391794
-----
794
train loss: 0.00512620760127902
-----
795
train loss: 0.002902240725234151
-----
796
train loss: 0.005885158199816942
-----
797
train loss: 0.003876326372846961
-----
798
train loss: 0.007002940401434898
-----
799
train loss: 0.0054488712921738625
-----
800
train loss: 0.0027574943378567696
-----
801
train loss: 0.0052681853994727135
-----
802
train loss: 0.003576607909053564
-----
803
train loss: 0.004476046189665794
-----
804
train loss: 0.004813601262867451
-----
805
train loss: 0.005421935580670834
-----
806
train loss: 0.0023145617451518774
-----
807
train loss: 0.0029700719751417637
-----
808
train loss: 0.008146115578711033
-----
809
train loss: 0.004988891072571278
-----
810
train loss: 0.005735334940254688
-----
811
train loss: 0.0022159856744110584
-----
812
train loss: 0.0029455346520990133
-----
813
train loss: 0.0068532251752913
-----
814
train loss: 0.0045059481635689735
-----
815
train loss: 0.0034443624317646027
-----
816
train loss: 0.005128600634634495
-----
817
train loss: 0.006895462051033974
-----
818
train loss: 0.0038803652860224247
-----
819
train loss: 0.0061580343171954155
-----
820
train loss: 0.0040589296258986
-----
821
train loss: 0.00684743095189333
-----
822
train loss: 0.0031473783310502768
-----
823
train loss: 0.004508711397647858
-----
824
train loss: 0.0033566229976713657
-----
825
train loss: 0.008116882294416428
-----
826
train loss: 0.0055582174099981785
-----
827
train loss: 0.00703379325568676
-----
828
train loss: 0.005781024694442749
-----
829
train loss: 0.003283665282651782
-----
830
train loss: 0.003494341392070055
-----
831
train loss: 0.002742182929068804
-----
832
train loss: 0.0037931313272565603
-----
833
train loss: 0.006817539222538471
-----
834
train loss: 0.006647976115345955
-----
835
train loss: 0.00261438125744462
-----
836
train loss: 0.004562245681881905
-----
837
train loss: 0.004492097068578005
-----
838
train loss: 0.003920156508684158
-----
839
train loss: 0.0041104089468717575
-----
840
train loss: 0.0026212825905531645
-----
841
train loss: 0.009296787902712822
-----
842
train loss: 0.004205298610031605
-----
843
train loss: 0.007189659867435694
-----
844
train loss: 0.003329972270876169
-----
845
train loss: 0.006862315349280834
-----
846
train loss: 0.00600042100995779
-----
847
train loss: 0.002359498990699649
-----
848
train loss: 0.0017406499246135354
-----
849
train loss: 0.006894072517752647
-----
850
train loss: 0.0035090798046439886
-----
851
train loss: 0.0020767448004335165
-----
852
train loss: 0.004672733601182699
-----
853
train loss: 0.0028318066615611315
-----
854
train loss: 0.005111007019877434
-----
855
train loss: 0.006350241601467133
-----
856
train loss: 0.0031468262895941734
-----
857
train loss: 0.007306346204131842
-----
858
train loss: 0.0026383432559669018
-----
859
train loss: 0.004145421553403139
-----
860
train loss: 0.0033856055233627558
-----
861
train loss: 0.008066150359809399
-----
862
train loss: 0.005356266163289547
-----
863
train loss: 0.0050241900607943535
-----
864
train loss: 0.005099146626889706
-----
865
train loss: 0.007856566458940506
-----
866
train loss: 0.004190235398709774
-----
867
train loss: 0.0041723959147930145
-----
868
train loss: 0.003755638375878334
-----
869
train loss: 0.0030953362584114075
-----
870
train loss: 0.011317387223243713
-----
871
train loss: 0.004314053803682327
-----
872
train loss: 0.005244744010269642
-----
873
train loss: 0.006125122308731079
-----
874
train loss: 0.005278941709548235
-----
875
train loss: 0.008388216607272625
-----
876
train loss: 0.007449178025126457
-----
877
train loss: 0.0047097522765398026
-----
878
train loss: 0.0056326198391616344
-----
879
train loss: 0.005508224479854107
-----
880
train loss: 0.005756266415119171
-----
881
train loss: 0.0075004142709076405
-----
882
train loss: 0.004860403947532177
-----
883
train loss: 0.004952459596097469
-----
884
train loss: 0.00473378598690033
-----
885
train loss: 0.00402901042252779
-----
886
train loss: 0.0033223950304090977
-----
887
train loss: 0.004797606263309717
-----
888
train loss: 0.004108820576220751
-----
889
train loss: 0.0058289095759391785
-----
890
train loss: 0.003252971451729536
-----
891
train loss: 0.009818591177463531
-----
892
train loss: 0.00513081718236208
-----
893
train loss: 0.004864265210926533
-----
894
train loss: 0.004512108862400055
-----
895
train loss: 0.0033361369278281927
-----
896
train loss: 0.004587830044329166
-----
897
train loss: 0.006792987696826458
-----
898
train loss: 0.006550808437168598
-----
899
train loss: 0.004790921695530415
-----
900
train loss: 0.005274628289043903
-----
901
train loss: 0.003012692788615823
-----
902
train loss: 0.002655451651662588
-----
903
train loss: 0.004087239038199186
-----
904
train loss: 0.00326728168874979
-----
905
train loss: 0.0017574557568877935
-----
906
train loss: 0.004193550441414118
-----
907
train loss: 0.004771220497786999
-----
908
train loss: 0.0040825968608260155
-----
909
train loss: 0.007397254928946495
-----
910
train loss: 0.004320511128753424
-----
911
train loss: 0.0055468217469751835
-----
912
train loss: 0.004541868343949318
-----
913
train loss: 0.003787807421758771
-----
914
train loss: 0.004135155584663153
-----
915
train loss: 0.0024616497103124857
-----
916
train loss: 0.00485584419220686
-----
917
train loss: 0.008366074413061142
-----
918
train loss: 0.006234540604054928
-----
919
train loss: 0.004957555793225765
-----
920
train loss: 0.0036407981533557177
-----
921
train loss: 0.0028367764316499233
-----
922
train loss: 0.00884687528014183
-----
923
train loss: 0.006489716470241547
-----
924
train loss: 0.00504364725202322
-----
925
train loss: 0.005594693589955568
-----
926
train loss: 0.004720862023532391
-----
927
train loss: 0.005089661572128534
-----
928
train loss: 0.008863851428031921
-----
929
train loss: 0.0025848322547972202
-----
930
train loss: 0.0043688141740858555
-----
931
train loss: 0.0050665210001170635
-----
932
train loss: 0.0069474829360842705
-----
933
train loss: 0.004159991629421711
-----
934
train loss: 0.008427570573985577
-----
935
train loss: 0.004374645184725523
-----
936
train loss: 0.00323082716204226
-----
937
train loss: 0.0033016186207532883
-----
938
train loss: 0.00387822138145566
-----
939
train loss: 0.0038893918972462416
-----
940
train loss: 0.003934784326702356
-----
941
train loss: 0.006061100400984287
-----
942
train loss: 0.004617972299456596
-----
943
train loss: 0.007725684903562069
-----
944
train loss: 0.002912061521783471
-----
945
train loss: 0.006615006364881992
-----
946
train loss: 0.0028259519021958113
-----
947
train loss: 0.004259889479726553
-----
948
train loss: 0.0061959815211594105
-----
949
train loss: 0.003978641238063574
-----
950
train loss: 0.0026823123916983604
-----
951
train loss: 0.006816450040787458
-----
952
train loss: 0.004808058030903339
-----
953
train loss: 0.0026256400160491467
-----
954
train loss: 0.003709889017045498
-----
955
train loss: 0.005978228058665991
-----
956
train loss: 0.0062943738885223866
-----
957
train loss: 0.008129402063786983
-----
958
train loss: 0.0053741419687867165
-----
959
train loss: 0.008113719522953033
-----
960
train loss: 0.0029744389466941357
-----
961
train loss: 0.004741031676530838
-----
962
train loss: 0.012008987367153168
-----
963
train loss: 0.008943362161517143
-----
964
train loss: 0.006863835267722607
-----
965
train loss: 0.006101646460592747
-----
966
train loss: 0.0031639181543141603
-----
967
train loss: 0.004552379250526428
-----
968
train loss: 0.0067212581634521484
-----
969
train loss: 0.004237546119838953
-----
970
train loss: 0.005040010903030634
-----
971
train loss: 0.0029531377367675304
-----
972
train loss: 0.0041776373982429504
-----
973
train loss: 0.0030299085192382336
-----
974
train loss: 0.004436427261680365
-----
975
train loss: 0.004317037761211395
-----
976
train loss: 0.007281626109033823
-----
977
train loss: 0.004514324478805065
-----
978
train loss: 0.00275967619381845
-----
979
train loss: 0.0022784904576838017
-----
980
train loss: 0.004355850629508495
-----
981
train loss: 0.003613875713199377
-----
982
train loss: 0.004116903990507126
-----
983
train loss: 0.0030515629332512617
-----
984
train loss: 0.00393400015309453
-----
985
train loss: 0.003426027949899435
-----
986
train loss: 0.003916120622307062
-----
987
train loss: 0.00844617746770382
-----
988
train loss: 0.007730367127805948
-----
989
train loss: 0.00380195421166718
-----
990
train loss: 0.0029507679864764214
-----
991
train loss: 0.002633808646351099
-----
992
train loss: 0.00441693514585495
-----
993
train loss: 0.005216182675212622
-----
994
train loss: 0.0029457691125571728
-----
995
train loss: 0.006861462257802486
-----
996
train loss: 0.005545699968934059
-----
997
train loss: 0.008247090503573418
-----
998
train loss: 0.0016274433583021164
-----
999
train loss: 0.003579107578843832
-----
1000
train loss: 0.004148002248257399
-----
1001
train loss: 0.008374535478651524
-----
1002
train loss: 0.003806294873356819
-----
1003
train loss: 0.0052002305164933205
-----
1004
train loss: 0.0064202044159173965
-----
1005
train loss: 0.0044057308696210384
-----
1006
train loss: 0.005306938197463751
-----
1007
train loss: 0.003382474184036255
-----
1008
train loss: 0.0030231878627091646
-----
1009
train loss: 0.007584286853671074
-----
1010
train loss: 0.003182426793500781
-----
1011
train loss: 0.004391954746097326
-----
1012
train loss: 0.005807229317724705
-----
1013
train loss: 0.003956664819270372
-----
1014
train loss: 0.005180845968425274
-----
1015
train loss: 0.0036937862168997526
-----
1016
train loss: 0.005620202980935574
-----
1017
train loss: 0.004179186187684536
-----
1018
train loss: 0.0028311354108154774
-----
1019
train loss: 0.0005783483502455056
-----
1020
train loss: 0.007912330329418182
-----
1021
train loss: 0.006312608253210783
-----
1022
train loss: 0.0020002704113721848
-----
1023
train loss: 0.004280261229723692
-----
1024
train loss: 0.0036638304591178894
-----
1025
train loss: 0.004568084608763456
-----
1026
train loss: 0.010182476602494717
-----
1027
train loss: 0.0065301768481731415
-----
1028
train loss: 0.007331532426178455
-----
1029
train loss: 0.004736769944429398
-----
1030
train loss: 0.007908721454441547
-----
1031
train loss: 0.004635216202586889
-----
1032
train loss: 0.005157337989658117
-----
1033
train loss: 0.0041860961355268955
-----
1034
train loss: 0.005591647233814001
-----
1035
train loss: 0.003913247026503086
-----
1036
train loss: 0.0045324345119297504
-----
1037
train loss: 0.002349376678466797
-----
1038
train loss: 0.008780207484960556
-----
1039
train loss: 0.0012374238576740026
-----
1040
train loss: 0.004702151753008366
-----
1041
train loss: 0.00578590203076601
-----
1042
train loss: 0.002901257248595357
-----
1043
train loss: 0.005051779560744762
-----
1044
train loss: 0.0023231832310557365
-----
1045
train loss: 0.006418799050152302
-----
1046
train loss: 0.002554928883910179
-----
1047
train loss: 0.0034610708244144917
-----
1048
train loss: 0.003570484695956111
-----
1049
train loss: 0.0028529365081340075
-----
1050
train loss: 0.003912388812750578
-----
1051
train loss: 0.004529097583144903
-----
1052
train loss: 0.003965727053582668
-----
1053
train loss: 0.003365539014339447
-----
1054
train loss: 0.002194365719333291
-----
1055
train loss: 0.007155236322432756
-----
1056
train loss: 0.005715929437428713
-----
1057
train loss: 0.0031882571056485176
-----
1058
train loss: 0.0044969115406274796
-----
1059
train loss: 0.0029193633235991
-----
1060
train loss: 0.004927361384034157
-----
1061
train loss: 0.00464113662019372
-----
1062
train loss: 0.007069859653711319
-----
1063
train loss: 0.004002206027507782
-----

  Average training loss: 0.00
  Validation Loss: 0.01


# create panda of training and validation (here equal to test) loss 
import pandas as pd

# Create a DataFrame from our training statistics.
df_stats = pd.DataFrame(data=training_stats)

# Use the 'epoch' as the row index.
df_stats = df_stats.set_index('epoch')

# Display the table.
df_stats
     
Training Loss	Valid. Loss
epoch		
1	0.033406	0.014511
2	0.013154	0.013552
3	0.008726	0.011240
4	0.006249	0.011230
5	0.004898	0.010817

#plot train and test loss over the epochs
import matplotlib.pyplot as plt
import seaborn as sns

# Use plot styling from seaborn.
sns.set(style='darkgrid')

# Increase the plot size and font size.
sns.set(font_scale=1.5)
plt.rcParams["figure.figsize"] = (12,6)

# Plot the learning curve.
plt.plot(df_stats['Training Loss'], 'b-o', label="Training")
plt.plot(df_stats['Valid. Loss'], 'g-o', label="Test")

# Label the plot.
plt.title("Training & Test Loss")
plt.xlabel("Epoch")
plt.ylabel("Loss")
plt.legend()
plt.xticks(range(1,epochs+1))

plt.show()
     


# predictions for the trained model 
def predict(model, dataloader, device):
    model.eval()
    output = []
    labels = []
    for batch in dataloader:
        batch_inputs, batch_masks, batch_labels = \
                                  tuple(b.to(device) for b in batch)
        batch_inputs = batch_inputs.to(torch.int64)
        batch_masks = batch_masks.to(torch.int64)
        labels += batch_labels.view(1,-1).tolist()[0]
        with torch.no_grad():
            output += model(batch_inputs, 
                            batch_masks).view(1,-1).tolist()[0]
    return output, labels
     

# get the predictions on the test data 
output, lables = predict(model, test_dataloader, device) 
     

# transform the numerical output back into a datetime for more intuitive possibilites of onterpretation
from datetime import datetime

#calculate difference in days 
first_date = decode(np.min(lables))
last_date = decode(np.max(lables))

print(f'the first date is {first_date} and the last one {last_date}')

# convert string to date object
d1 = datetime.strptime(first_date, "%Y-%m-%d")
d2 = datetime.strptime(last_date, "%Y-%m-%d")

# difference between dates in timedelta
delta = d2 - d1
print(f'Difference is {delta.days} days, what is equivalent to {int(delta.days/365)} years')


# decode difference in outouts into difference in days
#[-1,1]-->[0,61489] day
#([-1,1]+1)/2-->[0,1]
#[0,1]*61489-->[0,61489]
day_decode = lambda s: ((s+1)/2)*delta.days
     
the first date is 1923-06-01 and the last one 2021-06-30
Difference is 35824 days, what is equivalent to 98 years

# demonstrate the difference between prediction and true label visually 
difference = day_decode(np.array(output))-day_decode(np.array(lables))
date_labels = [decode(l) for l in np.array(lables)]

sort_ind = sorted(range(len(np.array(lables))), key=lambda k: np.array(lables)[k])
sort_ind = np.array(sort_ind).astype(int)

# filter some noise
from scipy.signal import lfilter
t_year = 365 #days of year
n = 50 # the larger n is, the smoother curve will be
b = [1.0 / n] * n
a = 1
yy = lfilter(b, a, difference[sort_ind]) #this is for removing some noise
x = np.arange(len(date_labels)) 
y = np.array(date_labels)[sort_ind]

fig, ax = plt.subplots()
plt.plot(x, yy/t_year)
plt.xticks(x, y, rotation=90)  
ax.locator_params(nbins=20, axis='x')
plt.title('difference of prediction and true date')
plt.xlabel('dates') 
plt.ylabel('delta year')
plt.show()
     

The overall discrepancy between the predicted dates and the true dates falls within a range of 5 years. It is plausible that during time periods when the language shifts from an older prediction to a more advanced one, a significant global event, such as a war, has occurred. This interruption in continuous development is followed by a transformative change in the world.


# helper function to find closest existing date in labels to given date
def closest_date(date):
  # convert string to date object
  datetime_dates = []
  datetime_dates = [datetime.strptime(d, "%Y-%m-%d") for d in date_labels]
  datetime_start = datetime.strptime(date, "%Y-%m-%d")


  # difference between dates in timedelta
  delta = [abs((dd-datetime_start).days) for dd in datetime_dates]
  #find closes date
  closest_date = date_labels[np.argmin(delta)]
  return closest_date
     

# helper function to find the index of the label for a given date
def date2ind(date):
  #find closest date in date_labels
  edate = encode(date)
  temp_array = abs(np.array(lables)-edate) 
  min_ind = np.argmin(temp_array[sort_ind])
  return min_ind
     

# find the min and max for a given area
#find the closest date
start_date = closest_date('1934-01-01') # type in the start date here
end_date = closest_date('1941-05-31') # type in the end date here 

start_ind = date2ind(start_date)
end_ind = date2ind(end_date)

max_date = (y[start_ind+np.argmax(yy[start_ind:end_ind])])
min_date = (y[start_ind+np.argmin(yy[start_ind:end_ind])])

print('extreme points in the chosen area: \n')
print(f'min date: {min_date}')
print(f'max date: {max_date}')
     
extreme points in the chosen area: 

min date: 1936-06-06
max date: 1939-06-25

# create word cloud for specific dates to be able to compare differences between dates
def token2cloud(date, n_region):
  ##date: that is plotted into a word cloud
  ##n_region: n closest dates

  #find closest dates in test data
  edate = encode(date)
  temp_array = abs(np.array(lables)[sort_ind]-edate)  
  sorted_ind = np.array(sorted(range(len(temp_array)), key=lambda k: temp_array[k]))
  ind = np.array(range(0,n_region+1)).astype(int) #first neighbour indices
  min_ind = sorted_ind[ind.astype(int)]
  temp_labels_uns = np.array(lables)[sort_ind]
  temp_labels = temp_labels_uns[min_ind]
  dec_labels = [decode(tl) for tl in temp_labels]
  print('\n')
  print('dates: \n')
  print(dec_labels)
  tokens_list = [tokenizer.convert_ids_to_tokens(test_inputs[ind]) for ind in min_ind]
  tokens = [item for sublist in tokens_list for item in sublist]
  #remove punctiution
  tokens = [char for char in tokens if char not in string.punctuation]
  # remove stopwords
  stop = stopwords.words('english')
  tokens = [token for token in tokens if token not in stop]
  # remove words less than three letters
  tokens = [word for word in tokens if len(word) >= 3]
  #remove PAD CLS SEP
  tokens = [word for word in tokens if word not in ['[PAD]', '[CLS]','[SEP]']]
  #remove article and title from tokens
  tokens = [word for word in tokens if word not in ['article', 'title']]
  #plot the word cloud
  plt.figure(figsize=(20,20))
  wc = WordCloud(max_font_size=50, max_words=100, background_color='white')
  wordcloud_jan = wc.generate_from_text(' '.join(tokens))
  plt.imshow(wordcloud_jan, interpolation='bilinear')
  plt.axis('off')
  plt.show()
     
I would like to examine the period around mid-1936 when the language and words used appeared to be more similar to those used approximately five years prior. Additionally, I want to explore the contrast with a period around three years later (also around mid-1936), when the language suddenly seems to resemble the language used five years later.


# show the word cloud for the chosen minimum (max negativ difference) with zero other close dates
token2cloud(min_date, 10)
     

dates: 

['1936-06-06', '1936-06-07', '1936-06-04', '1936-06-12', '1935-06-30', '1935-06-29', '1935-06-29', '1936-06-13', '1936-06-14', '1935-06-27', '1936-06-15']

Words that frequently appear and could be correlated to historical events are Berlin, triumph, victory, attack, leader, and deficit. Overall, there seems to be a significant presence of war-related language, which aligns with an older pattern and could explain why the language seems outdated for this time.


# show the word cloud for the chosen maximum with zero other close dates
token2cloud(max_date, 10)
     

dates: 

['1939-06-25', '1939-06-24', '1939-06-26', '1939-06-28', '1939-06-28', '1939-06-29', '1939-06-29', '1939-06-30', '1939-06-18', '1939-06-17', '1939-06-17']

Words that are frequently used include new, win, interest, credit, house, trade, and corruption. There doesn't seem to be any indication of the forthcoming Second World War, and the language appears to align more with an economic boom or a generally positive economy.

Conclusion
I have demonstrated that the BERT transformer model can be fine-tuned in a relatively straightforward manner for downstream regression tasks. The predicted labels fall within a range of less than 10 years from the true dates, which, considering a total time span of 100 years, can be considered satisfactory.

This fine-tuning with the date can be used as a task pre-training for which an integration of time would make sense.

Moreover, the model can be utilized to identify significant historical moments that have influenced the language used. If there is a similarity in the words used during a specific period in the past or future, deviating from a gradual progression over time, it could be indicative of one or multiple events that have impacted this linguistic development. Further analysis could attempt to establish correlations between this linguistic pattern and historical events.
