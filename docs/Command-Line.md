Command Line
============

Global Config
-------------

You can get command help via `pyspider --help`.

global options works for all subcommands.

```
Usage: pyspider [OPTIONS] COMMAND [ARGS]...

  A powerful spider system in python.

Options:
  -c, --config FILENAME    a json file with default values for subcommands.
                           {"webui": {"port":5001}}
  --debug                  debug mode
  --queue-maxsize INTEGER  maxsize of queue
  --taskdb TEXT            database url for taskdb, default: sqlite
  --projectdb TEXT         database url for projectdb, default: sqlite
  --resultdb TEXT          database url for resultdb, default: sqlite
  --amqp-url TEXT          amqp url for rabbitmq, default: built-in Queue
  --phantomjs-proxy TEXT   phantomjs proxy ip:port
  --data-path TEXT         data dir path
  --help                   Show this message and exit.
```

#### --config

Config file is a json file with config values for global options or subcommands (with a sub-dict with subcommand name as key )

#### --queue-maxsize

Queue size limit, 0 for non-limit

#### --taskdb, --projectdb, --resultdb

```
mysql:
    mysql+type://user:passwd@host:port/database
sqlite:
    # relative path
    sqlite+type:///path/to/database.db
    # absolute path
    sqlite+type:////path/to/database.db
    # memory database
    sqlite+type://
mongodb:
    mongodb+type://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
    more: http://docs.mongodb.org/manual/reference/connection-string/
sqlalchemy:
    sqlalchemy+postgresql+type://user:passwd@host:port/database
    sqlalchemy+mysql+mysqlconnector+type://user:passwd@host:port/database
    more: http://docs.sqlalchemy.org/en/rel_0_9/core/engines.html
```

#### --amqp-url

See [https://www.rabbitmq.com/uri-spec.html](https://www.rabbitmq.com/uri-spec.html)

#### --phantomjs-proxy

The phantomjs proxy address, you need a phantomjs installed and running phantomjs proxy with `phantomjs pyspider/fetcher/phantomjs_fetcher.js 25555`. See [Deployment](Deployment)

#### --data-path

SQLite database and counter dump file save path


all
---

running components in subprocess or threads

```
Usage: pyspider all [OPTIONS]

Options:
  --fetcher-num INTEGER         instance num of fetcher
  --processor-num INTEGER       instance num of processor
  --result-worker-num INTEGER   instance num of result worker
  --run-in [subprocess|thread]  run each components in thread or subprocess.
                                always using thread for windows.
  --help                        Show this message and exit.
```


bench
-----

do bench test

```
Usage: pyspider bench [OPTIONS]

Options:
  --fetcher-num INTEGER         instance num of fetcher
  --processor-num INTEGER       instance num of processor
  --result-worker-num INTEGER   instance num of result worker
  --run-in [subprocess|thread]  run each components in thread or subprocess.
                                always using thread for windows.
  --total INTEGER               total url in test page
  --show INTEGER                show how many urls in a page
  --help                        Show this message and exit.
```


scheduler
---------

run scheduler

```
Usage: pyspider scheduler [OPTIONS]

Options:
  --xmlrpc / --no-xmlrpc
  --xmlrpc-host TEXT
  --xmlrpc-port INTEGER
  --inqueue-limit INTEGER  size limit of task queue for each project, tasks
                           will been ignored when overflow
  --delete-time INTEGER    delete time before marked as delete
  --active-tasks INTEGER   active log size
  --loop-limit INTEGER     maximum number of tasks due with in a loop
  --scheduler-cls TEXT     scheduler class to be used.
  --help                   Show this message and exit.
```

#### --scheduler-cls

set this option to use customized Scheduler class


fetcher
-------

```
Usage: pyspider fetcher [OPTIONS]

Options:
  --xmlrpc / --no-xmlrpc
  --xmlrpc-host TEXT
  --xmlrpc-port INTEGER
  --poolsize INTEGER      max simultaneous fetches
  --proxy TEXT            proxy host:port
  --user-agent TEXT       user agent
  --timeout TEXT          default fetch timeout
  --fetcher-cls TEXT      Fetcher class to be used.
  --help                  Show this message and exit.
```

#### --proxy

Default proxy used by fetcher, can been override by `self.crawl` option. [DOC](apis/self.crawl/#fetch)


processor
---------

```
Usage: pyspider processor [OPTIONS]

Options:
  --processor-cls TEXT  Processor class to be used.
  --help                Show this message and exit.
```

result_worker
-------------

```
Usage: pyspider result_worker [OPTIONS]

Options:
  --result-cls TEXT  ResultWorker class to be used.
  --help             Show this message and exit.
```


webui
-----

```
Usage: pyspider webui [OPTIONS]

Options:
  --host TEXT            webui bind to host
  --port INTEGER         webui bind to host
  --cdn TEXT             js/css cdn server
  --scheduler-rpc TEXT   xmlrpc path of scheduler
  --fetcher-rpc TEXT     xmlrpc path of fetcher
  --max-rate FLOAT       max rate for each project
  --max-burst FLOAT      max burst for each project
  --username TEXT        username of lock -ed projects
  --password TEXT        password of lock -ed projects
  --need-auth TEXT       need username and password
  --fetcher-cls TEXT     Fetcher class to be used.
  --webui-instance TEXT  webui Flask Application instance to be used.
  --help                 Show this message and exit.
```

#### --cdn

JS/CSS libs cdn service, URL must compatible with [cdnjs](https://cdnjs.com/)

#### --fercher-rpc

XML-RPC path uri for fetcher XMLRPC server. If not set, use a Fetcher instance.

#### --need-auth

If ture, all pages of webui need a basic auth with the `--username` and `--password`.