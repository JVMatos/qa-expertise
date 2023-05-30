# Portfólio de QA Engineer

Este é o repositório contendo meu portfólio de trabalho como QA Engineer, onde compartilho minhas habilidades e experiência em projetos relacionados à área de QA.

## Sobre mim

Meu nome é João Vitor, tenho 24 anos e atualmente resido em Capão Bonito, São Paulo. Em 2018, concluí minha graduação em Análise e Desenvolvimento de Sistemas pela FATEC de Itapetininga, localizada em São Paulo.

No ano de 2022, tive a oportunidade de ingressar na área de QA, desempenhando minhas atividades remotamente. Desde então, descobri uma verdadeira paixão por essa área. Sempre tive uma abordagem crítica em relação a diversos aspectos e isso me permitiu desenvolver habilidades de avaliação e teste de softwares no meu dia a dia.

Anteriormente, trabalhei como designer, o que também contribui para uma melhor compreensão de determinados aspectos dos sistemas, como a interface do usuário (UI) e a experiência do usuário (UX). Acredito que essa bagagem prévia me proporciona uma visão mais abrangente e a capacidade de identificar oportunidades de aprimoramento em sistemas.

## Habilidades

- Testes Manuais

- Testes Automatizados

- Relatórios de Bugs

- Roteiros de Teste

- Teste de Regressão

- Teste Funcionais

- Teste de Usabilidade

- Automação de teste utilizando ferramentas como Cypress e Robot Framework

- Teste de API

- Conhecimentos básicos de linguagens de programação como C#, C++, Python e Java

- Conhecimento intermediário em JavaScript e SQL

- Conhecimento de metodologias ágeis, como Scrum e Kanban

## Projetos

Aqui cito alguns dos projetos em que já trabalhei:

1.  **Banian** - 07/2022 até 12/2022 - [https://materiais.levelgroup.com.br/banian](https://materiais.levelgroup.com.br/banian)

O Banian é um sistema de homologação de fornecedores, cujo principal objetivo é ajudar os clientes a construírem toda sua cadeia de suprimentos de maneira sólida e estruturada, ao mesmo tempo em que os auxilia a acessar informações sobre seus parceiros e fornecedores.

2.  **PAGU Unicamp** - 06/2022 até 12/2022 - [https://www.pagu.unicamp.br/](https://www.pagu.unicamp.br/)

O Pagu é uma plataforma de um Núcleo de Estudos de Gênero da Universidade de Campinas utilizada para publicações, gerenciamento de eventos, publicações de pesquisas e afins.

3.  **Neto Veículos** - 12/2022 até 03/2023

Sistema de gerenciamento, controle de estoque, vendas, cadastro de clientes, fornecedores e afins de uma concessionária.

4.  **Em Desenvolvimento - Sistema de Despachante** 01/2023 - Atualmente

Atualmente estou trabalhando em um sistema voltado para a área de despachantes, fazendo a intermediação entre o cliente e o prestador de serviço.

## Certificações

- Curso de automação Básico, Intermediário e End-To-End com Cypress - Udemy

- Curso de DevOps - Udemy

- Curso de Integração Contínua com Testes utilizando Jenkins - Udemy

- Curso básico e avançado de automação com Robot Framework - Udemy

- Discover - Trilhar Fundamentar e Conectar - Rocketseat

- Fundamentos de Qualidade de Software - DIO

- Metodologias Ágeis e Ciclo de Desenvolvimento de Software - DIO

## Contato

- E-mail: joao.matos@fatecitapetininga.edu.br

- LinkedIn: [https://www.linkedin.com/in/jv-matos/](https://www.linkedin.com/in/jv-matos/)

## Idiomas

- Inglês Intermediário

- Espanhol Básico

- Português Nativo

## Exemplos de Automação de Testes

Aqui estão alguns trechos de código que demonstram minhas habilidades em automação de testes com Cypress:

Teste de Login via UI com custom command e cy.session

Bloco do teste:

```javascript
it('Login com sucesso no sistema', () => {
	// Chama o comando customizado enviando os dados de login e senha que estão no arquivo cypress.env.json

	cy.doLogin(Cypress.env('user_email'), Cypress.env('user_password'), options);
});
```

Comando Customizado:

```javascript
Cypress.Commands.add(
	'doLogin',
	(
		user = Cypress.env('user_email'),
		password = Cypress.env('user_password'),
		{ cacheSession = true } = {}
	) => {
		// Variável que contém os comandos para realizar o login via UI
		const login = () => {
			cy.visit('/');
			cy.get("input[name='email']").type(user);
			cy.get("input[name='senha']").type(password, {
				log: false,
			});
			cy.intercept('POST', '/login').as('getToken');
			cy.contains("button[name='login']", 'Login').click();
			cy.wait('@getToken').its('response.statusCode').should('eq', 200);
			cy.contains('div.message', 'Olá, ' + Cypress.env('user_name')).should(
				'be.visible'
			);
		};

		// Variável para validação do Login
		const validate = () => {
			cy.visit('/');
			cy.location('pathname', { timeout: 1000 }).should('not.eq', '/');
		};

		// Variável com as opções do comando Session
		const options = {
			cacheAcrossSpecs: true,
			validate,
		};

		// Condicional para carregar a sessão anterior ou realizar um novo login
		if (cacheSession) {
			cy.session(user, login, options);
		} else {
			login();
		}
	}
);
```

Exemplo de comando customizado para login via API:

```javascript
Cypress.Commands.add(
	'postLogin',
	(
		user = Cypress.env('user_email'),
		password = Cypress.env('user_password')
	) => {
		// Comando que realiza a requisição POST na API
		cy.request({
			method: 'POST',
			url: `${Cypress.env('baseUrlApi')}/login`,
			body: {
				email: user,
				senha: password,
			},
		}).then((response) => {
			// Recebe os dados retornados da API e os define no localStorage do navegador
			localStorage.setItem('access', response.body.token);
			localStorage.setItem(
				'user',
				JSON.stringify({
					fullName: response.body.usuario.nome,
					email: response.body.usuario.email,
					avatar: response.body.usuario.imagem
						? 'data:image/jpeg;base64,' + response.body.usuario.imagem
						: undefined,
					status: response.body.usuario.status,
					id: response.body.usuario.id,
				})
			);
		});
	}
);
```

Exemplo de comando customizado para post com FormData:

```javascript
Cypress.Commands.add('postImage', (image) => {
	// Recebe uma imagem utilizando o comando cy.fixture e constrói um FormData
	cy.fixture('img/test_img.png').then((fileContent) => {
		const blob = Cypress.Blob.binaryStringToBlob(fileContent, 'image/png');
		const formData = new FormData();
		formData.append('titulo', image.titulo);
		formData.append('descricao', image.descricao);
		formData.append('imagem', blob, 'image.png');

		// Realiza uma requisição que pega o token do login para realizar o POST da imagem
		cy.getToken(Cypress.env('user_email'), Cypress.env('user_password')).then(
			() => {
				cy.request({
					method: 'POST',
					url: `${Cypress.env('baseUrlApi')}/images`,
					headers: {
						Authorization: `Bearer ${Cypress.env('token')}`,
						'content-type': 'multipart/form-data',
					},
					body: formData,
				})

					.then(({ status, body }) => {
						// Valida o status da requisição e converte o arrayBuffer para leitura, salvando-o em um arquivo .json
						expect(status, 'Status').to.eq(201);
						const decoder = new TextDecoder('utf-8');
						responseBody = decoder.decode(body);
						cy.writeFile(
							'cypress/fixtures/write-file/postImage.json',
							responseBody
						);
					})

					.then(() => {
						// Valida as informações da requisição após a conversão
						const responseObj = JSON.parse(responseBody);
						cy.wrap(responseObj).then((wrappedResponse) => {
							expect(wrappedResponse.descricao).to.equal(image.descricao);
							expect(wrappedResponse.titulo).to.equal(image.titulo);
						});
					});
			}
		);
	});
});
```

Exemplo de comando customizado utilizando cy.task para interagir diretamente com o banco de dados:

Conexão com o banco:

```javascript
const { defineConfig } = require('cypress');
const mysql = require('mysql');
function queryTestDb(query, config) {
	const connection = mysql.createConnection(config.env.db);
	connection.connect();
	return new Promise((resolve, reject) => {
		connection.query(query, (err, result) => {
			if (err) reject(err);
			else {
				connection.end();
				return resolve(result);
			}
		});
	});
}

module.exports = defineConfig({
	e2e: {
		setupNodeEvents(on, config) {
			on('task', {
				queryDb: (query) => {
					return queryTestDb(query, config);
				},
			});
		},

		env: {
			db: {
				host: 'localhost',
				user: 'root',
				password: 'root',
				database: 'database',
			},
		},
	},
});
```

Comando customizado que deleta todos os usuários cadastrados em uma tabela:

```javascript
Cypress.Commands.add('db_deleteUsers', () => {
	cy.task('queryDb', 'DELETE FROM `usuarios`');
});
```
