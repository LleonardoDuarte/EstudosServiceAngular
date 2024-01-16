# As 4 camadas do Angular

# components

Components sao elementos visuais, customizaveis e reutilizaveis
Ex: Botao, card na tela,

# Gerenciamento de estado

Responsavel por garantir as informaçoes dos componentes nao estão dessincronizadas e facilita a comunicação entre os componentes
Ex: quando voce adiciona um item de uma loja em um carrinho

# Roteamento

Responsavel pela forma de navegar fazendo a troca de URL sem recarregar a página, mudando somente os componentes mostrados na tela

# Reinderização

Responsável por decidir a melhor maneira de acessar e entregar para o browser o componente pronto para que ele seja desenhado na janela sem complicações.

Angular e react são 100% client

# COMANDOS NG

ng new nomedoprojeto - (cria um projeto inteiro angular)
ng serve (ng s) - cria uma instancia local de servidor e inicia a aplicação
ng generate component nomedocomponente (ng g c nome) - cria um componente
ng g m nomedomodule - cria um novo módulo (adendo: normalmente por boas práticas é criado um módulo chamado shared que irá comportar todos os componentes que irao ser usado em todas as páginas criadas, além disso por boas práticas também é criado um módulo chamado pages que ira comportar todas os componentes de páginas.)

# diretivas de atributo ()

ng class - adiciona ou remove conjunto de classes css
ng style - adiciona ou remove connjunto de estilos ao html
ng model - adiciona vinculação de dados bidirecionais a um elemento de formulario

# diretivas estruturais

ng if - Condicional que verifica se modelo deve ser visualizado ou nao
ng for - repete um elemento para cada item de uma lista
ng switch - utilizado para alternar entre comportamentos alternativos

# COMANDOS GIT HUB

git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/LleonardoDuarte/EstudosAngular.git
git push -u origin main

---

git remote add origin https://github.com/LleonardoDuarte/EstudosAngular.git
git branch -M main
git push -u origin main

# ANOTAÇÕES SERVICE

- Um service serve para cuidar do serviço lógico da aplicação, o mesmo pode ser atrelado a vários componentes, porem como boa prática é indicavel conter 1 service para realizar uma tarefa especifica e nao realizar coisas distintas pois isso dificulta a manutenção de algum projeto

- O service é um arquivo provedor que realiza a comunicação com uma API por exemplo, o service trabalha como um injectable, ou seja, ele pode ser injetado em qualquer componente desejado.

- Para criar um service usa-se ng g s (g de generate e s de service) e o nome do arquivo ou pasta/nome
  exemplo: ng g s services/pokemonService

- Para injetar o service no componente desejado, no arquivo do ts do componente, dentro do construtor, voce ira realizar uma chamada colocando o private dando um nome e tipando com o nome export do service.
  exemplo: private service:pokemonService

# CONSUMO DE API

- Para consumir uma api com o angular primeiramente no arquivo environment.ts dentro do export nos damos um nome a api (url, api, o que voce quiser) e colocamos o website lá.

- Para conseguir chamar a API dentro do nosso service primeiramente importamos o arquivo environment dentro do nosso service, depois criamos um private com um nome que desejamos dar com uma string vazia '', apos isso dentro dos parenteses do nosso construtor fazemos uma chamada com o nome, exemplo:
  <!-- em baixo da classe -->
  export class PokemonService {
  private baseURL: string = '';}

<!-- dentro do constructor -->

constructor() {
this.baseURL = environment.pokeApi;
}

- Para realizar uma requisição HTTP primeiramente necessitamos importar o httpclientmodule dentro do nosso arquivo app.module.ts e colocar o import dentro dos imports, isso serve para que possamos usar todas as funções do pacote http.
  import:import { HttpClientModule } from '@angular/common/http';

  - No arquivo service podemos entao já usar o arquivo httpcliente que iremos importar do mesmo pacote do httpclientmodule e depois criaremos dentro do constructor um private http:httpClient, depois já podemos usar o método onde quisermos passando o this.http.método

    - Também é importante lembrar que apos as tipagens, ja no nosso componente é necessario realizar um .subscribe apos a chamada do nosso getpokemon passando um next e um error para que funcione a chamada, se nao nada irá ser exibido na tela.

  # Observables

  - Observable é um método angular muito parecido com as promises do javascript, ou seja, em promises se nao usarmos um await para as chamadas nao iremos retornar os dados, normalmente se retorna um objeto vazio, no angular é a mesma coisa porem nao são promises e sim observables. Para usar o observable primeiramente colocamos ele apos a tipagem da nossa função, o observable possui um argumento que necessita ser colocado, no caso ele espera uma tipagem para que possa funcionar, normalmente cria-se um export type em algum outro arquivo e usa-se nele como typagem generic <>

  # Anotações de código

  # service

  import { Injectable } from '@angular/core';

import { Injectable } from '@angular/core';
import { environment } from 'src/environments/environment';
@Injectable({
providedIn: 'root',
})
export class PokeApiService {
private baseURL: string = '';

constructor() {
this.baseURL = environment.pokeAPI;
}
getPokemon(pokemonName: String) {
console.log(this.baseURL);
}
}

# PARTE 2

import { Injectable } from '@angular/core';
import { environment } from 'src/environments/environment';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { PokemonData } from '../model/pokeType';

@Injectable({
providedIn: 'root',
})
export class PokeApiService {
private baseURL: string = '';
private pokeData: PokemonData | any;

constructor(private http: HttpClient) {
this.baseURL = environment.pokeAPI;
}
getPokemon(pokemonName: String): Observable<PokemonData> {
this.pokeData = this.http.get<PokemonData>(`${this.baseURL}${pokemonName}`);
return this.pokeData;
}
}

# PARTE 3

# card component

import { Component, OnInit } from '@angular/core';
import { PokeApiService } from 'src/app/services/poke-api.service';

@Component({
selector: 'app-card',
templateUrl: './card.component.html',
styleUrls: ['./card.component.css'],
})
export class CardComponent implements OnInit {
constructor(private service: PokeApiService) {}

ngOnInit(): void {
this.service.getPokemon('charizard');
}
}

# PARTE 2

import { Component, OnInit } from '@angular/core';
import { PokeApiService } from 'src/app/services/poke-api.service';

@Component({
selector: 'app-card',
templateUrl: './card.component.html',
styleUrls: ['./card.component.css'],
})
export class CardComponent implements OnInit {
constructor(private service: PokeApiService) {}

ngOnInit(): void {
this.service.getPokemon('charizard').subscribe({
next: (res) => console.log(res),
error: (error) => console.log(error),
});
}
}

# PARTE 3

import { Component, OnInit } from '@angular/core';
import { PokeApiService } from 'src/app/services/poke-api.service';

@Component({
selector: 'app-card',
templateUrl: './card.component.html',
styleUrls: ['./card.component.css'],
})
export class CardComponent implements OnInit {
constructor(private service: PokeApiService) {}

ngOnInit(): void {
this.service.getPokemon('charizard').subscribe({
next: (res) => {
console.log(res),
console.log(res.id)
console.log(res.sprites.front_default);

      },
      error: (error) => console.log(error),
    });

}
}

# PARTE 4 GUARDANDO EM UMA VARIAVEL

import { Component, OnInit } from '@angular/core';
import { PokemonData } from 'src/app/model/pokeType';
import { PokeApiService } from 'src/app/services/poke-api.service';

@Component({
selector: 'app-card',
templateUrl: './card.component.html',
styleUrls: ['./card.component.css'],
})
export class CardComponent implements OnInit {
pokemon?: PokemonData;
constructor(private service: PokeApiService) {}

ngOnInit(): void {
this.service.getPokemon('charizard').subscribe({
next: (res) => {
this.pokemon = {
id: res.id,
name: res.name,
sprites: res.sprites,
types: res.types,
};
console.log(this.pokemon);

        // o codigo acima é a mesma coisa que o codigo abaixo porem mais organizado dentro de uma variavel
        // console.log(res);
        // console.log(res.name);
        // console.log(res.id);
        // console.log(res.sprites.front_default);
        // console.log(res.types);
      },
      error: (error) => console.log(error),
    });

}
}
