# Stateless Component

```js
import React from 'react'
import {Text, View} from 'react-native'

const component = () => <View>
                            <Text>Estrutura Básica de um component</Text>
                        </View>

export default component
```

Acesso a `Props`

```js
import React from 'react'
import {Text, View} from 'react-native'

const component = (props) => <View>
                                <Text>{props.title}</Text>
                            </View>

export default component
```

Com a adição dos `Hooks` a api do React conseguimos utilizar `State` tbm em stateless components

https://pt-br.reactjs.org/docs/hooks-intro.html

https://pt-br.reactjs.org/docs/hooks-overview.html

## useState

```js
import React, { useState } from "react";
import { Button, Text, View } from "react-native";

const App = () => {
  // useState retorna um Array [dados, método para update do estado]
  const [todos, addTodo] = useState([]);

  return (
    <View>
      <Text>Total de Tarefas {todos.length}.</Text>
      {todos.map((todo, index) => (
        <Text key={index}>{todo}</Text>
      ))}
      <Button
        onPress={() => addTodo([...todos, `task - ${todos.length + 1}`])}
        title={`Adicionar Task`}
      >
        Adicionar Task
      </Button>
    </View>
  );
};

export default App;

```

[![Edit react-native-treinamento](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/hungry-thunder-l423b?fontsize=14&hidenavigation=1&theme=dark)

## useEffect
o hook `useEffect` pode resumir até 4 métodos do Ciclo de Vida do React

```js
import React, { useEffect, useState } from "react";
import { Text, View, Button } from "react-native";

const App = () => {
  const [count, setCount] = useState(0);
  useEffect(() => {
    // componentDidMount
    const interval = setInterval(() => {
      setCount(count + 1);
    }, 1000);

    return () => {
      // componentWillUnmount
      clearInterval(interval);
    };
  }, [count]); // componentDidUpdate -  só executa o efeito quando count mudar

  return (
    <View>
      <Text>Você clicou {count} vezes</Text>
      <Button title={`Clique Aqui`} onClick={() => setCount(count + 1)}>
        Clique aqui
      </Button>
    </View>
  );
};

export default App;
```

[![Edit react-hook-use-effect](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/crazy-torvalds-qg6zo?fontsize=14&hidenavigation=1&theme=dark)