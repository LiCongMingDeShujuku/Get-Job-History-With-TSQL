![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 使用TSQL获取作业历史记录
#### Get Job History With TSQL
**发布-日期: 2016年06月13日 (评论)**

![#](images/##############?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这里有一些基本上显示了工作历史，包括步骤历史和步骤持续时间的快速的SQL逻辑。

## English
Here’s some quick SQL Logic which basically shows you the job history, with step history and step durations.

---
## Logic
```SQL
use msdb;
set nocount on
 
select
    'job_name'      =   sj.name
-- when step_id is zero '0', then you are seeing the final report of the job status.
-- 当step_id为零“0”时，你将看到作业状态的最终报告。
,   'step_id'       =   sjh.step_id
,   'step_name'     =   case sjh.step_name
                            when '(Job outcome)' then 'Job Completed'
                            else sjh.step_name
                        end
,   'status'        =   case
                            when sjh.run_status = 0 then 'Failed'
                            when sjh.run_status = 1 then 'Succeeded'
                            when sjh.run_status = 2 then 'Retry'
                            when sjh.run_status = 3 then 'Canceled'
                            when sjh.run_status = 4 then 'In-Progress'
                            when sjh.run_status = 5 then 'unknown'
                        end
,   'start_time'    =   left(msdb.dbo.agent_datetime(sjh.run_date, sjh.run_time), 19)
-- duration is a conversion of seconds to: Days, Hours, Minutes, Seconds
-- 持续时间是指将秒转换为：天，小时，分钟，秒              
,   'duration'      = 
                        right('0' + cast(sjh.run_duration   /60/60/24%30    as varchar),2)  + ':' +
                        right('0' + cast(sjh.run_duration   / 3600          as varchar),2)  + ':' +
                        right('0' + cast((sjh.run_duration  / 60) % 60      as varchar),2)  + ':' +
                        right('0' + cast(sjh.run_duration   % 60            as varchar),2)
from       
    sysjobs sj inner join  sysjobhistory sjh on sjh.job_id = sj.job_id
order by
    sjh.instance_id, sjh.run_date, sjh.run_time asc


```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

