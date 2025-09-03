# 📄 Documentación de Infraestructura - Royal Servers

**Fecha:** 2025-06-16

---

## 🖥️ Infraestructura General

### 🔧 Servidores
| Nombre        | IP Interna       | Función                       |
|---------------|------------------|-------------------------------|
| Apache Proxy  | 192.168.50.210   | Reverse proxy, VirtualHosts  |
| Streamlit App | 192.168.50.19    | Landing page (en construcción) |
| Ark App       | 192.168.50.220   | Servidor Ark App (AMP)       |

---

## 🌐 Subdominios

| Dominio                    | Redirección Interna           | Servicio                  |
|---------------------------|-------------------------------|---------------------------|
| https://royalservers.com.ar     | 192.168.50.19:3000               | Página de bienvenida (Streamlit) |
| https://ark.royalservers.com.ar | 192.168.50.220:80               | Ark App (AMP)            |
| https://panel.royalservers.com.ar | 192.168.50.210:10000 (proxy) | Webmin                   |
| https://games.royalservers.com.ar | 192.168.50.210:8080 (proxy)  | AMP Admin                |

---

## 🔐 Certificados SSL (Let's Encrypt)

| Dominio                    | Ruta Certificado                                |
|---------------------------|--------------------------------------------------|
| royalservers.com.ar       | /etc/letsencrypt/live/royalservers.com.ar/      |
| ark.royalservers.com.ar   | /etc/letsencrypt/live/ark.royalservers.com.ar/  |

---

## 📁 Apache: `sites-enabled/`

```plaintext
/etc/apache2/sites-enabled/
├── games.royalservers.com.ar.conf
├── panel.royalservers.com.ar.conf
└── royalservers.conf
```

### `royalservers.conf` contiene:

- `VirtualHost *:80` con redirección HTTP → HTTPS para royalservers.com.ar y ark.royalservers.com.ar
- `VirtualHost *:443` para `royalservers.com.ar` apuntando a Streamlit (landing)
- `VirtualHost *:443` para `ark.royalservers.com.ar` apuntando a Ark App

---

## 🌐 pfSense - Reglas NAT activas

| Protocolo | Puerto | Redirección a           | Descripción              |
|-----------|--------|-------------------------|--------------------------|
| TCP/UDP   | 53     | 192.168.50.210:53       | DNS BIND                 |
| TCP       | 80     | 192.168.50.210:80       | HTTP → Apache Reverse Proxy |
| TCP       | 443    | 192.168.50.210:443      | HTTPS → Apache Reverse Proxy |

---

## ✅ Estado

- Apache funcional y estable
- Certificados válidos y activos
- Subdominios correctamente enrutados
- Sin conflictos ni archivos de configuración obsoletos

---

## 📝 Mantenimiento sugerido

- Renovar SSL periódicamente con: `certbot renew`
- Verificar estado con: `apachectl -S` y `systemctl status apache2`
- Mantener documentadas reglas de pfSense y servicios activos

