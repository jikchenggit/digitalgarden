---
{"author":"aming","email":"jikcheng@163.com","title":"SHELL test kingbase server ,And Start The Kingbase Database","creation_date":"2022-09-30 11:59","Last modified date":"2022-11-25 16:00","tags":"SHELL test kingbase server ,And Start The Kingbase Database","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux SHELL","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-shell/shell-test-kingbase-server-and-start-the-kingbase-database/","dgPassFrontmatter":true}
---



```bash
#!/bin/bash -
#===============================================================================
#
#          FILE: startup_check_kingbase.sh
#
#         USAGE: su - kingbase or su - root
#               ./startup_check_kingbase.sh
#
#   DESCRIPTION: Check whether the database is started. 
#                1. If it's runing ,ouput status info 
#                2. If it's notruning , start the database server
#
#       OPTIONS: ---
#  REQUIREMENTS: root user or kingbase user
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: kingbase 
#  ORGANIZATION: zh
#       CREATED: 09/27/2022 10:26:43 AM
#      REVISION:  v0.1
#===============================================================================

set -o nounset                                  # Treat unset variables as an error


#-------------------------------------------------------------------------------
# Example Set script running information
#-------------------------------------------------------------------------------
export KINGBASE_HOME=/KingbaseES/V8/Server
export KINGBASE_DATA=/data
export PATH=$PATH:/KingbaseES/V8/Server/bin
export KINGBASE_PORT=54321

#-------------------------------------------------------------------------------
# Set Database installation information 
#-------------------------------------------------------------------------------

ROOT_UID=0
INSTALLDIR=/KingbaseES/V8
USERNAME=kingbase
DATADIR="/data"
VERSION=V8
SERVICENAME=kingbase8d


#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  function kingbase_user
#   DESCRIPTION:  Checks whether the current user is Kingbase,and start database
#    PARAMETERS:  None
#       RETURNS:  None
#-------------------------------------------------------------------------------
function kingbase_user {
pid=`sys_ctl status  -D $KINGBASE_DATA | grep PID:  | sed {s/\(//} | sed {s/\)//} | awk '{print $6}'`
if [ "$pid" == ""  ]
then
    sys_ctl start -D /data
else
    sys_ctl status -D /data
fi
}

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  function root_user
#   DESCRIPTION:  Checks whether the current user is root,and startup database
#    PARAMETERS:  None
#       RETURNS:  None
#-------------------------------------------------------------------------------

function root_user {
pid=`service $SERVICENAME status | grep PID:  | sed {s/\(//} | sed {s/\)//} | awk '{print $6}'`

if [ "$pid" == ""  ]
then
    service $SERVICENAME start
else
    service $SERVICENAME status
fi
}


#------- MAIN------------------------------------------------------------------------
# Run as kingbase or root user, of course.
# main function
#-------------------------------------------------------------------------------
if [ x"$UID" ==  x"$ROOT_UID" ]
then
    echo "PROMOT:start or check as user root."
    root_user
else
    echo "PROMOT:start or check as user kingbase."
    kingbase_user
fi
```