<div align="center">

# 🔐 Security Practices

**Prácticas de seguridad informática: criptografía, certificados, HTTPS, firma electrónica, integridad, hardening y análisis de tráfico TLS**

[![OpenSSL](https://img.shields.io/badge/OpenSSL-721412?style=flat&logo=openssl&logoColor=white)]()
[![GnuPG](https://img.shields.io/badge/GnuPG-0093DD?style=flat&logo=gnuprivacyguard&logoColor=white)]()
[![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat&logo=wireshark&logoColor=white)]()
[![Linux](https://img.shields.io/badge/Linux-A81D33?style=flat&logo=debian&logoColor=white)]()

[Descripción](#-sobre-el-proyecto) · [Las ocho prácticas](#-las-ocho-prácticas) · [Ejemplos](#-ejemplos) · [Estructura](#-estructura-del-repositorio) · [Stack](#-stack-tecnológico) · [Notas](#-notas)

</div>

---

## 📖 Sobre el proyecto

Las **ocho prácticas** de la asignatura de *Seguridad en Sistemas Informáticos*: desde cifrar un fichero con OpenSSL hasta analizar con Wireshark qué se dicen un navegador y un servidor cuando abren una conexión segura. El objetivo no era solo entender la teoría, sino **usar las herramientas reales** y ver cómo se comportan.

> 📌 **Proyecto académico** de la asignatura de *Seguridad en Sistemas Informáticos* (curso 2024-2025, Universidad de Salamanca), realizado junto a **Alberto García Martín** ([@albergm02](https://github.com/albergm02)).

---

## 🧭 Las ocho prácticas

| # | Práctica | Qué se hizo |
| :---: | :--- | :--- |
| **1** | **Cifrado** | **Simétrico** con OpenSSL (DES, AES-256, RC2; modos CBC, CFB, ECB y OFB), **asimétrico** con GnuPG (ElGamal) y OpenSSL (RSA) y **firmas** separadas y del compendio SHA-256. |
| **2** | **Certificados X.509** | Certificados con **CAcert**: instalar la raíz, solicitar un certificado personal, generar una CSR con OpenSSL, firmar y cifrar correo. |
| **3** | **Web seguro** | Servidor **Apache con HTTPS** y **CA propia**: certificado del servidor, certificados personales y autenticación de clientes por certificado. |
| **4** | **Firma legal** | Formatos de firma electrónica avanzada (**AdES-BES, -T, -C, -X**), el marco legal español y firma de documentos con **Autofirma**. |
| **5** | **Vulnerabilidades** | Fuentes (Hispasec, NVD, INCIBE), **CVE y CVSS** (v2 frente a v3), un *exploit* documentado y el **OWASP Top Ten**. |
| **6** | **Integridad y control de acceso** | Sumas **SHA-256** de carpetas, detección de cambios con un **centinela**, y el mismo hash calculado con tres herramientas. |
| **7** | **Configuraciones seguras** | **SSH, FTP/SFTP y HTTPS**, **TCP Wrappers frente a iptables**, y análisis de servicios activos con `netstat`, `nmap` y Angry IP Scanner. |
| **8** | **Tráfico SSL/TLS** | Con **Wireshark**: el *handshake* TLS desde dos navegadores, los paquetes *Client Hello* y *Server Hello* y los *ciphersuites* ofrecidos y elegidos. |

---

## 📚 Ejemplos

### 🔐 Cifrado simétrico con OpenSSL

```bash
# Cifrar con AES-256 en modo CBC (la clave "123" evita pedir la contraseña a mano) y descifrar
openssl enc -aes-256-cbc -in EjA1.txt -out SalidaA1.txt -k 123
openssl enc -d -aes-256-cbc -in SalidaA1.txt -out EjA1_descifrado.txt -k 123
```

El texto cifrado empieza por `Salted__`: OpenSSL añade una sal aleatoria que hace que cifrar dos veces lo mismo dé resultados distintos.

### 🌐 CA propia y certificado de servidor

```bash
# 1. Clave y certificado autofirmado de la CA (10 años)
openssl genrsa -out key.pem 2048
openssl req -x509 -new -nodes -key key.pem -sha256 -days 3650 -out cert.pem

# 2. Clave y solicitud de firma (CSR) del servidor
openssl genrsa -out serverkey.pem 2048
openssl req -new -key serverkey.pem -out servercsr.pem
```

---

## 📂 Estructura del repositorio

```
Security-Practices/
├── 01-cifrado/                      # Salidas de las pruebas de cifrado (apartados A, B y C)
├── 02-certificados-x509/            # Memoria, claves, certificados y CSR (CAcert)
├── 03-web-seguro/                   # Memoria, CA propia y certificados de servidor y de usuario (Apache)
├── 04-firma-legal/ficheros/         # Mensajes de ejemplo de la práctica de firma
├── 05-vulnerabilidades/             # Memoria
├── 06-integridad-control-acceso/    # Memoria, centinelas y hashes
├── 07-configuraciones-seguras/      # Memoria y correcciones
└── 08-trafico-tls-wireshark/        # Memoria y correcciones
```

---

## 🖥️ Stack tecnológico

| Categoría | Herramientas |
| :--- | :--- |
| **Criptografía** | OpenSSL · GnuPG · SHA-256 · DES · AES · RC2 · RSA · ElGamal |
| **Certificados y firma** | X.509 · CAcert · CA propia · Autofirma · AdES |
| **Servidores** | Apache2 · SSH · FTP/SFTP |
| **Red y análisis** | Wireshark · nmap · netstat · iptables · TCP Wrappers |
| **Sistema** | Linux y Windows |

---

## 🎓 Qué aprendí

- Que **cifrar bien es más que elegir un algoritmo**: el modo de operación (ECB, CBC, CFB, OFB) cambia por completo el resultado.
- Cómo **se construye la confianza** en una conexión HTTPS: de la CA al certificado del servidor.
- A **detectar la alteración de ficheros** con sumas hash y un centinela.
- A **leer un handshake TLS** paquete a paquete.

---

## 📝 Notas

- Las **prácticas 2 y 3 se publican completas**: memoria, claves, certificados y peticiones de firma, tal y como se generaron en clase. Los mismos ficheros están también en el repositorio de [Alberto](https://github.com/albergm02/Security-Systems). Se crearon para las prácticas: no los reutilices para nada real.
- En el resto de prácticas **no se incluyen claves, certificados ni capturas de red** (`.pem`, `.key`, `.p12`, `.pfx`, `.pcap`); el `.gitignore` los excluye por defecto.
- Las memorias de las prácticas 1 y 4 no se publican.

---

## 👤 Autoría

**Unai García** y **Alberto García Martín** — Grado en Ingeniería Informática (Universidad de Salamanca)

- 🐙 GitHub: [@unaigb](https://github.com/unaigb)

---

<div align="center">

**⭐ Si este repositorio te resulta útil, considera darle una estrella.**

</div>
