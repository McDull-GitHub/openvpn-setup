#### server
```bash
sudo apt-get install openvpn -y
sudo cp /usr/share/doc/openvpn/examples/sample-keys/ca.crt /etc/openvpn
sudo gunzip -c /usr/share/doc/openvpn/examples/sample-keys/server.crt.gz > /tmp/server.crt
sudo mv /tmp/server.crt /etc/openvpn/
sudo cp /usr/share/doc/openvpn/examples/sample-keys/server.key /etc/openvpn/
sudo cp /usr/share/doc/openvpn/examples/sample-keys/dh2048.pem /etc/openvpn

cat>/etc/openvpn/server.conf<<EOF
port 1194
proto udp
dev tun
ca ca.crt
cert server.crt
key server.key
dh dh1024.pem
server 10.8.0.0 255.255.255.0
keepalive 10 120
user nobody
group nogroup
persist-key
persist-tun
verb 3
EOF
cd /etc/openvpn
openvpn --config server.config
```

```
cd
sudo cp /usr/share/doc/openvpn/examples/sample-keys/ca.crt ~
sudo gunzip -c /usr/share/doc/openvpn/examples/sample-keys/client.crt.gz > ~/client.crt
sudo cp /usr/share/doc/openvpn/examples/sample-keys/client.key ~
sudo cat>~/client.conf<<EOF
client
dev tun
proto udp
remote x.x.x.x 1194
ca ca.crt
cert client.crt
key client.key
user nobody
group nogroup
persist-key
persist-tun
verb 3
EOF
```
where x.x.x.x is the server's public ip

#### client (macOS)
```zsh
brew install openvpn # run brew info openvpn and see where it's installed
mkdir ./Downloads/openvpn
cd ./Downloads/openvpn # Download client.conf ca.crt client.crt client.key to ~/Downloads/openvpn
sudo openvpn --config config.json
```
