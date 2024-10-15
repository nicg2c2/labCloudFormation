# 📚📂 Plantilla de CloudFormation para Desplegar una Aplicación Web 📚📂

## Descripción 📃

Esta plantilla de CloudFormation permite desplegar una aplicación web en AWS, utilizando los siguientes recursos:

- Un bucket S3 para el almacenamiento y la configuración de un sitio web estático.
- Una VPC para la red virtual.
- Instancias EC2 con Apache para alojar la aplicación.
- Un Auto Scaling Group para manejar la escalabilidad.
- Un Balanceador de Carga (ELB) para distribuir el tráfico.
- Grupo de seguridad
- Outputs del laboratorio

## Recursos Desplegados 📝✅

### 1. Parámetros

- **InstanceType**: Tipo de instancia EC2 a utilizar. Valores permitidos:
  - `t2.micro`
  - `t2.small`
  - `t3.micro`
  - `t3.small`

- **KeyName**: Nombre del par de claves SSH existente para conectarse a la instancia EC2, es de aclarar que esta Key se debe generar y asignar antes de ejecutar la plantilla de CloudFormation

### 2. Mappings

- **RegionMap**: Mapa de AMIs que define la AMI a utilizar según la región (ej. `us-east-1`, `us-east-2`).

### 3. Recursos

- **S3 Bucket**: Se crea un bucket S3 configurado para alojamiento de sitio web.
  - **Documentos**: `index.html` (documento principal) y `error.html` (documento de error).
  
- **Bucket Policy**: Permite las acciones `s3:PutObject` y `s3:GetObject` para todos los usuarios.

- **VPC**: Se crea una VPC con soporte para DNS.

- **Internet Gateway**: Permite que los recursos en la VPC se comuniquen con Internet.

- **Route Table**: Crea una tabla de rutas asociada a la VPC.

- **Subredes**: Dos subredes públicas en diferentes zonas de disponibilidad.

- **Security Group**: Configura reglas de entrada para permitir tráfico HTTP, HTTPS y SSH.

- **Instancia EC2**: Se lanza una instancia EC2 con Apache y se configura mediante un script de User Data.

- **Auto Scaling Group**: Permite la escalabilidad de las instancias EC2 según la demanda.

## Consideraciones 👀

1. **Permisos IAM**: Nos debemos asegurar de crear un ROL en IAM con la politica correcta para que la instancia EC2 pueda realizar acciones en el servicio de bucket S3, para este caso se utilizo `EC2fullAccesToS3` la cual toca de igual forma validar que este correctamente configurada.

2. **Claves SSH**: Debes proporcionar un nombre de clave SSH válido que ya exista en tu cuenta de AWS para permitir el acceso a las instancias EC2 (esto es importante para poder ver logs de errores o poder realizar cualquier configuracion necesaria internamente en la instacia)

3. **Regiones**: debemos asegurarnos que las AMIs especificadas en el mapa de regiones sean válidas y existan en las regiones que deseamos desplegar.

4. **Recursos Públicos**: Las configuraciones de acceso público para el bucket S3 están habilitadas (esto solo se utiliza para Laboratorios ya que implican alto riesgo de seguridad)

5. **User Data**: Se crea un script de User Data que instala Apache y configura los archivos `index.html` y `error.html`, tener presente que para este caso se utiliza GitHub por lo cual el repositorio debe ser publico o si es privado tener las credenciales correctas para poder manipularlo 

6. **Costos**: Se tiene que revisar los costos asociados con el uso de EC2, S3, y otros recursos utilizados en la plantilla.

## Implementación ⛓️🦾

Para implementar esta plantilla, sigue estos pasos:

1. Abre la consola de AWS CloudFormation.
2. Crea una nueva pila y selecciona "Crear a partir de un archivo".
3. Carga el archivo de plantilla de CloudFormation.
4. Proporciona los valores para los parámetros requeridos.
5. Revisa y lanza la pila.
