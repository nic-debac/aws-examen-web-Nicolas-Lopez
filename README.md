# Examen AWS

## Pasos realizados

### 1 Creación de la VPC
- Nombre: `mi-vpc-TuNombre-Apellidos`
- CIDR Block: `10.0.0.0/16`

  ![VPC] (./img/VPC.jpg)

- Subredes:
  - `subnet-linux`: `10.0.1.0/24`
  - `subnet-windows`: `10.0.2.0/24`
 
  ![SUBNETS] (./img/SUBNETS.jpg)

- Creación de una Internet Gateway y configuración de la tabla de rutas.

![IGW] (./img/IGW)

![Tabla de rutas] (./img/ROUTE_TABLE.jpg)

### 2 Creación de Instancias EC2
#### - Instancia Windows
- Sistema Operativo: Windows Server 2022
- Tipo: `t3.micro`
- Security Group:
  - HTTP (80)
  - Vite (5173)
  - RDP (3389)

#### - Instancia Linux
- Sistema Operativo: Ubuntu 22.04
- IP pública asignada
- Security Group:
  - HTTP (80)
  - Vite (5173)
  - SSH (22)

  ![Instancias] (./img/INSTANCES_1.jpg)

  ![Instancias] (./img/INSTANCES_2.jpg)

### 3 Instalación y Despliegue Web
#### Windows
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force 
[System.Net.ServicePointManager]::SecurityProtocol = 'Tls12'
Invoke-WebRequest -Uri https://nodejs.org/dist/v20.11.0/node-v20.11.0-x64.msi -OutFile node-installer.msi 
Start-Process msiexec.exe -Wait -ArgumentList '/I node-installer.msi /quiet'
 
npm install -g vite serve

mkdir web-tunombre-apellidos

cd web-tunombre-apellidos

npm init vite@latest .

npm install

npm run build

serve -s dist -l 5173
```
![Windows] (./img/Windows_1.jpg)

![Windows] (./img/Windows_2.jpg)

#### Linux (Ubuntu SSH)
```bash
sudo apt update

sudo apt install nodejs npm -y

sudo npm install -g vite serve

mkdir web-tunombre-apellidos && cd web-tunombre-apellidos

npm init vite@latest .

npm install

npm run build

serve -s dist -l 5173
```
![VPC] (./img/Ubuntu_2.jpg)

![VPC] (./img/Ubuntu_3.jpg)

### 4 Configuración de Security Groups
| Tipo  | Puerto | Origen |
|-------|--------|--------|
| HTTP  | 80     | 0.0.0.0/0 |
| Vite  | 5173   | 0.0.0.0/0 |
| SSH   | 22     | Tu IP pública (solo Linux) |
| RDP   | 3389   | Tu IP pública (solo Windows) |

### 5 Pruebas y Capturas
Acceder a la web desde el navegador:
```
http://10.0.2.206:5173

http://10.0.1.206:5173
```

![Windows] (./img/Windows_3.jpg)

---

