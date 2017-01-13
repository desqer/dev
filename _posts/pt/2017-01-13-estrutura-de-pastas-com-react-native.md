---
layout: post
title: 'Estrutura de pastas com React Native'
description: 'Nesse artigo vou explicar a estrutura de pastas que escolhemos para o desenvolvimento do aplicativo do Desqer'
categories: mobile
tags: [artigo, react native]
lang: pt
thumbnail: estrutura-de-pastas-react-native.png
author: diego
---

Acho que o maior dilema de todo desenvolvedor ao iniciar um projeto é escolher a melhor organização das pastas e arquivos. No Desqer não foi diferente. O [React Native](https://facebook.github.io/react-native/), por ser um framework totalmente baseado em componentes, gera muitos arquivos. Cada componente pode herdar características de um componente pai e essa iteração pode ser infinita ocasionando uma bagunça no projeto.

Comecei procurando no Google alguns posts de desenvolvedores React para ter como inspiração na hora de escolher a melhor alternativa pro Desqer. Nesse post vou descrever a estrutura que escolhi sendo a mais adequada e como cada item dentro dela funciona.

## Estrutura de pastas

![Estrutura de pastas do Desqer](/assets/posts/estrutura-de-pastas-desqer.png){: .left}
A estrutura de pastas do Desqer ficou da seguinte maneira:

A pasta `app` fica reponsável por abrigar todo e qualquer código produzido pelo desenvolvedor, assim como componentes, serviços, etc.

Um problema que me incomodou bastante desenvolvendo com o React Native, foi a incapacidade de referenciar os caminhos de uma forma mais organizada, ou seja, caso meu componente esteja localizado em `app/scenes/sign/components/component.js` e eu precise importar um outro arquivo em `app/common/components` vou ter que descrever todo caminho relativo àquele componente, nesse caso ficaria:

```javascript
import { Component } from '../../../common/components/another.js'
```

Assim, se um dia o `component.js` mudasse de localização em meu projeto, todos seus imports também teriam que ser alterados.

Para resolver esse problema, na raiz do diretório app, incluí um arquivo `package.json` com o seguinte conteúdo:

```json
{
    "name": "app"
}
```

Dessa forma, podemos utilizar o `app` como namespace para nossos imports e o mesmo componente citado acima poderia ser importado da seguinte forma:

```javascript
import { Component } from 'app/common/components/another.js'
```

Fica muito mais fácil, não é? :D

Partindo para a estrutura de pastas em si, eu utilizo no Desqer duas pastas principais, a pasta common e outra chamada scenes. Na pasta common eu deixo apenas componentes ou serviços que vou utilizar em todo projeto (por exemplo o [TextInput com ícone]({% post_url 2017-01-12-utilizando-icones-dentro-do-textinput-no-react-native %}) que é bem útil).

![Estrutura da pasta scenes](/assets/posts/estrutura-pasta-scenes.png){: .left}
Dentro da outra pasta, scenes, eu mantenho todas as telas da aplicação (que podem estar divididas em sub-módulos para ficar mais organizado). Por exemplo, no caso do Desqer, a parte de autenticação do usuário está dentro de uma pasta sign dividida entre outras duas signin e signup. Essa divisão pode não ocorrer em casos onde a própria pasta dentro da scenes já seja suficiente para indicar a tela. A separação em mais diretórios possibilita o compartilhamento de componentes e serviços entre os sub-módulos. No Desqer, os campos de texto e botões entre as telas de autenticação e cadastro são extremamente parecidos, logo, dentro da pasta `scenes/sign/components`, adicionei um componente para botão e outro para campo de texto que são utilizados pelas duas telas de autenticação.

Ainda assim, as telas de autenticação e cadastro possuem seus próprios componentes que não devem ser compartilhados com outros módulos da aplicação, dessa forma, adicionei também uma pasta components dentro de `scenes/sign/scenes/signin` e outra para o cadastro. Assim, a tela de login, por exemplo, pode utilizar componentes definidos em seu próprio escopo, componentes do diretório sign e também componentes comuns definidos na pasta common tratada anteriormente.

Lembrando que nessa estrutura, um componente definido dentro do escopo de autenticação jamais poderia ser usado em uma tela de cadastro, por exemplo.

Bom, espero que a estrutura de pastas para React Native do Desqer possa ajudar você em seu projeto!
