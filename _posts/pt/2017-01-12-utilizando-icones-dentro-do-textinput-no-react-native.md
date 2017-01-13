---
layout: post
title: 'Utilizando ícones dentro do TextInput no React Native'
description: 'Nesse tutorial você aprenderá como incluir um componente de ícone dentro do componente TextInput do React Native'
categories: mobile
tags: [tutorial, react native]
lang: pt
thumbnail: icones-react-native.png
author: diego
---

Pelo fato do [React Native](https://facebook.github.io/react-native/) ainda estar em processo de desenvolvimento, algumas tarefas simples se tornam bem chatas. Uma delas é a manipulação de ícones na interface do usuário e seu posicionamento dentro de outros elementos. Nesse tutorial, vou ensinar como utilizar a biblioteca [React Native Vector Icons](https://github.com/oblador/react-native-vector-icons) para adicionar fontes de ícones ao projeto e posicioná-los dentro de um TextInput.

## Instalando

Para instalar o pacote dos ícones em cada sistema operacional, você pode seguir o próprio tutorial na documentação do repositório (Para [iOS](https://github.com/oblador/react-native-vector-icons#ios) e para [Android](https://github.com/oblador/react-native-vector-icons#android)). Outros sistemas também estão disponíveis na sessão de instalação.

Após instalação e a fonte configurada, você pode acessar a componente do ícone no código do seu projeto.

## Utilizando os ícones

No meu caso, instalei a fonte do [Ionicons](http://ionicframework.com/docs/v2/ionicons/) que possui vários ícones lineares bem legais que combinam com o projeto do Desqer. Assim, dentro do meu componente no React Native posso utilizar a seguinte declaração para importar essa fonte:

```javascript
import Icon from 'react-native-vector-icons/Ionicons';
```

A partir da importação, é possível utilizar os ícones dentro do JSX da seguinte forma:

```html
<Icon name="ios-call-outline" size={20} color="#666" />
```

O componente `Input` pode ser utilizado em qualquer contexto e pode receber estilizações semelhantes ao componente `Text` do React Native, são elas:

* backgroundColor
* borderWidth
* borderColor
* borderRadius
* padding
* margin
* color
* fontSize

## Incluindo ícones no TextInput

O TextInput do React Native funciona da mesma forma que o `<input>` no HTML, dessa forma não é possível adicionar o ícone dentro do input diretamente, e então, precisamos criar um novo componente para realizar essa tarefa. Primeiramente crie um arquivo em um local desejado, no meu caso vou incluir em `app/common/components/InputWithIcon.js` e adicione o seguinte código:

```javascript
import React, { Component } from 'react'
import {
  View,
  TextInput,
  TouchableWithoutFeedback,
  StyleSheet
} from 'react-native'

import Icon from 'react-native-vector-icons/Ionicons';

export default class InputWithIcon extends Component {
  constructor(props){
    super(props)
  }

  render(){
    return (
      <View style={styles.inputContainer}>
        <TouchableWithoutFeedback>
          <Icon style={styles.icon} name={this.props.icon} size={20} color="#666" />
        </TouchableWithoutFeedback>
        <TextInput
          style={[styles.input, this.props.style]}
          placeholderTextColor="#666"
          {...this.props}>
        </TextInput>
      </View>
    )
  }
}

SignInput.propTypes = {}

const styles = StyleSheet.create({
  inputContainer: {
    height: 50,
    backgroundColor: '#CCC',
    borderRadius: 3,
    paddingLeft: 20,
    marginTop: 10,
    flexDirection: 'row'
  },

  icon: {
    alignSelf: 'center',
    paddingRight: 20
  },

  input: {
    fontSize: 14,
    color: '#666',
    flex: 1
  }
})
```

Você pode notar que nesse exemplo utilizei um componente `View` principal que faz o papel de container do input dando o efeito que o input está cobrindo tudo, inclusive o ícone. Dentro desse container incluímos dois componentes, um para o ícone e outro para o input em si. Nesse caso escolhi o componente `TouchableWithoutFeedback` que vai nos dar a possibilidade de adicionar posteriormente a funcionalidade de clique para focar o cursor no input.

Assim, em outro arquivo que desejarmos utilizar esse componente, podemos acessar da seguinte forma:

```javascript
<InputWithIcon
  placeholder="Digite seu telefone"
  icon="ios-call-outline" />
```

E assim temos um ícone dentro de nosso TextInput :D
