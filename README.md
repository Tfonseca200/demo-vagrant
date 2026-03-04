# 🚀 Demo-Vagrant: Infraestrutura Multi-VM com KVM/libvirt

Este projeto automatiza o provisionamento de duas máquinas virtuais Ubuntu Server utilizando **Vagrant** e o provider **libvirt** localmente. O objetivo é criar um ambiente isolado com comunicação via rede interna para testes de conectividade e serviços.

## 🛠️ Tecnologias Utilizadas
* **Vagrant**: Orquestrador de infraestrutura como código.
* **KVM/libvirt**: Hypervisor e API de virtualização (nativa no Linux).
* **Box**: `generic/ubuntu2204` (Ubuntu Server 22.04 LTS).

## 🏗️ Estrutura do Ambiente
As máquinas estão configuradas com hardware otimizado e uma rede privada comum.

### Configurações de Hardware (Global)
* **CPUs**: 2 vCPUs por máquina.
* **Memória RAM**: 1000 MB.
* **Armazenamento Extra**: Disco de 10GB (`vdb`) para cada instância.

### Detalhes da Rede
As máquinas estão conectadas em uma rede privada denominada `demo_network`:

| VM | Hostname | IP Privado |
| :--- | :--- | :--- |
| **VM 1** | `vm1` | `10.10.0.40` |
| **VM 2** | `vm2` | `10.10.0.50` |

---

## 📋 Pré-requisitos

Para que o Vagrant consiga gerenciar as VMs via KVM no seu Linux, siga estes passos:

### Instalar pacotes do Hypervisor (Debian/Ubuntu)
```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients virt-manager ebtables dnsmasq-base -y
```

### Instalar pacotes do Hypervisor (Debian/Ubuntu)

O Vagrant precisa de um plugin específico para conversar com o libvirt:

```bash
vagrant plugin install vagrant-libvirt
```

## 🚀 Como Executar

### Subindo o Ambiente
Dentro da pasta onde o Vagrantfile está localizado, execute:

```bash
vagrant up
```
### Acessando as VMs
Para acessar via SSH, basta especificar o nome da máquina definida no código:

```bash
vagrant ssh vm1

vagrant ssh vm2
```

### Validando a Conectividade

```bash
ping 10.10.0.50 -c 4
```

## Explicando trechos de codigos:

### Definição da Imagem Base
```bash
config.vm.box = "generic/ubuntu2204"
```

### Configurações do Provider (Hardware)
```bash
config.vm.provider :libvirt do |lv|
  lv.uri = "qemu:///system"
  lv.cpus = 2
  lv.memory = 1000
  lv.storage :file, size: '10G', device: 'vdb'
end
```

### Definição das VMs e Rede Privada
```bash
vm1.vm.network "private_network",
  ip: "10.10.0.40",
  libvirt__network_name: "demo_network"
```

## Documentaçãos

* **[Vagrant](https://developer.hashicorp.com/vagrant/docs)**: Ferramenta para construção e manutenção de ambientes de desenvolvimento virtualizados.
* **[KVM (Kernel-based Virtual Machine)](https://www.linux-kvm.org/page/Main_Page)**: Tecnologia de virtualização nativa do Kernel Linux.
* **[Libvirt](https://libvirt.org/docs.html)**: API de virtualização que gerencia o KVM.