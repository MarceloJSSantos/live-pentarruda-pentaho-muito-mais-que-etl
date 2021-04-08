**INSTALAÇÃO MYSQL NO LINUX (baseado no Ubuntu)**



Instalação

- Atualizar o índice de pacotes do APT:

```bash
sudo apt update
```

- Instalar o pacote `mysql-server`:

```bash
sudo apt install mysql-server
```



Configuração

- Executar o script de segurança como `sudo`:

```bash
sudo mysql_secure_installation
```

- Escolher o uso ou não do plug-in para validar senha:

```
OutputSecuring the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: Y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG:
 2
```

- Definir senha:

```
OutputPlease set the password for root here.


New password:

Re-enter new password:
```

- nota: Se foi escolhido o plugin de validação de senha, aparecerá a forma da senha escolhida e se deseja continuar.

```
OutputEstimated strength of the password: 100
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : Y
```

Outras configurações

```
Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
```

```
Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
```

```
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
- Dropping test database...
Success.

- Removing privileges on test database...
Success.
```

```
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
```



Ajustar a autenticação e os privilégios do usuário (opcional)

```bash
sudo mysql
```

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10Server version: 8.0.23-0ubuntu0.20.10.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

- Verifique quais os métodos de autenticação cada conta de usuário do seu MySQL utilizam com o seguinte comando:

```mysql
mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;
```

```
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
| user             | authentication_string                                                  | plugin                | host      |
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
| debian-sys-maint | $A$005$wI\`WGzl    z▒

                                          ▒[Mpu8eAWLzSvfc9mUeQf3i5xbxALPe9Rs.EMAF9ysz/MB | caching_sha2_password | localhost |
| mysql.infoschema | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | caching_sha2_password | localhost |
| mysql.session    | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | caching_sha2_password | localhost |
| mysql.sys        | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED | caching_sha2_password | localhost |
| root             |                                                                        | auth_socket           | localhost |
+------------------+------------------------------------------------------------------------+-----------------------+-----------+
5 rows in set (0.00 sec)
```

- A instrução abaixo altera o tipo de autenticação e a senha (definida anteriormente) do usuário root:

```mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

- Então, execute `o comando FLUSH  o qual` diz para o servidor recarregar as tabelas de permissões e colocar as suas alterações em vigor:

```bash
mysql> FLUSH PRIVILEGES;
```

- Verifique novamente os métodos de autenticação se atentando para o root

```mysql
mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;
```

- Sair do shell

```bash
mysql> exit
```



Acessar o shell do MySQL c/ autenticação por senha habilitado

- depois das alterações anteriores só poderá acessar o shel do mysql através do comando abaixo, pois não será mais permitido`sudo mysql`.

```bash
mysql -u root -p
```



Testando

- Verifica o status do serviço.

```bash
systemctl status mysql.service
```

```
 mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2021-03-26 00:49:39 -03; 17min ago
   Main PID: 5851 (mysqld)
     Status: "Server is operational"
      Tasks: 39 (limit: 4599)
     Memory: 333.2M
     CGroup: /system.slice/mysql.service
             └─5851 /usr/sbin/mysqld

mar 26 00:49:37 lubuntu-virtualbox systemd[1]: Starting MySQL Community Server...
mar 26 00:49:39 lubuntu-virtualbox systemd[1]: Started MySQL Community Server.
```

- caso não esteja 'active (running)...'

```bash
sudo systemctl start mysql
```

- ver a versão

```bash
sudo mysqladmin -p -u root version
```

```
mysqladmin  Ver 8.0.23-0ubuntu0.20.10.1 for Linux on x86_64 ((Ubuntu))
Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version          8.0.23-0ubuntu0.20.10.1
Protocol version        10
Connection              Localhost via UNIX socket
UNIX socket             /var/run/mysqld/mysqld.sock
Uptime:                 20 min 41 sec

Threads: 2  Questions: 24  Slow queries: 0  Opens: 141  Flush tables: 3  Open tables: 60  Queries per second avg: 0.019
```

