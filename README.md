# Sistema CRUD - Banco de Dados Olimpíadas

Sistema completo de gerenciamento de dados olímpicos com interface web usando Streamlit e MySQL.

## Estrutura do Projeto

```
projeto-olimpiadas/
│
├── .env                          # Configurações do banco de dados
├── popdados.py                   # Script para popular o banco
├── interface.py                  # Interface Streamlit (CRUD)
├── olimpiadasfiltrado.csv        # Arquivo CSV com os dados
└── README.md                     # Este arquivo
```

## Instalação

### 1. Instale as dependências

```bash
pip install streamlit mysql-connector-python pandas python-dotenv
```

### 2. Configure o XAMPP

- Baixe e instale o XAMPP
- Inicie os módulos **Apache** e **MySQL**
- Acesse `http://localhost/phpmyadmin`

### 3. Crie os arquivos do projeto

#### **Arquivo 1: `.env`**
Crie um arquivo chamado `.env` na raiz do projeto:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=
DB_NAME=olimpiadas_db
```

#### **Arquivo 2: `popdados.py`**
Copie todo o código da seção "ARQUIVO 2" do artifact e cole em um arquivo chamado `popdados.py`

#### **Arquivo 3: `interface.py`**
Copie todo o código da seção "ARQUIVO 3" do artifact e cole em um arquivo chamado `interface.py`

## Como Usar

### Passo 1: Popular o Banco de Dados

Execute o script para criar e popular o banco:

```bash
python popdados.py
```

**O que ele faz:**
-  Remove o banco existente (se houver)
-  Cria o banco `olimpiadas_db`
-  Cria as tabelas: Pais, Olimpiada, Atleta, Evento, Compete
-  Importa os dados do CSV `olimpiadasfiltrado.csv`
-  Filtra dados de 2006 a 2016

**Saída esperada:**
```
✓ Conectado ao MySQL Server versão 8.x.x
  Banco de dados 'olimpiadas_db' removido (se existia)
✓ Banco de dados 'olimpiadas_db' criado com sucesso!
✓ Todas as tabelas foram criadas/verificadas com sucesso!
 Carregando CSV: olimpiadasfiltrado.csv
✓ CSV carregado: XXXXX registros
✓ Após filtro (2006-2016): XXXXX registros
...
✓ IMPORTAÇÃO CONCLUÍDA!
```

### Passo 2: Execute a Interface Web

Inicie o Streamlit:

```bash
streamlit run interface.py
```

O navegador abrirá automaticamente em `http://localhost:8501`

##  Estrutura do Banco de Dados

### Tabelas

1. **Pais**
   - `sigla` (PK) - Sigla do país (3 letras)
   - `nome` - Nome do país

2. **Olimpiada**
   - `ano` (PK) - Ano da olimpíada
   - `estacao` - Verão/Inverno
   - `sede` - Cidade sede

3. **Atleta**
   - `id_atleta` (PK, AUTO_INCREMENT)
   - `nome` - Nome do atleta
   - `sexo` - M/F
   - `peso` - Peso em kg
   - `altura` - Altura em metros
   - `idade` - Idade
   - `sigla_pais` (FK) - Referência ao país

4. **Evento**
   - `id_evento` (PK, AUTO_INCREMENT)
   - `esporte` - Nome do esporte
   - `modalidade` - Modalidade específica
   - `ano_olimpiada` (FK) - Referência à olimpíada

5. **Compete**
   - `id_atleta` (PK, FK)
   - `id_evento` (PK, FK)
   - `medalha` - Ouro/Prata/Bronze/Sem Medalha

##  Funcionalidades da Interface

### Para cada tabela você pode:

- ** Visualizar**: Lista todos os registros em formato de tabela
- ** Inserir**: Adiciona novos registros com formulário
- ** Atualizar**: Edita registros existentes
- ** Deletar**: Remove registros (com confirmação)

### Navegação

1. Selecione a tabela na **sidebar** (lado esquerdo)
2. Escolha a operação desejada
3. Preencha os formulários e execute as ações

##  Notas Importantes

### Restrições do Banco

- Não é possível deletar um **País** que tenha atletas cadastrados
- Não é possível deletar uma **Olimpíada** que tenha eventos
- Não é possível deletar um **Atleta** que tenha competições (exceto se usar CASCADE)
- Não é possível deletar um **Evento** que tenha competições

### Arquivo CSV

O arquivo `olimpiadasfiltrado.csv` deve conter as seguintes colunas:
- `nome`, `equipe`, `sigla`, `ano`, `temporada`, `cidade`
- `esporte`, `evento`, `medalha`, `peso`, `altura`, `idade`, `sexo`

**Ajuste o caminho do CSV em `popdados.py`:**
```python
CSV_FILE = 'caminho/para/seu/olimpiadasfiltrado.csv'
```

##  Repopular o Banco

Se precisar repopular o banco:

```bash
python popdados.py
```

Isso vai:
1. Deletar o banco antigo
2. Criar um novo banco limpo
3. Reimportar todos os dados do CSV

## 🛠️ Troubleshooting

### Erro: "Can't connect to MySQL server"
- ✅ Verifique se o XAMPP está rodando
- ✅ Verifique se o módulo MySQL está ativo no XAMPP

### Erro: "Unknown database 'olimpiadas_db'"
- ✅ Execute `python popdados.py` primeiro

### Erro: "No module named 'streamlit'"
- ✅ Instale as dependências: `pip install streamlit mysql-connector-python pandas python-dotenv`

### Erro ao inserir dados
- ✅ Verifique se as chaves estrangeiras existem
- ✅ Exemplo: Para inserir um Atleta, o País deve existir antes

## 📝 Customização

### Alterar o filtro de anos

Em `popdados.py`, linha final:
```python
db.processar_csv_unico(CSV_FILE, batch_size=500, 
                       ano_inicial=2006,  # Altere aqui
                       ano_final=2016)    # Altere aqui
```

### Alterar credenciais do banco

Edite o arquivo `.env`:
```env
DB_HOST=localhost
DB_USER=seu_usuario
DB_PASSWORD=sua_senha
DB_NAME=seu_banco
```

##  Suporte

Se encontrar problemas:
1. Verifique os logs no terminal
2. Confirme que o XAMPP está rodando
3. Verifique se o arquivo CSV está no local correto
4. Confirme que o arquivo `.env` está configurado corretamente

---

**Desenvolvido com ❤️ usando Streamlit + MySQL**
