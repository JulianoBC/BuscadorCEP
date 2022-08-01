import { useState} from 'react'
import { FiSearch } from 'react-icons/fi';
import './styles.css';
//viacep.com.br/ws/${input}/jasons/
import api from './services/api.js';

function App() {

  const [input, setInput] = useState('');
  const [cep, setCep] = useState({});

  async function handleSearch(){
    if(input === '') {
      alert('Preencha algum CEP !')
      return; 
    }

    try{
    const response = await api.get(`${input}/json/`);
      setCep(response.data)
      setInput("");

    }catch{
      alert("ops error ao buscar");
      setInput("")
    }
  }

  return (
    <div className="container">
      <h1 className="title">Buscador CEP</h1>

      <div className="containerInput">
        <input
        type="text" 
        placeholder="Digite seu cep..."
        maxLength={8}
        value={input}
        onChange={(event) => setInput(event.target.value) } 
        />

        <button id = "btn" className="buttonSearch" onClick={handleSearch}>
          <FiSearch size={25} color="FFF"/>
        </button>
      </div>



      {Object.keys(cep).length > 0 && (
        
        <main className="main">
        <h2>CEP: {cep.cep}</h2>

        <span>Rua: {cep.logradouro}</span>
        <span>Complemento: {cep.complemento}</span>
        <span>Bairro: {cep.bairro}</span>
        <span>Cidade: {cep. localidade} - {cep.uf}</span>

      </main>  
      )}
      
    </div>
  );
}

export default App;
