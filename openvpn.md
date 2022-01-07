# OpenVPN

## Server

### Generate all server keys and certs
```sh
apt install openvpn -y

cd /etc/openvpn
make-cadir easy-rsa

cd /etc/openvpn/easy-rsa
./easyrsa init-pki
./easyrsa build-ca nopass
./easyrsa gen-req server nopass
./easyrsa sign-req server server
./easyrsa gen-dh

openvpn --genkey --secret ta.key
```

### Configure server (`/etc/openvpn/server.conf`)
```config
server 10.8.0.0 255.255.255.0
proto tcp 
port 80
dev tun

ca /etc/openvpn/easy-rsa/pki/ca.crt
cert /etc/openvpn/easy-rsa/pki/issued/server.crt
key /etc/openvpn/easy-rsa/pki/private/server.key  # This file should be kept secret
dh /etc/openvpn/easy-rsa/pki/dh.pem

push "redirect-gateway def1"
push "dhcp-option DNS 8.8.8.8"
keepalive 10 120
tls-auth ta.key 0 # This file is secret
cipher AES-256-CBC
user nobody
group nogroup
persist-key
persist-tun
status /var/log/openvpn/openvpn-status.log
log         /var/log/openvpn/openvpn.log
log-append  /var/log/openvpn/openvpn.log
verb 3
# explicit-exit-notify 1 # isn't supported in tcp mode
```

Start and enable service:
```sh
systemctl start openvpn@server
systemctl enable openvpn@server
```

### Enable VPN traffic routing

```sh
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o enp1s0 -j MASQUERADE
```
See also: https://openvpn.net/community-resources/how-to/#redirect

Enable IPv4 forwarding:
```sh
# uncomment in /etc/sysctl.conf
net.ipv4.ip_forward=1
```

## Client

### Generate client keys on the server

```sh
cd /etc/openvpn/easy-rsa

./easyrsa gen-req client nopass
./easyrsa sign-req client client

cp pki/issued/client.crt /etc/openvpn/client/
cp pki/private/client.key /etc/openvpn/client/
```

### Create client configuration

Save into `client.ovpn`:
```config
client
dev tun
proto tcp
remote VPN_SERVER_PUBLIC_ADDR 80
redirect-gateway def1
resolv-retry infinite
nobind
user nobody
group nogroup
persist-key
persist-tun
remote-cert-tls server
cipher AES-256-CBC
verb 3
key-direction 1

# Fill from /etc/openvpn/easy-rsa/pki/issued/client.crt
<cert>
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
</cert>
# Fill from /etc/openvpn/easy-rsa/pki/issued/client.key
<key>
-----BEGIN PRIVATE KEY-----
...
-----END PRIVATE KEY-----
</key>

# Fill from /etc/openvpn/easy-rsa/pki/ca.crt
<ca>
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
</ca>
# Fill from /etc/openvpn/ta.key
<tls-auth>
-----BEGIN OpenVPN Static key V1-----
...
-----END OpenVPN Static key V1-----
</tls-auth>
```
