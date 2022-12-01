# LNG API aaaaaaa BBBBaBB

Essa jovem API faz analises em textos visando uma Linguagem Neutra de Gênero.

Uma escrita neutra de gênero eviasdasdta: 
- **generalizações masculinas** como  "_João e Maria são trabalhadores_", sugerindo "_João e Maria são pessoas trabalhadoras_";
- **o gênero masculino implícito** como "_precisamos pensar no usuário do aplicativo_", sugerindo "_precisamos pensar em quem usa o aplicativo_"

Essa API não sugere alternativas informais da língua portuguesa (_ex: todes, elu, amigxs, usuári@s, etc_).


> Feito com Python, FastAPI e 💚

## Como o algoritmo funciona 

A análise textual é feita utilizando a [bilioteca NLTK](https://www.nltk.org/) com o complemento [dessas POS-taggers para português](https://github.com/inoueMashuu/POS-tagger-portuguese-nltk) que possibilitam distinguir alguns termos entre verbo/adjetivo/substantivo/etc, e tornar a análise mais assertiva.


## Como executar
> Requisito: Python versão 3.8 ou mais recente

Crie um ambiente virtual utilizando o módulo [venv](https://docs.python.org/pt-br/3/library/venv.html)

```bash
python3 -m venv .venv-swf
```

Ative o ambiente virtual criado

```bash
source .venv-swf/bin/activate
```

Instale as dependências

```bash
python3 -m pip install -r dev-requirements.txt
```

Suba a aplicação localmente com o comando

```bash
python3 -m uvicorn main:app --reload
```

## Base de expressões analisadas

O arquivo `feedbacks.json` armazena as expressões que serão analisadas pelo algoritmo e as alternativas que serão sugeridas pela API, seguindo o formato a seguir:

```
{
  "feedbacks": {
    "os estudantes": "os_estudantes",
    "os trabalhadores": "os_trabalhadores"
  },
  "alternatives": {
    "os_estudantes": ["as pessoas que estudam", "quem estuda", "as pessoas estudantes"],
    "os_trabalhadores": ["quem trabalha", "as pessoas trabalhadoras"]
  }
}
```

👀 É possível realizar uma análise com termos e sugestões customizados, basta informá-los para a API no Body da chamada `POST "/"`. 

Para o caso de `"os trabalhadores"`, qualquer ocorrência de `trabalhadores` será alertada, pois assume-se que já existe uma generalização masculina do plural. 
> Exemplo: "Temos **trabalhadores** felizes na nossa empresa" 🔴

Para o caso de `"os estudantes"`, serão alertadas ocorrências de `estudantes` precedida ou sucedida de uma palavra com generalização masculina do plural (_palavras que não sejam verbos e terminem com "os", "ores", "ões", "ns" ou "ãos"_)
> Exemplo: "Temos estudantes felizes na nossa escola" 🟢 / "Temos **estudantes ativos** na nossa escola" 🔴

A ocorrência do termo `pessoas` precedendo `trabalhadores` ou `estudantes` cancelará o alerta.
