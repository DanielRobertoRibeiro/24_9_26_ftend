# Principais Hooks do React

## 1. O que é um Hook?

Um **Hook** é uma função especial do React que permite usar recursos do React dentro de componentes funcionais.

Antes dos Hooks, muitos recursos importantes, como estado e ciclo de vida, eram usados principalmente em componentes de classe.

Com Hooks, podemos fazer coisas como:

- Guardar valores na memória do componente.
- Executar ações quando o componente carrega.
- Compartilhar lógica entre componentes.
- Acessar referências do DOM.
- Melhorar performance em alguns cenários.

Exemplo simples:

```jsx
import { useState } from "react";

function Contador() {
  const [numero, setNumero] = useState(0);

  return (
    <div>
      <p>Valor: {numero}</p>
      <button onClick={() => setNumero(numero + 1)}>
        Somar
      </button>
    </div>
  );
}

export default Contador;
```

Nesse exemplo, `useState` é um Hook.

Ele permite criar uma variável de estado chamada `numero`.

---

## 2. Por que os Hooks foram criados?

Os Hooks foram criados para deixar os componentes funcionais mais poderosos.

Com eles, não precisamos transformar um componente em classe só para usar estado, efeitos ou outras funcionalidades.

Eles ajudam a:

- Reduzir código repetido.
- Organizar melhor a lógica.
- Reaproveitar comportamento entre componentes.
- Tornar o código mais direto.

Antes:

```jsx
class Contador extends React.Component {
  state = {
    numero: 0
  };

  render() {
    return <p>{this.state.numero}</p>;
  }
}
```

Com Hook:

```jsx
function Contador() {
  const [numero, setNumero] = useState(0);

  return <p>{numero}</p>;
}
```

Menos cerimônia. React sem gravata.

---

## 3. Regras dos Hooks

Hooks possuem algumas regras importantes.

### Regras principais

- Hooks devem ser chamados no topo do componente.
- Hooks não devem ser chamados dentro de `if`, `for`, `while` ou funções internas.
- Hooks só devem ser usados em componentes React ou em Hooks personalizados.
- Hooks personalizados devem começar com a palavra `use`.

Exemplo errado:

```jsx
function App() {
  if (true) {
    const [nome, setNome] = useState("");
  }

  return <p>Olá</p>;
}
```

Exemplo correto:

```jsx
function App() {
  const [nome, setNome] = useState("");

  if (nome === "") {
    return <p>Digite seu nome</p>;
  }

  return <p>Olá, {nome}</p>;
}
```

---

## 4. Como criar um Hook personalizado

Um Hook personalizado é uma função criada por nós para reaproveitar uma lógica.

Ele deve começar com `use`.

### Exemplo: Hook para controlar campo de formulário

```jsx
import { useState } from "react";

function useCampo(valorInicial) {
  const [valor, setValor] = useState(valorInicial);

  function aoMudar(evento) {
    setValor(evento.target.value);
  }

  return {
    valor,
    aoMudar
  };
}

export default useCampo;
```

Agora podemos usar esse Hook em um componente:

```jsx
import useCampo from "./useCampo";

function Formulario() {
  const nome = useCampo("");
  const email = useCampo("");

  return (
    <form>
      <input
        type="text"
        placeholder="Nome"
        value={nome.valor}
        onChange={nome.aoMudar}
      />

      <input
        type="email"
        placeholder="E-mail"
        value={email.valor}
        onChange={email.aoMudar}
      />

      <p>Nome: {nome.valor}</p>
      <p>Email: {email.valor}</p>
    </form>
  );
}

export default Formulario;
```

Aqui, a lógica do campo foi separada do componente.

Se outro formulário precisar da mesma lógica, é só reutilizar o Hook.

---

## 5. Principais Hooks do React

### 5.1 `useState`

O `useState` é usado para criar estados dentro de um componente.

Estado é uma informação que pode mudar e fazer o componente renderizar novamente.

### Quando usar

- Contadores.
- Campos de formulário.
- Controle de modal aberto ou fechado.
- Controle de tema claro ou escuro.
- Listas que mudam na tela.

### Exemplo prático

```jsx
import { useState } from "react";

function Curtidas() {
  const [curtidas, setCurtidas] = useState(0);

  return (
    <div>
      <p>Curtidas: {curtidas}</p>
      <button onClick={() => setCurtidas(curtidas + 1)}>
        Curtir
      </button>
    </div>
  );
}

export default Curtidas;
```

### Explicação

```jsx
const [curtidas, setCurtidas] = useState(0);
```

- `curtidas`: valor atual.
- `setCurtidas`: função que altera o valor.
- `0`: valor inicial.

---

### 5.2 `useEffect`

O `useEffect` é usado para executar efeitos colaterais.

Um efeito colateral é uma ação que acontece fora da renderização normal do componente.

Exemplos:

- Buscar dados em uma API.
- Alterar o título da página.
- Usar `setInterval`.
- Monitorar mudanças em uma variável.
- Salvar dados no `localStorage`.

### Exemplo: alterar o título da página

```jsx
import { useEffect, useState } from "react";

function TituloPagina() {
  const [nome, setNome] = useState("");

  useEffect(() => {
    document.title = nome;
  }, [nome]);

  return (
    <input
      type="text"
      placeholder="Digite seu nome"
      value={nome}
      onChange={(evento) => setNome(evento.target.value)}
    />
  );
}

export default TituloPagina;
```

### Explicação

```jsx
useEffect(() => {
  document.title = nome;
}, [nome]);
```

Esse efeito será executado sempre que `nome` mudar.

### Exemplo: executar apenas uma vez

```jsx
import { useEffect } from "react";

function BoasVindas() {
  useEffect(() => {
    console.log("Componente carregou");
  }, []);

  return <h1>Bem-vindo!</h1>;
}

export default BoasVindas;
```

O array vazio `[]` indica que o efeito será executado apenas quando o componente for carregado.

---

## 5.3 `useRef`

O `useRef` cria uma referência que pode guardar um valor sem causar nova renderização.

Também é muito usado para acessar elementos do DOM.

### Quando usar

- Focar automaticamente em um input.
- Guardar valores entre renderizações.
- Acessar elementos HTML diretamente.
- Controlar timers.

### Exemplo: focar em um input

```jsx
import { useRef } from "react";

function FocoInput() {
  const inputRef = useRef(null);

  function focarInput() {
    inputRef.current.focus();
  }

  return (
    <div>
      <input ref={inputRef} placeholder="Digite algo" />
      <button onClick={focarInput}>Focar no input</button>
    </div>
  );
}

export default FocoInput;
```

### Explicação

```jsx
const inputRef = useRef(null);
```

O React cria uma referência.

Depois, essa referência é ligada ao input:

```jsx
<input ref={inputRef} />
```

E acessamos o input com:

```jsx
inputRef.current
```

---

### 5.4 `useContext`

O `useContext` permite acessar dados globais sem precisar passar props manualmente de componente em componente.

Isso evita o famoso "prop drilling".

Prop drilling é quando uma informação precisa passar por vários componentes só para chegar em um componente filho.

### Exemplo prático: tema claro e escuro

```jsx
import { createContext, useContext } from "react";

const TemaContexto = createContext();

function Botao() {
  const tema = useContext(TemaContexto);

  return (
    <button>
      Tema atual: {tema}
    </button>
  );
}

function App() {
  return (
    <TemaContexto.Provider value="escuro">
      <Botao />
    </TemaContexto.Provider>
  );
}

export default App;
```

### Explicação

Criamos um contexto:

```jsx
const TemaContexto = createContext();
```

Fornecemos um valor:

```jsx
<TemaContexto.Provider value="escuro">
```

Acessamos esse valor com:

```jsx
const tema = useContext(TemaContexto);
```

---

### 5.5 `useReducer`

O `useReducer` é usado para controlar estados mais organizados, principalmente quando existem muitas ações possíveis.

Ele lembra bastante o funcionamento de um `switch`.

### Quando usar

- Carrinho de compras.
- Formulários maiores.
- Estados com várias ações.
- Quando o `useState` começa a ficar bagunçado.

### Exemplo prático: contador com reducer

```jsx
import { useReducer } from "react";

function reducer(estado, acao) {
  switch (acao.tipo) {
    case "somar":
      return { contador: estado.contador + 1 };

    case "subtrair":
      return { contador: estado.contador - 1 };

    case "zerar":
      return { contador: 0 };

    default:
      return estado;
  }
}

function ContadorReducer() {
  const [estado, dispatch] = useReducer(reducer, { contador: 0 });

  return (
    <div>
      <p>Valor: {estado.contador}</p>

      <button onClick={() => dispatch({ tipo: "somar" })}>
        Somar
      </button>

      <button onClick={() => dispatch({ tipo: "subtrair" })}>
        Subtrair
      </button>

      <button onClick={() => dispatch({ tipo: "zerar" })}>
        Zerar
      </button>
    </div>
  );
}

export default ContadorReducer;
```

### Explicação

```jsx
const [estado, dispatch] = useReducer(reducer, { contador: 0 });
```

- `estado`: valor atual.
- `dispatch`: função usada para enviar ações.
- `reducer`: função que decide como o estado será alterado.
- `{ contador: 0 }`: estado inicial.

---

### 5.6 `useMemo`

O `useMemo` memoriza o resultado de um cálculo.

Ele evita que cálculos sejam refeitos sem necessidade.

### Quando usar

- Cálculos pesados.
- Filtros em listas grandes.
- Ordenações.
- Valores derivados que não precisam ser recalculados sempre.

### Exemplo prático: filtrar produtos

```jsx
import { useMemo, useState } from "react";

function ListaProdutos() {
  const [busca, setBusca] = useState("");

  const produtos = [
    "Notebook",
    "Mouse",
    "Teclado",
    "Monitor",
    "Cadeira"
  ];

  const produtosFiltrados = useMemo(() => {
    return produtos.filter((produto) =>
      produto.toLowerCase().includes(busca.toLowerCase())
    );
  }, [busca]);

  return (
    <div>
      <input
        type="text"
        placeholder="Buscar produto"
        value={busca}
        onChange={(evento) => setBusca(evento.target.value)}
      />

      <ul>
        {produtosFiltrados.map((produto) => (
          <li key={produto}>{produto}</li>
        ))}
      </ul>
    </div>
  );
}

export default ListaProdutos;
```

### Explicação

```jsx
const produtosFiltrados = useMemo(() => {
  return produtos.filter(...);
}, [busca]);
```

O filtro só será recalculado quando `busca` mudar.

Atenção: não use `useMemo` em tudo. Às vezes o remédio pesa mais que a doença.

---

### 5.7 `useCallback`

O `useCallback` memoriza uma função.

Ele é útil quando uma função é passada para componentes filhos e queremos evitar recriações desnecessárias.

### Quando usar

- Componentes filhos otimizados com `React.memo`.
- Funções passadas como props.
- Cenários em que recriar funções pode causar renderizações extras.

### Exemplo prático

```jsx
import { useCallback, useState } from "react";

function Botao({ aoClicar }) {
  return <button onClick={aoClicar}>Clique</button>;
}

function App() {
  const [contador, setContador] = useState(0);

  const somar = useCallback(() => {
    setContador((valorAtual) => valorAtual + 1);
  }, []);

  return (
    <div>
      <p>Contador: {contador}</p>
      <Botao aoClicar={somar} />
    </div>
  );
}

export default App;
```

### Explicação

```jsx
const somar = useCallback(() => {
  setContador((valorAtual) => valorAtual + 1);
}, []);
```

A função `somar` será mantida entre renderizações.

---

### 5.8 `useId`

O `useId` gera IDs únicos e estáveis para elementos do componente.

É muito útil para acessibilidade, principalmente quando usamos `label` e `input`.

### Exemplo prático

```jsx
import { useId } from "react";

function CampoNome() {
  const idNome = useId();

  return (
    <div>
      <label htmlFor={idNome}>Nome</label>
      <input id={idNome} type="text" />
    </div>
  );
}

export default CampoNome;
```

### Explicação

```jsx
const idNome = useId();
```

O React gera um ID único para ligar o `label` ao `input`.

Isso melhora a acessibilidade e evita IDs repetidos.

---

### 5.9 `useTransition`

O `useTransition` ajuda a separar atualizações urgentes de atualizações menos urgentes.

Ele pode deixar a interface mais fluida quando uma atualização pesada acontece.

### Quando usar

- Filtros em listas grandes.
- Busca com muitos resultados.
- Telas que demoram para atualizar.
- Mudanças visuais que podem esperar um pouco.

### Exemplo prático

```jsx
import { useState, useTransition } from "react";

function BuscaLista() {
  const [texto, setTexto] = useState("");
  const [lista, setLista] = useState([]);
  const [pendente, startTransition] = useTransition();

  const itens = Array.from({ length: 5000 }, (_, index) => `Item ${index}`);

  function buscar(evento) {
    const valor = evento.target.value;
    setTexto(valor);

    startTransition(() => {
      const resultado = itens.filter((item) =>
        item.toLowerCase().includes(valor.toLowerCase())
      );

      setLista(resultado);
    });
  }

  return (
    <div>
      <input
        value={texto}
        onChange={buscar}
        placeholder="Buscar item"
      />

      {pendente && <p>Carregando...</p>}

      <ul>
        {lista.map((item) => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </div>
  );
}

export default BuscaLista;
```

### Explicação

```jsx
const [pendente, startTransition] = useTransition();
```

- `pendente`: indica se a atualização ainda está acontecendo.
- `startTransition`: marca uma atualização como menos urgente.

O input continua respondendo bem enquanto a lista é atualizada.

---

### 5.10 `useDeferredValue`

O `useDeferredValue` adia a atualização de um valor.

Ele é parecido com o `useTransition`, mas trabalha diretamente com um valor.

### Quando usar

- Busca em tempo real.
- Filtros pesados.
- Listas grandes.
- Componentes que demoram para renderizar.

### Exemplo prático

```jsx
import { useDeferredValue, useState } from "react";

function BuscaComAtraso() {
  const [busca, setBusca] = useState("");
  const buscaAdiada = useDeferredValue(busca);

  const produtos = [
    "Notebook",
    "Mouse",
    "Teclado",
    "Monitor",
    "Cadeira Gamer"
  ];

  const filtrados = produtos.filter((produto) =>
    produto.toLowerCase().includes(buscaAdiada.toLowerCase())
  );

  return (
    <div>
      <input
        value={busca}
        onChange={(evento) => setBusca(evento.target.value)}
        placeholder="Buscar"
      />

      <p>Buscando por: {buscaAdiada}</p>

      <ul>
        {filtrados.map((produto) => (
          <li key={produto}>{produto}</li>
        ))}
      </ul>
    </div>
  );
}

export default BuscaComAtraso;
```

### Explicação

```jsx
const buscaAdiada = useDeferredValue(busca);
```

O React pode atrasar a atualização de `buscaAdiada` para manter a interface mais responsiva.

---

## 6. Comparando os principais Hooks

| Hook | Para que serve | Exemplo de uso |
|---|---|---|
| `useState` | Criar estados simples | Contador, formulário |
| `useEffect` | Executar efeitos colaterais | API, título da página |
| `useRef` | Guardar referência sem renderizar | Focar input |
| `useContext` | Compartilhar dados globais | Tema, usuário logado |
| `useReducer` | Controlar estados mais complexos | Carrinho, formulário grande |
| `useMemo` | Memorizar resultado de cálculo | Filtro, ordenação |
| `useCallback` | Memorizar função | Função passada para filho |
| `useId` | Criar IDs únicos | Label e input |
| `useTransition` | Priorizar atualizações | Busca pesada |
| `useDeferredValue` | Adiar atualização de valor | Filtro em tempo real |

---

## 7. Exemplo prático integrando vários Hooks

### Mini projeto: filtro de alunos favoritos

```jsx
import { useMemo, useState } from "react";

function Alunos() {
  const [busca, setBusca] = useState("");
  const [favoritos, setFavoritos] = useState([]);

  const alunos = [
    "Ana",
    "Carlos",
    "Mariana",
    "João",
    "Beatriz",
    "Lucas"
  ];

  const alunosFiltrados = useMemo(() => {
    return alunos.filter((aluno) =>
      aluno.toLowerCase().includes(busca.toLowerCase())
    );
  }, [busca]);

  function alternarFavorito(aluno) {
    if (favoritos.includes(aluno)) {
      setFavoritos(favoritos.filter((item) => item !== aluno));
    } else {
      setFavoritos([...favoritos, aluno]);
    }
  }

  return (
    <div>
      <h1>Lista de alunos</h1>

      <input
        value={busca}
        onChange={(evento) => setBusca(evento.target.value)}
        placeholder="Buscar aluno"
      />

      <ul>
        {alunosFiltrados.map((aluno) => (
          <li key={aluno}>
            {aluno}

            <button onClick={() => alternarFavorito(aluno)}>
              {favoritos.includes(aluno) ? "Remover" : "Favoritar"}
            </button>
          </li>
        ))}
      </ul>

      <h2>Favoritos</h2>

      <ul>
        {favoritos.map((aluno) => (
          <li key={aluno}>{aluno}</li>
        ))}
      </ul>
    </div>
  );
}

export default Alunos;
```

### Hooks usados

- `useState` para controlar a busca.
- `useState` para controlar favoritos.
- `useMemo` para filtrar alunos.

---

## 8. Atividade prática para os alunos

### Proposta

Crie uma aplicação React chamada **Painel de Tarefas**.

A aplicação deve permitir:

- Cadastrar uma tarefa.
- Listar as tarefas cadastradas.
- Marcar uma tarefa como concluída.
- Remover uma tarefa.
- Filtrar tarefas por texto.
- Mostrar a quantidade total de tarefas.
- Mostrar a quantidade de tarefas concluídas.

### Hooks obrigatórios

- `useState`
- `useMemo`
- Pelo menos um Hook personalizado

### Sugestão de Hook personalizado

Crie um Hook chamado `useTarefas`.

Ele pode retornar:

```jsx
{
  tarefas,
  adicionarTarefa,
  removerTarefa,
  alternarConcluida
}
```

### Desafio extra

Salvar as tarefas no `localStorage` usando `useEffect`.

---

## 9. Exercício guiado: criando `useLocalStorage`

Este Hook salva e recupera dados do `localStorage`.

```jsx
import { useEffect, useState } from "react";

function useLocalStorage(chave, valorInicial) {
  const [valor, setValor] = useState(() => {
    const itemSalvo = localStorage.getItem(chave);

    if (itemSalvo) {
      return JSON.parse(itemSalvo);
    }

    return valorInicial;
  });

  useEffect(() => {
    localStorage.setItem(chave, JSON.stringify(valor));
  }, [chave, valor]);

  return [valor, setValor];
}

export default useLocalStorage;
```

### Usando o Hook

```jsx
import useLocalStorage from "./useLocalStorage";

function App() {
  const [nome, setNome] = useLocalStorage("nome", "");

  return (
    <div>
      <input
        value={nome}
        onChange={(evento) => setNome(evento.target.value)}
        placeholder="Digite seu nome"
      />

      <p>Nome salvo: {nome}</p>
    </div>
  );
}

export default App;
```

---

## 10. Erros comuns ao usar Hooks

### Chamar Hook dentro de condição

Errado:

```jsx
if (usuarioLogado) {
  const [nome, setNome] = useState("");
}
```

Certo:

```jsx
const [nome, setNome] = useState("");

if (!usuarioLogado) {
  return <p>Faça login</p>;
}
```

---

### Esquecer dependências no `useEffect`

Errado:

```jsx
useEffect(() => {
  console.log(nome);
}, []);
```

Certo:

```jsx
useEffect(() => {
  console.log(nome);
}, [nome]);
```

---

### Usar `useMemo` sem necessidade

Nem todo cálculo precisa ser memorizado.

Exemplo exagerado:

```jsx
const nomeMaiusculo = useMemo(() => {
  return nome.toUpperCase();
}, [nome]);
```

Para algo simples, isso basta:

```jsx
const nomeMaiusculo = nome.toUpperCase();
```

Nem todo parafuso precisa de furadeira industrial.

---

## 11. Roteiro sugerido para a aula

### Parte 1 — Introdução

Explique:

- O que é um Hook.
- Por que Hooks existem.
- Diferença entre componente funcional simples e componente com Hook.

### Parte 2 — Estado com `useState`

Faça um contador ao vivo.

Depois, transforme em um campo de formulário.

### Parte 3 — Efeitos com `useEffect`

Mostre:

- Alteração do título da página.
- Uso de `localStorage`.
- Busca simples em API, se quiser expandir.

### Parte 4 — Hooks para organização

Apresente:

- `useContext`
- `useReducer`
- Hooks personalizados

### Parte 5 — Hooks de performance

Apresente com cuidado:

- `useMemo`
- `useCallback`
- `useTransition`
- `useDeferredValue`

Deixe claro que esses Hooks não devem ser usados como enfeite de árvore de Natal.

### Parte 6 — Prática

Peça para os alunos criarem o **Painel de Tarefas**.

---

## 12. Resumo final

Hooks são funções que permitem usar recursos do React em componentes funcionais.

Os principais Hooks são:

- `useState`
- `useEffect`
- `useRef`
- `useContext`
- `useReducer`
- `useMemo`
- `useCallback`
- `useId`
- `useTransition`
- `useDeferredValue`

Hooks personalizados permitem reaproveitar lógica entre componentes.

A ideia principal é simples:

> Quando uma lógica se repete entre componentes, talvez ela mereça virar um Hook.

---

## 13. Perguntas para revisão

1. O que é um Hook?
2. Por que Hooks devem ser chamados no topo do componente?
3. Qual a diferença entre `useState` e `useReducer`?
4. Para que serve o `useEffect`?
5. Quando faz sentido criar um Hook personalizado?
6. Qual a diferença entre `useMemo` e `useCallback`?
7. Por que não devemos usar Hooks de performance sem necessidade?
8. Como o `useContext` evita o prop drilling?
9. Para que serve o `useRef`?
10. Como salvar informações no `localStorage` usando Hooks?

---
