# Redux Saga

Consiste em uma biblioteca (i. é., um middleware para Redux) responsável por facilitar o gerenciamento dos efeitos colaterais (requisições assíncronas, como chamadas para API, transformações impuras, como acessar o cache do navegador), bem como sua "testabilidade" e manutenção.

### Pq, quando e onde utilizar?
Tendo em vista que o Redux suporta apenas fluxo de dados síncrono, se torna essencial o emprego de middlewares para este gerenciamento. Algumas libs comumente utilizadas são o Redux-Thunk, Redux-Saga, Redux-Observable. O primeiro apresenta a abordagem mais simples e atende às demandas menos complexas. Por outro lado, quando gerenciamentos mais complexos são necessários (cancelamento de requisições, etc), o Redux-Saga (e o Redux-Observable) é uma ferramenta mais poderosa, fornecendo um controle mais granular através do emprego de funções Generator.

### Middleware
A ideia por trás do conceito de middlewares do Redux é muito similar à ideia utilizada em frameworks como o ExpressJS, onde é possível injetar códigos que são executados entre o request e o response de uma solicitação.
Sendo assim, o Redux permite que sejam injetadas extensões de terceiros que o manipulam durante o dispatch de uma action e o momento em que essa action chega ao reducer. Esses middlewares podem ser utilizados para realização de log, crash reporting, requisições assíncronas, etc.

#### Adicionando middleware ao projeto
Exemplo de injeção do Middleware ao fluxo do Redux.
`createStore.js` :
```js
import { createStore, applyMiddleware } from "redux";
import createSagaMiddleware from "redux-saga";

import profile from "./profile";

import { ProfileSaga } from "./profileSaga";

// Utiliza as 'Sagas'de Profile para construir o Middleware
const sagaMiddleware = createSagaMiddleware(ProfileSaga);

// Adiciona ao createStore o SagaMiddleware possibilitando o uso de Side-Effects
export default createStore(profile, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(ProfileSaga);
```
#### Saga
As "Sagas" fazem uso do recurso de functions Generators disponível na API do Javascript.

[https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/function*](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/function*)

ex: 
```js
import { call, put, takeLatest, all } from "redux-saga/effects";
import { HttpService } from "../services/http.service";

function* getProfiles() {
  try {
    const httpService = new HttpService();

    const url = "https://instagram-api-cwi.herokuapp.com/profile/";
    const data = yield call(() => httpService.get(url));
	
	// Dispatch da Action 'SET_PROFILE'
    yield put({ type: "SET_PROFILE", data });
  } catch (error) {
  
	// Dispatch da Action 'SET_PROFILE_ERROR'
    yield put({ type: "SET_PROFILE_ERROR", error });
  }
}

function* watchGetProfiles() {
  /* Sempre que a Action 'GET_PROFILE' for solicitada é interceptada e
   `getProfiles()` é chamado. */
  
  yield takeLatest("GET_PROFILE", getProfiles);
}

export function* ProfileSaga() {
  yield all([watchGetProfiles()]);
}

```

Na `HomeScreen.js` não precisamos fazer alterações.
A fn mapeada `getProfiles()` chama Action 'GET_PROFILE' que vai executar a Saga(side-effect) + Reducer(update do estado).

```js
import React, { useEffect } from "react";
import { FlatList, SafeAreaView } from "react-native";
import { connect } from "react-redux";

import Post from "./post.component";

const HomeScreen = props => {
  useEffect(() => {
    props.getProfiles();
  }, []);

  return (
    <SafeAreaView style={{ flex: 1 }}>
      <FlatList
        data={props.profile.posts}
        renderItem={({ item }) => <Post post={item} />}
        keyExtractor={(item, index) => `${index}`}
      />
    </SafeAreaView>
  );
};

const mapDispatchToProps = dispatch => ({
  getProfiles: () => dispatch({ type: "GET_PROFILE" })
});

const mapStateToProps = state => ({
  profile: state.profile
});

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(HomeScreen);

```

**Código completo:**

[![Edit react-hooks-redux-saga](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/react-hooks-redux-saga-o9uw8?fontsize=14&hidenavigation=1&theme=dark)

### Alguns métodos utilizados no treinamento: 
**call:**
Descreve um efeito que instrui o middleware a invocar uma função fn com os argumentos.

Ex.:
```js
yield call(Api.fetch, '/products')
```

**put:**
Descreve um efeito que instrui o middleware a agendar o dispatch de uma action para o store. O dispatch pode não ser imediato, já que outras tarefas podem estar à frente na fila do Saga ou ainda estarem em progresso.

Ex.:
```js
yield put('GET_PRODUCTS_SUCCESS', products)
``` 


**takeLatest**
Executa uma saga toda vez que uma action cujo padrão corresponda ao pattern especificado é executada, cancelando as tasks da saga anteriores que ainda estejam em execução.

Ex:
```js
yield takeLatest('GET_PRODUCTS_REQUEST', getProducts)
``` 


**all**
Descreve um efeito que instrui o middleware a executar múltiplos efeitos em paralelo e aguardar que todos eles finalizem.

Ex:
```js
function* productsSaga() {
  yield all([takeLatest(getProductsRequest)])
}
``` 

