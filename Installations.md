# Oracle 19c on Ubuntu with Docker
Includes installation steps and user info for Oracle Database. Start with ***sudo su***.

## Installing Git
Original Git [website](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) has all the information but single line of code is enough actually.
> sudo apt install git-all

## Installing Docker
[This](https://sistemdostu.com/ubuntu-20-04-docker-kurulumu/) guy's guide has everything needed.

## Installing Oracle Database 19c
[This](https://mkc110891.medium.com/oracle-19c-on-ubuntu-18-04-docker-6898cd2916f9) guy's guide is useful but needed some arrangements.
- At 5th step, I used the oracle files in **var** instead of **opt**.
- User info doesn't show up in the terminal so I changed password to **oracle**.

## Oracle Enterprise Manager Database Express Info
**URL:** [Here](https://localhost:5500/em/shell)
**Username:** system
**Password:** oracle
**Container:** orclpdb1
- Change password by:
> `docker exec oracle19c ./setPassword.sh NewPassword`

## Docker Container Commands
**Version Control**
> `docker version`

**Show Images**
> `docker images`

**Show Containers**
> `docker ps -a`

**Start Database**
> `docker start oracle19c1`

**Connect Database** (Once?)
> `docker exec -ti oracle19c1 sqlplus system/oracle@orclpdb1`

## DBeaver Oracle Connection Info
**Host:** localhost
**Port:** 1521
**Database:** ORCLCDB
**Username:** system
**Password:** oracle
**Role:** Normal
