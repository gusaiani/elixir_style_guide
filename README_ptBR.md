# [O Guia de Estilo Elixir][Guia de Estilo Elixir]

## Índice

* __[Introdução](#introducao)__
* __[O Guia de Estilo Elixir](#o-guia)__
  * [Layout do Código](#layout-codigo)
  * [Sintaxe](#sintaxe)
  * [Nomenclatura](#nomenclatura)
  * [Comentários](#comentarios)
    * [Anotações em Comentários](#anotacoes-comentarios)
  * [Módulos](#modulos)
  * [Documentação](#documentacao)
  * [Typespecs](#typespecs)
  * [Structs](#structs)
  * [Exceções](#excecoes)
  * _Coleções_
  * [Strings](#strings)
  * _Expressões Regulares_
  * [Metaprogramação](#metaprogramming)
  * [Testes](#testes)
  * [Guias de Estilo Alternativos](#guias-estilo-alternativos)
  * [Ferramentas](#ferramentas)
* __[Participando](#participando)__
  * [Contribuindo](#contribuindo)
  * [Espalhe](#espalhe)
* __[Cópias](#copias)__
  * [Licença](#licenca)
  * [Atribuição](#atribuicao)

## Introdução

> Arquitetura líquida. É como o jazz - você improvisa, você trabalha junto,
> você se inspira nos outros, você cria alguma coisa, eles criam alguma coisa.
>
> —Frank Gehry

Estilo é importante.
[Elixir] tem muito estilo, mas como todas as linguagens, ele pode ser estragado.
Não estrague o estilo.

## O Guia

Este é o guia de estilo da comunidade para a [Linguagem de Programação Elixir][Elixir].
Sinta-se à vontade para enviar pull requests e sugestões, e seja parte da
vibrante comunidade do Elixir.

Se você está procurando outros projetos para contribuir, por gentileza confira o [site do gerenciador de pacotes Hex][Hex].
If you're looking for other projects to contribute to please see the
[Hex package manager site][Hex].

<a name="traducoes"></a>
Traduções deste guia estão disponíveis nas seguintes línguas:

* [Chinês Tradicional]
* [Coreano]
* [Espanhol]
* [Inglês]
* [Japonês]
* [Português]

### Layout do Código Fonte

* <a name="indentacao-com-espacos"></a>
  Use dois **espaços** por nível de recuo.
  Não use hard tabs.
  <sup>[[link](#indentacao-com-espacos)]</sup>

  ```elixir
  # não preferido - quatro espaços
  def alguma_funcao do
      fazer_algo
  end

  # preferido
  def alguma_funcao do
    fazer_algo
  end
  ```

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
  Enum.map(["um", <<"dois">>, "três"], fn num -> IO.puts num end)
  ```

* <a name="sem-espacos"></a>
  Não use espaço depois de operadores que não são palavras; ou ao redor do
  operador range.
  <sup>[[link](#sem-espacos)]</sup>

  ```elixir
  0 - 1 == -1
  ^pinned = alguma_func()
  5 in 1..10
  ```

* <a name="espacamento-def"></a>
  Use linhas em branco entre `def`s para quebrar uma função em seus parágrafos
  lógicos.
  <sup>[[link](#espacamento-def)]</sup>

  ```elixir
  def alguma_func(algum_dado) do
    dado_alterado = Module.function(algum_dado)
  end

  def alguma_func do
    resultado
  end

  def alguma_outra_func do
    outro_resultado
  end

  def uma_funcao_mais_longa do
    um
    dois

    três
    quatro
  end
  ```

* <a name="defs-de-uma-linha"></a>
  ...mas coloque junto `def`s de uma linha só que correspondam à mesma função.
  <sup>[[link](#defs-de-uma-linha)]</sup>

  ```elixir
  def alguma_funcao(nil), do: {:err, "Nenhum Valor"}
  def alguma_funcao([]), do: :ok
  def alguma_funcao([primeiro | resto]) do
    alguma_funcao(resto)
  end
  ```

* <a name="dos-longos"></a>
  Se você usar a sintaxe `do:` com funções, e o corpo da função tiver uma linha longa, coloque o `do:` em uma nova linha com mais um nível de indentação do que a linha anterior.
  function body is long, put the `do:` on a new line indented one level more
  than the previous line.
  <sup>[[link](#dos-longos)]</sup>

  ```elixir
  def alguma_funcao(args),
    do: Enum.map(args, fn(arg) -> arg <> " está em uma linha muito longa!" end)
  ```

  Quando você usar a convenção acima e tiver mais que uma cláusula
 usando a sintaxe `do:`, coloque o `do:` em uma nova linha para cada cláusula:

  ```elixir
  # não preferido
  def alguma_funcao([]), do: :vazio
  def alguma_funcao(_),
    do: :linha_muito_longa_aqui

  # preferido
  def alguma_funcao([]),
    do: :vazio
  def alguma_funcao(_),
    do: :linha_muito_longa_aqui
  ```

* <a name="multiplos-defs-funcao"></a>
  Se você tiver mais que um `def` multilinha, não use `def`s de uma linha.
  <sup>[[link](#multiplos-defs-funcao)]</sup>

  ```elixir
  def alguma_funcao(nil) do
    {:err, "Nenhum Valor"}
  end

  def alguma_funcao([]) do
    :ok
  end

  def alguma_funcao([primeiro | resto]) do
    alguma_funcao(resto)
  end

  def alguma_funcao([primeiro | resto], opts) do
    alguma_funcao(resto, opts)
  end
  ```

* <a name="operador-pipe"></a>
  Use o operador pipe (`|>`) para encadear funções.
  <sup>[[link](#operador-pipe)]</sup>

  ```elixir
  # não preferido
  String.strip(String.downcase(alguma_string))

  # preferido
  alguma_string |> String.downcase |> String.strip

  # Pipelines multilinha não recebem indentação extra
  alguma_string
  |> String.downcase
  |> String.strip

  # Pipelines multilinha do lado direito de um pattern match
  # devem ser indentados em uma nova linha
  string_sanitizada =
    alguma_string
    |> String.downcase
    |> String.strip
  ```

  Mesmo este sendo o método preferido, lembre-se que copiar e colar
  pipelines multilinha no IEx pode resultar em erro de sintaxe, já que o
  IEx vai avaliar a primeira linha sem perceber que a próxima linha tem um pipeline.

* <a name="evite-pipelines-solitarios"></a>
  Evite usar o operador pipe uma vez só.
  <sup>[[link](#evite-pipelines-solitarios)]</sup>

  ```elixir
  # não preferido
  alguma_string |> String.downcase

  # preferido
  String.downcase(alguma_string)
  ```

* <a name="variaveis-sozinhas"></a>
  Use variáveis _sozinhas_ na primeira parte de uma cadeia de funções.
  <sup>[[link](#variaveis-sozinhas)]</sup>

  ```elixir
  # PIOR JEITO!
  # Isto é interpretado como String.strip("não" |> String.downcase).
  String.strip "não" |> String.downcase

  # não preferido
  String.strip(alguma_string) |> String.downcase |> String.codepoints

  # preferido
  alguma_string |> String.strip |> String.downcase |> String.codepoints
  ```

* <a name="declaracao-list-multilinha"></a>
  Ao declarar uma linha que abrange múltiplas linhas, comece a lista em uma nova linha,
  e indente os elementos para mantê-los alinhados.
  <sup>[[link](#declaracao-list-multilinha)]</sup>

  ```elixir
  # não preferido - sem indentação
  lista = [:primeiro_item, :segundo_item, :proximo_item,
  :ultimo_item]

  # better, but não preferido - with indentation
  lista = [:primeiro_item, :segundo_item, :proximo_item,
          :ultimo_item]

  # preferido - lista começa em uma linha própria
  # bom para listas menores
  lista =
    [:primeiro_item, :segundo_item, :proximo_item,
     :ultimo_item]

  # também preferido - com cada elemento em uma linha
  # bom para listas longas, listas com elementos longos ou listas com comentários
  lista = [
    :primeiro_item,
    :segundo_item,
    :proximo_item,
    # comentário
    :muitos_itens,
    :ultimo_item
  ]
  ```

* <a name="espaco-branco-direita"></a>
  Evite espaço em branco no fim das linhas
  <sup>[[link](#espaco-branco-direita)]</sup>

* <a name="eof-nova-linha"></a>
  Termine todos os arquivos com uma nova linha.
  <sup>[[link](#eof-nova-linha)]</sup>

### Sintaxe

* <a name="parentheses"></a>
  Use parentheses when a `def` has arguments, and omit them when it doesn't.
  <sup>[[link](#parentheses)]</sup>

  ```elixir
  # não preferido
  def alguma_funcao arg1, arg2 do
    # body omitted
  end

  def alguma_funcao() do
    # body omitted
  end

  # preferido
  def alguma_funcao(arg1, arg2) do
    # body omitted
  end

  def alguma_funcao do
    # body omitted
  end
  ```

* <a name="add-blank-line-after-multiline-assignment"></a>
  Add a blank line after a multiline assignment as a
  visual cue that the assignment is 'over'.
  <sup>[[link](#add-blank-line-after-multiline-assignment)]</sup>

  ```elixir
  # não preferido
  alguma_string =
    "Hello"
    |> String.downcase
    |> String.strip
  outra_string <> alguma_string

  # preferido
  alguma_string =
    "Hello"
    |> String.downcase
    |> String.strip

  outra_string <> alguma_string
  ```

  ```elixir
  # also não preferido
  something =
    if x == 2 do
      "Hi"
    else
      "Bye"
    end
  something |> String.downcase

  # preferido
  something =
    if x == 2 do
      "Hi"
    else
      "Bye"
    end

  something |> String.downcase
  ```

* <a name="do-with-multi-line-if-unless"></a>
  Never use `do:` for multi-line `if/unless`.
  <sup>[[link](#do-with-multi-line-if-unless)]</sup>

  ```elixir
  # não preferido
  if some_condition, do:
    # a line of code
    # another line of code
    # note no end in this block

  # preferido
  if some_condition do
    # some
    # lines
    # of code
  end
  ```

* <a name="do-with-single-line-if-unless"></a>
  Use `do:` for single line `if/unless` statements.
  <sup>[[link](#do-with-single-line-if-unless)]</sup>

  ```elixir
  # preferido
  if some_condition, do: # some_stuff
  ```

* <a name="unless-with-else"></a>
  Never use `unless` with `else`.
  Rewrite these with the positive case first.
  <sup>[[link](#unless-with-else)]</sup>

  ```elixir
  # não preferido
  unless success? do
    IO.puts 'failure'
  else
    IO.puts 'success'
  end

  # preferido
  if success? do
    IO.puts 'success'
  else
    IO.puts 'failure'
  end
  ```

* <a name="true-as-last-condition"></a>
  Use `true` as the last condition of the `cond` special form when you need a
  clause that always matches.
  <sup>[[link](#true-as-last-condition)]</sup>

  ```elixir
  # não preferido
  cond do
    1 + 2 == 5 ->
      "Nope"
    1 + 3 == 5 ->
      "Uh, uh"
    :else ->
      "OK"
  end

  # preferido
  cond do
    1 + 2 == 5 ->
      "Nope"
    1 + 3 == 5 ->
      "Uh, uh"
    true ->
      "OK"
  end
  ```

* <a name="function-names-with-parentheses"></a>
  Never put a space between a function name and the opening parenthesis.
  <sup>[[link](#function-names-with-parentheses)]</sup>

  ```elixir
  # não preferido
  f (3 + 2) + 1

  # preferido
  f(3 + 2) + 1
  ```

* <a name="function-calls-and-parentheses"></a>
  Use parentheses in function calls, especially inside a pipeline.
  <sup>[[link](#function-calls-and-parentheses)]</sup>

  ```elixir
  # não preferido
  f 3

  # preferido
  f(3)

  # não preferido and parses as rem(2, (3 |> g)), which is not what you want.
  2 |> rem 3 |> g

  # preferido
  2 |> rem(3) |> g
  ```

* <a name="macro-calls-and-parentheses"></a>
  Omit parentheses in macro calls when a do block is passed.
  <sup>[[link](#macro-calls-and-parentheses)]</sup>

  ```elixir
  # não preferido
  quote(do
    foo
  end)

  # preferido
  quote do
    foo
  end
  ```

* <a name="parentheses-and-function-expressions"></a>
  Optionally omit parentheses in function calls (outside a pipeline) when the
  last argument is a function expression.
  <sup>[[link](#parentheses-and-function-expressions)]</sup>

  ```elixir
  # preferido
  Enum.reduce(1..10, 0, fn x, acc ->
    x + acc
  end)

  # also preferido
  Enum.reduce 1..10, 0, fn x, acc ->
    x + acc
  end
  ```

* <a name="parentheses-and-functions-with-zero-arity"></a>
  Use parentheses for calls to functions with zero arity, so they can be
  distinguished from variables.
  Starting in Elixir 1.4, the compiler will warn you about
  locations where this ambiguity exists.
  <sup>[[link](#parentheses-and-functions-with-zero-arity)]</sup>

  ```elixir
  defp do_stuff, do: ...

  # não preferido
  def my_func do
    do_stuff # is this a variable or a function call?
  end

  # preferido
  def my_func do
    do_stuff() # this is clearly a function call
  end
  ```

* <a name="keyword-list-syntax"></a>
  Always use the special syntax for keyword lists.
  <sup>[[link](#keyword-list-syntax)]</sup>

  ```elixir
  # não preferido
  some_value = [{:a, "baz"}, {:b, "qux"}]

  # preferido
  some_value = [a: "baz", b: "qux"]
  ```

* <a name="keyword-list-brackets"></a>
  Omit square brackets from keyword lists whenever they are optional.
  <sup>[[link](#keyword-list-brackets)]</sup>

  ```elixir
  # não preferido
  alguma_funcao(foo, bar, [a: "baz", b: "qux"])

  # preferido
  alguma_funcao(foo, bar, a: "baz", b: "qux")
  ```

* <a name="with-clauses"></a>
  Indent and align successive `with` clauses.
  Put the `do:` argument on a new line, indented normally.
  <sup>[[link](#with-clauses)]</sup>

  ```elixir
  with {:ok, foo} <- fetch(opts, :foo),
       {:ok, bar} <- fetch(opts, :bar),
    do: {:ok, foo, bar}
  ```

* <a name="with-else"></a>
  If the `with` expression has a `do` block with more than one line, or has an
  `else` option, use multiline syntax.
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

### Nomenclatura

* <a name="snake-case"></a>
  Use `snake_case` para atoms, funções e variáveis.
  <sup>[[link](#snake-case)]</sup>

  ```elixir
  # não preferido
  :"some atom"
  :SomeAtom
  :someAtom

  algumaVar = 5

  def algumaFuncao do
    ...
  end

  def AlgumaFuncao do
    ...
  end

  # preferido
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
  # não preferido
  defmodule Algummodulo do
    ...
  end

  defmodule Algum_Modulo do
    ...
  end

  defmodule AlgumXml do
    ...
  end

  # preferido
  defmodule AlgumModulo do
    ...
  end

  defmodule AlgumXML do
    ...
  end
  ```

* <a name="nome-de-macro-de-predicado-com-guards"></a>
  Os nomes das macros predicadas (funções de geração de tempo de compilação que retornam um
  valor booleano) _que podem ser usadas dentro de guards_ devem ser prefixados com `is_`.
  Para uma lista de expressões permitidas, veja nos documentos [Guard][Guard Expressions].
  <sup>[[link](#nome-de-macro-de-predicado-com-guards)]</sup>

  ```elixir
  defmacro is_cool(var) do
    quote do: unquote(var) == "cool"
  end
  ```

* <a name="nome-de-macro-de-predicado-sem-guards"></a>
  Os nomes de funções predicadas _que não podem ser usadas em guards_ devem
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
  def sum(list), do: do_sum(list, 0)

  # funções privadas
  defp do_sum([], total), do: total
  defp do_sum([head | tail], total), do: do_sum(tail, head + total)
  ```

### Comentários

* <a name="expressive-code"></a>
  Write expressive code and try to convey your program's intention through
  control-flow, structure and naming.
  <sup>[[link](#expressive-code)]</sup>

* <a name="comment-leading-spaces"></a>
  Use one space between the leading `#` character of the comment and the text of
  the comment.
  <sup>[[link](#comment-leading-spaces)]</sup>

  ```elixir
  String.first(alguma_string) # não preferido
  String.first(alguma_string) # preferido
  ```

* <a name="comment-grammar"></a>
  Comments longer than a word are capitalized, and sentences use punctuation.
  Use [one space][Sentence Spacing] after periods.
  <sup>[[link](#comment-grammar)]</sup>

  ```elixir
  # não preferido
  # these lowercase comments are missing punctuation

  # preferido
  # Capitalization example
  # Use punctuation for complete sentences.
  ```

#### Anotaçóes em Comentários

* <a name="annotations"></a>
  Annotations should usually be written on the line immediately above the
  relevant code.
  <sup>[[link](#annotations)]</sup>

* <a name="annotation-keyword"></a>
  The annotation keyword is uppercase, and is followed by a colon and a space,
  then a note describing the problem.
  <sup>[[link](#annotation-keyword)]</sup>

  ```elixir
  # TODO: Deprecate in v1.5.
  def alguma_funcao(arg), do: {:ok, arg}
  ```

* <a name="exceptions-to-annotations"></a>
  In cases where the problem is so obvious that any documentation would be
  redundant, annotations may be left at the end of the offending line with no
  note.
  This usage should be the exception and not the rule.
  <sup>[[link](#exceptions-to-annotations)]</sup>

  ```elixir
  start_task()
  Process.sleep(5000) # FIXME
  ```

* <a name="todo-notes"></a>
  Use `TODO` to note missing features or functionality that should be added at a
  later date.
  <sup>[[link](#todo-notes)]</sup>

* <a name="fixme-notes"></a>
  Use `FIXME` to note broken code that needs to be fixed.
  <sup>[[link](#fixme-notes)]</sup>

* <a name="optimize-notes"></a>
  Use `OPTIMIZE` to note slow or inefficient code that may cause performance
  problems.
  <sup>[[link](#optimize-notes)]</sup>

* <a name="hack-notes"></a>
  Use `HACK` to note code smells where questionable coding practices were used
  and should be refactored away.
  <sup>[[link](#hack-notes)]</sup>

* <a name="review-notes"></a>
  Use `REVIEW` to note anything that should be looked at to confirm it is
  working as intended.
  For example: `REVIEW: Are we sure this is how the client does X currently?`
  <sup>[[link](#review-notes)]</sup>

* <a name="custom-keywords"></a>
  Use other custom annotation keywords if it feels appropriate, but be sure to
  document them in your project's `README` or similar.
  <sup>[[link](#custom-keywords)]</sup>

### Módulos

* <a name="one-module-per-file"></a>
  Use one module per file unless the module is only used internally by another
  module (such as a test).
  <sup>[[link](#one-module-per-file)]</sup>

* <a name="underscored-filenames"></a>
  Use `snake_case` file names for `CamelCase` module names.
  <sup>[[link](#underscored-filenames)]</sup>

  ```elixir
  # file is called some_module.ex

  defmodule SomeModule do
  end
  ```

* <a name="module-name-nesting"></a>
  Represent each level of nesting within a module name as a directory.
  <sup>[[link](#module-name-nesting)]</sup>

  ```elixir
  # file is called parser/core/xml_parser.ex

  defmodule Parser.Core.XMLParser do
  end
  ```

* <a name="defmodule-spacing"></a>
  Don't put a blank line after `defmodule`.
  <sup>[[link](#defmodule-spacing)]</sup>

* <a name="module-block-spacing"></a>
  Put a blank line after module-level code blocks.
  <sup>[[link](#module-block-spacing)]</sup>

* <a name="module-attribute-ordering"></a>
  List module attributes and directives in the following order:
  <sup>[[link](#module-attribute-ordering)]</sup>

  1. `@moduledoc`
  1. `@behaviour`
  1. `use`
  1. `import`
  1. `alias`
  1. `require`
  1. `defstruct`
  1. `@type`
  1. `@module_attribute`
  1. `@callback`
  1. `@macrocallback`
  1. `@optional_callbacks`

  Add a blank line between each grouping, and sort the terms (like module names)
  alphabetically.
  Here's an overall example of how you should order things in your modules:

  ```elixir
  defmodule MyModule do
    @moduledoc """
    An example module
    """

    @behaviour MyBehaviour

    use GenServer

    import Something
    import SomethingElse

    alias My.Long.Module.Name
    alias My.Other.Module.Example

    require Integer

    defstruct name: nil, params: []

    @type params :: [{binary, binary}]

    @module_attribute :foo
    @other_attribute 100

    @callback alguma_funcao(term) :: :ok | {:error, term}

    @macrocallback macro_name(term) :: Macro.t

    @optional_callbacks macro_name: 1

    ...
  end
  ```

* <a name="module-pseudo-variable"></a>
  Use the `__MODULE__` pseudo variable when a module refers to itself. This
  avoids having to update any self-references when the module name changes.
  <sup>[[link](#module-pseudo-variable)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    defstruct [:name]

    def name(%__MODULE__{name: name}), do: name
  end
  ```

* <a name="alias-self-referencing-modules"></a>
  If you want a prettier name for a module self-reference, set up an alias.
  <sup>[[link](#alias-self-referencing-modules)]</sup>

  ```elixir
  defmodule SomeProject.SomeModule do
    alias __MODULE__, as: SomeModule

    defstruct [:name]

    def name(%SomeModule{name: name}), do: name
  end
  ```

* <a name="repetitive-module-names"></a>
  Avoid repeating fragments in module names and namespaces.
  This improves overall readability and
  eliminates [ambiguous aliases][Conflicting Aliases].
  <sup>[[link](#repetitive-module-names)]</sup>

  ```elixir
  # não preferido
  defmodule Todo.Todo do
    ...
  end

  # preferido
  defmodule Todo.Item do
    ...
  end
  ```

### Documentação

Documentation in Elixir (when read either in `iex` with `h` or generated with
[ExDoc]) uses the [Module Attributes] `@moduledoc` and `@doc`.

* <a name="moduledocs"></a>
  Always include a `@moduledoc` attribute in the line right after `defmodule` in
  your module.
  <sup>[[link](#moduledocs)]</sup>

  ```elixir
  # não preferido

  defmodule SomeModule do

    @moduledoc """
    About the module
    """
    ...
  end

  defmodule AnotherModule do
    use SomeModule
    @moduledoc """
    About the module
    """
    ...
  end

  # preferido

  defmodule SomeModule do
    @moduledoc """
    About the module
    """
    ...
  end
  ```

* <a name="moduledoc-false"></a>
  Use `@moduledoc false` if you do not intend on documenting the module.
  <sup>[[link](#moduledoc-false)]</sup>

  ```elixir
  defmodule SomeModule do
    @moduledoc false
    ...
  end
  ```

* <a name="moduledoc-spacing"></a>
  Separate code after the `@moduledoc` with a blank line.
  <sup>[[link](#moduledoc-spacing)]</sup>

  ```elixir
  # não preferido

  defmodule SomeModule do
    @moduledoc """
    About the module
    """
    use AnotherModule
  end

  # preferido
  defmodule SomeModule do
    @moduledoc """
    About the module
    """

    use AnotherModule
  end
  ```

* <a name="heredocs"></a>
  Use heredocs with markdown for documentation.
  <sup>[[link](#heredocs)]</sup>

  ```elixir
  # não preferido

  defmodule SomeModule do
    @moduledoc "About the module"
  end

  defmodule SomeModule do
    @moduledoc """
    About the module

    Examples:
    iex> SomeModule.alguma_funcao
    :result
    """
  end

  # preferido
  defmodule SomeModule do
    @moduledoc """
    About the module

    ## Examples

        iex> SomeModule.alguma_funcao
        :result
    """
  end
  ```

### Typespecs

Typespecs are notation for declaring types and specifications, for
documentation or for the static analysis tool Dialyzer.

Custom types should be defined at the top of the module with the other
directives (see [Modules](#modulos)).

* <a name="typedocs"></a>
  Place `@typedoc` and `@type` definitions together, and separate each
  pair with a blank line.
  <sup>[[link](#typedocs)]</sup>

  ```elixir
  defmodule SomeModule do
    @moduledoc false

    @typedoc "The name"
    @type name :: atom

    @typedoc "The result"
    @type result :: {:ok, term} | {:error, term}

    ...
  end
  ```

* <a name="union-types"></a>
  If a union type is too long to fit on a single line, add a newline
  and indent with spaces to keep the types aligned.
  <sup>[[link](#union-types)]</sup>

  ```elixir
  # não preferido - no indentation
  @type long_union_type :: some_type | another_type | some_other_type |
  a_final_type

  # preferido
  @type long_union_type :: some_type | another_type | some_other_type |
                           a_final_type

  # also preferido - one type per line
  @type long_union_type :: some_type |
                           another_type |
                           some_other_type |
                           a_final_type
  ```

* <a name="naming-main-types"></a>
  Name the main type for a module `t`, for example: the type specification for a
  struct.
  <sup>[[link](#naming-main-types)]</sup>

  ```elixir
  defstruct name: nil, params: []

  @type t :: %__MODULE__{
    name: String.t | nil,
    params: Keyword.t
  }
  ```

* <a name="spec-spacing"></a>
  Place specifications right before the function definition,
  without separating them by a blank line.
  <sup>[[link](#spec-spacing)]</sup>

  ```elixir
  @spec alguma_funcao(term) :: result
  def alguma_funcao(some_data) do
    {:ok, some_data}
  end
  ```

### Structs

* <a name="nil-struct-field-defaults"></a>
  Use a list of atoms for struct fields that default to `nil`, followed by the
  other keywords.
  <sup>[[link](#nil-struct-field-defaults)]</sup>

  ```elixir
  # não preferido
  defstruct name: nil, params: nil, active: true

  # preferido
  defstruct [:name, :params, active: true]
  ```

* <a name="struct-def-brackets"></a>
  Omit square brackets when the argument of a `defstruct` is a keyword list.
  <sup>[[link](#struct-def-brackets)]</sup>

  ```elixir
  # não preferido
  defstruct [params: [], active: true]

  # preferido
  defstruct params: [], active: true

  # required - brackets are not optional, with at least one atom in the list
  defstruct [:name, params: [], active: true]
  ```

* <a name="additional-struct-def-lines"></a>
  Indent additional lines of the struct definition, keeping the first keys
  aligned.
  <sup>[[link](#additional-struct-def-lines)]</sup>

  ```elixir
  defstruct foo: "test", bar: true, baz: false,
            qux: false, quux: 1
  ```

### Exceções

* <a name="exception-names"></a>
  Make exception names end with a trailing `Error`.
  <sup>[[link](#exception-names)]</sup>

  ```elixir
  # não preferido
  defmodule BadHTTPCode do
    defexception [:message]
  end

  defmodule BadHTTPCodeException do
    defexception [:message]
  end

  # preferido
  defmodule BadHTTPCodeError do
    defexception [:message]
  end
  ```

* <a name="lowercase-error-messages"></a>
  Use lowercase error messages when raising exceptions, with no trailing
  punctuation.
  <sup>[[link](#lowercase-error-messages)]</sup>

  ```elixir
  # não preferido
  raise ArgumentError, "This is not valid."

  # preferido
  raise ArgumentError, "this is not valid"
  ```

### Coleções

_No guidelines for collections have been added yet._

### Strings

* <a name="strings-matching-with-concatenator"></a>
  Match strings using the string concatenator rather than binary patterns:
  <sup>[[link](#strings-matching-with-concatenator)]</sup>

  ```elixir
  # não preferido
  <<"my"::utf8, _resto>> = "my string"

  # preferido
  "my" <> _resto = "my string"
  ```

### Expressões Regulares

_No guidelines for regular expressions have been added yet._

### Metaprogramação

* <a name="avoid-metaprogramming"></a>
  Avoid needless metaprogramming.
  <sup>[[link](#avoid-metaprogramming)]</sup>

### Testes

* <a name="testing-assert-order"></a>
  When writing [ExUnit] assertions, be consistent with the order of the expected
  and actual values under testing.
  Prefer placing the expected result on the right, unless the assertion is a
  pattern match.
  <sup>[[link](#testing-assert-order)]</sup>

  ```elixir
  # preferido - expected result on the right
  assert actual_function(1) == true
  assert actual_function(2) == false

  # não preferido - inconsistent order
  assert actual_function(1) == true
  assert false == actual_function(2)

  # required - the assertion is a pattern match
  assert {:ok, expected} = actual_function(3)
  ```

### Guias de Estilo Alternativos

* [Aleksei Magusev's Elixir Style Guide](https://github.com/lexmag/elixir-style-guide#readme)
  — An opinionated Elixir style guide stemming from the coding style practiced
  in the Elixir core libraries.
  Developed by [Aleksei Magusev](https://github.com/lexmag) and
  [Andrea Leopardi](https://github.com/whatyouhide), members of Elixir core team.
  While the Elixir project doesn't adhere to any specific style guide,
  this is the closest available guide to its conventions.

* [Credo's Elixir Style Guide](https://github.com/rrrene/elixir-style-guide#readme)
  — Style Guide for the Elixir language, implemented by
  [Credo](http://credo-ci.org) static code analysis tool.

### Ferramentas

Refer to [Awesome Elixir][Code Analysis] for libraries and tools that can help
with code analysis and style linting.

## Participando

### Contribuindo

It's our hope that this will become a central hub for community discussion on
best practices in Elixir.
Feel free to open tickets or send pull requests with improvements.
Thanks in advance for your help!

Check the [contributing guidelines][Contribuindo]
and [code of conduct][Code Of Conduct] for more information.

### Espalhe

A community style guide is meaningless without the community's support. Please
tweet, [star][Stargazers], and let any Elixir programmer know
about [this guide][Elixir Style Guide] so they can contribute.

## Cópias

### License

![Creative Commons Licença](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a
[Creative Commons Attribution 3.0 Unported License][Licença]

### Atribuição

The structure of this guide, bits of example code, and many of the initial
points made in this document were borrowed from the [Ruby community style guide].
A lot of things were applicable to Elixir and allowed us to get _some_ document
out quicker to start the conversation.

Here's the [list of people who have kindly contributed][Contributors] to this
project.

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
[Inglês]: https://github.com/christopheradams/elixir_style_guide/README.md
[Japonês]: https://github.com/kenichirow/elixir_style_guide/blob/master/README-jaJP.md
[Licença]: http://creativecommons.org/licenses/by/3.0/deed.en_US
[Module Attributes]: http://elixir-lang.org/getting-started/module-attributes.html#as-annotations
[Português]: https://github.com/gusaiani/elixir_style_guide/blob/master/README_ptBR.md
[Ruby community style guide]: https://github.com/bbatsov/ruby-style-guide
[Sentence Spacing]: http://en.wikipedia.org/wiki/Sentence_spacing
[Stargazers]: https://github.com/christopheradams/elixir_style_guide/stargazers
