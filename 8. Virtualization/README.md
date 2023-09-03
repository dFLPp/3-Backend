# Virtualization

> É o ato de representar/criar virtualmente (ao invés de fisicamente) algo, ao mesmo tempo em que mantém o mesmo nível de abstração que a versão 'fisica'

Para leigos/mortais, virtualização é usada principalmente para criar ambientes computacionais separados onde, ao invés de, por exemplo, fazer o deploy de um webapp no meu pc, fazer isso através de um serviço de cloud hosting.
Basicamente é o ato de separar virtualmente seções do hardware. Essa ideia de virtualização já é relativamente antiga.

No mundo de virtualization existem 2 conceitos importantes, **Virtual machines** e **Containerization**. Veja as definições e então conclua se elas são tecnologias adversárias ou soluções complementares.

1. **Virtual machines** (VM's):

   - É a utilização da tecnologia de virtualização para simular um sistema operacional dentro de um sistema operacional. De modo resumido, você baixa um programa, esse programa vai possue/ser um _Hypervisor_, que é um software responsável por gerenciar os recursos do seu computador (fisico) e distribuir ele através dos computadores virtuais (falsos) que seram criados.
   - Uma das desvantagens de VM's é que você cria um OS completo, e em muitas situações, existem arquivos desnecessários quem vem junto. Uma das vantagens é que, independente do seu computador fisico, você pode gerar virtual machines com quaisquer outros sistemas operacionais. Por exemplo, se você está no linux, o linux pode ter uma VM do windows, ou até mesmo de outro linux.

2. **Containerization** (Conteiners):
   - Assim como VM's, utiliza da virtualização para simular OS's, só que, ao invés de um hypervisor ele possue um _runtime engine_. Essa engine vai, através de um arquivo de "manisfesto" (tipo um package.json), criar uma imagem do sistema/app que você quer virtualizar e então vai armazenar ele em um container(VM's que ocupam pouco espaço).
   - Algumas das vantagens são: É amplamente utilizada por empresas; Ocupa menos espaço/recursos do OS fisico; VM's também conseguem, porém é mais fácil interconectar containers. A maior desvantagem que o runtime engine só gera containers com o mesmo OS que o do seu PC fisico. Ou seja, em um linux só é possivel ter linux containers.
   - A tecnologia/solução mais usada atualmente, em relação à containers, é o **Docker**. O Docker é um ecossistema que provê, gerencia e mantém containers (é tipo um (npm + github) só que de containers). Está fortemente ligado à arquitetura de _microservices_ e interage com **Kubernetes**, um gerenciador de Docker containers.
