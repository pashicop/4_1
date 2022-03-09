# 4_1
# 1
```
vagrant@vagrant:~/bash$ a=1
vagrant@vagrant:~/bash$ b=2
vagrant@vagrant:~/bash$ c=a+b
vagrant@vagrant:~/bash$ d=$a+$b
vagrant@vagrant:~/bash$ e=$(($a+$b))
vagrant@vagrant:~/bash$ echo c
c
vagrant@vagrant:~/bash$ echo $c
a+b
vagrant@vagrant:~/bash$ echo $d
1+2
vagrant@vagrant:~/bash$ echo $e
3
```
|Переменная	|Значение	|Обоснование|
|:---------:|:-------:|-----------|
|c| a+b |	Выводит просто строку |
|d|	1+2 | выводит строку, включающую значения переменных из-за '$' перед 'a' и 'b'  |
|e|	3  	| выводит результат сложения переменных, так как конструкция $(()) позволяет вычислить значения операций внутри скобок |
# 2
добавил sleep 5, чтобы лог-файл не заполнялся быстро. Добавил break для корректного выхода из while.
```
while ((1==1))
do
        echo start
        curl https://localhost:4757
        if (($? != 0))
        then
                date >> curl.log
                sleep 5
        else
                echo end
                break
        fi
done
```
# 3
```
vagrant@vagrant:~/bash$ cat 3.sh
#!/usr/bin/env bash
#ip_1=192.168.0.1
#ip_2=173.194.222.113
#ip_3=87.250.250.242
/dev/null > log
array=( "192.168.0.1" "173.194.222.113" "87.250.250.242" "192.168.0.55")
for n in ${array[@]}
do
        for i in {1..5}
        do
                nc -zvw3 $n 80
                if (($? != 0))
                then
                        echo "$n порт 80 НЕДОСТУПЕН!" >> log
                else
                        echo "$n порт 80 ДОСТУПЕН!" >> log
                fi
        done
done
exit
vagrant@vagrant:~/bash$ cat log
192.168.0.1 порт 80 ДОСТУПЕН!
192.168.0.1 порт 80 ДОСТУПЕН!
192.168.0.1 порт 80 ДОСТУПЕН!
192.168.0.1 порт 80 ДОСТУПЕН!
192.168.0.1 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
192.168.0.55 порт 80 НЕДОСТУПЕН!
192.168.0.55 порт 80 НЕДОСТУПЕН!
192.168.0.55 порт 80 НЕДОСТУПЕН!
192.168.0.55 порт 80 НЕДОСТУПЕН!
192.168.0.55 порт 80 НЕДОСТУПЕН!
vagrant@vagrant:~/bash$
```
# 4
Изменил скрипт
```
vagrant@vagrant:~/bash$ cat 3.sh
#!/usr/bin/env bash
#ip_1=192.168.0.1
#ip_2=173.194.222.113
#ip_3=87.250.250.242
echo "Start log" > log
echo "" > error
array=( "173.194.222.113" "87.250.250.242" "10.1.4.81")
while (( 1==1 ))
do
        for n in ${array[@]}
        do
                nc -zvw3 $n 80
                if (($? != 0))
                then
                        echo "$n порт 80 НЕДОСТУПЕН!" >> log
                        echo "$n" > error
                        exit
                else
                        echo "$n порт 80 ДОСТУПЕН!" >> log
                        sleep 3
                fi
        done
done
exit
vagrant@vagrant:~/bash$ vagrant@vagrant:~/bash$ ./3.sh
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
Connection to 10.1.4.81 80 port [tcp/http] succeeded!
Connection to 173.194.222.113 80 port [tcp/http] succeeded!
Connection to 87.250.250.242 80 port [tcp/http] succeeded!
nc: connect to 10.1.4.81 port 80 (tcp) timed out: Operation now in progress
vagrant@vagrant:~/bash$ cat log
Start log
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 ДОСТУПЕН!
173.194.222.113 порт 80 ДОСТУПЕН!
87.250.250.242 порт 80 ДОСТУПЕН!
10.1.4.81 порт 80 НЕДОСТУПЕН!
vagrant@vagrant:~/bash$ cat error
10.1.4.81
vagrant@vagrant:~/bash$
```





