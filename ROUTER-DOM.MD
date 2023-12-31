# O QUE É SPA

- ESLint, sort, splice, window.location.pathname, reutilização de componentes, pagina padrão, pagina coringa (*).
- Browser Router, Routes, Route, Link, outlet, useNavigation, useLocation, useParams.

## ESLINT

- `npm init @eslint/config`
- `npx eslint ./src --fix`
--> 'React' must be in scope when using JSX  react/react-in-jsx-scope
    - Esse erro é devido à configuração de estilo que escolhemos, em que nos obriga a importar o React no topo do código. Podemos resolvê-lo navegando até o arquivo de configuração do ESLint chamado .eslintrc.json, e adicionarmos na propriedade rules a seguinte regra:
        - “react/react-in-jsx-scope”: “off”,

## CRIANDO O INICIO

- Vamos criar uma página de inicio do nosso site.
- Agora vamos importar o itens.json para exibir na página inicial, entretanto, ficar navegando até esses arquivos é algo maçante. Para isso, vamos colocar esse json em outro local, pois ele é algo utilizado em toda as paginas.

# ESTRUTURA BASICA DO ROUTER DOM

## PRATOS RECOMENDADOS

- Vamos criar uma pasta data, que vai fornecer todos esses dados da aplicação
- Nesse inicio vamos ter um array de itens do cardapio que é feito de forma aleatoria com o sort e o splice
    ```js
          let pratosRecomendados = [...cardapio];
          pratosRecomendados = pratosRecomendados.sort(() => 0.5 - Math.random()).splice(0,3);
    ```
- Agora vamos fazer um map nos pratosRecomendados:
    ```js
        {pratosRecomendados.map(item => (
          <div key={item.id} className={styles.recomendado}>
            <div className={styles.recomendado__imagem}>
              <img src={item.photo} alt={item.title}/>
            </div>
          </div>
        ))}
    ```

## RESOLVENDO ROTAS COM JS

- O React trabalha com SPA, ou seja, ele tem apenas uma página HTML e injeta coisas com Javascript nessa página, sem precisar que ela seja atualizada.
- Vamos fazer a principio utilizando o JS nativo, com a propriedade global location.pathname, que pega o path que estamos na nossa URL.
    - `window.location.pathName === '/' ? <Inicio/> : <Cardapio/>`
- Entretanto, é muito chato ter que ficar fazendo isso, pois nós temos partes da aplicação que queremos reaproveitar. E uma das soluções para esse problema é a biblioteca do `router-dom`

## USANDO ROUTER DOM

- `npm i react-router-dom`
- Nas aplicações React é comum termos um arquivo chamado de `routes.js`, que é o local aonde a gente gerencia as rotas da aplicação
    - Vamos importar do react-router-dom o:
        - `BrowserRouter`, que nos dá acesso ao pathname
        - `Routes`, funciona como o switch, para evitar a sobreposição de rotas
        - `Route`, é a rota em si, recebe dois parâmetros: `path`, que é caminho que vai ser acessado e `element`, que é o componente que vai ser renderizado
- Agora vamos colocar esse arquivo `routes.js` como o elemento renderizado no root
- [Documentacao](https://v5.reactrouter.com/web/api/Switch)

# CRIANDO AS ROTAS

## REAPROVEITANDO MENU

- Tem certas partes da nossa aplicação que nós desejamos reaproveitar na nossa aplicação, um exemplo disso é o menu, que não muda, e permanece la em cima não importa qual rota nós estejamos acessando
    - Para isso vamos retirar o menu do cardapio e colocar ele dentro de uma pasta components que fica na src, refatorando o estilo e as importações
    - Agora vamos importar o componente para ambas páginas: `Cardapio` e `Inicio`
- Entretanto, existe uma forma da gente não precisar copiar o Menu em várias páginas, utilizando ele apenas na routes.js

## REAPROVEITAMENTO COM REACT-ROUTER

- Para resolver isso, nós devemos colocar fora do `Routes`, que é nosso componente que possui as rotas da aplicação, o nosso `Menu`.
    - Agora estamos renderizando o mesmo componente, e não renderizando dois componentes.
- Criando nossos links no menu
    - Criar um array de objetos, que vai ter label e to, para que a gente possa renderizar eles utilizando o map
- Entretanto, nós estamos atualizando nossa página toda vez que acessamos outra rota da aplicação, o que quebra o principio do React que é ser SPA. isso acontece pois estamos utilizando a tag a, que faz com que nos mande para um HTML.

## TROCANDO A TAG A

- Extensão React Developer Tools do Google Chrome. Essa extensão nos permite ver quando um componente é renderizado tal como as informações dos componentes como props e seus filhos.
- Para resolver o problema anterior vamos utilizar a tag `Link` do react-router-dom
    - Esse componente funciona igual ao a, só que a propriedade href vira to
    - Agora podemos observar que os componentes são renderizados novamente toda vez que alteramos a rota da aplicação. Ou seja, ele não pega um doc novo e fica mudando de um componente para outro.

## REAPROVEITAMENTO DO HEADER

- Reaproveitar o header
    - Vamos refatorar o routes.js, colocando uma tag main antes do conteúdo.
    - Refatorar Cardapio.tsx, retirando a tag header e colocando ele no routes.js adicionado seu estilo
    - Entretanto, mesmo que esteja funcionando, quando acessamos uma página não indexada a aplicação continua exibindo o header. Além disso, nosso route está tendo responsabilidade a mais.

## PAGINA PADRÃO

- Para resolver esse problema, vamos criar uma pagina padrão que vai conter nosso header dentro da pasta components
- Agora vamos colocar no element do `Route` o nosso <PaginaPadrao/> e colocar dentro de `<Route></Route>` com o element={PaginaPadrao} as rotas da aplicação.
    - Entretanto, mesmo que a gente coloque dentro do nosso Route os nossos filhos, nós não conseguimos exibir ele. Para fazer isso funcionar, vamos importar dentro do componente `PaginaPadrao.tsx` o componente `outlet` do react-router-dom, o qual vai ser colocado dentro da PaginaPadrao. Assim o router dom vai passar a reconhecer os componentes de acordo com a rota. Esse é children do react router.
    - Estamos usando nesse caso nested routes
    - Além disso, agora que estamos exibindo a pagina padrão apenas para os filhos dele, caso a gente entre em uma pagina que não esta indexada ele não vai mostrar o conteudo da paginapadrao

# ROUTER DOM HOOKS

## ESTILO TEMA

- Vamos criar um tema.module.scss dentro da pasta styles para utilizar o title e o nosso wrapper em toda a aplicação, retirando do Cardapio e do Inicio o css do `titulo e container`.
    - Para reutilzar o tema vamos importar o módulo stylesTema a partir da page Cardapio e Inicio, subtituindo nos titulos por stylesTema.titulo
    - Agora basta nós colocarmos o stylesTema.container da tag div que engloba o Outlet dentro da PaginaPadrao
    ```js
        export default function PaginaPadrao() {
        return (
            <>
            <header className={styles.header}>
                <div className={styles.header__text}>
                A casa do código e da massa
                </div>
            </header>
            <div className={stylesTema.container}>
                <Outlet/>
            </div>
            </>
        );
        }   
    ```

## TERMINANDO A TELA INICIO
## TERMINANDO A TELA DE SOBRE

## CRIANDO O NOTFOUND

- Criar o footer (componente)
- Criar pagina de NotFound e seu estilo
- Adicionar pagina para o routes.js
    - Para isso, vamos criar um `<Route path='*'/>` que recebe como path tudo que não for o que ja foi definido.
- Entretanto, agora nossa aplicação está pegando o header do paginaPadrao, e nós não queremos isso.

## VOLTANDO PARA OUTRA TELA

- Resolvendo erro do header
- Voltando para tela anterior
    - Para voltar para a tela anterior vamos criar um botão, e esse botão vai chamar uma função que volta para a tela anterior
    - O react-router-dom possui um histórico que funciona como uma pilha. Ou seja, se ele estiver em uma rota e pedir para ele voltar com base no histórico ele vai voltar para a rota de baixo dele.
    ```js
    import { useNavigate } from 'react-router-dom';

    export default function NotFound() {
    const navigate = useNavigate();
    return (
        <div className={classNames({
        [styles.container]: true,
        [stylesTema.container]: true 
        })}>
        <div className={styles.voltar}>
            <button onClick={() => navigate(-1)}>
            {'<Voltar'}
            </button>
        </div>
        <NotFoundImage />
        </div>
    );
    }
    ```
    - Nesse caso utilizamos o useNavigate que é um hook do react router, e pegamos uma função do useNavigate, essa função pode receber como argumentos um número ou uma string, sendo o -1 a pagina anterior e a strin a pagina que queremos q o usuario seja redirecionado.


# FINALIZANDO O PROJETO

## Criando a rota Prato

- Vamos criar uma page prato, e importar ela no nosso arquivo routes.js foram do route que recebe nossa paginaPadrao.
    - Nessa pagina vamos precisar usar id dinâmico, pois quando o usuario clicar em uma receita ele vai ser redirecionado para uma pagina /pratos/id sendo id o id do prato que ele clicou em ler mais.
    - Para isso, nós vamos usar os ids dinâmicos e depois pegar ele com o `useParams` ou o `useLocation`
    ```js
        <Route path='prato/:id' element={<Prato/>}/>
    ```
    - Vamos importar dentro do Prato.tsx o useParams
    ```js
        import { useParams } from 'react-router-dom';
    ```

## useLocation e state

- No useLocation nós temos mais propriedades para pegar, diferentemente no useParams, que pega apenas o valor que vem depois dos dois pontos ":". Além disso, dentro dele temos também:
    - hash:
    - key:
    - pathname: URL que nós estamos. Exemplo: `/prato/2`
    - search: query params da nossa aplicação. Exemplo: pratos/2`?descricao=teste`
    - state: variavel passada pelo useNavigate da aplicação que redirecionar para a pagina
- Para enviar o prato para a nossa pagina de pratos vamos ter que redirecionar da tela de inicio a partir de um clique no botão de ver mais:
    ```js
        function redirecionarParaDetalhes(prato: typeof cardapio[0]) {
            navigate(`/prato/${prato.id}`, { state: { ...prato }});
        }
    ```
    - Essa função executa o navigate, passando todas as propriedades do prato que chamou a função.
    - Além disso, no próprio window.history nós temos esse state.
    - Outro parâmetro do navigate é o replace, que nada mais é do que um boolean, e faz com que a gente navegue para a rota Prato e substitua na pilha a pagina que enviou para a prato o Prato em si.
    - Um exemplo de quando usamos isso é o Login, pois quando realizamos ele e você já fez ele a gente não quer que no historico tenha a tela de Login.

## Detalhes do prato

- Essa parte não tem segredo, é só aplicar o CSS com o module e colocar os dados dos pratos nas respectivas divs. Além disso, devemos tipar o state que é recebido pelo useLocation
    ```js
      const { prato } = state as { prato: typeof cardapio[0]}; 
    ```
- Entretanto, tem um gap de erro na nossa aplicação, pois o estado do useLocation pode vir vazio se o usuario tentar acessar a nossa aplicação diretamente.

## Usando o useParams

- Como a gente não quer que exiba uma tela em branco caso o usuario vá direto na página de ler mais nós devemos realizar um condicional para caso não exista um state, pegando o id com o useParams e realizando um find no nosso cardápio e exibindo o componente na tela.
- Para resolver isso, vamos refatorar o nosso código anterior e substituir o useLocation pelo useParams, colocando apenas um prato que faz o find no cardápio e não retorna nada se o não houver prato para aquele id. Além disso, vamos usar o useNavigate() para redirecionar o usuario para a pagina anterior com o botão voltar
- Por fim, vamos criar dentro de `src` uma pasta chamada de `types`

## Refatorando TAGS

- Vamos criar um componente das tags (preço, porção, categoria).
    - Passar as props do componente, desestruturando apenas o que vamos utilizar.
    - Refatorar o Item.tsx e Prato.tsx, renderizando o componente com o spread operator de um unico prato.
- Vamos fazer com que o header apareça no Prato.tsx, movendo ele para dentro do Route do PaginaPadrao.

# AJUSTES NO NOTFOUND E HEADER

## Notfound na tela de detalhes

- Vamos ao invés de retornar uma string vazia, retornar o componente de NotFound quando não existir um prato para aquele ID.
- Além disso, vamos implementar a função de ler mais sobre o prato na página do cardápio para melhor a UX, para isso vamos atribuir um evento de click para a nossa div e usar o useNavigate para enviar o usuario para a pagina de pratos com o id.
- **OBS:** `<Navigate/>`
    ```Incrível! Podemos utilizar esse componente quando quisermos que, ao carregar a página, sejamos redirecionados para uma outra página. Essa técnica pode ser utilizada quando queremos barrar um usuário de acessar uma página, ou redirecionar para uma NotFound, por exemplo. Passamos como parâmetro o to, que recebe como valor a rota a ser redirecionada.```

## Resolvendo o Header

- Uma forma de resolver o problema do header é passando para o PaginaPadrao um children nas suas props e exibindo ele dentro do container que nós definimos, e quando formos renderizar o prato.tsx ele vai renderizar com o header, pois o PaginaPadrao vai estar englobando o elemento tsx
    ```js
        export default function PaginaPadrao({ children }: { children?: React.ReactNode }) {
        return (
            <>
            <header className={styles.header}>
                <div className={styles.header__text}>
                A casa do código e da massa
                </div>
            </header>
            <div className={stylesTema.container}>
                <Outlet/>
                {children}
            </div>
            </>
        );
        }

          if (!prato) {
                return <NotFound/>;
            }

            return (
                <PaginaPadrao>
                <button className={styles.voltar} onClick={() => { navigate(-1); }}>
                    {'< Voltar '}
                </button>
                <section className={styles.container}>
                    <h1 className={styles.titulo}>
                    {prato.title}
                    </h1>
                    <div className={styles.imagem}>
                    <img src={prato.photo} alt={prato.title} />
                    </div>
                    <div className={styles.conteudo}>
                    <p className={styles.conteudo__descricao}>
                        {prato.description}
                    </p>
                    <TagsPrato {...prato}/>
                    </div>
                </section>
                </PaginaPadrao>
            );
    ```
- 2º Maneira: Podemos criar um novo route dentro de Prato.tsx e dentro dele nós vamos colocar um Route que tem a pagina padrão e vai ter um filho Route que tem o path index e que tem como elemento o conteudo de prato que tinhamos anteriormente dentro de fragment.