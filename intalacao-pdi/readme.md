**INSTALAÇÃO DO PENTAHO DATA INTEGRATION (PDI) NO LINUX**



Instalando o PDI

- Baixado a última versão do PDi em https://sourceforge.net/projects/pentaho/

- criar uma pasta para descompactar o arquivo baixado

- Instalar o Java 8.0 (versões 9 e 11 apresentavam problemas)

  ```bash
  # atualizar o repositório do APT
  sudo apt update
  # instalar o pacaote 
  sudo apt install openjdk-8-jdk
  # para confirmar a instalação
  java -version
  ```

  ```
  openjdk version "1.8.0_282"
  OpenJDK Runtime Environment (build 1.8.0_282-8u282-b08-0ubuntu1~20.10-b08)
  OpenJDK 64-Bit Server VM (build 25.282-b08, mixed mode)
  ```

- Instalar "libwebkitgtk-1.0-0" conforme sugerido ao abrir o spoon

  ```bash
  # modificar o arquivo adicionando a entrada "deb http://cz.archive.ubuntu.com/ubuntu bionic main universe"
  sudo nano /etc/apt/sources.list
  # atualizar o repo
  sudo apt-get update
  #registrar a chave pública caso apresente o erro "As assinaturas a seguir não puderam ser verificadas devido à chave pública não estar disponível: NO_PUBKEY A8AA1FAA3F055C03"
  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A8AA1FAA3F055C03
  # instala
  sudo apt-get install libwebkitgtk-1.0-0
  ```

- Abrir (a partir da pasta ~)

  ```bash
  ./Desktop/pdi/data-integration/spoon.sh
  ```

  





