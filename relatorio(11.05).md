## Como funcionará o Trimestre
### O que deverá ser desenvolvido/entregue
- Persistência
### Pré-requisitos necessários
- Pré-requisitos
	- 2 CRUD simples
	- 2 CRUD assosciação
### Critérios de avaliação e Datas importantes
- 25/05 Cod Review (DAO relacional)
	- Criar
	- Ler fromMap/toJson 
	- Atualizar
	- Deletar
- 01/06
	- Vídeo Classe Conexão
     - Escrito a mão

## Programação
### Conceitos fundamentais de Persistência
#### Porque escrever tudo na mão
- É bom, pois geram entendimento e não utilizar apenas Frameworks
- Ter noção técnica
- Pode usar IA (conscientemente)
	- Desde que atenda 100% a demanda que você precise
- ORM - Object-Relational Mapping
#### Dependências no Flutter
flutter pub get -> pega a melhor versão disponível na máquina
- Caso seja manual, as vezes o computador nao aguenta, mas tras seletividade.
#### Programação Assíncrona
- Banco de dados demora para processar
- Síncrona
	- Só executa o próximo comando, quando o atual é concluído.

### `Future`
Representa um valor que estará disponível no futuro. Tipo uma promessa de resultado.
Não devolve uma String, mas diz "Em algum momento no futuro, essa função entregará uma String"

### `async`
Marca uma função assíncrona.
Mesmo que o código não seja 100% executado, ele não irá travar a aplicação restante.
Se uma função usa `await`, ela precisa ser `async`.

### await
Espera um `Future` terminar.
O programa pausa apenas essa função até o resultado chegar, enquanto a aplicação restante continua funcionando.

#### O que faz
1. Sem `await` o `nome` será `Future<String>`
2. Com `await` o `nome` será `'Luiz'`

### Funcionamento da Persistência em aplicações Flutter
#### Persistências (Melhor? Depende)
##### Local
- BD: no próprio dispositivo
- Velocidade: mais rápido
- Offline: funciona melhor
- Segurança: exige cuidado no aparelho
- Manutenção: pode exigir migraçrões no app
- Dados compartilhados: mais difícil
- Create Table: app
- Mantém BD: app
- Como muda o BD: atualiza pelo app
- App verifica se a estrutura existe: sim
##### Remota
- BD: servidor
- Velocidade: depende da rede
- Offline: depende da conexão
- Segurança: tende a ter mais controle no servidor
- Manutenção: tende a ter mais controle no servidor
- Dados compartilhados: mais adequada
- Create Table: pessoa/equipe backend
- Mantém BD: pessoa/equipe backend
- Como muda o BD: alteração no servidor
- App verfica se a estrutura existeL não, para o banco remoto
##### Sincronização
- BD: dispositovo e servidor
- Velocidade: depende docaso
- Offline: pode funcionar
- Segurança: exige cuidado nos dosi lados
- Manutenção: mais complexa
- Dados compartilhados: possível, mas exige controle
- Create Table: app local e equipe do backend
- Mantém BD: app e backend
- Como muda o BD: atualização do app e/ou servidor
- App verifica se a estrutura existe: sim, para a parte local

Sincronizar os dados -> Difícil temporal e 
Commit em duas fases -> inicia transação, manda pra um banco e no outro outras operações, quando tudo OK, commit.

#### Persistência (Salvar)
- Para salvar
	- Precisamos conexão.
		- Conexão é a parte mais cara do programa
	- SQL
		- DDL -> define estrutura (create table)
		- DML -> linguagem de manipulação de dados (update table, delete, insert)
		- Opcional -> DCL -> linguagem de controle de dados (usuário)
	- Organização de código
		- DTO -> Data Tansfer Object (Transferir os dados)
			- Converte os dados do app (classe) para JSON (toMap, toCase)
			- Não tem validações
		- Model
			- Os objetos (entidade que será salvo)
			- Atributos
			- validações
		- DAO (próximo ao banco de dados)
			- Conversa com o banco
			- Acesso aos dados do objeto
				- Salvar
				- Alterar
				- Atualizar
				- Deletar
		- Repository (trabalhar com dados no nível abstrato (aplicação))
			- Manda SALVAR uma venda
			- Salvar
			- Alterar
			- Atualizar
			- Deletar
	- Mapeamento
		- Mapeamento Object-Relacional
### Sequência lógica do processo de Persistência
#### Telas e fluxos para Persistência
- Salva -> Lista (editar/excluir) 
- Fluxo de abrir cadastro
	- Campos seleção vem primeiro
	- DropdownMenuItem
		- value: dropdownbutton
		- items: array de dropdownitems
		- .map: função de uma lista
			- gera uma nova lista
				- forEach: traz para cada ciclo um objeto
- Fluxo de salvar e ir na lista
	- Lsita tem que ser um stateFull (tem um estado)
		- Ser dinâmico
		- Repinta a tela
		- setState
- Fluxo de editar
	- Edita -> volta para o formulário -> enviar uma informação
	- buildContext -> envia via contexto
- Fluxo de deletar
	- Apagar -> atualiza novamente a lista
#### Código de Persistência nas telas
- POO -> Programação Orientada a Objetos
- PON -> Programação Orientada a Necessidade
- Criar conexão -> reútiliza código (coesão)
	- Facilita a manutenção, legitibilidade, padronização, desempenho,
	- Tela -> DAO -> Conexão
		- Otimização: Ter um modelo, que recebe os dados das classes e trabalhar ali.
			- MVC: Model/View/Controller
				- Tela: limpa
				- Controller: controle da tela (rotas, DAO, instância)
				- Modelo: domínio de dados e validação
				- Interface do DAO
					- Quais operações vai ter
					- Associar com o DAO
					- Inversão de dependência
						- Pega exatamente do que precisa
						- E não tudo que você tem no banco de dados
- DAO Especifico é melhor que genérico (facilita a escrita do código, mas não é melhor)
## Code
### 01-sincrona
Tudo acontece de forma bloqueada e sequencial.
`sleep(Duration(seconsd: 2));`
Não temos async/await/future
#### Fluxo
1. Desen. estrutura
2. Desen. campo
3. Desen. logo
4. Busca dados
5. Espera 2 seg.
6. Busca email
7. Espera 4 seg.
8. Apresenta dados
### 02-async.dart
Transforma o código em assíncrono, não trava a interface:
- `future` - futuro
- `await` - esperar
- `async` - assíncrono
`await Future.delayed()`
em vez de `sleep()`.
#### Fluxo
1. Desen. estrutura
2. Desen. campo
3. Desen. logo
4. Espera `buscarEmailUsuario()`
5. Busca dados
6. Espera 2 seg.
7. Espera 4 seg.
8. Apresenta dados
### 03-sem_await_mail.dart
Mesma coisa do 02
- Não tem await, passa reto o código e só quando terminar, vai retornar o Future buscarUsuário
- O restante do código continua executando imediatamente.
	- Ordem incorreta de execução
		- Mostra dados incompletos
		- Acessa info. não carregadas
#### Fluxo
1. Desen. estrutura
2. Desen. campo
3. Desen. logo
4. Inicia busca email
5. Apresenta dados na hora
6. Finaliza `main`
7. Só depois o email carrega

### 04-primeiro_oqdemora.dart
- Operações assíncronas (otimização)
	- `Future<String> emailFuturo = buscarEmailUsuario();`
		começa imediatamente, enquanto o email está carregando, a interface continua sendo desenhada.
	- Depois `String email = await emailFuturo;`
- `await` é adiado para o momento em que será necessário
- Melhor aproveitamento do tempo
### 05-cuidado_await.dart
1. Faltou `await`
	- `var usuario = buscarDadosUsuario();`
2. uso do `var`
	- Esconde o tipo real, usuário acha que é um string, mas é `Future<String>`

### 06-correto.dart
Corrige o 05
1. `await` utilizando coretamente:
	`String usuario = await buscarDadosUsuario();` -> agora sim é um `String`.
2. Tipagem explícita
	- Não esconde o tipo real
3. Trata erros
	- Evita falhas inesperadas
		`try {
		} catch (e) {
		}`

## Projeto
BASE: database.dart
1. Quatro DAO's
2. Telas ilimitadas
3. Uma conexão
4. Controle de versão
`Map` -> chave (valor que será acessados), dynamic (qualquer tipo -> INT, STRING)

<img width="888" height="307" alt="image" src="https://github.com/user-attachments/assets/685f0da8-a487-4c12-8f61-73f6231f8ff2" />

