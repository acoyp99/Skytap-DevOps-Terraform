# Skytap-DevOps-Terraform💻

## Requerimientos 

* Una maquina virtual ubuntu 
* Terraform v0.12.24
* Terraform-skytap provider v0.14.0

### 1. Instale Terraform en su máquina local

a. Cree una carpeta en su sistema local que se llama terraform y navegue a su carpeta.

`mkdir terraform && cd terraform`

b. [Descargue la ùltima versión de Terraform CLI en su máquina local (en este caso es la 0.12.24)](https://releases.hashicorp.com/terraform/)

El archivo de instalación quedara alojado en la carpeta de descargas que tenga configurada por defecto, por lo que debe entrar a la c arpeta de descargas para extraer el archivo.

`cd $HOME/Downloads`

c. Extraiga el paquete Terraform y copie el archivo binario en su directorio terraform.

`unzip terraform_0.12.24_linux_amd64.zip`<br />
`sudo mv terraform $HOME/terraform`

d. Apunte la variable de entorno $ PATH a su archivo binario Terraform.

`export PATH=$PATH:$HOME/terraform`

e. Verifique que la instalación sea exitosa 

`terraform --version`

Vera una salida como la siguiente:

`Terraform v0.12.24`

### 2. Descargue el complemento skytap provider 

a. [Descargue la última versión del archivo binario de Skytap Provider.](https://releases.hashicorp.com/terraform-provider-skytap/)

Ingrese a la carpeta de descargas y extraiga el archivo binario del plug-in, para este caso se ha descargado la versión para Linux de 64 bits.

`cd $HOME/Downloads
unzip terraform-provider-skytap_0.14.0_linux_amd64.zip`

b. Cree una carpeta oculta para su complemento.

`mkdir $HOME/.terraform.d/plugins`

c. Mueva el complemento de Skytap Provider a la carpeta oculta que acaba de crear

`mv $HOME/Downloads/terraform-provider-ibm* $HOME/.terraform.d/plugins/`

d. Ingrese a la carpeta occulta y verifique que la instalación se haya terminado

`cd $HOME/.terraform.d/plugins && ./terraform-provider-ibm_*`

Vera una salida como la siguiente:

-
-
-

### 3. Configure el elemento Skytap provider

a. Cree una carpeta en su máquina local para su primer proyecto Terraform y navegue hacia la carpeta. Esta carpeta se utiliza para almacenar todos los archivos de configuración y definiciones de variables.

`cd $HOME`<br />
`mkdir myproject && cd myproject`

b. Cree un archivo de configuración config.tf, utilice este archivo para configurar el complemento de Skytap Provider con las credenciales

`nano config.tf`

El archivo tendrá la siguiente estructura


<pre><code>

provider "skytap" {
  username = "mario.Olarte@ibm.com"
  api_token = "b5cf5669428ebf250ea62b56107fe882881144a0"
}


resource "skytap_environment" "enviroment"{
  template_id = "1863063"
  name = "Prueba"
  description = "Skytap terraform provider example environment."
}
resource "skytap_vm" "lpar1" {
  template_id = 1863063
  vm_id = 39131371
  environment_id = skytap_environment.enviroment.id
  name = "Node 2 - AIX 7.2 TL3 SP2"
}
output "address" {
  value = https://cloud.skytap.com/configurations/skytap_environment.enviroment.id
}
resource "skytap_network" "network" {
  environment_id = skytap_environment.env.id
  name = "iSCSI"
  domain = "iscsi"
  subnet = "192.168.1.0/24"
  gateway = "192.168.1.254"
  tunnelable = false
}

</pre></code>


