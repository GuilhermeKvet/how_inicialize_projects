## Iniciando projetos TypeScript <img align="center" alt="Gui-TypeScript" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" />

---

### **Obrigatórios para iniciar 🚀**

 Primeiro devemos iniciar o npm

`npm init -y`

###
Iniciar o git para versionar o código

`git init`

###
Criaremos o arquivo `.gitignore` contendo:

```
node_modules
```

###
Instalamos o TypeScript

`npm i typescript -D`

###
Para iniciar o TypeScript com o arquivo tsconfig.json

`npx tsc —init`

###
Para o node executar arquivos TypeScript precisamos do ts-node-dev

`npm i ts-node-dev -D`

###
Instalamos o Express juntamente com a Tipagem

`npm i express @types/express -D`

###
Criaremos então a pasta `src` com o arquivo `app.ts` contendo o seguinte codigo: 

```
import express, { Request, Response } from 'express'

const app = express()

app.use(express.json())

app.get('/', (req: Request, res: Response) => res.status(200).json({ message: 'OK }))

export default app
```

###
No mesmo nível do arquivo app, criamos o `server.ts` com o seguinte código:

```
import app from "./app"

const PORT = 3001 //ou process.env.PORT caso utilize o dotenv

app.listen(PORT, () => console.log(`Server running on port ${PORT}`))
```

###
Devemos agora adicionar no `package.json` um novo script:

```
...
"scripts": {
  ...
  "dev": "tsnd src/server.ts" <-- Adicione essa linha ---
}
```

##
### **Passos para testes 🛠**

#### Jest
[Documentação](https://jestjs.io/docs/getting-started)

Para instalar o jest, juntamente com os tipos e com transformador de arquivos test .ts:

`npm i jest @types/jest ts-jest -D`

Depois para criar as configurações, execute

`npx jest --init`

Opções recomendadas: [yes, no, node, yes, v8, yes]

No arquivo `jest.config.js` procure pela chave `transform` e adicione o valor

```
...
transform: {
  '.+\\.ts$: 'ts-jest'
}
...
```

Ainda no arquivo `jest.config.js` procure por `testMatch` e adicione:
```
...
testMatch: [
  "**/test/integration/*.test.ts" <-- Local onde estão os testes
],
...
```

No package json, no script `npm test`, adicione:
```
...
script: {
  "test": "jest --runInBand --passWithNoTests"
}
...
```

⚠️ Caso tenha instalado o Husky e criado o arquivo `pre-commit`, acrescente no final: 

```
...
npm test
```
##
#### Lint-staged
[Documentação](https://www.npmjs.com/package/lint-staged)

Para realizar determinados comando apenas nos arquivos que estão em staged:

`npm i lint-staged -D`

⚠️ Caso tenha o package do Husky, no arquivo `pre-commit` substitua os comandos presentes e adicione a instrução:

`npx lint-staged`

Na sequência, cria-se um arquivo chamado `.lintstagedrc.json` com o seguinte código:

```
{
  "*.ts" : [
    "eslint .", <-- Adicione os códigos que queira que execute a cada commit.
    "npm test"
  ]
}
```


##
### **Úteis, mas não obrigatórios 🎨**

###
#### Linter
[Documentação](https://www.npmjs.com/package/eslint-config-standard-with-typescript)

`npm i eslint-config-standard-with-typescript -D`

###
Após instalar, precisamos criar o arquivo de configuração `.eslintrc.json` com o seguinte código:
```
{
  "exyends: "standard-with-typescript",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "rules": {   // onde podemos criar novas regras
    // exemplo
    //"@typescript-eslint/no-misused-promisses": "off",
  }
}
```

###
#### Commit-msg-linter
[Documentação](https://www.npmjs.com/package/git-commit-msg-linter)

Para bloquear commits não semanticos

`npm i git-commit-msg-linter -D`

###
#### Husky
[Documentação](https://www.npmjs.com/package/husky)

Para verificação do projeto antes de dar um commit, como linter e testes.

Instalar o husky

`npm i husky --save-dev`

`npm set-script prepare "husky install"`

`npm run prepare`

###
Para adicionar um hook (codigo a ser executado antes do commit, para bloquea-lo ou não)

`npx husky add .husky/pre-commit "npx eslint ."`

###
⚠️ Caso tenha instalado o `git-commit-msg-linter`, devera adicionar um outro hook:

`npx husky add .husky/commit-msg ".git/hooks/commit-msg"`

Criando um hook para executar testes antes do PUSH

`npx husky add .husky/pre-push "npm test"`