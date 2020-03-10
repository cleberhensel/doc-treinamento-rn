
# Class Component

```javascript
import React, { Component } from 'react'
import { Text, View } from 'react-native'

export default class BasicComponent extends Component {
    // Método obrigatório, retorna o JSX que será renderizado na tela
    render() {
        return <Text>Estrutura básica de um component</Text>
    }
}
```

Ao herdar de `Component` passamos também a ter acesso aos métodos do ciclo de vida do componente.
```javascript
export default class BasicComponent extends Component {
    // O construtor é chamado antes que este componente seja montado
    constructor(props) {
        // Deve ser a primeira coisa a ser chamada
        // Caso contrário o this.props fica undefined no construtor, o que pode gerar bugs
        super(props)
    }

    // Invocado somente uma vez, antes de renderizar o componente pela primeira vez
    componentWillMount() {}

    // Invocado depois que o componente tiver terminado de renderizar
    componentDidMount() {}

    // Invocado quando alguma prop ou state muda, executado ANTES do render
    componentWillUpdate(newProps, newState) {}

    // Invocado quando alguma prop ou state muda, executado APÓS do render
    componentDidUpdate(prevProps, prevState) {}

    // Invocado antes do componente ser desmontado
    componentWillUnmount() {}

	// Método obrigatório, retorna o JSX que será renderizado na tela
	// Invocado sempre que ocorre uma mudança em alguma prop ou state do componente
    render() {
        return <Text>Estrutura básica de um component</Text>
    }
}
```

Para customizar os componentes com valores trazidos, muitas vezes, das telas que eles estão inseridos, utilizamos as `props`.
```javascript
import React, { Component } from 'react'
import { Text, View } from 'react-native'

export default class BasicComponent extends Component {
    render() {
        return <Text>{this.props.title}</Text>
    }
}

// Utilização do BasicComponent em uma tela, passando a prop title
export default class BasicScreen extends Component {
    render() {
        return <BasicComponent title={'Título'}>
    }
}
```

Outro cenário comum é utilizarmos `state` para controlar os diferentes estados que nossos componentes podem ter. No exemplo, utilizamos o `state` para armazenar o valor digitado no input de texto.
```javascript
import React, { Component } from 'react'
import { Text, View } from 'react-native'

export default class InputComponent extends Component {
	constructor (props) { 
		super(props) 
		this.state = { input: '' } 
	}
	
	handleChangeInput = (text) => { this.setState({ input: text }) }

	render () {
		const { input } = this.state
		
		return <TextInput onChangeText={this.handleChangeInput} value={input}/>
	}
}
```

Exemplo utilizando  `props` e `state`
```javascript
export class MovieCard extends Component {
    render() {
	    const { title, year, director } = this.props
        return (
			<View>
				<Text>Titulo Original: {title}<Text>
				<Text>Ano de Lançamento: {year}<Text>
				<Text>Diretor: {director}<Text>
			<View>
		)       
    }
}
```

```javascript
export class MovieList extends Component {
    constructor() {
        this.state = [ movies: ...this.service.getMovies()]
    }
    
    render() {
	    const { movies } = this.state
        return { movies.map((movie) => (
	        <MovieCard
				title={movie.title}
				year={movie.year}
				director={movie.director}
			/>
        )}
    }
}
```
