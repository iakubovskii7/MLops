sudo service jupyterhub status - посмотреть почему упало и упало ли

sudo service jupyterhub restart - перезапустить

killall -u $USER - прибить свои процессы

ps ax -o pid,ni,cmd,user | grep jupyter - Посмотреть приоритеты текущих задач в юпитере по пользователю

for pid in `pgrep -f jupyter -u $USER`; do { sudo renice 19 $pid; }; done

Изменение приоритета процессов, меняет для текущего юзера, процессы которые содержат юпитер, можно модифицировать под люббые задачи

Печатает загрузку проца по юзерам

#!/bin/sh
#
# print total CPU usage in percent of all available users
# but skips the ones with a CPU usage of zero
#
# 1st column: user
# 2nd column: aggregated CPU usage
# 3rd column: normalized CPU usage according to the number of cores
#
# to sort by CPU usage, pipe the output to 'sort -k2 -nr'
#

set -e

own=$(id -nu)
cpus=$(lscpu | grep "^CPU(s):" | awk '{print $2}')
echo $cpus
for user in $(getent passwd | awk -F ":" '{print $1}' | sort -u)
do
    # print other user's CPU usage in parallel but skip own one because
    # spawning many processes will increase our CPU usage significantly
    if [ "$user" = "$own" ]; then continue; fi
    (top -b -n 1 -u "$user" | awk -v user=$user -v CPUS=$cpus 'NR>7 { sum += $9; } END { if (sum > 0.0) print user, sum, sum/CPUS; }') &
    # don't spawn too many processes in parallel
    sleep 0.05
done
wait

# print own CPU usage after all spawned processes completed
top -b -n 1 -u "$own" | awk -v user=$own -v CPUS=$cpus 'NR>7 { sum += $9; } END { print user, sum, sum/CPUS; }'

60

pleshko 250 4.16667

root 5.9 0.0983333

irmatov 12.5 0.208333

Пример вывода. Первая цифра процент загрузки, вторая цифра процент от общедоступного процессорного времени

