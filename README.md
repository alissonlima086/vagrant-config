# Vagrant


## Como iniciar?
Para inicializar o vagrant, vamos executar o seguinte comando no terminal, que irá gerar o vagrant file no diretorio em que foi executado
```
vagrant init
```


Será gerado no arquivo um código com instruções


Pode-se também utilizar o mesmo comando especificando que box deve ser utilizada
```
vagrant init hashicorp/bionic64
```



Que adicionaria no arquivo a seguinte configuração no vagrantfile:
```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
end
```
(hashicorp/bionic64 é uma das box oficiais disponibilizada e mantida pela HashiCorp)


---

Para rodar:
```
vagrant up
```

---
Ao abrir o VirtualBox, é possivel visualizar que a vm está rodando
---
![image](https://github.com/user-attachments/assets/6196845d-e242-4c4e-9671-b90ad7089cae)

---
Podemos também abrir a vm, que seremos levados ao terminal da imagem
---
![image](https://github.com/user-attachments/assets/0e05f573-3760-4459-a192-175167957072)

---
### Acessando a maquina virtual
Podemos acessar a maquina virtual utilizando ssh
```
vagrant ssh
```
* É necessario estar no mesmo repositorio do vagrantfile

E assim ganhamos acesso a maquina virtual
---
![image](https://github.com/user-attachments/assets/12d58835-de16-420a-82de-381485f2dd95)

![image](https://github.com/user-attachments/assets/1ff39ccf-05a0-40ac-bf32-c4be9cc46167)

---

Podemos utilizar o seguinte comando para obter informações sobre as vms:
```
vagrant global-status
```
![image](https://github.com/user-attachments/assets/bf08cc3e-49a5-43d7-a9df-29ef3bc687ef)

Assim podemos acessar a maquina virtual a partir de qualquer diretório passando o id da maquina:

![image](https://github.com/user-attachments/assets/9dd97c05-d25e-4481-b1f4-acf6052edeb8)

---

Para verificar o status, podemos utilizar o comando dentro do repositório do vagrantfile:
```
vagrant status
```

Caso não esteja no repositório, deve-se passar o id da maquina

![image](https://github.com/user-attachments/assets/bd7f986a-0159-401b-bfe5-6005e826c141)

Para reiniciar a maquina:
```
vagrant reload
```

E para parar a maquina o comando:
```
vagrant halt
```

e ela será finalizada

![image](https://github.com/user-attachments/assets/30f16866-2ef8-4c50-bf0b-2593bf706684)

![image](https://github.com/user-attachments/assets/67f05887-f36c-4ac4-a2c5-cd70ff2a9f07)

---

## Vagrantfile

Agora, para a configuração do vagrantfile, já temos uma parte do arquivo
```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
end
```

Podemos agora acrescentar o nome do host:

```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.hostname = "unitins" # altera o nome do host
end
```

Podemos também acrescentar as configurações do virtualbox:
```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.hostname = "unitins" # altera o nome do host
  config.vm.provider "virtualbox" do |vb| # define as configurações no virtualbox
    vb.memory = "1024" # configuração de memoria
    vb.cpus = 2 # definição de núcleos do processador
    vb.name = "comp-servicos" # define o nome da maquina no vbox
end
```

Aqui definimos o nome do host como unitins, determinamos o uso de memoria para 1GB(1024MB), reservamos 2 nucleos da CPU e alteramos o nome da vm para comp-serviços

![image](https://github.com/user-attachments/assets/260885de-ed9f-4991-8569-60b73c6fd4a6)

Podemos agora adicionar também um script para ser executado ao iniciar a maquina pela primeira vez:
```
config.vm.provision "shell", inline: <<-SHELL # executa um script com os seguintes comandos
  sudo apt update # atualiza o sistema
  sudo apt install neofetch -y # instala o neofetch
  sudo apt install apache2 -y # instala o apache
  sudo systemctl start apache2 # inicia o apache
  sudo systemctl enable apache2 # habilita o inicio do apache ao iniciar o sistema
  SHELL
```
Aqui foi determinado que ele deve atualizar o sistema, instalar o neofetch, instalar o apache e iniciar o apache no sistema.

Alternativamente, podemos criar um shell script no diretório do vagrantfile com os comandos e chamar a partir de:
```
config.vm.provision :shell, path: "script.sh"
```

Com o atual codigo rodando o apache na virtualização, podemos também acrescentar um comando para fazer o redirecionamento da porta
```
config.vm.network "forwarded_port", guest: 80, host: 8090
```

Tornando o apache acessivel no computador fora da virtualização:

![image](https://github.com/user-attachments/assets/deb779b1-b99e-4210-98ba-002db83209df)

---

## Multiplas maquinas virtuais

Agora, criaremos um arquivo vagrant que configure duas maquinas virtuais, uma para um servidor web e outra para o banco de dados

```
# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64" # define a imagem que será utilizada

  # servidor web
  config.vm.define "web" do |web|
    web.vm.hostname = "servidor-web"
    web.vm.network "forwarded_port", guest: 80, host: 8090

    web.vm.provision "shell", inline: <<-WEB
      apt update
      apt install apache2 -y
      systemctl restart apache2
    WEB
  end

  # banco de dados
  config.vm.define "db" do |db|
    db.vm.hostname = "database"
    db.vm.network "private_network", type: "static", ip: "192.168.50.10" 
    db.vm.provision "shell", inline: <<-DB
      apt update
      apt install mariadb-server -y
      systemctl restart mariadb.service
    DB
  end

end

```

Ao rodar com o provisionamento, temos agora as duas maquinas criadas e configuradas

![image](https://github.com/user-attachments/assets/e4018635-c385-4f56-b4ed-818041b251a4)

Agora podemos acessar o ssh especificando uma das maquinas

```
vagrant ssh db
```

O que nos dá acesso a maquina com o maria db

![image](https://github.com/user-attachments/assets/b5e4c811-708a-40a5-8b83-5794a0e3867c)

E podemos verificar o serviço

![image](https://github.com/user-attachments/assets/114ee870-a34c-4c1d-84be-6b9d85948a0a)
