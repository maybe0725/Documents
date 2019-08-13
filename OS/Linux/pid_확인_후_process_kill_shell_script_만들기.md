## PID 확인하기.

``````````````````````````````````````````````````````````````````
ps -ef | grep esp-iomanage
ps -ef | grep esp-iomanage | awk '{print $1, $2, $3, $8, $10}'

PID=`ps -ef | grep esp-iomanage | awk '{print $2}'`
echo $PID
``````````````````````````````````````````````````````````````````

## PID kill shell script

``````````````````````````````````````````````````````````````````
#/bin/sh

echo 'esp-iomanage kill...'
kill -9 `ps -ef | grep esp-iomanage | awk '{print $2}'`
echo 'esp-iomanage kill... success'
``````````````````````````````````````````````````````````````````
