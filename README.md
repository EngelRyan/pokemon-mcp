## MCP Pokémon – Servidor FastMCP

Este é um pequeno servidor MCP (Model Context Protocol) chamado **"poke"** que usa a **PokeAPI** para fornecer informações sobre Pokémon diretamente para o GitHub Copilot (ou outro cliente MCP compatível).

O servidor foi implementado com **FastMCP** e expõe três ferramentas principais:

- `get_pokemon_info(name: str)` – busca dados detalhados de um Pokémon.
- `create_tournament_squad()` – monta uma equipe forte de seis Pokémon para torneios.
- `list_popular_pokemon()` – lista alguns Pokémon populares competitivamente.

---

### Estrutura do projeto

- `poke.py` – implementação do servidor MCP e das ferramentas.
- `main.py` – ponto de entrada opcional (se você quiser rodar algo localmente/testes).
- `pyproject.toml` – configuração do projeto Python (dependências e metadados).

---

### Pré‑requisitos

- Python 3.10+ instalado.
- Acesso à internet (para chamar a PokeAPI em `https://pokeapi.co`).

Recomendado (mas opcional):

- Ambiente virtual (`venv`, `conda`, etc.).

---

### Instalação

No diretório do projeto (`pokemon/`):

1. Crie e ative um ambiente virtual (opcional, mas recomendado):

	```bash
	python -m venv .venv
	.venv\Scripts\activate  # Windows
	```

2. Instale as dependências definidas no `pyproject.toml` (por exemplo, usando **uv** ou **pip**):

	```bash
	pip install -e .
	```

> Se você não estiver usando um gerenciador específico, basta garantir que `httpx` e `mcp` (FastMCP) estejam instalados.

---

### Como rodar o servidor MCP

O arquivo principal do servidor é `poke.py`. Ele já chama `mcp.run(transport="stdio")` no bloco `if __name__ == "__main__":`, então você pode rodar com:

```bash
python poke.py
```

Normalmente, porém, você **não** roda o MCP diretamente no terminal; em vez disso, configura o cliente (por exemplo, GitHub Copilot em VS Code) para usar este servidor como provedor MCP. O cliente então executa o comando acima automaticamente e se comunica via `stdio`.

---

### Ferramentas disponíveis

#### `get_pokemon_info`

Retorna informações detalhadas de um Pokémon:

- Nome
- Tipos
- Habilidades
- Status base (HP, Attack, Defense, etc.)

Exemplo de chamada conceitual:

```text
get_pokemon_info(name="greninja")
```

Saída:

```text
Name: Greninja
Types: water, dark
Abilities: torrent, protean
Stats: hp: 72, attack: 95, defense: 67, special-attack: 103, special-defense: 71, speed: 122
```

#### `create_tournament_squad`

Monta uma equipe com seis Pokémon fortes para torneios, usando uma lista de tops como base (por exemplo, Charizard, Garchomp, Lucario, Dragonite, Metagross, Gardevoir) e verificando seus dados na PokeAPI.

Saída aproximada:

```text
Tournament Squad:
Charizard
Garchomp
Lucario
Dragonite
Metagross
Gardevoir
```

#### `list_popular_pokemon`

Retorna uma lista simples em texto com alguns Pokémon populares no competitivo:

```text
Charizard
Garchomp
Lucario
Dragonite
Metagross
Gardevoir
```

---

### Como usar com o GitHub Copilot (MCP)

1. Configure o arquivo `mcp.json` do VS Code (ou outro cliente) para apontar para este projeto, executando `python poke.py` no diretório do projeto.
```
{
	"servers": {
		"poke": {
			"command": "uv",
			"args": [
				"run",
				"--with",
				"mcp[cli]",
				"mcp",
				"run",
				"C:\\Users\\Windows 10 Pro\\OneDrive\\Área de Trabalho\\IA\\pokemon\\poke.py"
			]
		}
	}
}
```
2. Reinicie o Copilot / VS Code se necessário.
3. Dentro do chat do Copilot, você poderá pedir, por exemplo:

	- "Use o servidor MCP 'poke' para buscar informações sobre Greninja."
	- "Monte uma equipe de torneio usando o servidor de Pokémon."

O cliente MCP chamará as ferramentas `get_pokemon_info`, `create_tournament_squad` e `list_popular_pokemon` conforme necessário.

---

### Tutorial Steps

- ```uv init```
- ```uv venv```
- ```.venv\Scripts\activate```