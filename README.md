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
(* É necessario estar no mesmo repositorio do vagrantfile)

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

