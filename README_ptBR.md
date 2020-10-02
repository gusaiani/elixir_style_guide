# [O Guia de Estilo Elixir][Guia de Estilo Elixir]

## Índice

* __[Introdução](#introdução)__
* __[Sobre](#sobre)__
* __[Formatação](#formatação)__
  * [Espaçamento](#espaçamento)
  * [Identação](#identação)
  * [Parenteses](#parenteses)
* __[O Guia de Estilo Elixir](#o-guia-de-estilo-elixir)__
  * [Expressões](#expressões)
  * [Nomenclatura](#nomenclatura)
  * [Comentários](#comentários)
    * [Anotações em Comentários](#anotações-em-comentários)
  * [Módulos](#módulos)
  * [Documentação](#documentação)
  * [Typespecs](#typespecs)
  * [Structs](#structs)
  * [Exceções](#exceções)
  * [Coleções](#coleções)
  * [Strings](#strings)
  * _Expressões Regulares_
  * [Metaprogramação](#metaprogramação)
  * [Testes](#testes)
* __[Recursos](#Recursos)__
  * [Guias de Estilo Alternativos](#guias-de-estilo-alternativos)
  * [Ferramentas](#ferramentas)
* __[Participando](#participando)__
  * [Contribuindo](#contribuindo)
  * [Espalhe](#espalhe)
* __[Cópias](#copias)__
  * [Licença](#licença)
  * [Atribuição](#atribuição)

## Introdução

> Arquitetura líquida. É como o jazz — você improvisa, você trabalha junto,
> você se inspira nos outros, você cria alguma coisa, eles criam alguma coisa.
>
> —Frank Gehry

Estilo é importante.
[Elixir] tem muito estilo, mas como todas as linguagens, ele pode ser estragado.
Não estrague o estilo.

## Sobre

Este é o guia de estilo da comunidade para a [Linguagem de Programação Elixir][Elixir].
Sinta-se à vontade para enviar pull requests e sugestões, e seja parte da
vibrante comunidade do Elixir.

Se você está procurando outros projetos para contribuir, por gentileza confira o [site do gerenciador de pacotes Hex][Hex].

<a name="traducoes"></a>
Traduções deste guia estão disponíveis nas seguintes línguas:

* [Chinês Simplificado]
* [Chinês Tradicional]
* [Coreano]
* [Espanhol]
* [Francês]
* [Inglês]
* [Japonês]
* [Português]

## Formatação

Elixir 1.6 introduziu as tasks [Code Formatter][Code Formatter] e [Mix format][Mix format]. É preferível o formatador para todos os novos projetos e códigos.

As regras nesta seção são aplicadas automaticamento pelo formatador de código, estão apresentados aqui como exemplos de estilo.

### Espaçamento

* <a name="espaco-branco-direita"></a>
  Evite espaço em branco no fim das linhas
  <sup>[[link](#espaco-branco-direita)]</sup>

* <a name="eof-nova-linha"></a>
  Termine todos os arquivos com uma nova linha.
  <sup>[[link](#eof-nova-linha)]</sup>

* <a name="quebra-de-linha"></a>
  Use quebra de linha estilo Unix (\*Usuários de BSD/Solaris/Linux/OSX por padrão
  estão cobertos, usuários Windows tem que tomar muito cuidado).
  <sup>[[link](#quebra-de-linha)]</sup>

* <a name="autocrlf"></a>
  Se você está usando Git talvez você queira adicionar a seguinte
  configuração para evitar quebras de linha Windows adicionadas acidentalmente:
  <sup>[[link](#autocrlf)]</sup>

  ```sh
  git config --global core.autocrlf true
  ```

* <a name="tamanho-linha"></a>
  Limite de 98 caracteres por linha. Caso queira alterar, defina `:line_length` no arquivo `.formatter.exs`.
  <sup>[[link](#tamanho-linha)]</sup>

* <a name="espacos"></a>
  Use espaço ao redor de operadores, depois de vírgulas, dois pontos e ponto e vírgulas.
  Não coloque espaço ao redor de pares correspondentes como parênteses, colchetes e chaves.
  Espaço pode ser (na maioria das vezes) irrelevante no runtime do Elixir, mas
  seu uso apropriado é essencial para se escrever código leǵivel.
  <sup>[[link](#espacos)]</sup>

  ```elixir
  soma = 1 + 2
  {a, b} = {2, 3}
  [primeiro | resto] = [1, 2, 3]
  Enum.map(["um", <<"dois">>, "três"], fn num -> IO.puts(num) end)
  ```

* <a name="sem-espacos"></a>
  Não use espaço depois de operadores que não são palavras e recebem um argumento;
  ou ao redor do operador range.
  <sup>[[link](#sem-espacos)]</sup>

  ```elixir
  0 - 1 == -1
  ^pinned = alguma_funcao()
  5 in 1..10
  ```

* <a name="espacamento-def"></a>
  Use linhas em branco entre `def`s para quebrar uma função em seus parágrafos
  lógicos.
  <sup>[[link](#espacamento-def)]</sup>

  ```elixir
  def alguma_funcao(algum_dado) do
    algum_dado |> other_function() |> List.first()
  end

  def alguma_funcao do
    resultado
  end

  def alguma_outra_funcao do
    outro_resultado
  end

  def uma_funcao_mais_longa do
    um
    dois

    três
    quatro
  end
  ```

* <a name="espacamento-defmodule"></a>
  Não use uma linha em branco após um `defmodule`.
  <sup>[[link](#espacamento-defmodule)]</sup>

* <a name="dos-longos"></a>
  Se você usar a sintaxe `do:` em funções, e o corpo da função tiver uma linha longa, coloque o `do:` em uma nova linha com um nível de indentação a mais do que a linha anterior.
  <sup>[[link](#dos-longos)]</sup>

  ```elixir
  def alguma_funcao([:foo, :bar, :baz] = args),
    do: Enum.map(args, fn arg -> arg <> " está em uma linha muito longa!" end)
  ```

  Quando você usar a convenção acima e tiver mais que uma cláusula
 usando a sintaxe `do:`, coloque o `do:` em uma nova linha para cada cláusula:

  ```elixir
  # não recomendado
  def alguma_funcao([]), do: :vazio
  def alguma_funcao(_),
    do: :linha_muito_longa_aqui

  # recomendado
  def alguma_funcao([]), do: :vazio

  def alguma_funcao(_),
    do: :linha_muito_longa_aqui
  ```

* <a name="adicione-linha-em-branco-apos-declaracao-multilinha"></a>
  Adicione uma linha em branco após uma declaração multilinha
  sinalizando que a declaração terminou.
  <sup>[[link](#adicione-linha-em-branco-apos-declaracao-multilinha)]</sup>

  ```elixir
  # não recomendado
  alguma_string =
    "Oi"
    |> String.downcase()
    |> String.trim()
  outra_string <> alguma_string

  # recomendado
  alguma_string =
    "Oi"
    |> String.downcase()
    |> String.trim()

  outra_string <> alguma_string
  ```

  ```elixir
  # também não recomendado
  alguma_coisa =
    if x == 2 do
      "Oi"
    else
      "Tchau"
    end
  String.downcase(alguma_coisa)

  # recomendado
  alguma_coisa =
    if x == 2 do
      "Oi"
    else
      "Tchau"
    end

  String.downcase(alguma_coisa)
  ```
  
* <a name="declaracao-multilinha"></a>
  Se uma list, map ou struct abrange múltiplas linhas, coloque cada elemento, assim como o colchete de abertura e fechamento em sua própria linha, e indente os elementos para mantê-los alinhados.
  <sup>[[link](#declaracao-multilinha)]</sup>

  ```elixir
  # não recomendado
  [:primeiro_item, :segundo_item, :proximo_item,
  :ultimo_item]

  # recomendado
  [
    :primeiro_item,
    :segundo_item,
    :proximo_item,
    :ultimo_item
  ]
  ```

* <a name="declaracao-com-colchetes"></a>
  Ao atribuir uma list, map ou struct mantenha o colchete de abertura na mesma linha.
  <sup>[[link](#declaracao-com-colchetes)]</sup>

  ```elixir
  # não recomendado
  lista = 
  [
    :primeiro_item,
    :segundo_item
  ]

  # recomendado
  lista = [
    :primeiro_item,
    :segundo_item
  ]
  ```

* <a name="case-multilinha"></a>
  Se algum `case` ou `cond` precisar de mais de uma linha (por ultrapassar o limite de caracteres, mulitiplas expressões ou tamanho da cláusula, etc.), use a sintaxe de multilinha para todas as cláusulas e separe cada uma com uma linha em branco.
  <sup>[[link](#case-multilinha)]</sup>

  ```elixir
  # não recomendado
  case argumento do
    true -> IO.puts("ok"); :ok
    false -> :error
  end

  # não recomendado
  case argumento do
    true ->
      IO.puts("ok")
      :ok
    false -> :error
  end

  # recomendado
  case argumento do
    true ->
      IO.puts("ok")
      :ok
    false ->
      :error
  end
  ```

* <a name="lugar-comentario"></a>
  Coloque comentários acima da linha que deseja comentar
  <sup>[[link](#lugar-comentario)]</sup>

  ```elixir
  String.first(alguma_string) # não recomendado

  # recomendado
  String.first(alguma_string)
  ```

* <a name="espaco-comentario"></a>
  Coloque um espaço entre o texto do comentário e o caracter de comentário `#`
  <sup>[[link](#espaco-comentario)]</sup>

  ```elixir
  #não recomendado
  String.first(alguma_string)

  # recomendado
  String.first(alguma_string)
  ```

### Identação

* <a name="clausulas-with"></a>
  Indente e alinhe cláusulas `with` sucessivas. Coloque o argumento `do:` em uma nova linha, alinhada com as cláusulas anteriores.
  <sup>[[link](#clausulas-with)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, bar} <- fetch(opts, :bar),
       do: {:ok, foo, bar}
  ```
  
* <a name="with-else"></a>
  Se a expressão `with` tiver um bloco `do` com mais de uma linha, ou tem uma opção `else`, use a sintaxe multilinha.
  <sup>[[link](#with-else)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, bar} <- fetch(opts, :bar) do
    {:ok, foo, bar}
  else
    :error ->
      {:error, :bad_arg}
  end
  ```

### Parenteses

* <a name="operador-pipe-parenteses"></a>
  Use parênteses em funções de aridade um ao usar o operador pipe (`|>`)
  <sup>[[link](#operador-pipe-parenteses)]</sup>

  ```elixir
  # não recomendado
  alguma_string |> String.downcase |> String.trim

  # recomendado
  alguma_string |> String.downcase() |> String.trim()
  ```

* <a name="nomes-de-funcao-com-parenteses"></a>
  Nunca coloque um espaço entre o nome da função e a abertura dos parênteses.
  <sup>[[link](#nomes-de-funcao-com-parenteses)]</sup>

  ```elixir
  # não recomendado
  f (3 + 2)

  # recomendado
  f(3 + 2)
  ```

* <a name="chamada-de-funcao-com-parenteses"></a>
  Use parênteses em chamadas de função, especialmente dentro de uma pipeline.
  <sup>[[link](#chamada-de-funcao-com-parentese)]</sup>

  ```elixir
  # não recomendado
  f 3

  # recomendado
  f(3)

  # não recomendado e é convertido como rem(2, (3 |> g)), que não é o que você quer.
  2 |> rem 3 |> g

  # recomendado
  2 |> rem(3) |> g
  ```

* <a name="colchetes-listas-palavras-chave"></a>
  Omita colchetes de listas de palavras-chave sempre que eles forem opcionais.
  <sup>[[link](#colchetes-listas-palavras-chave)]</sup>

  ```elixir
  # não recomendado
  alguma_funcao(foo, bar, [a: "baz", b: "qux"])

  # recomendado
  alguma_funcao(foo, bar, a: "baz", b: "qux")
  ```

## O Guia de Estilo Elixir

As regras nesta seção podem não ser aplicadas pelo formatador de código, mas são geralmente a prática preferida.

### Expressões

* <a name="def-linha-unica-com-def-multilinha"></a>
  Mantenha `def`s de única linha juntos, mas separe do `def`s de multilinha com uma linha em branco.
  <sup>[[link](#def-linha-unica-com-def-multilinha)]</sup>

  ```elixir
  def alguma_funcao(nil), do: {:error, "Sem valor"}
  def alguma_funcao([]), do: :ok

  def alguma_funcao([primeiro | resto]) do
    alguma_funcao(resto)
  end
  ```

* <a name="varios-def-multilinha"></a>
  Caso tenha mais de um `def` multilinha, não use `def` de uma linha.
  <sup>[[link](#colchetes-listas-palavras-chave)]</sup>

  ```elixir
  def alguma_funcao(nil) do
    {:error, "Sem valor"}
  end

  def alguma_funcao([]) do
    :ok
  end
  
  def alguma_funcao([primeiro | resto]) do
    alguma_funcao(resto)
  end

  def alguma_funcao([primeiro | resto], opcoes) do
    alguma_funcao(resto, opcoes)
  end
  ```

* <a name="operador-pipe"></a>
  Use o operador pipe (`|>`) para encadear funções.
  <sup>[[link](#operador-pipe)]</sup>

  ```elixir
  # não recomendado
  String.trim(String.downcase(alguma_string))

  # recomendado
  alguma_string |> String.downcase() |> String.trim()

  # Pipelines multilinha não recebem indentação extra
  alguma_string
  |> String.downcase()
  |> String.trim()

  # Pipelines multilinha do lado direito de um pattern match
  # devem ser indentados em uma nova linha
  string_sanitizada =
    alguma_string
    |> String.downcase()
    |> String.trim()
  ```

  Mesmo este sendo o método recomendado, lembre-se que copiar e colar pipelines multilinha no IEx pode resultar em erro de sintaxe, já que o IEx vai avaliar a primeira linha sem perceber que a próxima linha tem um pipeline. Para evitar isso, coloque o código copiado entre parênteses.

* <a name="evite-pipelines-solitarios"></a>
  Evite usar o operador pipe uma vez só.
  <sup>[[link](#evite-pipelines-solitarios)]</sup>

  ```elixir
  # não recomendado
  alguma_string |> String.downcase()

  System.version() |> Version.parse()

  # recomendado
  String.downcase(alguma_string)
  
  Version.parse(System.version())

* <a name="variaveis-sozinhas"></a>
  Use variáveis _sozinhas_ na primeira parte de uma cadeia de funções.
  <sup>[[link](#variaveis-sozinhas)]</sup>

  ```elixir
  # não recomendado
  String.trim(alguma_string) |> String.downcase() |> String.codepoints()

  # recomendado
  alguma_string |> String.trim() |> String.downcase() |> String.codepoints()
  ```

* <a name="parenteses"></a>
  Use parênteses quando um `def` tiver argumentos, e omita os parênteses quando ele não tiver argumentos.
  <sup>[[link](#parenteses)]</sup>

  ```elixir
  # não recomendado
  def alguma_funcao arg1, arg2 do
    # corpo omitido
  end

  def alguma_funcao() do
    # corpo omitido
  end

  # recomendado
  def alguma_funcao(arg1, arg2) do
    # corpo omitido
  end

  def alguma_funcao do
    # corpo omitido
  end
  ```

* <a name="do-de-uma-linha-só-if-unless"></a>
  Use `do:` para `if/unless` de uma linha só.
  <sup>[[link](#do-de-uma-linha-só-if-unless)]</sup>

  ```elixir
  # recomendado
  if alguma_condicao, do: # alguma_coisa
  ```

* <a name="unless-com-else"></a>
  Nunca use `unless` com `else`.
  Reescreva estes com apenas casos positivos.
  <sup>[[link](#unless-com-else)]</sup>

  ```elixir
  # não recomendado
  unless successo do
    IO.puts('falha')
  else
    IO.puts('successo')
  end

  # recomendado
  if successo do
    IO.puts('successo')
  else
    IO.puts('falha')
  end
  ```

* <a name="true-como-ultima-condicao"></a>
  Use `true` como a última condição do `cond` quando você precisa de uma
  cláusula que sempre dê match.
  <sup>[[link](#true-como-ultima-condicao)]</sup>

  ```elixir
  # não recomendado
  cond do
    1 + 2 == 5 ->
      "Não"

    1 + 3 == 5 ->
      "Oh, oh"

    :else ->
      "OK"
  end

  # recomendado
  cond do
    1 + 2 == 5 ->
      "Não"

    1 + 3 == 5 ->
      "Oh, oh"

    true ->
      "OK"
  end
  ```

* <a name="parenteses-e-funcoes-aridade-zero"></a>
  Use parênteses quando chamar funções de aridade zero, para que elas
  fiquem distintas de variáveis.
  A partir do Elixir 1.4 o compilador vai emitir `warnings` em lugares onde
  esta ambiguidade existir.
  <sup>[[link](#parenteses-e-funcoes-aridade-zero)]</sup>

  ```elixir
  defp fazer_algo, do: ...

  # não recomendado
  def minha_funcao do
    # isto é uma variável ou uma chamada de função?
    fazer_algo
  end

  # recomendado
  def minha_funcao do
    # isto claramente é uma chamada de função
    fazer_algo()
  end
  ```

### Nomenclatura

* <a name="snake-case"></a>
  Use `snake_case` para átomos, funções e variáveis.
  <sup>[[link](#snake-case)]</sup>

  ```elixir
  # não recomendado
  :"algum átomo"
  :AlgumAtomo
  :algumAtomo

  algumaVar = 5

  def algumaFuncao do
    ...
  end

  def AlgumaFuncao do
    ...
  end

  # recomendado
  :algum_atom

  alguma_var = 5

  def alguma_funcao do
    ...
  end
  ```

* <a name="camel-case"></a>
  Use `CamelCase` para módulos (mantenha siglas como HTTP, RFC, XML em maiúsculo).
  <sup>[[link](#camel-case)]</sup>

  ```elixir
  # não recomendado
  defmodule Algummodulo do
    ...
  end

  defmodule Algum_Modulo do
    ...
  end

  defmodule AlgumXml do
    ...
  end

  # recomendado
  defmodule AlgumModulo do
    ...
  end

  defmodule AlgumXML do
    ...
  end
  ```

* <a name="nome-de-macro-com-predicado-em-guards"></a>
  Os nomes das macros com predicado (funções geradas na hora da compilação que retornam um
  valor booleano) _que podem ser usadas dentro de guards_ devem ser prefixados com `is_`.
  Para uma lista de expressões permitidas, veja nos documentos [Guard][Guard Expressions].
  <sup>[[link](#nome-de-macro-com-predicado-em-guards)]</sup>

  ```elixir
  defguard is_cool(var) when var == "cool"
  defguardp is_very_cool(var) when var == "very cool"
  ```

* <a name="nome-de-macro-de-predicado-sem-guards"></a>
  Os nomes de funções com predicados _que não podem ser usadas em guards_ devem
  ter um ponto de interrogação à direita (`?`) em vez do prefixo `is_` (ou similar).
  <sup>[[link](#nome-de-macro-de-predicado-sem-guards)]</sup>

  ```elixir
  def cool?(var) do
    # Verificação complexa se var for cool não é possível em uma função pura.
  end
  ```

* <a name="funcoes-privadas-com-mesmo-nome-de-publicas"></a>
  Funções privadas com o mesmo nome de funções públicas devem começar com `do_`.
  <sup>[[link](#funcoes-privadas-com-mesmo-nome-de-publicas)]</sup>

  ```elixir
  def soma(list), do: do_soma(list, 0)

  # funções privadas
  defp do_soma([], total), do: total
  defp do_soma([head | tail], total), do: do_soma(tail, head + total)
  ```

### Comentários

* <a name="codigo-expressivo"></a>
  Escreva código expressivo e busque transmitir a intenção do seu programa
  através de fluxo de controle, estrutura e nomenclaturas.
  <sup>[[link](#codigo-expressivo)]</sup>

* <a name="gramatica-comentario"></a>
  Comentários de mais de uma palavra começam com maiúscula, e frases recebem pontuação. Use [um espaço][Sentence Spacing] depois dos pontos finais.
  <sup>[[link](#gramatica-comentario)]</sup>

  ```elixir
  # não recomendado
  # este comentário em minúsculas não tem pontuação

  # recomendado
  # Exemplo com maiúscula
  # Use pontuação em frases completas.
  ```

* <a name="limite-comentario"></a>
  Limite comentário a 100 caracteres por linha.
  <sup>[[link](#limite-comentario)]</sup>

#### Anotações em Comentários

* <a name="anotacoes"></a>
  Anotações normalmente devem vir na linha imediatamente acima do respectivo código.
  <sup>[[link](#anotacoes)]</sup>

* <a name="palavra-chave-anotacao"></a>
  A palavra-chave de anotação é inteira em maiúsculas, seguida por dois-pontos e espaço, e então uma descrição do problema.
  <sup>[[link](#palavra-chave-anotacao)]</sup>

  ```elixir
  # TODO: Deprecate in v2.0.
  def alguma_funcao(arg), do: {:ok, arg}
  ```

* <a name="excecoes-anotacoes"></a>
  Nos casos em que o problema é tão óbvio que qualquer documentação seria
  redundante, as anotações podem vir no fim da linha em questão sem comentário adicional.
  Esta forma deve ser a exceção e não a regra.
  <sup>[[link](#excecoes-anotacoes)]</sup>

  ```elixir
  start_task()
  
  # FIXME
  Process.sleep(5000)
  ```

* <a name="notas-todo"></a>
  Use `TODO` para anotar funcionalidades que faltam ou funcionalidades que devem
  ser acrescentadas no futuro.
  <sup>[[link](#notas-todo)]</sup>

* <a name="notas-fixme"></a>
  Use `FIXME` para anotar código quebrado que precisa ser corrigido.
  <sup>[[link](#notas-fixme)]</sup>

* <a name="notas-optimize"></a>
  Use `OPTIMIZE` para anotar código lento ou ineficiente que pode causar
  problemas de desempenho.
  <sup>[[link](#notas-optimize)]</sup>

* <a name="notas-hack"></a>
  Use `HACK` para anotar lugares onde existem práticas questionáveis e que
  deveriam ser refatoradas.
  <sup>[[link](#notas-hack)]</sup>

* <a name="notas-review"></a>
  Use `REVIEW` para anotar qualquer coisa que precise ser revista para confirmar
  que está funcionando conforme pretendido.
  Exemplo: `REVIEW: Temos certeza que é assim que o cliente faz X hoje em dia?`
  <sup>[[link](#notas-review)]</sup>

* <a name="palavras-chave-customizadas"></a>
  Use outras palavras-chave customizadas se apropriado, mas se certifique de
  documentá-las no `README` do seu projeto ou algo similar.
  <sup>[[link](#palavras-chave-customizadas)]</sup>

### Módulos

* <a name="um-modulo-por-arquivo"></a>
  Use um módulo por arquivo, a não ser que o módulo seja usado apenas
  internamente por outro módulo (um teste, por exemplo).
  <sup>[[link](#um-modulo-por-arquivo)]</sup>

* <a name="nomes-de-arquivo-underscore"></a>
  Use nomes de arquivo em `snake_case` e nomes de módulo em`CamelCase`.
  <sup>[[link](#nomes-de-arquivo-underscore)]</sup>

  ```elixir
  # arquivo chamado algum_modulo.ex

  defmodule AlgumModulo do
  end
  ```

* <a name="aninhamento-nomes-modulo"></a>
  Expresse cada nível de aninhamento em um nome de módulo como um diretório.
  <sup>[[link](#aninhamento-nomes-modulo)]</sup>

  ```elixir
  # arquivo chamado parser/core/xml_parser.ex

  defmodule Parser.Core.XMLParser do
  end
  ```

* <a name="ordenacao-atributos-modulo"></a>
  Liste atributos e diretivas de módulo na seguinte ordem:
  <sup>[[link](#ordenacao-atributos-modulo)]</sup>

  1. `@moduledoc`
  1. `@behaviour`
  1. `use`
  1. `import`
  1. `alias`
  1. `require`
  1. `@module_attribute`
  1. `defstruct`
  1. `@type`
  1. `@callback`
  1. `@macrocallback`
  1. `@optional_callbacks`
  1. `defmacro`, `defmodule`, `defguard`, `def`, etc.  

  Adicione uma linha em branco entre cada agrupamento, e ordene os termos (como nomes de módulos) alfabeticamente. Abaixo um exemplo geral de como você deve ordenar as coisas em seus módulos:

  ```elixir
  defmodule MeuModulo do
    @moduledoc """
    Exemplo de módulo
    """

    @behaviour MeuBehaviour

    use GenServer

    import AlgumaCoisa
    import OutraCoisa

    alias Meu.Nome.Comprido.Modulo
    alias Meu.Outro.Exemplo.Modulo

    require Integer

    @module_attribute :foo
    @other_attribute 100

    defstruct [:name, params: []]

    @type params :: [{binary, binary}]

    @callback alguma_funcao(termo) :: :ok | {:error, termo}

    @macrocallback nome_macro(termo) :: Macro.t()

    @optional_callbacks nome_macro: 1

    @doc false
    defmacro __using__(_opts), do: :no_op

    @doc """
    Determina quando o termo é `:ok`. Permitido em guards.
    """
    defguard is_ok(termo) when term == :ok

    @impl true
    def init(state), do: {:ok, state}

    # Defina outras funções aqui.
  end
  ```

* <a name="pseudo-variavel-modulo"></a>
  Use a pseudo variável `__MODULE__` quando um módulo se referir a si mesmo. Isto previne que tenhamos que atualizar auto-referências quando o nome do módulo mudar.
  <sup>[[link](#pseudo-variavel-modulo)]</sup>

  ```elixir
  defmodule AlgumProjeto.AlgumModulo do
    defstruct [:nome]

    def nome(%__MODULE__{nome: nome}), do: nome
  end
  ```

* <a name="alias-modulos-auto-referentes"></a>
  Se você quiser um nome mais bonito para uma auto-referência de módulo, crie um alias.
  <sup>[[link](#alias-modulos-auto-referentes)]</sup>

  ```elixir
  defmodule AlgumProjeto.AlgumModulo do
    alias __MODULE__, as: AlgumModulo

    defstruct [:nome]

    def nome(%AlgumModulo{nome: nome}), do: nome
  end
  ```

* <a name="nomes-modulo-repetitivos"></a>
  Evite repetir fragmentos em nomes e namespaces de módulos. Isto melhora a legibilidade e elimina [aliases ambíguos][Conflicting Aliases].
  <sup>[[link](#nomes-modulo-repetitivos)]</sup>

  ```elixir
  # não recomendado
  defmodule Todo.Todo do
    ...
  end

  # recomendado
  defmodule Todo.Item do
    ...
  end
  ```

### Documentação

Documentação em Elixir (quando lida seja no `iex` com `h` seja gerada com
[ExDoc]) usa os [Atributos de Módulo] `@moduledoc` e `@doc`.

* <a name="moduledocs"></a>
  Inclua sempre um atributo `@moduledoc` na linha imediatamente posterior ao `defmodule`
 em seu módulo.
  <sup>[[link](#moduledocs)]</sup>

  ```elixir
  # não recomendado
  defmodule OutroModulo do
    use AlgumModulo

    @moduledoc """
    Informação sobre o módulo.
    """
    ...
  end

  # recomendado
  defmodule TerceiroModulo do
    @moduledoc """
    Informação sobre o módulo.
    """

    use AlgumModulo
    ...
  end
  ```

* <a name="moduledoc-false"></a>
  Use `@moduledoc false` se você não pretende documentar o módulo.
  <sup>[[link](#moduledoc-false)]</sup>

  ```elixir
  defmodule AlgumModulo do
    @moduledoc false
    ...
  end
  ```

* <a name="espacamento-moduledoc"></a>
  Separe o código que vem depois do `@moduledoc` com uma linha em branco.
  <sup>[[link](#espacamento-moduledoc)]</sup>

  ```elixir
  # não recomendado
  defmodule AlgumModulo do
    @moduledoc """
    Informação sobre o módulo.
    """
    use OutroModulo
  end

  # recomendado
  defmodule AlgumModulo do
    @moduledoc """
    Informação sobre o módulo.
    """

    use OutroModulo
  end
  ```

* <a name="heredocs"></a>
  Use heredocs com markdown na documentação.
  <sup>[[link](#heredocs)]</sup>

  ```elixir
  # não recomendado
  defmodule AlgumModulo do
    @moduledoc "Informação sobre o módulo."
  end

  defmodule AlgumModulo do
    @moduledoc """
    Informação sobre o módulo.

    Exemplos:
    iex> AlgumModulo.alguma_funcao
    :resultado
    """
  end

  # recomendado
  defmodule AlgumModulo do
    @moduledoc """
    Informação sobre o módulo.

    ## Exemplos

        iex> AlgumModulo.alguma_funcao
        :resultado
    """
  end
  ```

### Typespecs

Typespecs são notações para declarar tipos e especificações, para documentação ou para o Dialyzer (ferramenta de análise estática).

Tipos customizados devem ser definidos no topo do módulo com as outras diretivas (veja [Módulos](#módulos)).

* <a name="typedocs"></a>
  Aplique definições `@typedoc` e `@type` juntas, e separe cada par
  com uma linha em branco.
  <sup>[[link](#typedocs)]</sup>

  ```elixir
  defmodule AlgumModulo do
    @moduledoc false

    @typedoc "O nome"
    @type name :: atom

    @typedoc "O resultado"
    @type result :: {:ok, termo} | {:error, termo}

    ...
  end
  ```

* <a name="tipos-uniao"></a>
  Se um tipo de união for longo demais para caber em um só linha, adicione uma nova linha e indente com espaços para alinhar os tipos.
  <sup>[[link](#tipos-uniao)]</sup>

  ```elixir
  # não recomendado
  @type tipo_de_uniao_longo :: 
          algum_tipo | outro_tipo | algum_outro_tipo | um_ultimo_tipo

  # recomendado
  @type tipo_de_uniao_longo :: 
          algum_tipo
          | outro_tipo
          | algum_outro_tipo
          | um_ultimo_tipo

  ```

* <a name="nomeando-tipos-principais"></a>
  Nomeie como `t` o tipo principal de um módulo, por exemplo: a especificação de tipo para um struct.
  <sup>[[link](#nomeando-tipos-principais)]</sup>

  ```elixir
  defstruct [:nome, params: []]

  @type t :: %__MODULE__{
    nome: String.t() | nil,
    params: Keyword.t()
  }
  ```

* <a name="espacamento-specs"></a>
  Escreva especificações logo acima da definicação de uma função, sem separá-los com linha em branco.
  <sup>[[link](#espacamento-specs)]</sup>

  ```elixir
  @doc """
  Alguma descrição da função
  """
  @spec alguma_funcao(termo) :: result
  def alguma_funcao(algum_dado) do
    {:ok, algum_dado}
  end
  ```

### Structs

* <a name="defaults-campos-struct-nil"></a>
  Use uma lista de átomos para campos de struct que tenham default de `nil`, seguidos pelas outras palavras-chave.
  <sup>[[link](#defaults-campos-struct-nil)]</sup>

  ```elixir
  # não recomendado
  defstruct nome: nil, params: nil, ativo: true

  # recomendado
  defstruct [:nome, :params, ativo: true]
  ```

* <a name="colchetes-defstruct"></a>
  Omita colchetes quando o argumento de um `defstruct` for uma lista de palavras-chave.
  <sup>[[link](#colchetes-defstruct)]</sup>

  ```elixir
  # não recomendado
  defstruct [params: [], ativo: true]

  # recomendado
  defstruct params: [], ativo: true

  # obrigatório - colchetes não são opcionais, com ao menos um átomo na lista
  defstruct [:nome, params: [], ativo: true]
  ```

* <a name="linhas-defstruct-adicionais"></a>
  Indente linhas adicionais de defstruct, mantendo cada elemento em linhas separadas  e alinhados.
  <sup>[[link](#linhas-defstruct-adicionais)]</sup>

  ```elixir
  defstruct foo: "teste",
            bar: true,
            baz: false,
            qux: false,
            quux: 1
  ```

* <a name="linhas-defstruct-adicionais-colchetes"></a>
  Formate como lista multilinha structs multilinha que precisa de colchetes.
  <sup>[[link](#linhas-defstruct-adicionais)]</sup>

  ```elixir
  defstruct [
    :nome,
    params: [],
    ativo: true
  ```

### Exceções

* <a name="nomes-excecoes"></a>
  Adicione o sufixo `Error` aos nomes de exceções.
  <sup>[[link](#nomes-excecoes)]</sup>

  ```elixir
  # não recomendado
  defmodule BadHTTPCode do
    defexception [:mensagem]
  end

  defmodule BadHTTPCodeException do
    defexception [:mensagem]
  end

  # recomendado
  defmodule BadHTTPCodeError do
    defexception [:mensagem]
  end
  ```

* <a name="mensagens-erro-minusculas"></a>
  Use minúsculas em mensagens de erro quando emitindo exceções, sem pontuação ao final.
  <sup>[[link](#mensagens-erro-minusculas)]</sup>

  ```elixir
  # não recomendado
  raise ArgumentError, "Isto não é válido."

  # recomendado
  raise ArgumentError, "isto é válido"
  ```

### Coleções

* <a name="sintaxe-especial-lista"></a>
  Sempre usar a sintaxe especial para lista de palavras-chave.
  <sup>[[link](#sintaxe-especial-lista)]</sup>

  ```elixir
  # não recomendado
  algum_valor = [{:a, "baz"}, {:b, "qux"}]

  # recomendado
  algum_valor = [a: "baz", b: "qux"]
  ```

* <a name="map-todos-atomos"></a>
  Use a sintaxe abreviada chave-valor para maps onde todos as chaves são átomos.
  <sup>[[link](#map-todos-atomos)]</sup>

  ```elixir
  # não recomendado
  %{:a => 1, :b => 2, :c => 0}

  # recomendado
  %{a: 1, b: 2, c: 0}
  ```

* <a name="map-todos-atomos"></a>
  Use a sintaxe verbosa chave-valor para maps se qualquer chave não for átomo.
  <sup>[[link](#map-todos-atomos)]</sup>

  ```elixir
  # não recomendado
  %{"c" => 0, a: 1, b: 2}

  # recomendado
  %{:a => 1, :b => 2, "c" => 0}
  ```

### Strings

* <a name="match-strings-com-concatenador"></a>
  Dê match em strings usando o concatenador de strings, e não padrões binários.
  <sup>[[link](#match-strings-com-concatenador)]</sup>

  ```elixir
  # não recomendado
  <<"minha"::utf8, _resto>> = "minha string"

  # recomendado
  "minha" <> _resto = "minha string"
  ```

### Expressões Regulares

_A secão de Coleções do guia ainda não foi adicionada._

### Metaprogramação

* <a name="evite-metaprogramacao"></a>
  Evite metaprogramação desnecessária.
  <sup>[[link](#evite-metaprogramacao)]</sup>

### Testes

* <a name="testando-ordem-assert"></a>
  Ao escrever asserções [ExUnit], mantenha a consistência entre a ordem dos
  valores esperados e reais que estão sob teste.
  Prefira usar o resultado esperado à direita, a não ser que a asserção
  seja um pattern match.
  <sup>[[link](#testando-ordem-assert)]</sup>

  ```elixir
  # não recomendado
  assert false == minha_funcao(2)

  # recomendado
  assert minha_funcao(1) == true

  # obrigatório - a asserção é um pattern match
  assert {:ok, expected} = minha_funcao(3)
  ```

## Recursos

### Guias de Estilo Alternativos

* [Aleksei Magusev's Elixir Style Guide](https://github.com/lexmag/elixir-style-guide#readme)
  — Um guia de estilo Elixir opinativo inspirado nas práticas encontradas nas bibliotecas 'core' do Elixir. Desenvolvido por [Aleksei Magusev](https://github.com/lexmag) e [Andrea Leopardi](https://github.com/whatyouhide), membros da equipe principal do Elixir. Apesar do projeto do Elixir não aderir a um guia de estilo específico, este é o guia mais próximo de suas convenções.

* [Credo's Elixir Style Guide](https://github.com/rrrene/elixir-style-guide#readme)
  — Guia de Estilo para a linguagem Elixir, implementada pelo [Credo](http://credo-ci.org), ferramenta de análise estática de código.

### Ferramentas

Confira o [Awesome Elixir][Code Analysis] para encontrar bibliotecas e
ferramentas para te ajudar em análise e linting de código.

## Participando

### Contribuindo

Esperamos que este se torne um ponto de encontro para discussões da comunidade
sobre as melhores práticas em Elixir.
Sinta-se bem-vindo(a) a abrir issues ou enviar pull requests com melhorias.
Obrigado desde já por sua ajuda!

Confira as [dicas de contribuição - em inglês][Contribuindo]
e o [código de conduta][Code Of Conduct] para saber mais.

### Espalhe

Um guia de estilo de uma comunidade não faz sentido sem o apoio da comunidade.
Por gentileza tuíte, [dê estrelas][Stargazers] e compartilhe [este guia][Elixir Style Guide] com todos, para que todos possam contribuir.

## Cópias

### Licença

![Licença Creative Commons](http://i.creativecommons.org/l/by/3.0/88x31.png)
Este trabalho é licenciada sob uma
[Licença Creative Commons Attribution 3.0 Unported][Licença]

### Atribuição

A estrutura deste guia, alguns trechos de código dos exemplos, e muito pontos
iniciais deste documento foram empresados do [Guia de Estilo da Comunidade Ruby].
Muitas coisas se aplicavam ao Elixir e nos permitiram publicar _algum_ documento
mas rápido e dar início ao projeto.
out quicker to start the conversation.

Aqui está a [lista das pessoas que gentilmente contribuíram][Contributors] com este
projeto.

<!-- Links -->
[Chinês Tradicional]: https://github.com/elixirtw/elixir_style_guide/blob/master/README_zhTW.md
[Code Analysis]: https://github.com/h4cc/awesome-elixir#code-analysis
[Code Of Conduct]: https://github.com/christopheradams/elixir_style_guide/blob/master/CODE_OF_CONDUCT.md
[Conflicting Aliases]: https://elixirforum.com/t/using-aliases-for-fubar-fubar-named-module/1723
[Contribuindo]: https://github.com/elixir-lang/elixir/blob/master/CODE_OF_CONDUCT.md
[Contributors]: https://github.com/christopheradams/elixir_style_guide/graphs/contributors
[Coreano]: https://github.com/marocchino/elixir_style_guide/blob/new-korean/README-koKR.md
[Elixir]: http://elixir-lang.org
[Espanhol]: https://github.com/albertoalmagro/elixir_style_guide/blob/spanish/README_esES.md
[ExDoc]: https://github.com/elixir-lang/ex_doc
[ExUnit]: https://hexdocs.pm/ex_unit/ExUnit.html
[Guard Expressions]: http://elixir-lang.org/getting-started/case-cond-and-if.html#expressions-in-guard-clauses
[Guia de Estilo Elixir]: https://github.com/christopheradams/elixir_style_guide
[Hex]: https://hex.pm/packages
[Inglês]: https://github.com/christopheradams/elixir_style_guide/blob/master/README.md
[Japonês]: https://github.com/kenichirow/elixir_style_guide/blob/master/README-jaJP.md
[Licença]: http://creativecommons.org/licenses/by/3.0/deed.en_US
[Atributos de Módulo]: http://elixir-lang.org/getting-started/module-attributes.html#as-annotations
[Português]: https://github.com/gusaiani/elixir_style_guide/blob/master/README_ptBR.md
[Guia de Estilo da Comunidade Ruby]: https://github.com/bbatsov/ruby-style-guide
[Sentence Spacing]: http://en.wikipedia.org/wiki/Sentence_spacing
[Stargazers]: Use://github.com/christopheradams/elixir_style_guide/stargazers
[Code Formatter]: https://hexdocs.pm/elixir/Code.html#format_string!/2
[Mix format]: https://hexdocs.pm/mix/Mix.Tasks.Format.html
[Guard Expressions]: https://hexdocs.pm/elixir/guards.html#list-of-allowed-expressions
[Elixir Style Guide]: https://github.com/christopheradams/elixir_style_guide
[Chinês Simplificado]: https://github.com/geekerzp/elixir_style_guide/blob/master/README-zhCN.md
[Francês]: https://github.com/ronanboiteau/elixir_style_guide/blob/master/README_frFR.md
