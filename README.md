# engsw2-2018-1
Repositório para a disciplina de Engenharia de Software 2 - 2018/1

# PREPARAÇÃO

Escolha a linguagem de sua preferência, desde que a mesma possua framework de testes (algumas boas escolhas são: Java, Kotlin, C#, Ruby, Python) e instale o SDK da mesma.

Escolha a IDE ou editor de texto de sua preferência (algumas boas escolhas são: VSCode, IntellJ, Eclipse, Visual Studio, Sublime, Atom).

Clone este repositório - git clone https://github.com/khamulbr/engsw2-2018-1.git

Crie uma branch para trabalhar - git checkout -b [name_of_your_new_branch]

# TAREFA

Vamos criar um componente de Gamificação, que armazena vários tipos de conquistas de um determinado usuário de nosso jogo. Para implementar tal componente, a especificação abaixo inclui também alguns padrões de projeto que precisamos usar. Pede-se também a implementação de um pequeno exemplo para o uso e testes do componente.

A correção do exercício levará em consideração:

* Funcionalidades criadas
* Utilização dos padrões de projeto definidos
* Criação dos testes unitários

### Instruções passo a passo da tarefa

#### Representação das conquistas

Crie uma classe chamada Conquista para representar uma conquista. Cada instância desta deve possuir uma propriedade "nome" que identifica a Conquista. Essa classe deve possuir as seguintes subclasses para representar diferentes tipos de conquista:

* Pontos: possui um valor numérico. Quando for adicionada uma Conquista desse tipo com a quantidade de pontos ganhos, deve ser somado ao anterior. Não devem haver 2 Conquistas do tipo Pontos com o mesmo nome para um usuário.

* Insígnia: representa um objetivo. Não faz sentido adicionar duas Conquistas desse tipo com o mesmo nome para um mesmo usuário.

A diferença de como cada classe deve ser tratada ao ser adicionada deve estar em cada uma delas! Crie um método na superclasse que delega essa tarefa para um método implementado na subclasse.

#### Armazenamento das conquistas
O componente deve possibilitar diversas formas de armazenamento das informações. Para esse exercício iremos utilizar apenas o armazenamento em memória.

Crie uma interface chamada ArmazenamentoConquista, com os métodos "void addConquista(String usuario, Conquista conquista)", "List<Conquista> getConquistas(String usuario)" e "Conquista getConquista(String usuario, String nomeConquista)". Essa interface terá apenas uma implementação chamada ArmazenamentoMemoriaConquista que irá armazenar essas informações em memória.
  
Crie uma classe chamada FabricaArmazenamentoConquista com o método estático "ArmazenamentoConquista getArmazenamentoConquista()". Essa classe deve retornar a instância de ArmazenamentoConquista configurada através de outro método "void setArmazenamentoConquista(ArmazenamentoConquista armazenamentoConquista)" no início da aplicação. Deve ser utilizado um padrão que permita que exista apenas uma instância de ArmazenamentoConquista sendo utilizada em toda aplicação.

#### Invocação da Funcionalidade de Gamification

A ideia é que a chamada do componente de Gamificação ocorra de forma transparente à aplicação. Dessa forma a adição das Conquistas irá ocorrer a partir de um proxy que irá envolver as classes da aplicação. O proxy só deve chamar o componente de gamificação depois que o método original for invocado na classe que está sendo encapsulada.

Para o exemplo, deve ser criado o proxy para a interface chamada ServicoForum com os seguintes métodos e regras de gamificação:

* void adicionarTopico(String usuario, String topico) - Deve adicionar 5 pontos do tipo "CRIAÇÃO". Deve adicionar a insígnia "EU FALO, SABIA?"

* void adicionarComentario(String usuario, String topico, String comentario) - Deve adicionar 3 pontos do tipo "PARTICIPAÇÃO". Deve adicionar a insígnia "MEUS DOIS CENTS"

* void darLikeTopico(String usuario, String topico, String topicoUsuario) - Deve adicionar 1 ponto do tipo "CRIAÇÃO".

* void darLikeComentario(String usuario, String topico, String comentario, String comentarioUsuario) - Deve adicionar 1 ponto do tipo "PARTICIPAÇÃO".

O proxy criado deve-se chamar ServicoForumGamificacaoProxy.

PS: não é preciso criar a implementação da interface, somente o proxy.

#### Observando as Conquistas

Deve haver uma interface chamada ObservadorConquista com o método "void atualizarConquista(String usuario, Conquista a)". Instâncias de classes que implementam essa interface podem ser adicionadas em classes com a interface ArmazenamentoConquista. Toda vez que uma nova Conquista for recebida, todas as classes desse tipo precisam ser notificadas. Se for do tipo Pontos, a notificação acontece com a quantidade total de pontos.

Para o exemplo, devem ser implementadas os seguintes observadores:

* Quando a quantidade de pontos do tipo "CRIAÇÃO" chegar a 100, o usuário deve receber a insígnia "INVENTOR"
* Quando a quantidade de pontos do tipo "PARTICIPAÇÃO" chegar a 100, o usuário deve receber a insígnia "MEMBRO DA COMUNIDADE"

#### Teste da Implementação

Deve ser feito o teste automatizado que verifica o funcionamento da implementação. Deve-se fazer o teste em cima do proxy criado e adicionando na classe ArmazenamentoConquista os observadores pedidos. Um nova instância de ArmazenamentoMemoriaConquista deve ser configurada a cada teste. Deve-se criar um mock object para fazer o papel da classe que está sendo encapsulada pelo proxy.

Os seguintes testes devem ser feitos:

* Chamar o método adicionarTopico() e ver se as conquistas foram adicionadas da forma correta
* Chamar o método adicionarComentario() e ver se as conquistas foram adicionadas da forma correta
* Chamar o método darLikeTopico() e ver se as conquistas foram adicionadas da forma correta
* Chamar o método darLikeComentario() e ver se as conquistas foram adicionadas da forma correta
* Chamar o método adicionarTopico() duas vezes e ver se os pontos foram somados e se a insígnia está presente apenas uma vez
* Fazer um teste invocando vários métodos e verificar se o resultado é o esperado.
* Fazer o mock lançar uma exceção para algum método e verificar se as Conquistas não foram adicionadas.
* Atingir 100 pontos de "CRIAÇÃO" e verificar se o usuário recebe a insígnia "INVENTOR"
* Atingir 100 pontos de "PARTICIPAÇÃO" e verificar se o usuário recebe a insígnia "MEMBRO DA COMUNIDADE"
