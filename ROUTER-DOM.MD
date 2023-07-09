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
    