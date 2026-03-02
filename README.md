Ciclo BDD e TDD no RottenPotatoes
===

Nesta tarefa a ideia é juntar Desenvolvimento Guiado por Comportamento com Desenvolvimento Guiado por Testes, usando Cucumber e RSpec, para acrescentar ao RottenPotatoes o recurso "encontrar filmes do mesmo diretor" e publicar o resultado no Heroku.

Para começar, faça um fork desse repositório para a sua conta e copie o endereço de clone do seu próprio fork. No topo do workspace, execute:

`git clone <endereço do seu fork>`

Em seguida, prepare o ambiente assim:

---
1) Entre na pasta do projeto: cd hw-bdd-tdd-cycle/rottenpotatoes
2) Rode bundle install --without production para garantir que todas as gems estejam no lugar.
3) Rode bundle exec rake db:migrate para aplicar as migrações.
4) Gere as pastas de suporte das duas ferramentas, aceitando sobrescrever o que já existir:

```shell
rails generate cucumber:install capybara
rails generate cucumber_rails_training_wheels:install
rails generate rspec:install
```
5) Confira a instalação rodando as tarefas `rspec` e `cucumber`.

Como ainda não há cenários escritos, as duas devem terminar sem falhar, apenas avisando que não existem testes para executar. Algumas versões do rspec também reclamam da ausência de arquivos _spec.rb.

**Parte 1: acrescentar o campo de diretor à tabela de filmes**

Crie e aplique uma migração que adiciona a coluna de diretor na tabela de filmes, guardando o nome como texto.
A dica é usar o método de inclusão de coluna da migração do ActiveRecord.

Não esqueça de liberar a nova coluna na lista de atributos permitidos dentro de `movie_params`, no controlador de filmes.

Depois de aplicar a migração, rode também a preparação do banco de testes para carregar o esquema novo.

**Parte 2: usar BDD + TDD até os cenários passarem**

Existem três cenários de Cucumber prontos que guiam a criação do caminho feliz da busca por diretor.
O primeiro cobre o cadastro de diretor num filme já existente e não exige novas telas nem novas ações no controlador, mas pede ajustes nas telas atuais, uma definição de passo nova e talvez uma ou duas linhas em `features/support/paths.rb`.

O segundo cobre o clique num link novo na página de detalhes do filme, que lista os demais títulos do mesmo diretor. Aqui é preciso mexer na tela de detalhes e criar rota, tela e ação no controlador para essa busca.

O terceiro trata o caminho triste: o filme aberto não tem diretor cadastrado e a busca é acionada mesmo assim.

Implemente um passo de cada vez, criando com RSpec as especificações de controlador e de modelo que conduzem o código. No mínimo você vai escrever testes que direcionem:

+ uma rota REST para a busca de filmes parecidos. Use a sintaxe de casamento de rotas e, se quiser, um apelido para gerar o helper correspondente. Você provavelmente não vai testar isso direto no rspec, mas se a rota estiver errada algum passo vai quebrar.

+ uma ação de controlador que receba o clique e capture o identificador do filme de origem da busca;

+ um método no modelo que devolva os filmes cujo diretor coincide com o do filme atual. Isso implica pelo menos duas especificações de controlador (com diretor preenchido e sem diretor) e duas de modelo (encontra iguais, não encontra diferentes).

Fica a seu critério tratar o caso sem diretor no controlador ou no modelo, mas o teste da opção escolhida é obrigatório. Lembre-se de exigir o arquivo de apoio do Rails no topo dos arquivos de especificação.

Também queremos ver a cobertura de código.

Adicione a gem de cobertura ao grupo de testes do seu arquivo de dependências e rode a instalação novamente sem o grupo de produção.

Depois coloque as duas linhas de inicialização no topo do apoio do rspec e do ambiente do cucumber.

A partir daí, toda execução de rspec ou cucumber gera um relatório na pasta de cobertura. As duas ferramentas são usadas em conjunto com tanta frequência que a gem sabe combinar os resultados, sem uma sobrescrever a outra.

Para ver o relatório no Cloud9, abra o arquivo index.html dentro da pasta de cobertura e use o botão de execução no topo: ele sobe um servidor com um link para a página.

Melhore a cobertura escrevendo testes de unidade e/ou cenários para partes ainda sem cobertura. Vale investir nas ações de atualização, remoção e criação do controlador, e talvez reaproveitar cenários de tarefas anteriores.

**Entrega:**

Compacte os diretórios relevantes da aplicação:

* app/
* config/
* db/migrate
* features/
* spec/
* Gemfile
* Gemfile.lock

Se você alterou outros arquivos, inclua também. Em sistemas unix-like, vá até a raiz do projeto e rode o comando de compactação com essa lista de caminhos, gerando um único arquivo compactado para envio.