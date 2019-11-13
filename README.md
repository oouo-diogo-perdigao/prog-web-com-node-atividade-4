# Exercício 4
Neste exercício iremos estudar a forma de autenticação de usuários baseada no OAuth, utilizando o Github com o Passportjs.

## Passo 1
Iremos implementar a estratégia de autenticação no Github. Se você ainda não possui uma conta, crie aqui. Em seguida, siga o tutorial do próprio github para criar uma aplicação e ter acesso a um CLIENT_ID e um CLIENT_SECRET, necessários para autenticação.
https://developer.github.com/apps/building-github-apps/creating-a-github-app/

Em Homepage URL e User authorization callback URL substitua por http://localhost:3000 e http://localhost:3000/auth/github/callback, respectivamente.

## Passo 2
Clonar o repositório abaixo:
https://github.com/samwx/node-passportjs

## Passo 3
Iremos desenvolver uma aplicação com autenticação no github e uma página protegida. Para isso, após clonar o repositório do Passo 2, certifique que a aplicação funciona normalmente rodando o comando npm install e em seguida npm start.

## Passo 4
Copie o CLIENT_ID e CLIENT_SECRET gerados no passo 1 e substitua-os no arquivo github.strategy.js dentro da pasta “configs”.

## Passo 5
Iremos implementar o nosso botão de login no github. Para isso, no arquivo index.hbs dentro da pasta views, crie um link (utilizando a tag <a> 😁) para a rota /auth/github (iremos implementá-la nos passos seguintes).

## Passo 6
No arquivo index.js da pasta routes, implemente as rotas /auth/github e /auth/github/callback substituindo o código pelo seguinte trecho:

```js
var express = require('express');
var router = express.Router();
var passport = require('passport');
/* GET home page. */
router.get('/', function(req, res, next) {
	res.render('index', { title: 'Express' });
});

router.get('/auth/github',
	passport.authenticate('github'));

router.get('/auth/github/callback',
	passport.authenticate('github', { failureRedirect: '/login' }),
	function(req, res) {
		// Successful authentication, redirect home.
		res.redirect('/admin');
	});
module.exports = router;
```

## Passo 7
Iremos implementar uma função para verificar se a sessão atual é válida e se o usuário está autenticado em alguma das nossas estratégias definidas. Para isso, no arquivo index.js, adicione o código abaixo logo abaixo da linha var passport = require('passport');

```js
function ensureAuthenticated(req, res, next) {
if (req.isAuthenticated()) {
// req.user is available for use here
return next(); }
// denied. redirect to login
res.redirect('/')
}
```

## Passo 8
Ainda no arquivo index.js, Implemente a rota /admin, com verificação de sessão ativa conforme o exemplo abaixo:

```js
router.get('/admin', ensureAuthenticated, function(req, res) {
res.render('admin', { user: req.session.passport.user })
});
```

O objeto “user” possui as propriedades “displayName” e “provider”. Crie um arquivo admin.hbs na pasta views e exiba uma mensagem de boas-vindas ao usuário logado, juntamente com a informação “provider”.

## Passo 9
Por último, faça o seguinte teste: abra o link http://localhost:3000/admin em uma janela anônima e certifique de que o usuário não terá acesso a área logada. Caso esteja tudo ok, no arquivo admin.hbs criado no passo 8, coloque um link para a rota /logout para deslogar o usuário do sistema.

