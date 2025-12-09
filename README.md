# LinkedIn Profiles Outreach Automation

Um projeto Python que automatiza o processo de prospecção e envio de mensagens personalizadas em perfis do LinkedIn, utilizando web scraping com Bright Data, integração com Google Sheets e geração de mensagens com Google Gemini.

## 📋 Visão Geral

Este projeto combina várias APIs e ferramentas para criar um pipeline completo de outreach em LinkedIn:

1. **Coleta de dados** - Web scraping via Bright Data API para buscar perfis do LinkedIn no Google (usando indexação comum)
2. **Processamento de currículo** - Extração automática de informações do currículo em PDF usando Gemini
3. **Geração de mensagens personalizadas** - Criação de mensagens customizadas com IA (Google Gemini)
4. **Integração com Google Sheets** - Armazenamento e gerenciamento de dados dos contatos

## 🔧 Tecnologias Utilizadas

- **Python 3.x**
- **Bright Data API** - Para web scraping de perfis LinkedIn
- **Google Gemini API** - Para extração de currículo e geração de mensagens
- **Google Sheets API** - Para gerenciamento de dados
- **gspread** - Cliente Python para Google Sheets
- **Pydantic** - Validação de dados
- **requests** - Requisições HTTP

## 📦 Dependências

```
requests
json
gspread
google-oauth2
googleapiclient
pydantic
google-genai
```

## 🚀 Instalação

1. Clone o repositório:
```bash
git clone <seu-repositorio>
cd linkedin-outreach-automation
```

2. Instale as dependências caso esteja em ambiente local:
```bash
pip install -r requirements.txt
```

3. Configure as credenciais no Google Colab ou localmente:
   - **Bright Data API Key** - Obtenha em [Bright Data](https://brightdata.com)
   - **Google Gemini API Key** - Configure em [Google AI Studio](https://ai.google.dev)
   - **Google Sheets Credentials** - Baixe o arquivo JSON da sua conta Google

4. Configure as constantes no notebook/script:
   - `BRIGHT_DATA_API_KEY` - Sua chave de API do Bright Data
   - `SPREADSHEET_ID` - ID da planilha Google Sheets
   - `CURRICULO_PATH` - Caminho para seu PDF de currículo

## 💻 Uso

### 1. Extrair Informações do Currículo

O projeto processa um arquivo PDF do currículo e extrai automaticamente, salvando a resposta para evitar gastos desnecessários:
- Nome completo
- Localização
- Email
- Telefone
- Introdução/Bio
- Experiência profissional
- Habilidades técnicas

```python
curriculo = extrair_curriculo_gemini('/content/curriculo_erick.pdf')
```

### 2. Buscar Perfis no LinkedIn

Utiliza a API do Bright Data para buscar perfis que correspondam aos critérios:
- Engenheiros de Software
- Recrutadores Técnicos
- Profissionais de Talent Acquisition
- Especializações em React.js, Next.js, TypeScript/JavaScript

```python
# A API realiza busca com palavras-chave parametrizadas na ferramenta de busca do Google
# Retorna URLs e informações dos perfis encontrados
```

### 3. Gerar Mensagens Personalizadas

Cria mensagens de outreach em inglês, usando template pronto ou sendo em forma personalizada com Gemini:

```python
mensagem = gerar_mensagem_template(
    nome="John Doe",
    empresa="Google",
    cargo="Senior Technical Recruiter",
    curriculo=curriculo
)
```

As mensagens seguem uma estrutura profissional:
- Saudação personalizada
- Apresentação com skills relevantes
- Proposta de valor
- Call-to-action

## 📁 Estrutura do Projeto

```
linkedin-outreach-automation/
├── notebook.ipynb           # Notebook principal do Colab
├── requirements.txt         # Dependências Python
├── credentials.json         # Credenciais Google (não commitar)
└── README.md               # Este arquivo
```

## 🔐 Variáveis de Ambiente

Configure as seguintes variáveis:

- `BRIGHT_DATA_API_KEY` - Chave de API Bright Data
- `SPREADSHEET_ID` - ID da planilha Google Sheets
- `CURRICULO_PATH` - Caminho do PDF do currículo

## 🎯 Funcionalidades Principais

### Funções Disponíveis

- **`extrair_curriculo_gemini(path)`** - Extrai dados do currículo em PDF
- **`gerar_mensagem_gemini(nome, empresa, cargo, curriculo)`** - Gera mensagem com IA
- **`gerar_mensagem_template(nome, empresa, cargo, curriculo)`** - Gera mensagem com template
- **`salvar_json(dados, caminho)`** - Salva dados em JSON
- **`carregar_json(caminho)`** - Carrega dados de JSON

## 📊 Fluxo de Funcionamento

```
1. Extrair dados do currículo (PDF → JSON via Gemini)
2. Buscar perfis no LinkedIn (Google Search via Bright Data)
3. Para cada perfil encontrado:
   - Extrair nome, empresa, cargo
   - Gerar mensagem personalizada
   - Armazenar em Google Sheets
4. Exportar resultados
```

## ⚙️ Configuração de Busca

Os filtros de busca incluem:
- **Plataforma**: LinkedIn
- **Termos**: Software Engineer, Technical Recruiter, Talent Acquisition
- **Tecnologias**: React.js, Next.js, TypeScript, JavaScript
- **Idioma**: Inglês
- **País**: Estados Unidos
- **Páginas**: 1-10

## 📝 Estrutura de Dados - Currículo

```json
{
  "firstName": "string",
  "lastName": "string",
  "location": "string",
  "emailAddress": "string",
  "telephoneNumber": "string",
  "introduction": "string",
  "experience": [
    {
      "company": "string",
      "jobTitle": "string",
      "description": "string"
    }
  ],
  "skills": ["string"]
}
```

## 🔄 Processamento em Background

O projeto utiliza o Bright Data Dataset para:
1. Disparar uma coleta (snapshot)
2. Aguardar o status ficar "ready"
3. Fazer download dos resultados em JSON

## 📚 API Endpoints Utilizados

- **Bright Data**: `https://api.brightdata.com/datasets/v3/`
- **Google Sheets**: `https://www.googleapis.com/`
- **Google Gemini**: Integração via SDK Python

## ⚠️ Limitações e Considerações

- O projeto roda em Google Colab
- Respeitar rate limits das APIs
- Garantir conformidade com Termos de Serviço do LinkedIn
- Armazenar credenciais de forma segura
- PDF do currículo deve estar em formato legível

## 🛠️ Troubleshooting

### Erro de autenticação no Google Sheets
- Verifique o arquivo `credentials.json` que é feito usando Dashboard do GCP + IAM 
- Confirme as permissões da conta de serviço, o e-mail gerado deve ser adicionado como editor dentro da planilha criada no Google Sheets

### Gemini não extrai dados do PDF
- Verifique se o PDF é legível (não criptografado)
- Tente com um PDF de qualidade maior

### Bright Data não retorna resultados
- Confirme o dataset ID
- Verifique a cota disponível de requisições
- Valide a chave de API

## 📄 Licença

Este projeto é de uso pessoal.

## 👤 Autor

Desenvolvido para automação de outreach em busca de oportunidades de emprego por https://github.com/itseriqq.

## 🤝 Contribuições

Sinta-se à vontade para melhorar este projeto!

Assinado por
Erick :)
