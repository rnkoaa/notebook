#!/bin/sh

hosts=(pi@pi2swarm1.local
pi@pi2swarm2.lcoal
pi@pi2swarm3.lcoal
pi@pi2swarm4.lcoal
pi@pi2swarm5.lcoal
)

exec_cmd(){
    echo "[$1] $2"
    echo ""
    ssh $1 $2
    echo ""
}

for host in "${hosts[@]}"
do
    exec_cmd host "$@"
done


./most "docker swarm leave ; docker swarm join \
            --token <TOKEN> \
            Ip address:port "