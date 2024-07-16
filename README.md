API de Pokémon

Esta aplicación utiliza la PokéAPI para obtener datos de los Pokémon. Aquí tienes un resumen de cómo se manejan las solicitudes:

Se obtiene un conjunto de Pokémon aleatorio usando un offset aleatorio.
Se seleccionan 20 Pokémon de la lista obtenida.
Para cada Pokémon, se hace una solicitud para obtener su imagen y nombre.


Solicitud a la API :

//const randomOffset = Math.floor(Math.random() * 151);
const URL_BASE = `https://pokeapi.co/api/v2/pokemon/?limit=60&offset=${randomOffset}`;

try {
  const responseApi = await axios.get(URL_BASE);
  const pokemonList = responseApi.data.results;

  const shuffledPokemons = pokemonList.sort(() => 0.5 - Math.random());
  const selectedPokemons = shuffledPokemons.slice(0, 20);

  const pokemonResponse = await Promise.all(
    selectedPokemons.map(async (pokemon) => {
      const { data } = await axios.get(pokemon.url);
      return {
        name: data.name,
        image: data.sprites.other.dream_world.front_default,
        guessed: false,
      };
    })
  );

  return pokemonResponse;
} catch (error) {
  console.error("Error fetching Pokémon data: ", error);
  return [];
}
//



# entrega

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
