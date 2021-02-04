# Neo4j

## [Install](https://neo4j.com/docs/operations-manual/current/installation/linux/)

### JDK 11

```bash
sudo apt install -y openjdk-11-jdk-headless
```

### Add the repository

```bash
VERSION="4.2"
wget -O - https://debian.neo4j.com/neotechnology.gpg.key | sudo apt-key add -
echo "deb https://debian.neo4j.com stable $VERSION" | sudo tee -a /etc/apt/sources.list.d/neo4j.list
sudo apt update
```

Enable `universe` repository:

```bash
# Depends: daemon but it is not installable
sudo add-apt-repository universe
```

List:

```bash
apt list -a neo4j

Listing... Done
neo4j/stable 1:4.2.3 all
neo4j/stable 1:4.2.2 all
neo4j/stable 1:4.2.1 all
neo4j/stable 1:4.2.0 all
```

### Install

```bash
sudo apt install -y neo4j=1:4.2.3
```

### File locations

[File locations](https://neo4j.com/docs/operations-manual/current/configuration/file-locations)

- [/etc/neo4j/neo4j.conf](https://neo4j.com/docs/operations-manual/current/configuration/neo4j-conf/)

### IP setup

```bash
sudo vi /etc/neo4j/neo4j.conf
```

```bash
dbms.default_listen_address=0.0.0.0
```

### Admin

- [Admin](https://neo4j.com/docs/operations-manual/current/tools/neo4j-admin/)
- [Administration](https://neo4j.com/docs/cypher-manual/current/administration/)

```bash
sudo neo4j-admin set-initial-password 1234
```

### Operation: systemd

```bash
sudo systemctl enable neo4j
sudo systemctl start neo4j
sudo systemctl status neo4j
```

### Log

```bash
sudo journalctl -e -u neo4j
```

---

## Browser

[http://192.168.50.10:7474/browser/](http://192.168.50.10:7474/browser/)

### Server Connect

```sql
:server connect
```

- Connect URL: `neo4j://192.168.50.10:7687`
- Database - leave empty for default
- Authentication type: `Username / Password`
- Username: `neo4j`
- Password: `1234`
