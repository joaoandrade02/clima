Sobre a atividade: É feito o consumo da API de um site de climatologia que consegue disponibilizar em tempo real a temperatura e outros fatores climatológicos com base em sua localização geográfica.
Para conseguir a chave, é preciso criar uma conta no site (https://home.openweathermap.org) e depois ir para o seguinte endereço: https://home.openweathermap.org/api_keys
Como realizar tal atividade:
Inicialmente, é preciso inicializar a aplicação com o create-react-app <nome do projeto>
Após isso, é preciso abrir o arquivo no VSCode e, no terminal, instalar o axios <yarn add axios>
Com isso feito, agora é preciso partir para a codificação
No meu caso, estarei colocando a codificação finalizada, pois a passo a passo pode ser vista no tutorial deixado pelo professor André no moodle.
  No App.js localiza-se a imensa maioria da codificação da aplicação:
  
  // AQUI SETAMOS AS FERRAMENTAS A SEREM UTILIZADAS
  import React, { Fragment, useEffect, useState } from 'react';
import axios from 'axios';

// NOSSOS STATES
function App() {
  const [location, setLocation] = useState(false);
  const [weather, setWeather] = useState(false);

  // POR QUE let e não const/var? PORQUE EU QUERO (BRINKS, É PORQUE let é flexível a valores em constante mudança)
  // LOGO ABAIXO ESTAMOS CONFIGURANDO O MODO QUE OS DADOS DA API APARECERÃO NA INTERFACE
  let getWeather = async (lat, long) => {
    let res = await axios.get("http://api.openweathermap.org/data/2.5/weather", {
      params: {
        lat: lat,
        lon: long,
        appid: process.env.REACT_APP_OPEN_WHEATHER_KEY,
        lang: 'pt',
        units: 'metric'
      }  
    });
    setWeather(res.data);
  }

  useEffect(()=> {
    navigator.geolocation.getCurrentPosition((position)=> {
      getWeather(position.coords.latitude, position.coords.longitude);
      setLocation(true)
    })
  }, [])
 
  if(location === false) {
    return (
      <Fragment>
        Você precisa habilitar a localização no browser o/
      </Fragment>
    )
  } else if (weather === false) {
    return (
      <Fragment>
        Carregando o clima...
      </Fragment>
    )
} else{
    return (
      <Fragment>
         <h3>Clima nas suas Coordenadas ({weather['weather'][0]['description']})</h3>
      <hr/>
      <ul>
        <li>Temperatura atual: {weather['main']['temp']}°</li>
        <li>Temperatura máxima: {weather['main']['temp_max']}°</li>
        <li>Temperatura minima: {weather['main']['temp_min']}°</li>
        <li>Pressão: {weather['main']['pressure']} hpa</li>
        <li>Humidade: {weather['main']['humidity']}%</li>
      </ul>
      </Fragment>
    );
}
}
 
export default App;

