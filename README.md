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



Que adicionaria no arquivo a seguinte configuração:
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
Ao abrir o VirtualBox, é possivel ver que a vm está rodando
---
![image](https://github.com/user-attachments/assets/6196845d-e242-4c4e-9671-b90ad7089cae)

