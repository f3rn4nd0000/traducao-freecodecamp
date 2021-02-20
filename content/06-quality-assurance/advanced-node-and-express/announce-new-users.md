---
id: 589fc832f9fc0f352b528e78
title: Announce New Users
challengeType: 2
forumTopicId: 301546
dashedName: announce-new-users
---

# --description--

Muitas salas de bate-papo tem a possibilidade de anunciar quando um usuário é conectado ou desconectado e mostrar aquilo a todos os usuários conectados naquela sala. Considerando que você já estivesse emitindo um evento ao se conectar ou desconectar, você só precisa modificar esse evento para oferecer suporte a tal recurso. A maneira mais lógica de fazer isso é enviar 3 pieces de informação de dados com o evento: o nome do usuário que se conectou/desconectou, o número de usuários atual, e se o usuário se conectou ou desconectou.

Modifique o nome do evento para `'user'`, e  passe um objeto com ele contendo os campos 'name' , 'currentUsers', e 'connected' (para ser 'true' em caso de conexão e 'false' em caso de desconexão do usuário enviado no evento). Tenha certeza de que mudou ambos os campos 'user count' e definiu o usuário desconectado para enviar `false` para o campo 'connected' em vez de `true` como quando ocorre evento de conexão.

```js
io.emit('user', {
  name: socket.request.user.name,
  currentUsers,
  connected: true
});
```
Agora seu cliente terá todas as informações necessárias para mostrar corretamente o número de usuários conectados corretamente e anunciar quando um usuário se conecta ou desconecta! Para gerenciar esse evento na interface do cliente devemos fazer a aplicação esperar por `'user'`, então atualizar o contador de usuários usando jQuery para mudar o texto de `#num-users` para `'{NUMBER}users online'`, mas também acrescentar uma `<li>` para a lista não ordenada com id `messages` com `'{NAME} has {joined/left} the chat'`.

Uma implementação disso fica parecida com o trecho abaixo: 

```js
socket.on('user', data => {
  $('#num-users').text(data.currentUsers + ' users online');
  let message =
    data.name +
    (data.connected ? ' has joined the chat.' : ' has left the chat.');
  $('#messages').append($('<li>').html('<b>' + message + '</b>'));
});
```

Submita sua página quando achar que estiver tudo pronto. Se você está encontrando erros, você pode checar os projetos já completados até agora [aqui](https://gist.github.com/camperbot/bf95a0f74b756cf0771cd62c087b8286). 

# --hints--

Evento `'user'` deve ser emitido com name, currentUsers, e connected.

```js
(getUserInput) =>
  $.get(getUserInput('url') + '/_api/server.js').then(
    (data) => {
      assert.match(
        data,
        /io.emit.*('|")user\1.*name.*currentUsers.*connected/gis,
        'You should have an event emitted named user sending name, currentUsers, and connected'
      );
    },
    (xhr) => {
      throw new Error(xhr.statusText);
    }
  );
```
O cliente deve gerenciar propriamente e mostrar o novo dado do evento `'user'`.

```js
(getUserInput) =>
  $.get(getUserInput('url') + '/public/client.js').then(
    (data) => {
      assert.match(
        data,
        /socket.on.*('|")user\1[^]*num-users/gi,
        'You should change the text of "#num-users" within on your client within the "user" event listener to show the current users connected'
      );
      assert.match(
        data,
        /socket.on.*('|")user\1[^]*messages.*li/gi,
        'You should append a list item to "#messages" on your client within the "user" event listener to announce a user came or went'
      );
    },
    (xhr) => {
      throw new Error(xhr.statusText);
    }
  );
```

# --solutions--

```js
/**
  Backend challenges don't need solutions, 
  because they would need to be tested against a full working project. 
  Please check our contributing guidelines to learn more.
*/
```
