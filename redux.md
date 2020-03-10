# Redux

[https://redux.js.org/basics/usage-with-react/](https://redux.js.org/basics/usage-with-react/)

## Qual problema Redux tenta resolver?

Já sabemos que o `state` gerencia o estado de um componente. Mas como fazer para compartilhar esse estado com outros componentes, podendo eles estar em diferentes níveis da hierarquia.

Primeiramente, o Redux é uma biblioteca que gerencia os estados globais de uma aplicação. Ele resolve esse problema utilizando o estado global (`single source of truth`). O redux é composto por 3 partes básicas:

- `actions`: funções que são disparadas para alterar o estado da aplicação;
- `reducers`: recebem e tratam as informações recebidas das actions para que sejam ou não enviadas à store;
- `store`: container que centraliza o estado da aplicação (Única fonte de verdade). É imutável.

## Quando utilizar?

Conforme escrevemos uma aplicação, começamos a ver a necessidade de compartilhar estados entre componentes. Mas como fazer isso de forma correta?

```javascript
import React, { Component } from 'react'
import { View } from 'react-native'

import { NavBar } from './NavBar'
import { List } from './List'
import { TabBar } from './TabBar'

export class App extends Component {
  render() {
    return (
      <View>
        <NavBar />
        <List />
        <Footer />
      </View>
    )
  }
}
```

Se, por exemplo, algo no `TabBar` precisa ser alterado por uma ação da `NavBar`, devemos utilizar o Redux? Não!
Nesse caso, podemos gerenciar parte do estado da TabBar no App, assim evitamos o uso do Redux.

## Mas e se fosse um caso mais complexo?

Subir o gerenciamento do estado para um componente funciona, mas tem um limite:

- É necessário re-renderizar a aplicação inteira? (Sem necessidade)
- Como seria se ao invés de 3, estivéssemos 20 componentes? (Seria necessário ficar passando funções com callbacks que alterariam o estados de outros componentes, causando a repetição de informações)

Com o redux, conseguimos remover esse fluxo. Utilizando o `connect`, é possível fazer com que os componentes disparem `actions` e que sejam atualizados quando a `store` for atualizada.
Pense assim, o componente começa a "ouvir" alterações existentes no estado da aplicação, assim como, se torna capaz de alterar esse mesmo state.

Exemplificando:

![Exemplificando](https://miro.medium.com/max/3500/1*JfU2tL6Pik6VGVE2hkNbjg.png)


Lifecycle:
![Redux](https://miro.medium.com/max/1838/1*BcmtHcMHN6PT7IniIWniHg.png)

Integrar com service

## Store
```js
import { createStore } from  'redux'
import  profile  from  './profile'
export  default  createStore(profile)
```

## Reducer
```js
const initialState = {
  profiles: [],
}

function ProfilesReducer(state = initialState, action) {
  switch (action.type) {
    case 'SET_PROFILES':
      return {
        ...state,
        profiles: action.profiles,
      }
    case 'LOGOUT':
      return {
        ...state,
        profiles: initialState.profiles,
      }
    default:
      return state
  }
}

export default ProfilesReducer

```

## Provider
```js
/* eslint-disable react/jsx-filename-extension */

import  React  from  'react'
import { Provider } from  'react-redux'
import { HomeScreen } from  'screens'
import  store  from  './src/store/createStore'

export  default  class  App  extends  React.Component {

	render() {
		return (
			<Provider  store={store}>
				<AppContainer  />
			</Provider>

	)}
}
```
Fluxo Redux:

```mermaid
graph LR
	A(Action Creators)  --> B(Store)
	C(React Components) --> A
	B --> D(Reducers)
	D --> B
	B --> C
```
