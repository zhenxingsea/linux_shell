`
```
#!/bin/sh
### BEGIN INIT INFO
# Provides:          my_service
# Required-Start:    $local_fs $remote_fs $network $syslog $named
# Required-Stop:     $local_fs $remote_fs $network $syslog $named
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# X-Interactive:     true
# Short-Description: my service model
# Description:       Start My service model
#  This script will start the apache2 web server.
### END INIT INFO
DESC="this is my service model"
NAME=/ss/server/GsSyslog
SETUP=/ss/server/config/syslog/config.ini
LOGGER=/ss/server/config/syslog/logger.yml


. /lib/lsb/init-functions

start()
{
        if test $(ps -ef|awk '{print $2" "$8}'|grep -w "$NAME"|awk '{print $1}'|wc -l) -eq 0
        then
                        $NAME $LOGGER $SETUP >/dev/null 2>&1 &
                if test $(ps -ef|awk '{print $2" "$8}'|grep -w "$NAME"|awk '{print $1}'|wc -l) -eq 0
                then
                        echo "$NAME start fial."
                else
                        echo "$NAME is started."
                fi
        else
                echo "$NAME is started."
        fi
}
stop()
{   if test $(ps -ef|awk '{print $2" "$8}'|grep -w "$NAME"|awk '{print $1}'|wc -l) -eq 0
    then
                echo "$NAME is not running."
    else
                                for line in $(ps -ef|awk '{print $2" "$8}'|grep -w "$NAME"|awk '{print $1}'|cat)
                                do
                                        echo $line
                                        kill -9 $line
                                done
                if test $(ps -ef|awk '{print $2" "$8}'|grep -w "$NAME"|awk '{print $1}'|wc -l) -eq 0
                then
                        echo "$NAME is stoped."
                else
                        echo "$NAME stop fial."
                fi
        fi
}
restart()
{
    if test $(ps -ef|awk '{print $2" "$8}'|grep -w "$NAME"|awk '{print $1}'|wc -l) -eq 0
        then
        start
    else
        stop
        start
    fi
}
status()
{
        if test $(ps -ef|awk '{print $2" "$8}'|grep -w "$NAME"|awk '{print $1}'|wc -l) -eq 0
        then
        echo "$NAME is stop."
    else
        echo "$NAME is running."
    fi
}
console()
{
    $NAME $LOGGER $SETUP
}
case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    restart)
        restart
        ;;
    status)
        status
        ;;
    console)
        console
        ;;
    *)
        echo "Usage: service $EXEC {start|stop|restart|console|status}"
        exit 1
esac
exit $?

```
`
