Saga
- Introdução + Instalação
Consiste em uma biblioteca (i. é., um middleware para Redux) responsável por facilitar o gerenciamento dos efeitos colaterais (requisições assíncronas, como chamadas para API, transformações impuras, como acessar o cache do navegador), bem como sua testabilidade e manutenção.

- Pq, quando e onde utilizar?
Tendo em vista que o Redux suporta apenas fluxo de dados síncrono, se torna essencial o emprego de middlewares para edste gerenciamento. Algumas libs comumente utilizadas são o Redux-Thunk, Redux-Saga, Redux-Observable. O primeiro apresenta a abordagem mais simples e atende às demandas menos complexas. Por outro lado, quando gerenciamentos mais complexos são necessários (cancelamento de requisições, etc), o Redux-Saga (e o Redux-Observable) é uma ferramenta mais poderosa, fornecendo um controle mais granular através do emprego de funções Generator.

- Middleware
A ideia por trás do conceito de middlewares do Redux é muito similar à ideia utilizada em frameworks como o ExpressJS, onde é possível injetar códigos que são executados entre o request e o response de uma solicitação.
Sendo assim, o Redux permite que sejam injetadas extensões de terceiros que o manipulam durante o dispatch de uma action e o momento em que essa action chega ao reducer. Esses middlewares podem ser utilizados para realização de log, crash reporting, requisições assíncronas, etc.

- Adicionando middleware ao projeto

- call:
Descreve um efeito que instrui o middleware a invocar uma função fn com os argumentos args.
Ex.: yield call(Api.fetch, '/products')

- put:
Descreve um efeito que instrui o middleware a agendar o dispatch de uma action para o store. O dispatch pode não ser imediato, já que outras tarefas podem estar à frente na fila do Saga ou ainda estarem em progresso.
Ex.: yield put('GET_PRODUCTS_SUCCESS', products)


- takeLatest:
Executa uma saga toda vez que uma action cujo padrão corresponda ao pattern especificado é executada, cancelando as tasks da saga anteriores que ainda estejam em execução.
Ex: yield takeLatest('GET_PRODUCTS_REQUEST', getProducts)


- all:
Descreve um efeito que instrui o middleware a executar multiplos efeitos em paralelo e aguardar que todos eles finalizem.
Ex: function* productsSaga() {
  yield all(\[takeLatest(getProductsRequest)])
}

