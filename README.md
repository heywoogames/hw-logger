@cdyw/logger
========

@cdyw/logger is a [log4js](https://github.com/nomiddlename/log4js-node) wrapper  which provides some useful features.  

## Installation
```
npm install @cdyw/logger
```

## Features
### log prefix
besides category, you can output prefix as you like in your log  
prefix can be filename, serverId, serverType, host etc  
to use this feature, you just pass prefix params to getLogger function  
```
var logger = require('@cdyw/logger').getLogger(category, prefix1, prefix2, ...);
```
 log output msg will output with prefix ahead   

### get line number in debug
when in debug environment, you may want to get the line number of the log  
to use this feature, add this code   
```
process.env.LOGGER_LINE = true;
```

you just configure the log4js file and set **lineDebug** for true  
```
{
  "appenders": {
    "console":{ "type":"console"},
    "con-log":{
        "type": "file",
        "filename": "${opts:base}/logs/con-log-${opts:serverId}.log",
        "maxLogSize": 1048576,
        "layout": {"type": "basic"},
        "backups": 5
    },
  },

  "categories": {
    "con-log": { "appenders": ["con-log"], "level":"all" },
    "comm": { "appenders": ["console","comm"], "level":"WARN" }
  }, 

  "reloadSecs": 10,

  "lineDebug": false
}
```

### log raw messages
in raw message mode, your log message will be simply your messages, no prefix and color format strings  
to use this feature, add this code  
```
process.env.RAW_MESSAGE = true;
```

you just configure the log4js file and set **rawMessage** for true  
```
{
  "appenders": {
    "console":{ "type":"console"},
    "con-log":{
        "type": "file",
        "filename": "${opts:base}/logs/con-log-${opts:serverId}.log",
        "maxLogSize": 1048576,
        "layout": {"type": "basic"},
        "backups": 5
    },
  },

  "categories": {
    "con-log": { "appenders": ["con-log"], "level":"all" },
    "comm": { "appenders": ["console","comm"], "level":"WARN" }
  }, 

  "reloadSecs": 10,

  "lineDebug": false
}
```

### dynamic configure logger level
logger configuration file log4js.json, you can add reloadSecs option. The reloadSecs means reload logger configuration file every given time. For example
```
{
	"reloadSecs": 30
}
```
the above configuration means reload the configuration file every 30 seconds. You can dynamic change the logger level, but it does not support dynamiclly changing configuration of appenders.

## Example
log.js
```
var logger = require('@cdyw/logger').getLogger('log', __filename, process.pid);

process.env.LOGGER_LINE = true;
logger.info('test1');
logger.warn('test2');
logger.error('test3');
```


