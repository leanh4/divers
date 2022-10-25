
An excellent article abour Unix Socket
[Interprocess Communication With Unix Sockets](https://www.baeldung.com/linux/communicate-with-unix-sockets)

A socket in Linux is a bidirectional communication pipe. Unlike standard FIFOs or pipes, 
work with sockets is done using the sockets interface as opposed to the file interface.

To deal with socket, one can use the `nc` command, which is short for `netcat`. The `netcat` utility can be used for many tasks involving networking in Linux.
```bash
nc -U /tmp/demo.sock -l
```

- -U parameter tells netcat to use a Unix Socket file, which we have specified. 
- -l parameter tells it to act as the server-side and listen on the specified socket for incoming connections.

Unix socket can be requested with
- netcat/nc
- socat
- curl


For instance,
### Zookeeper
```bash
echo stat | nc <zookeeper ip> 2181
echo mntr | nc <zookeeper ip> 2181
echo isro  | nc <zookeeper ip> 2181
```
One other way would be to use 4 letter commands to validate if zookeeper service is healthy or not
More details on the documentation link below https://zookeeper.apache.org/doc/r3.1.2/zookeeperAdmin.html#sc_zkCommands

### Docker
[cf . Docker Socket File for IPC](https://lobster1234.github.io/2019/04/05/docker-socket-file-for-ipc/=)
#### - Socat version
```bash 
echo -e "GET /images/json HTTP/1.1\r\n" | socat unix-connect:/var/run/docker.sock STDIO
```

#### - nc version (netcat-freebsd)
```bash 
echo -e "GET /images/json HTTP/1.0\r\n" | nc -U /var/run/docker.sock
```
#### - curl
```bash 
curl --unix-socket /var/run/docker.sock http:/images/json
```

