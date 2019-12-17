# Transaction Ops

## Rules
1. Every Operation must has **RollBack Operation**

## Transaction Ops Rule
1. Stages
    1. Backup Stage
        * backup current system state
        * GoTo Next if Success
        * Exit if it fails
    2. PreTest Stage
        * in order to generate test rule
        * make sure system working before apply operation
        * GoTo Next if Success
        * Exit if it fails
    3. Operate Stage
        * do the operation that we want to do
        * GoTo Next if Success
        * GoTo **RollBack Stage** if it fails
    4. PostTest Stage
        * apply custom-built test rule
        * apply test rule generate in **PreTest Stage**
        * GoTo **RollBack Stage** if it fails
    5. RollBack Stage
        * only exec while Failed **PostTest Stage**
        * apply **Backup Stage** to rollback system
2. Use Transaction Ops Framework

## Example
1. Update Network Route
```
#!/bin/bash

set -u

# notify
notify(){
    # do notify
}

# 1. backup network route
backup_route(){
    # do something
}
# 2. generate test-case
generate_testcase(){
    # do something
    # nameserver in /etc/hosts
    # other connection via "contrack" or "ss" command
}
# 3. update network route
update_route(){
    # do something
}
# 4. apply test-case
apply_testcase(){
    # do something
}
# 5. rollback network route
rollback_route(){
    # do something
}

# main
main(){
    backup_route \
        && generate_testcase \
        && update_route \
        && (apply_testcase || rollback_route)
    if [ $? -ne 0 ]
    then
        notify
    fi
}

main
```
