## Cypress Brasil 🇧🇷 FAQ
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-2-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

Este documento foi criado para ajudar a solucionar os problemas mais comuns ao usar o Cypress. Se você não encontrar a resposta para sua pergunta aqui, fique à vontade para abrir uma issue e dar sua sugestão!

<details>
  <summary>
    <a>Geral</a>
  </summary>
  
  <a name="geral"></a>
  
  - [Cypress não abre após a instalação](#o-cypress-não-abre-após-a-instalação)
</details>

<details>
  <summary>
    <a>force:true</a>
  </summary>
  
  <a name="force:true"></a>
  
  - [Evitando o uso do force:true](#evitando-o-uso-do-force-true)
</details>

<details>
  <summary>
    <a>uncaught exception</a>
  </summary>
  
  <a name="uncaught exception"></a>  
  
  - [Lidando com uncaught exception](#lidando-com-uncaught-exception)
</details>




## Geral
### O Cypress não abre após a instalação.

Certifique-se de que você tenha a versão mínima necessária do Node.js e npm. Além disso, tente deletar a pasta `node_modules` e o arquivo `package-lock.json`, e então reinstale as dependências. Você pode deletar manualmente ou rodando o seguinte comando no terminal (lembre-se de estar na raiz do projeto):

```bash
rm -rf node_modules package-lock.json
npm install
```
## Evitando o uso do force: true

O `force: true` é usado quando o elemento a ser clicado está coberto ou fora de visualização. Ou seja, utilizar essa abordagem com frequência é uma má prática. Você pode usar algumas alternativas:

### Aguarde até que o elemento esteja visível:
Pode ser que o elemento ainda não tenha sido renderizado na tela no momento do clique. Utilize should('be.visible') para garantir que o elemento esteja visível antes de tentar interagir com ele.

```javascript
cy.get('seletor').should('be.visible').click();
```

### Scroll até o elemento
Se o elemento não estiver no topo, você pode usar o comando `scrollIntoView` para garantir que o elemento esteja visível na janela de visualização.

```javascript
cy.get('seletor').scrollIntoView().click();
```

### Aguarde até que o elemento não esteja oculto:
Certifique-se de que o elemento não esteja oculto usando should('not.be.hidden').

```javascript
cy.get('seletor').should('not.be.hidden').click();
```

### Interagir com um elemento filho:
Às vezes, clicar em um elemento filho pode ser mais eficaz.

```javascript
cy.get('seletor').find('child-seletor').click();
```

### Ajustar a posição de clique:
Especificar a posição do clique pode ajudar a evitar elementos sobrepostos.

```javascript
cy.get('seletor').click('center');
```

### Verificar se há algum problema com a aplicação:
Elementos sobrepostos ou fora de visualização podem indicar problemas na aplicação em si. Verifique se há algo errado no layout ou no comportamento da aplicação que possa ser corrigido.

### Usar force: true somente quando necessário:
Em casos onde não é possível evitar o uso de force: true, documente bem o motivo e avalie se há melhorias que possam ser feitas no código ou no estrutura da aplicação para evitar a necessidade desse uso.

```javascript
cy.get('seletor').click({ force: true });
```

Ao seguir essas alternativas, você poderá criar testes mais robustos e evitar o uso do force: true, que pode mascarar problemas escondidos na aplicação.

## Lidando com uncaught exception

### O Cypress exibe erro de uncaught exception durante a execução dos testes

Em alguns casos, o Cypress pode encontrar exceções não capturadas (erros de JavaScript) que faz o teste falhar. Para continuar a execução dos testes, você pode usar um comando para ignorar essa exceção.

#### Como usar `Cypress.on('uncaught:exception')`?

Adicione o comando abaixo no arquivo de configuração (`cypress/support/e2e.js`) para ignorar todas as exceções não capturadas:

```javascript
Cypress.on('uncaught:exception', (err, runnable) => {
  // Retorna false para impedir que o Cypress falhe o teste
  return false
})
```
### Quando devo usar esta abordagem?

Essa abordagem deve ser usada com cuidado. Ignorar exceções pode ocultar problemas reais que precisam ser corrigidos na sua aplicação. Use se você sabe que é um erro conhecido que não afeta os testes que você está executando.

### Há alguma alternativa para lidar com exceções específicas?

Sim, se você quiser lidar com exceções específicas, você pode adicionar lógica dentro da função de callback para filtrar erros dependendo da mensagem ou outros atributos:

```javascript
Cypress.on('uncaught:exception', (err, runnable) => {
  // Ignora exceções específicas com base na mensagem de erro
  if (err.message.includes('Expected error message')) {
    return false
  }
  // Permite que outras exceções interrompam os testes
  return true
})
```
### Exemplo Prático

O exemplo abaixo ignora um erro específico de uma biblioteca de terceiros, mas permite que outras exceções interrompam os testes:

```javascript
Cypress.on('uncaught:exception', (err, runnable) => {
  // Ignora erros da biblioteca 'ResizeObserver'
  if (err.message.includes('ResizeObserver loop limit exceeded')) {
    return false
  }
  // Permite que outras exceções interrompam os testes
  return true
})
```


Feito com ❤️ por Adriano Driuzzo 

👋🏽 [Entre em contato!](https://www.linkedin.com/in/adriano-driuzzo/)

## Colaboradores ✨

Agradecimentos aos colaboradores: ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/leolpc21"><img src="https://avatars.githubusercontent.com/u/43275999?v=4?s=100" width="100px;" alt="Leonardo Costa"/><br /><sub><b>Leonardo Costa</b></sub></a><br /><a href="https://github.com/driuzzo/cypress-faq/commits?author=leolpc21" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/driuzzo"><img src="https://avatars.githubusercontent.com/u/16465816?v=4?s=100" width="100px;" alt="Adriano Driuzzo"/><br /><sub><b>Adriano Driuzzo</b></sub></a><br /><a href="https://github.com/driuzzo/cypress-faq/commits?author=driuzzo" title="Code">💻</a></td>
    </tr>
  </tbody>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!