#Intro ao Docker

* Docker: Uma das tecnologias de virtualização de containers. Possue uma engine e um Daemon.
* Docker Image: Coleção de arquivos e metadados que caracterizam o container e possiblitam ele funcionar. É através dessa imagem que o container é criado
* O principal comando é o ```docker run <image>```. Uma vez que o docker já está instalado na sua maquina fisica, ao rodar esse comando, a docker engine vai procurar pela imagem especifica e seu ela existir localmente, ele cria um novo container (repare que ele sempre que criar um novo container). Se essa imagem localmente não exitir, o docker vai fazer o ``` docker pull <image>``` e pegar essa imagem do *docker hub*, instalar ela, e criar o container.
* Instale o docker [aqui](https://docs.docker.com/engine/install/debian/) OBS: Os containers ficam em ```/var/lib/docker/```. Você pode "bricar" [aqui](https://labs.play-with-docker.com/)
* Como você já percebeu, o docker hub é bastante similar ao github. Existe um desktop app para linux mas não precisa baixar.

* Comandos:
	* ```docker run -it alpine``` vai fazer o pull da imagem, criar o container e abrir um shell (devdo ao -it) dentro do alpine. Quando você faz "docker run" você está criando um container baseado nessa imagem.
	* Por padrão os arquivos que você cria dentro do container não são deletados depois que ele para de executar. Para que o container seja removido depois que ele é montado, usa-se o ```docker run --rm alpine```.
	* ```docker ps``` vai mostrar quais os containers estão ativos/rodando. Ao usar a flag ```-d``` você monta o container no "background" e ao fazer ``` docker stop <container-id> ``` ele vai parar.
	* ```docker image ls``` vai mostrar quais as imagens que você tem localmente e ao usar ```docker image rm <image-name>``` você deletar a imagem.
	* ```docker container ls -a``` vai listar todos os Containers que você criou. Para criar um container, ou você usa o "run" ou usa ```docker container create --name <container-name> <image-name> <command>```.
	*  Para "Ligar" o container usa-se ```docker container start <container-name/id>``` e para parar basta trocar o start por "stop". Para executar um comando em um container que está ativo usa-se ```docker container exec <container-name/id> <command>```
	* Para remover todos os containers que estão inativos usa-se ``` docker container prune ```
	* Por padrão quando um container é criado (Em cima de uma imagem), o docker gera uma network privada para esse container (bridge) que se comunica com a sua network de modo "criptografado" (não sei como funciona exatamente).

* Layered file-system: O docker funciona em camandas. Se dois containers usam um mesmo conjunto de arquivos, esses arquivos vão ser reutilizados por ambos (ao invés de ter um para cada)

* Docker volume: Consiste em salvar arquivos de dentro do container para a maquina hospedeira. Ao usar a flag "-v" no comando ```docker container run -d --name <container-name> -v "<dir-host:dir-container>" image```  estamos "Linkando" os dados no meu diretorio "dir-host" com o diretorio "dir-container" do container.
	* Ex: "```sudo docker container run -d --name alpine-test -v "/home/flp/bananas:/tmp"  alpine sleep 20000```. Dai então crie um arquivo dentro da pasta bananas e veja se ele apareceu dentro do container. Agora tente criar um arquivo nesse mesmo diretório de dentro do container e veja se o arquivo local mudou.
	* OBS: Em alguns casos os container pode ter virus ou etc, então passar o path completo, ou conectar o seu teclado com o container pode ser perigoso. No primeiro caso, você pode trocar o path no local OS por uma variável de ambiente local.


#### Docker file

Basicamente você vai criar um container, instalar tudo que você precisar nele e então usar. Entretanto fazer esse mesmo processo várias vezes é repetitivo e sujeito a erros. Uma alternaiva é usar uma imagem pronta do docker hub (ao invés de criar a sua do zero), ou então criar uma docker file.

Docker file é um script com o passo a passo que um conteiner tem seguir. A sintaxe dele consiste em INSTRUÇÕES (all caps) seguidas de argumentos. Seguem algumas instruções importantes:

1. ```RUN <command>```: 
	* vai executar o comando enquanto o container está sendo criado. Pode-se ter varios RUN
2. ```CMD <command>```: 
	* vai executar um comando depois que o container for criado.
3. ```LABEL <key>=<value>```: 
	* Maneira de definir key value pairs, normalmente usada em documentação
4. ```EXPOSE ```:
	* É usado para documentar o link entre uma porta do container e uma porta local
	* Ex: No container localhost:8080, pode ser aberto em localhost:5000 na maquina local, mas para isso eu preciso rodar o comando "docker run" com a flag -p seguido de ```portaNoHost:portaNoContainer```
5. ```ENTRYPOINT ```:
	* Outra maneira de usar o CMD
6. Entre outros


[Exemplo de Dockerfile](/home/flp/Downloads/Arch/C/Python/Data Science/pics/ex-dockerfile.png)
Para salvar essa imagem customizada no docker hub, você precisaria fazer login pela CLI, definir um parametro tag e fazer outras coisas. Se necessário, procure.


#### Docker compose

Imagine que você tem um front end sobre uma votação, ele se conecta com uma database in-memory, que entra em contato com um woker e então atualiza a persistent database. As modificações são reproduzidas em outro front-end, que mostra o resultado da votação.

Em termos de docker, cada um desses serviços é um container. Para interligar eles você precisa usar a flag "-links \<container-name\>:\<container-name\>". Além disso, é necessário configurar a rede/network para que os serviços se conectem. Tudo isso dá trabalho, ainda mais se feito pela CLI. É por isso que existe o docker compose.

Docker compose é um .yaml (json diferenciado) que, assim como o dockerfile, contém dados e instruções sobre como interligar vários serviços/containers diferentes, com um alto nível de abstração. Eu poderia passar mais tempo falando dos detalhes, mas prática é algo mais importante.

Para aprender mais, ou melhor, para realmente aprender, se coloque em uma situação onde você precisa usar essas ferramentas. Estudar elas de forma avulsa não vai ser algo produtivo, afinal as anotações só são úteis se alguem relamente lê elas, entende as intruções e APLICA o que está documentado.


#### Método
Basicamente, ao invés de você criar um único docker container gigante e pesado que tem tudo que você precisa em um só lugar, você Deve ter vários docker containers separados, cada um no seu quadrado, e então interligar eles com outra ferramenta, como docker compose, kurbenetes, etc.
