# Projeto Integrador Transdisciplinar de Banco de Dados I  
### Autor: Thiago Daniel Pessoa de Azevedo  
### Email: thiago.d.pessoa@gmail.com  
### RGM: 42207649  

## Descrição
Foi solicitado que se projetasse um banco de dados de acordo com as necessidades de um cliente, tendo alguns tópicos para suprir e entregar com o projeto.  
As estapas são as que seguem:  
* 01: Elaborar um texto sobre o problema e o desafio de se gerir uma empresa não informatizada, podendo citar exemplos que tenha conhecido ou vivenciado;  
* 02: Elaborar um texto informando qual SGDB escolheu para utilizar na empresa. E se justificar;  
* 03: Elaborar um texto informando como será feita a implementação do banco de dados. E se justificar;  
* 04: Elaborar um levantamento de requisitos funcionais e não funcionais;  
* 05: Criar os modelos:  
    + Conceitual;
    + Lógico; e  
    + Físico.
* 06: Elaborar um texto com a história, o que irá fazer, os processos, a verificação e a entrega do projeto.  
## Situação Problema
"Eduardo, proprietário de uma marcenaria, percebeu aumento significativo no número de clientes novos, causando desorganização e lentidão no acesso aos dados desses clientes nos futuros pedidos. Até o presente momento a empresa não tem um sistema informatizado para manter os dados dos clientes antigos e novos. Ciente da necessidade de automatizar os serviços de acesso às informações, Eduardo contatou a empresa Soluções Inovadoras TI de desenvolvimento de sistemas para elaboração de um projeto de implementação de um banco de dados que atenda a sua demanda que hoje está em crescimento."  
  
## Etapa 01 - Das dificuldades de um sistema não informatizado:
Não possuir um banco de dados centralizado e projetado para o negócio torna difícil o mantenimento e a dedicação em acrescentar novos dados, seja anotando em um livro ou inserindo em arquivo específico, a falta de relacionamentos e consultas especializadas torna a leitura poluída e/ou de difícil abstração.  
Um banco de dados projetado do conceitual ao físico torna as inserções e as consultas dos dados rápidas, seguras e sucintas.  
No meu emprego atual fui incumbido com a tarefa de fazer um banco de dados para controlar estoque e retiradas de artigos farmacêuticos, sendo que este controle era antes feito em um arquivo Excel. Tal arquivo não era histórico - pois era composto de duas colunas, a primeira, uma lista dos artigos e a segunda, a quantidade, supostamente em estoque, onde se era adicionado o valor que chegava e subtraído a quantidade que era retirada -, não era possível realizar operações que seriam vitais para as análises da própria gestora que alterava (e não alimentava) o arquivo.
Um banco de dados dedicado ao negócio é capaz de armazenar informações relevantes ao público-alvo e permite gerar relatórios de valor que permitam o gestor/cliente/usuário tirar conclusões sobre o próprio negócio.
## Etapa 02 - Da escolha do SGDB:
A minha escolha é o MsAccess. Visto que o usuário não precisa de atualizações ou inserções de forma on-line, além de ser um aplicativo completo, sendo possível fazer o front-end e o back-end na mesma plataforma, tornando os testes e correções mais rápidos.
## Etapa 03 - Da implementação do sistema: 
O sistema será salvo em um PC ou laptop, em uma pasta contendo o arquivo front-end compilado e o arquivo de dados.
## Etapa 04 - Dos requisitos:
* Requisitos Funcionais:
    * Armazenar:  
        * Clientes;
        * Materiais;
        * Tipo de Material;
        * Unidade de Material;
        * Estque;
        * Serviços; e
        * Pedidos.
* Requisitos Não Funcionais:
    * Tabelas normalizadas;
    * Atributos tipificados conforme as necessidades de cada um;
    * Estrutura pensada na utilização desde o dia 0;
    * Projetado visando a facilitação do front-end e extração de informações.
## Etapa 05 - Dos Modelos:
### Entidade-Relacionamento (Conceitual):
Arquivo `Modelo Entidade-Relacionamento.pdf`  
### Lógico:
Arquivo `Modelo Logico.pdf`  
### Físico:
Arquivo `Modelo Fisico.pdf`  
## Etapa 06 
### Do conceito:
 - O banco possui três tabelas centrais: `Cliente`, `Pedido` e `Projeto`, às quais respondem, respectivamente, as questões chave: "Quem?", "Quando?" e "O que?".  
 - O cliente faz um ou mais pedidos, e cada um desses pedidos contém um ou mais projetos (móveis, fixações, decoração, acabamentos etc).  
 - Daí surgiram outras possíveis necessidades implícitas: controles de estoque e financeiro.  
    - Visto que um projeto demanda materiais para ser feito, sendo estes materiais os quais entram e saem do estoque, foram criadas mais três tabelas secundárias: `Material`, `Entrada` e `Saida`.  
    - Um ou mais registros de `Material` se relacionam com um ou mais registros de `Projeto` por meio de um único registro de `Projeto_Detalhe`, que define qual material (e sua quantidade) é necessário na fabricação de um projeto.  
    - Além disso, um ou mais registros de `Material` também se conectam com um ou mais registros de `Entrada` por meio de um único registro de `Entrada_Detalhe`, que especifica o material (sua quantidade e seu custo) chegou no estoque, vindo de um fornecedor.  
    - Por fim, um ou mais registros de `Material` estão relacionado a um ou mais registros de `Saida` por meio de um único registro de `Saida_Detalhe`, que informa qual material (e sua quantidade) foi retirado para a execução de um projeto que faz parte de um pedido.
- Assim, passa a existir a capacidade de se comparar o somatório das entradas e saídas de cada produto, obtendo o valor do estoque.
- Além de tornar possível apurar quanto um projeto custa em material, mão de obra e toda a receita do negócio ao longo do tempo.
    
### Do fluxo:
Para facilitar o entendimento da rotina, classifiquei cada tabela em quatro categorias que seguem:  
1. Cadastros: `Cliente`, `Fornecedor`, `Material` e `Projeto`;  
1. Eventos: `Entrada`, `Pedido` e `Saida`;  
1. Categorias: `Material_Tipo` e `Material_Unidade`;  
1. Listas: `Entrada_Detalhe`, `Pedido_Detalhe`, `Projeto_Detalhe` e `Saida_Detalhe`.  
  
Como a ideia principal é criar uma forma de organização de clientes e pedidos, esta é a parte primária do funcionamento:
> Cadastrar clientes e projetos para serem usados nos eventos de pedidos.  

A parte secundária é cadastrar fornecedores e materiais, para detalharem as listas quando utilizados nos eventos de estoque e de pedidos.  
Se for de interesse do marceneiro ter estes controles, ele pode:  
> - cadastrar registros em `Produto` e `Fornecedor`;   
> - listar qual registro de `Material` é utilizado em certo registro de `Projeto`, relacionando-os em `Projeto_Detalhe`;
> - indicar qual registro de `Fornecedor` realizou um registro de `Entrada`;
> - listar qual registro de `Material` chega em um registro de `Entrada`, relacionando-os em `Entrada_Detalhe`;  
> - qual registro de `Saida` foi criado a partir de um registro de `Pedido_Detalhe`;
> - listar qual registro de `Material` é retirado em um registro de `Saida`, relacionando-os em `Saida_Detalhe`; e
> - listar qual registro de `Projeto` é realizado em um registro de `Pedido`, relacionando-os em `Pedido_Detalhe`.
### Da entrega:
- O banco de dados será acessado via GUI feita no próprio MsAccess. 
- O cliente será treinado para utiliza-la, bem como, instruido a fazer backup manual do banco.
