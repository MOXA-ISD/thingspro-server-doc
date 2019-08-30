# Security Tips

## Firewall / Used Ports

### Firewall

#### Outbound
- allow all

#### Inbound
| Description    | Port |   |
|----------------|------|---|
| HTTP           | 80   |   |
| HTTPS          | 443  |   |
| Remote Control | 8883 |   |

## Sensitive information
- .env (recommend: `chmod 600 .env`, only root user could access this file)
- /var/log (logs may contains psk or other sensitive data)

## HTTPS certificate

- Generate a self-signed certificate
```
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
	-keyout ssl.key \
	-out ssl.crt

openssl dhparam -out dhparam.pem 2048

```
