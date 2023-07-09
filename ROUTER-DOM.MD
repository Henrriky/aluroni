# O QUE É SPA

- Browser Router, Routes, Route, Link, useNavigation, useLocation, useParams.

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





