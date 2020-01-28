![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Use SQL To Find Which Process Is Taking Up Space In TempDB
**Post Date: May 9, 2014**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Here's a script I threw together to find what process is taking up space in the TempDB, and where the process came from.</p>      


## SQL-Logic
```SQL
-- find what objects are taking up the most TempDB space and where the process is coming from.
use master;
set nocount on
select
    sddssu.session_id
,   'login name'        = sdes.login_name
,   'from device'       = sdes.host_name
,   'database'      = db_name(sddssu.database_id)
,   'space consumed in gb'  = (sddssu.user_objects_alloc_page_count *8 / 1024 / 2014)
,   'process'       = sder.command
,   'statement'     = sdest.text
from   
    sys.dm_db_session_space_usage sddssu 
    join sys.dm_exec_requests sder on sddssu.session_id = sder.session_id
    cross apply sys.dm_exec_sql_text(sder.sql_handle) sdest
    join sys.dm_exec_sessions sdes on sdes.session_id   = sder.session_id
where  
    db_name(sddssu.database_id) in ('tempdb')
    and sddssu.user_objects_alloc_page_count > 0
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

      
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

