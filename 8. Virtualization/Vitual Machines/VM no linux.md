#Criando uma VM

1. Primeiro, rode o comando: ``` egrep -c '(vmx|svm)' /proc/cpuinfo```. Se o resulto for negativo, você pode criar vm's. Se for 0, você precisa modificar a BIOS e permitir a virtualização.
2. Se possivel criar VM's, execute: ```apt update && apt upgrade -y && apt install qemu-kvm qemu-system libvirt-daemon libvirt-clients libvirt-daemon-system bridge-utils virtinst```. Para controlar/criar as VM's ou você faz pela CLI ou você faz pela GUI (mais fácil). Para usar a GUI instale o package 'virt-manager'.
3. Por padrão você vem com uma virtual network pre-configurada (default) para checar ela use ```sudo virsh net-list --all``` para ligar ela ao ligar o pc faça ```sudo virsh net-autostart default```
4. Uma configuração que eu não sei pra que serve mas está no tutorial, é ativar o vhost-net module. Para isso faça ```modprobe vhost_net``` e dentro do arquivo /etc/modules, adicionar a linha 'vhost_net'
5. Proximo passo é configura sua Bridge. Você teria que modificar o arquivo '/etc/network/interfaces' e adicionar uma nova network interface (camada que criptografa minha internet e passa ela para o container/VM), mas isso é feito automaticamente se você usa algum network manager (Ao abrir o meu, já encontrei tanto o do docker quanto o usada pra VM's já prontos)
6. Apartir daqui você teria que rodar um comando bem longo com varias opções etc, porem é mais simples e direto usar a GUI. O processo de criação lá é bem intuitivo, a única que coisa que você precisa mudar é na ultima página, antes de finalizar, você vai em network settings é troca para bridge device então coloca o nome da bride que está no network manager (ex: virbr0)
7. No VMM você precisa ficar atento nas opções (para mostrar os detalhes), especialmente as configs de boot. Se você não setar corretamente a VM não liga. Além disso você também precisa dar acesso ao seu user no processo de virtualização ao usar ```sudo usermod -aG libvirt flp && sudo usermod -aG kvm flp```

### Processo:
Não tenho certeza de nada mas acho que funciona da seguinte forma:
1. O qemu e o kvm são os hypervisors ou semelhantes.
2. O libvirt é toda a estrutura e dependencias necessárias para manter a VM

De modo resumido, você precisa de uma iso do sistema que você quer "emular", além disso você precisa conectar a internet do seu pc local com a internet da VM. Para isso são utilizadas as *bridges*. Você pode configurar essas virtual networks manualmente (pela CLI) ou pela GUI do VMM.



###Referências:
[wiki kvm](https://wiki.debian.org/KVM)
[youtube tutorial](https://www.youtube.com/watch?v=cUCHzAotkWk)
[criando uma virtual network](https://wiki.libvirt.org/page/VirtualNetworking)
