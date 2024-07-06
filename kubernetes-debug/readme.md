# Kubernetes debug

## Содержание
- [посмотреть логи на ноде ](#p)





## 🧐 Посмотреть логи на ноде  <a name = "p"></a>

```
kubectl  debug node/cl1vmt8d77ghvdn8rr0v-ujed -it --image=busybox
cat /host/var/log/pods/default_node-debugger-cl1vmt8d77ghvdn8rr0v-ujed-4254n_59ba4144-aeda-43b8-a0e1-956e70b206df/debugger/0.log
```


## 🧐 directory  <a name = "p"></a>

```
kubectl debug -n default nginx --image=busybox -ti --target=nginx-distroless
```

```
ls -la proc/1/root/etc/nginx/


total 48
drwxr-xr-x    1 root     root          4096 Apr 17 16:53 .
drwxr-xr-x    1 root     root          4096 Apr 18 09:07 ..
drwxr-xr-x    1 root     root          4096 Apr 18 09:07 conf.d
-rw-r--r--    1 root     root          1007 Apr 16 14:29 fastcgi_params
-rw-r--r--    1 root     root          5349 Apr 16 14:29 mime.types
lrwxrwxrwx    1 root     root            22 Apr 16 15:39 modules -> /usr/lib/nginx/modules
-rw-r--r--    1 root     root           648 Apr 16 15:39 nginx.conf
-rw-r--r--    1 root     root           636 Apr 16 14:29 scgi_params
-rw-r--r--    1 root     root           664 Apr 16 14:29 uwsgi_params
```


## 🧐 tcpdump  <a name = "p"></a>

```
kubectl debug -n default nginx --image=nicolaka/netshoot -ti --target=nginx-distroless




app-deployment-75c457977f-lkg69  ~  tcpdump -nn -i any -e port 80

tcpdump: data link type LINUX_SLL2
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on any, link-type LINUX_SLL2 (Linux cooked v2), snapshot length 262144 bytes
10:05:47.075001 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 80: 127.0.0.1.58830 > 127.0.0.1.80: Flags [S], seq 771759953, win 65495, options [mss 65495,sackOK,TS val 27479336 ecr 0,nop,wscale 7], length 0
10:05:47.075008 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 80: 127.0.0.1.80 > 127.0.0.1.58830: Flags [S.], seq 1484136602, ack 771759954, win 65483, options [mss 65495,sackOK,TS val 27479336 ecr 27479336,nop,wscale 7], length 0
10:05:47.075015 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 72: 127.0.0.1.58830 > 127.0.0.1.80: Flags [.], ack 1, win 512, options [nop,nop,TS val 27479336 ecr 27479336], length 0
10:05:47.192972 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 151: 127.0.0.1.58830 > 127.0.0.1.80: Flags [P.], seq 1:80, ack 1, win 512, options [nop,nop,TS val 27479454 ecr 27479336], length 79: HTTP: GET / HTTP/1.1
10:05:47.192984 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 72: 127.0.0.1.80 > 127.0.0.1.58830: Flags [.], ack 80, win 511, options [nop,nop,TS val 27479454 ecr 27479454], length 0
10:05:47.193062 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 310: 127.0.0.1.80 > 127.0.0.1.58830: Flags [P.], seq 1:239, ack 80, win 512, options [nop,nop,TS val 27479454 ecr 27479454], length 238: HTTP: HTTP/1.1 200 OK
10:05:47.193072 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 72: 127.0.0.1.58830 > 127.0.0.1.80: Flags [.], ack 239, win 511, options [nop,nop,TS val 27479455 ecr 27479454], length 0
10:05:47.193082 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 687: 127.0.0.1.80 > 127.0.0.1.58830: Flags [P.], seq 239:854, ack 80, win 512, options [nop,nop,TS val 27479455 ecr 27479455], length 615: HTTP
10:05:47.193084 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 72: 127.0.0.1.58830 > 127.0.0.1.80: Flags [.], ack 854, win 507, options [nop,nop,TS val 27479455 ecr 27479455], length 0
10:05:48.274479 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 72: 127.0.0.1.58830 > 127.0.0.1.80: Flags [F.], seq 80, ack 854, win 512, options [nop,nop,TS val 27480536 ecr 27479455], length 0
10:05:48.274526 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 72: 127.0.0.1.80 > 127.0.0.1.58830: Flags [F.], seq 854, ack 81, win 512, options [nop,nop,TS val 27480536 ecr 27480536], length 0
10:05:48.274533 lo    In  ifindex 1 00:00:00:00:00:00 ethertype IPv4 (0x0800), length 72: 127.0.0.1.58830 > 127.0.0.1.80: Flags [.], ack 855, win 512, options [nop,nop,TS val 27480536 ecr 27480536], length 0

```


## 🧐 strace  <a name = "p"></a>


```
kubectl debug -n default nginx --image=quay.io/mwasher/crictl:0.0.1 -ti --target=nginx-distroless -- /bin/sh

```
Эфимерные поды не имеют доступа к процессам и не работают с securityContext

```
/ # strace -p 1
strace: attach: ptrace(PTRACE_SEIZE, 1): Operation not permitted
```


```
securityContext:
      capabilities:
        add:
        - SYS_PTRACE
```