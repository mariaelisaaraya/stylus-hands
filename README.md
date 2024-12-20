# **Arbitrum Node Setup Guide**

Este documento describe los pasos necesarios para configurar y ejecutar un nodo de Arbitrum, incluyendo la instalación de Rust, Docker, y cargo-stylus, así como la solución de problemas encontrados durante el proceso.

---

## **Requisitos Previos**

- **Sistema operativo:** Ubuntu ejecutándose en un entorno virtual dentro de Windows.  
- **Virtualización habilitada:** Activa la virtualización en tu BIOS si aún no lo has hecho.  
- **Docker instalado y funcionando:** Docker será necesario para ejecutar el nodo de Arbitrum.  

---

## **Pasos para Configurar el Nodo**

### **1. Configurar Docker**

1. Instala Docker y asegúrate de que está funcionando correctamente:  
   ```bash
   sudo apt update
   sudo apt install docker.io
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

2. Verifica la instalación de Docker:  
   ```bash
   docker --version
   ```

---

### **2. Instalar Rust**

Rust es necesario para herramientas relacionadas con el nodo de Arbitrum.

#### **2.1 Instalar Rust usando Rustup**  
Instala Rust ejecutando el siguiente comando:  
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
Sigue las instrucciones y luego reinicia tu terminal o ejecuta:  
```bash
source $HOME/.cargo/env
```

Verifica la instalación:  
```bash
rustc --version
```

#### **2.2 Actualizar Rust**  
Si necesitas una versión específica de Rust (como 1.81.0):  
```bash
rustup install 1.81.0
rustup default 1.81.0
```

Confirma que la versión es correcta:  
```bash
rustc --version
```

---

### **3. Instalar Dependencias de Sistema**

Antes de instalar cargo-stylus, instala las herramientas necesarias:  
```bash
sudo apt update
sudo apt install pkg-config libssl-dev
```

---

### **4. Instalar cargo-stylus**

Instala cargo-stylus con:  
```bash
cargo install cargo-stylus
```

Verifica la instalación:  
```bash
cargo stylus --version
```

---

### **5. Configurar y Ejecutar el Nodo de Arbitrum**

Usa Docker para descargar y ejecutar la imagen del nodo de Arbitrum.

#### **5.1 Descargar la imagen del [nodo](https://github.com/OffchainLabs/nitro-devnode)**   
```bash
docker pull offchainlabs/nitro-dev-node
```

#### **5.2 Ejecutar el contenedor del [nodo](https://github.com/OffchainLabs/nitro-devnode)**  
```bash
docker run -d -p 8547:8547 offchainlabs/nitro-dev-node
```
Esto iniciará el nodo en el puerto `8547`.

#### **5.3 Verificar que el nodo está corriendo:**
   ```bash
   curl http://localhost:8547
   ```
   Deberías recibir una respuesta indicando que el nodo está en funcionamiento.

#### **5.4 Configuración del Nodo y Resultados**

El nodo Nitro dev se configura correctamente con los siguientes parámetros:
- **Chain ID:** 412346
- **Base de datos:** Pebble (por defecto)
- **Bloque Génesis:** Hash = ada78e..915675

#### **5.5  Despliegue de Contratos**
- **Cache Manager** desplegado en la dirección `0x11...e49`.
- El contrato fue registrado exitosamente como **WASM Cache Manager**.

## Uso

### Conexión al nodo
Puedes conectarte al nodo local utilizando herramientas como `Hardhat`, `Remix` o `Web3.js`. Asegúrate de usar la dirección `http://localhost:8547` como punto de conexión.

### Ejemplo con Web3.js
```javascript
const Web3 = require('web3');
const web3 = new Web3('http://localhost:8547');

web3.eth.getBlockNumber().then(console.log);
```

## Problemas conocidos
- La advertencia `parent-chain-is-arbitrum` puede aparecer durante la configuración. Esto no afecta la funcionalidad del nodo, pero en futuras actualizaciones podrías incluir esta configuración.
- El mensaje `feedOneMsg failed to send` ocurre al inicializar el nodo, pero es inofensivo.
- `parent-chain-is-arbitrum`: Informativa, no afecta al funcionamiento actual.
- `feedOneMsg failed to send`: Ocurre al inicializar pero no genera errores críticos.
---

## **Solución de Problemas**

### **Error: pkg-config not found**  
Si encuentras este error, instala la herramienta:  
```bash
sudo apt install pkg-config
```

### **Error: OpenSSL not found**  
Si falta OpenSSL o sus dependencias de desarrollo:  
```bash
sudo apt install libssl-dev
```

### **Rutas incorrectas en Rust**  
Si hay problemas con las rutas, asegúrate de que `$HOME/.cargo/bin` está en tu variable PATH:  
```bash
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

---
